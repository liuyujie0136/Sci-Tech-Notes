# WEHI RNA-seq Workshop 2019

> School of Life Sciences, Nanjing University

## Introduction

From the 30th November - 2nd December, five bioinformaticians from the Walter and Eliza Hall Institute for Medical Research will be visiting Nanjing University to provide a series of training workshops on bioinformatics methods to analyse different kinds of data. This is an exciting opportunity for staff and students of the university to learn about bioinformatics analysis methods from experienced researchers with many years of experience in the analysis of data and the development of analysis methods. 

## RNA-Sequencing Analysis Workshop

In this workshop, you will be learning how to analyse RNA-seq count data using R. This will include reading the data into R, pre-processing data and performing differential expression analysis, with a focus on the limma-voom analysis workflow. You will learn how to generate common plots for analysis and visualisation of gene expression data, such as boxplots and heatmaps. 
 
This workshop is aimed at biologists interested in learning how to perform differential expression analysis of RNA-seq data when reference genomes are available. The material we will be covering on RNA-seq is available at https://f1000research.com/articles/5-1408/v3​.

The course is aimed at PhD students, Master's students, and third & fourth year undergraduate students.  Some basic R knowledge is assumed - this is not an introduction to R course. If you are not familiar with the R statistical programming language it is compulsory that you work through an introductory R course before you attend this workshop - the Software Carpentry ​[R for Reproducible Scientific Analysis](http://swcarpentry.github.io/r-novice-gapminder/)​ lessons up to and including vectorisation (topic 9) is recommended. 

## Single-Cell RNA-Sequencing Analysis Workshop

In this workshop, you will be learning how to analyse single-cell RNA-sequencing count data produced by the Chromium 10x platform using R. This will include reading the data into R, pre-processing data, normalization, feature selection, dimensional reduction and downstream analysis such as clustering and cell type annotation. You will learn how to generate common plots for analysis and visualisation of single cell gene expression data, such as diagnostic plots to assess the data quality as well as dimensionality reduction techniques such as principal components analysis and t-distributed stochastic neighbourhood embedding (t-SNE). The material we will be covering on RNA-seq is available at https://osca.bioconductor.org​.

## Preparations

### For RNA-seq Workshop

1. Download R and RStudio
2. Open RStudio, Select `tools-global options-packages-CRAN mirror-change-China (Beijing) [https] - TUNA Team, Tsinghua University`
3. Create and set working directory: Select `tools-global options-general-default working directory` (**Note: chinese characters is not allowed**)
4. Install R packages (**Note: update all the packages when installing**)
```r
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("RNAseq123")
```
5. Download data from GEO
```r
install.packages("R.utils")
url <- "https://www.ncbi.nlm.nih.gov/geo/download/?acc=GSE63310&format=file"
utils::download.file(url, destfile="GSE63310_RAW.tar", mode="wb")
utils::untar("GSE63310_RAW.tar", exdir = ".")
files <- c("GSM1545535_10_6_5_11.txt", "GSM1545536_9_6_5_11.txt", "GSM1545538_purep53.txt", "GSM1545539_JMS8-2.txt", "GSM1545540_JMS8-3.txt", "GSM1545541_JMS8-4.txt", "GSM1545542_JMS8-5.txt", "GSM1545544_JMS9-P7c.txt", "GSM1545545_JMS9-P8c.txt")
for(i in paste(files, ".gz", sep="")) R.utils::gunzip(i, overwrite=TRUE)
```
6. Check your packages and files (**Note: This will produce output even if everything has worked. Please read the output carefully to make sure there are no 'error' or 'warning' messages**)
```r
library(limma)
library(edgeR)
library(Mus.musculus)
library(RColorBrewer)
library(Glimma)
library(gplots)
read.delim(files[1], nrow=5)
```

### For scRNA-seq Workshop

1. **Assumption:** You have run the code above, because RNA-seq is the basis of scRNA-seq.
2. Install R packages

```r
install.packages("BiocManager")
BiocManager::install(version = "3.10")

install.packages("igraph")
install.packages("Matrix")

BiocManager::install("msigdbr")
BiocManager::install(c("scran", "scater", "edgeR", "pheatmap", "TENxPBMCData", "iSEE", "SingleCellExperiment", "scRNAseq", "AnnotationHub", "ensembldb", "robustbase", "DropletUtils", "BiocFileCache"), version = "3.10")
BiocManager::install("PCAtools")

install.packages("ggplot2")
install.packages("tidyverse")
install.packages("GGally")
install.packages("gridExtra")
```
3. Check your packages

```r
# Load libraries
library(scran)
library(TENxPBMCData)
library(SingleCellExperiment)
library(AnnotationHub)
library(scRNAseq)
library(scater)
library(robustbase)
library(BiocFileCache)
library(DropletUtils)
library(Matrix)

# Data sets
library(scRNAseq)
library(AnnotationHub)
library(DropletUtils)
library(PCAtools)

# Plotting
library(tidyverse)
library(GGally)

# data base
library(msigdbr)
```
4. Download data sets

```r
# Data sets
library(scRNAseq)
library(AnnotationHub)
library(DropletUtils)
library(PCAtools)

# 416b
sce.416b <- LunSpikeInData(which="416b")
sce.416b$phenotype <- ifelse(grepl("induced", sce.416b$phenotype), "induced", "wild type")
sce.416b$block <- factor(sce.416b$block)

# mmv97
ens.mm.v97 <- AnnotationHub()[["AH73905"]]

# Grun
sce.grun <- GrunPancreasData()
untar(bfcrpath(BiocFileCache("raw_data", ask = FALSE), file.path("http://cf.10xgenomics.com/samples", "cell-exp/2.1.0/pbmc4k/pbmc4k_raw_gene_bc_matrices.tar.gz")), exdir=file.path(tempdir(), "pbmc4k"))

# PBMC
sce.pbmc <- read10xCounts(file.path(tempdir(), "pbmc4k/raw_gene_bc_matrices/GRCh38"), col.names=TRUE)

# Zeisel
sce.zeisel <- ZeiselBrainData(ensembl = FALSE)

# Richard
sce.richard <- RichardTCellData()
sce.richard <- sce.richard[,sce.richard$`single cell quality`=="OK"]

# 8qc
library(BiocFileCache)
bfc <- BiocFileCache(ask=FALSE)
qcdata <- bfcrpath(bfc, "https://github.com/LuyiTian/CellBench_data/blob/master/data/mRNAmix_qc.RData?raw=true")

env <- new.env()
load(qcdata, envir=env)
sce.8qc <- env$sce8_qc

# Function
build_plot_for_UNI_counts = function(bcrank){
	uniq <- !duplicated(bcrank$rank) # Only showing unique points for plotting speed.
	
	plot(
		bcrank$rank[uniq],
		bcrank$total[uniq],
		log = "xy",
		xlab = "Rank",
		ylab = "Total UMI count",
		cex.lab = 1.2
	)
	
	abline(h = metadata(bcrank)$inflection,
				 col = "darkgreen",
				 lty = 2)
	abline(h = metadata(bcrank)$knee,
				 col = "dodgerblue",
				 lty = 2)
	
	legend(
		"bottomleft",
		legend = c("Inflection", "Knee"),
		col = c("darkgreen", "dodgerblue"),
		lty = 2,
		cex = 1.2
	)
}
```

