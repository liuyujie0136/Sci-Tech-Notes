# Single Cell RNA-seq Code with Comments

> 10X 单细胞测序数据分析

## 数据预处理

```r
#--- loading ---#
library(BiocFileCache)
#bfc <- BiocFileCache("raw_data", ask = FALSE)
#raw.path <- bfcrpath(bfc, file.path("http://cf.10xgenomics.com/samples",
#    "cell-exp/2.1.0/pbmc4k/pbmc4k_raw_gene_bc_matrices.tar.gz"))
#untar(raw.path, exdir=file.path(tempdir(), "pbmc4k"))

library(DropletUtils)
#fname <- file.path(tempdir(), "pbmc4k/raw_gene_bc_matrices/GRCh38")

fname="data/raw_gene_bc_matrices/GRCh38"
sce.pbmc <- read10xCounts(fname, col.names=TRUE)

#--- gene-annotation ---#
library(scater)
rownames(sce.pbmc) <- uniquifyFeatureNames(
    rowData(sce.pbmc)$ID, rowData(sce.pbmc)$Symbol)

#library(EnsDb.Hsapiens.v86)
#location <- mapIds(EnsDb.Hsapiens.v86, keys=rowData(sce.pbmc)$ID, 
#    column="SEQNAME", keytype="GENEID")

#--- cell-detection ---#
set.seed(100)
e.out <- emptyDrops(counts(sce.pbmc))
sce.pbmc <- sce.pbmc[,which(e.out$FDR <= 0.001)]

#--- quality-control ---#
#stats <- perCellQCMetrics(sce.pbmc, subsets=list(Mito=which(location=="MT")))
stats <- perCellQCMetrics(sce.pbmc, subsets=list(Mito=which(grepl("^MT-",rownames(sce.pbmc)))))
high.mito <- isOutlier(stats$subsets_Mito_percent, type="higher")
sce.pbmc <- sce.pbmc[,!high.mito]

#--- normalization ---#
library(scran)
set.seed(1000)
clusters <- quickCluster(sce.pbmc)
sce.pbmc <- computeSumFactors(sce.pbmc, cluster=clusters)
sce.pbmc <- logNormCounts(sce.pbmc)

#--- variance-modelling ---#
set.seed(1001)
dec.pbmc <- modelGeneVarByPoisson(sce.pbmc)
top.pbmc <- getTopHVGs(dec.pbmc, prop=0.1)

#--- dimensionality-reduction ---#
set.seed(10000)
sce.pbmc <- denoisePCA(sce.pbmc, subset.row=top.pbmc, technical=dec.pbmc)

set.seed(100000)
sce.pbmc <- runTSNE(sce.pbmc, dimred="PCA")

set.seed(1000000)
sce.pbmc <- runUMAP(sce.pbmc, dimred="PCA")
```

```r
library(SingleR)
library(RColorBrewer)
sce.pbmc = readRDS(file="data/sce.pbmc_after_qc.Rds")
```

## 聚类分析

### 使用`louvain`算法进行聚类

`louvain`算法是一个基于图的聚类算法，我们首先构建Shared Nearest Neighbor（SNN,两个细胞共有的邻居）图，输入为使用主成分分析（PCA）降维后得到的矩阵，默认使用前50个主成分（PC），'d=50'。在得到SNN图`g`之后，我们使用它作为`louvain`算法的输入。

```r
g <- buildSNNGraph(sce.pbmc, k=10, use.dimred = 'PCA')
clust <- igraph::cluster_louvain(g)$membership

sce.pbmc$cluster <- factor(clust)
table(clust)
```

### 使用`tSNE`对聚类进行可视化

```r
plotReducedDim(sce.pbmc, "TSNE", colour_by="cluster",text_by="cluster")
```

## 寻找聚类特异表达基因

在获得了聚类之后，我们想要知道每个聚类都哪些高表达的基因，这些基因往往是能区分不同细胞类型的marker gene，也可以帮助我们了解不同聚类的生物学功能。

```r
markers.pbmc <- findMarkers(sce.pbmc, sce.pbmc$cluster, 
    pval.type="all", direction="up") #up-regulated in 1 than 2
```

```r
markers.pbmc[[1]][order(markers.pbmc[[1]]$FDR),] 
#Do not choose FDR<0.05 as threshold due to lack of strict mathematical analysis and statistical hypothesis
```

```r
# NOTE: this is not efficient for large iteration.
marker_genes = c()

for (i in 1:length(markers.pbmc)){
  tmp = markers.pbmc[[i]][order(markers.pbmc[[i]]$FDR),]
  marker_genes = c(marker_genes, rownames(tmp)[1:5])
}

marker_genes = unique(marker_genes)
marker_genes
```

```r
plotExpression(sce.pbmc, x=I(factor(sce.pbmc$cluster)), 
    features="MS4A1")
```

使用热图对marker gene进行可视化

```r
getPalette = colorRampPalette(brewer.pal(9, "Set1"))

anno_df = as.data.frame(colData(sce.pbmc))
anno_df = anno_df[,"cluster", drop=FALSE]

col_cluster = getPalette(nlevels(anno_df$cluster))
names(col_cluster) =  levels(anno_df$cluster)

annotation_colors = list(cluster=col_cluster)

tmp_expr = logcounts(sce.pbmc)[marker_genes,]
tmp_expr = t(scale(t(tmp_expr)))
tmp_expr[tmp_expr<(-2.5)]=-2.5
tmp_expr[tmp_expr>2.5]=2.5
colnames(tmp_expr) = colnames(sce.pbmc)

pheatmap::pheatmap(tmp_expr[,order(sce.pbmc$cluster)],
         cluster_cols = FALSE, 
         cluster_rows = FALSE,
         annotation_col = anno_df,
         annotation_colors=annotation_colors,
         show_colnames = FALSE,
         fontsize_row=6)
```

## 使用`singleR`进行细胞类型注释

> 以下一步需要下载文件，较难进行

```r
ref <- BlueprintEncodeData()
pred <- SingleR(test=sce.pbmc, ref=ref, labels=ref$label.main)
table(pred$labels)
```

```r
plotScoreHeatmap(pred)
```

```r
tab <- table(Assigned=pred$pruned.labels, Cluster=sce.pbmc$cluster)

# Adding a pseudo-count of 10 to avoid strong color jumps with just 1 cell.
library(pheatmap)
pheatmap(log2(tab+10), color=colorRampPalette(c("white", "blue"))(101))
```

## 数据整合

这里我们使用`MNN`算法整合多个数据并消除批次效应（batch effects）

首先对两个PBMC单细胞数据集进行预处理

```r
#--- loading ---#
#library(TENxPBMCData)
#pbmc4k <- TENxPBMCData('pbmc4k')
pbmc4k <- readRDS(file="data/pbmc4k.Rds")

#--- quality-control ---#
is.mito <- grep("^MT-", rowData(pbmc4k)$Symbol_TENx)

library(scater)
stats <- perCellQCMetrics(pbmc4k, subsets=list(Mito=is.mito))
high.mito <- isOutlier(stats$subsets_Mito_percent, type="higher")
pbmc4k <- pbmc4k[,!high.mito]

#--- normalization ---#
pbmc4k <- logNormCounts(pbmc4k)

#--- variance-modelling ---#
library(scran)
dec4k <- modelGeneVar(pbmc4k)
chosen.hvgs <- getTopHVGs(dec4k, prop=0.1)

#--- dimensionality-reduction ---#
set.seed(10000)
pbmc4k <- runPCA(pbmc4k, subset_row=chosen.hvgs, ncomponents=25,
    BSPARAM=BiocSingular::RandomParam())

set.seed(100000)
pbmc4k <- runTSNE(pbmc4k, dimred="PCA")

set.seed(1000000)
pbmc4k <- runUMAP(pbmc4k, dimred="PCA")

#--- clustering ---#
g <- buildSNNGraph(pbmc4k, k=10, use.dimred = 'PCA')
clust <- igraph::cluster_walktrap(g)$membership
pbmc4k$cluster <- factor(clust)
```

```r
#--- loading ---#
library(TENxPBMCData)
pbmc3k <- TENxPBMCData('pbmc3k')

#--- quality-control ---#
is.mito <- grep("MT", rowData(pbmc3k)$Symbol_TENx)

library(scater)
stats <- perCellQCMetrics(pbmc3k, subsets=list(Mito=is.mito))
high.mito <- isOutlier(stats$subsets_Mito_percent, type="higher")
pbmc3k <- pbmc3k[,!high.mito]

#--- normalization ---#
pbmc3k <- logNormCounts(pbmc3k)

#--- variance-modelling ---#
library(scran)
dec3k <- modelGeneVar(pbmc3k)
chosen.hvgs <- getTopHVGs(dec3k, prop=0.1)

#--- dimensionality-reduction ---#
set.seed(10000)
pbmc3k <- runPCA(pbmc3k, subset_row=chosen.hvgs, ncomponents=25,
    BSPARAM=BiocSingular::RandomParam())

set.seed(100000)
pbmc3k <- runTSNE(pbmc3k, dimred="PCA")

set.seed(1000000)
pbmc3k <- runUMAP(pbmc3k, dimred="PCA")

#--- clustering ---#
g <- buildSNNGraph(pbmc3k, k=10, use.dimred = 'PCA')
clust <- igraph::cluster_louvain(g)$membership
pbmc3k$cluster <- factor(clust)
```

由于两个数据集所表达的基因不尽相同，我们取两个数据集表达基因的交集并且过滤掉不同的基因，以保证两个数据集的每一行都是一样的基因。

```r
universe <- intersect(rownames(pbmc3k), rownames(pbmc4k))
length(universe)
```

```r
# Subsetting the SingleCellExperiment object.
pbmc3k <- pbmc3k[universe,]
pbmc4k <- pbmc4k[universe,]

# Also subsetting the variance modelling results, for convenience.
dec3k <- dec3k[universe,]
dec4k <- dec4k[universe,]
```

```r
combined.dec <- combineVar(dec3k, dec4k)
chosen.hvgs <- combined.dec$bio > 0
sum(chosen.hvgs)
```

```r
rescaled <- multiBatchNorm(pbmc3k, pbmc4k)
pbmc3k <- rescaled[[1]]
pbmc4k <- rescaled[[2]]
```

```r
# Synchronizing the metadata for cbind()ing.
rowData(pbmc3k) <- rowData(pbmc4k)
colData(pbmc3k) = colData(pbmc3k)[,colnames(colData(pbmc4k))]
pbmc3k$batch <- "3k"
pbmc4k$batch <- "4k"
uncorrected <- cbind(pbmc3k,pbmc4k)

# Using RandomParam() as it is more efficient for file-backed matrices.
library(scater)
uncorrected <- runPCA(uncorrected, subset_row=chosen.hvgs,
    BSPARAM=BiocSingular::RandomParam())
```

```r
uncorrected <- runTSNE(uncorrected, dimred="PCA")
plotTSNE(uncorrected, colour_by="batch")
```

```r
# Using randomized SVD here, as this is faster than 
# irlba for file-backed matrices.
mnn.out <- fastMNN(pbmc3k, pbmc4k, d=50, k=20, subset.row=chosen.hvgs,
    BSPARAM=BiocSingular::RandomParam(deferred=TRUE))
mnn.out
```

```r
library(scater)
mnn.out <- runTSNE(mnn.out, dimred="corrected")

mnn.out$batch <- factor(mnn.out$batch)
plotTSNE(mnn.out, colour_by="batch")
```

```r
ggplot()+
  geom_point(alpha=0.5,size=0.5,aes(x=reducedDim(mnn.out,"TSNE")[,1],y=reducedDim(mnn.out,"TSNE")[,2],col=mnn.out$batch))+
  theme_bw()
```

