# WEHI RNA-seq Workshop - Code with Comments
> School of Life Sciences, Nanjing University
>
> 2019.11.30

```r
#Load packages
library(limma)
library(edgeR)
library(Mus.musculus)
library(RColorBrewer)

#Load data
files=list.files(pattern='txt$')
files

read.delim(files[1],nrow=5)

#Read and Merge a Set of Files Containing Count Data
x=readDGE(files,columns=c(1,3))
class(x)
dim(x)
x

#Create sample names
samplenames=substring(colnames(x),12,nchar(colnames(x)))
samplenames
colnames(x)=samplenames

#Add group labels
group=as.factor(c('LP','ML','Basal','Basal','ML','LP','Basal','ML','LP'))
x$samples$group=group

#Add lane labels
lane=as.factor(rep(c('L004','L006','L008'),c(3,4,2)))
x$samples$lane=lane
x$samples

#Gene annotation
geneid=rownames(x)
genes=select(Mus.musculus,keys=geneid,columns=c('SYMBOL','TXCHROM'),keytype='ENTREZID') #AnnotationDbi::select
head(genes) #First 6 rows

genes=genes[!duplicated(genes$ENTREZID),] #Remove duplications
x$genes=genes
x


#Data pre-processing
#Filtering
table(rowSums(x$counts==0)==9) #Genes with zero counts in all samples

keep.exprs=filterByExpr(x,group=group) #Remove genes with very low counts across all groups
x=x[keep.exprs,,keep.lib.sizes=F]
dim(x)

#Normalisation: Remove systematic bias due to technical effects
x=calcNormFactors(x,method='TMM')
x$samples$norm.factors

#Data visualisation
#Boxplots
lcpm=cpm(x,log=T) #log Counts per Million
boxplot(lcpm,las=2,col='grey',main="Distribution of samples",ylab="Log-cpm") #las=1: all axis labels horizontal,las=2: vertical

#MDS plots: Multidimensional scaling plot
plotMDS(lcpm)
plotMDS(lcpm,labels=group)

#colour by group
col.group=group
levels(col.group)=brewer.pal(nlevels(col.group),'Set1')
plotMDS(lcpm,labels=group,col=as.character(col.group))

#Colour and label MDS plot by sequencing lane
col.lane=lane
levels(col.lane)=brewer.pal(nlevels(col.lane),'Set2')
plotMDS(lcpm,labels=lane,col=as.character(col.lane)) #Default: dim=c(1,2)
plotMDS(lcpm,labels=lane,col=as.character(col.lane),dim=c(3,4)) #what is dim??
#Results: Batch effect observed, sequencing lane information included in model


#Differential expression analysis by limma (linear modelling for microarrays (and RNAseq))
#Design matrix
design=model.matrix(~0+group+lane)
colnames(design)=gsub('group','',colnames(design)) #gsub(pattern,replacement,x)
design #Effects are estimated for parameters specified by the design matrix

#Contrasts
contr.matrix=makeContrasts(BasalvsLP=Basal-LP,BasalvsML=Basal-ML,LPvsML=LP-ML,levels=colnames(design))
contr.matrix #Let lane=0 to ignore batch effect
#Y=b1x1+b2x2+...+e ??

#Estimate mean-variance relationship
v=voom(x,design,plot=T) #voom: Use mean-var trend to assign weights to each observation
v #Weights got from voom used in linear modelling removes mean-variance trend

#Fit linear model
vfit=lmFit(v,design) #Fits a linear model for each gene
vfit=contrasts.fit(vfit,contrast=contr.matrix) #Computes statistics and fold changes for comparisons of interest
efit=eBayes(vfit) #Computes more precise estimates by sharing gene information using empirical Bayes moderation
plotSA(efit,main='Final model: Mean-variance trend') #Plots residual standard deviation versus average log expression for fitted model

#Test for Differential Expression(DE)
summary(decideTests(efit)) #adjusted p-value < 0.05
 #Down: Genes that have significantly lower expression in basal compared to LP
 #Up: Genes that have significantly higher expression in basal compared to LP

#Top 10 genes from DE analysis
topTable(efit,coef='BasalvsLP')
 #LogFC: Log fold change, Fold change = ratio between two values
 #AveExpr: Average gene expression across all samples


##Test for DE relative to a threshold: when number of DE genes is large
tfit=treat(vfit,lfc=1) #logFC >= 1
dt=decideTests(tfit)
summary(dt)

#Number of DE genes in common  across two comparisons
de.common=which(dt[,'BasalvsLP']!=0 & dt[,'BasalvsML']!=0)
length(de.common)
head(tfit$genes$SYMBOL[de.common],n=20)
vennDiagram(dt[,1:2],circle.col=c("turquoise","salmon"))
#write.fit(tfit,dt,file="results.txt")

##Top genes from DE analysis
basal.vs.lp=topTreat(tfit,coef='BasalvsLP',n=Inf)
head(basal.vs.lp)

#Mean-difference plot
plotMD(tfit,column=1,status=dt[,1],main=colnames(tfit)[1],xlim=c(-8,13))

#Interactive version of MDS & MD plot using the Glimma package
library(Glimma)
glMDSPlot(lcpm,labels=paste(group,lane,sep="_"),groups=x$samples[,c(2,5)],launch=T)
glMDPlot(tfit,coef=1,status=dt,main=colnames(tfit)[1],side.main='ENTREZID',counts=lcpm,groups=group)

#Heatmap
library(gplots)
basal.vs.lp.topgenes=basal.vs.lp$ENTREZID[1:100]
i=which(v$genes$ENTREZID %in% basal.vs.lp.topgenes)
mycol=colorpanel(1000,"blue","white","red")
heatmap.2(lcpm[i,],scale="row",labRow=v$genes$SYMBOL[i],labCol=group,col=mycol,trace="none",density.info="none",margin=c(8,6),lhei=c(2,10),dendrogram="column")
```
