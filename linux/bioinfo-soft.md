# 生物信息学常用Linux平台软件简介

## Aspera

### NCBI-SRA和EBI-ENA数据库

#### SRA数据库(Sequence Read Archive)

隶属NCBI (National Center for Biotechnology Information)，它是一个保存大规模平行测序原始数据以及比对信息和元数据 (metadata) 的数据库，所有已发表的文献中高通量测序数据基本都上传至此，方便其他研究者下载及再研究。其中的数据则是通过压缩后以.sra文件格式来保存的，SRA数据库可以用于搜索和展示SRA项目数据，包括SRA主页和 Entrez system，由 NCBI 负责维护。SRA数据库中的数据分为Studies, Experiments, Samples和相应的Runs四个层次：
* Study：accession number 以 DRP，SRP，ERP 开头，表示的是一个特定目的的研究课题，可以包含多个研究机构和研究类型等。study 包含了项目的所有 metadata，并有一个 NCBI 和 EBI 共同承认的项目编号（universal project id），一个 study 可以包含多个实验（experiment）。
* Sample：accession number以 DRS，SRS，ERS 开头，表示的是样品信息。样本信息可以包括物种信息、菌株(品系) 信息、家系信息、表型数据、临床数据,组织类型等。可以通过 Trace 来查询。
* Experiment：accession number 以 DRX，SRX，ERX 开头。表示一个实验记载的实验设计（Design），实验平台（Platform）和结果处理 （processing）三部分信息。实验是 SRA 数据库的最基本单元，一个实验信息可以同时包含多个结果集（run）。
* Run：accession number 以DRR，SRR，ERR 开头。一个 Run 包括测序序列及质量数据。
* Submission：一个 study 的数据，可以分多次递交至 SRA 数据库。比如在一个项目启动前期，就可以把 study，experiment 的数据递交上去，随着项目的进展，逐批递交 run 数据。study 等同于项目，submission 等同于批次的概念。

#### ENA数据库(European Nucleotide Archive)
隶属EBI (European Bioinformatics Institute)，功能等同SRA，并且对保存的数据做了注释，界面相对于SRA更友好，对于有数据需求的研究人员来说，ENA数据库最诱人的点应该是可以直接下载fastq.gz文件，且可以方便地确认下载数据的完整性。

### SRA/FASTQ文件下载方式

需要获取他人发表的公开测序数据，来帮助自己的研究领域，下载.sra文件是为了获取该sra相对应的fastq或者sam文件，通过文件格式转换就可以和自己的pipeline对接上，用于直接分析，所以：

1. 我们需要到SRA或者ENA上搜索我们选择好的SRR号或者SRS号或者SRP号，先在ENA上搜索，如没有再去SRA上搜索，因为ENA下载比SRA快。
2. 下载数据，从SRA数据库下载数据有多种方法。可以用`ascp`快速的来下载sra文件，也可以用`wget`或`curl`等传统命令从FTP服务器上下载sra文件（但是wget和curl下载的sra文件有时候会不完整），另外NCBI的`sratoolkit`工具集中的`prefetch`、`fastq-dump`和`sam-dump`也支持直接下载。

### 使用Aspera从EBI-ENA下载FASTQ数据

推荐使用`conda`进行安装，使用以下命令：
```bash
conda install aspera-cli
# 或：
conda install -c hcc aspera-cli 
```

安装完成后，先了解`ascp`命令的使用方法与常用参数：
```bash
ascp [参数] 目标文件 保存路径

OPTIONS：
-v  verbose mode 唠叨模式，能让你实时知道程序在干啥，方便查错。
-T  取消加密，否则有时候数据下载不了
-i  提供私钥文件的地址，免密从ENA下载，不能少，如：~/miniconda3/pkgs/aspera-cli-3.7.7-0/etc/asperaweb_id_dsa.openssh
-l  设置最大传输速度，一般200m到500m，如果不设置反而速度会比较低
-k  重要！！设置断点续传，一般设为1
-Q  用于自适应流量控制，磁盘限制所需
-P  用于SSH身份验证的TCP端口，一般是33001
```

下载示例：

* ENA数据库的数据存放位置是`fasp.sra.ebi.ac.uk`，ENA在Aspera的用户名是`era-fasp`，即需要使用`era-fasp@fasp.sra.ebi.ac.uk`。如果要从ENA数据库下载SRR1240070，先搜索之。
* 我们可以自己设定显示哪些column（如：study_accession、scientific_name、fastq_aspera、fastq_md5等），再点击下载TSV保存之。
* 拷贝fastq_aspera中的链接，写入代码，运行。
```bash
ascp -QT -l 200m -k 1 -P 33001 \
    -i ~/miniconda3/pkgs/aspera-cli-3.7.7-0/etc/asperaweb_id_dsa.openssh \
    era-fasp@fasp.sra.ebi.ac.uk:/vol1/fastq/SRR124/000/SRR1240070/SRR1240070_1.fastq.gz \
    ~/data/fastq/
```

批量下载：

* 如搜索Study Accession: PRJNA171289，希望下载其全部数据。
* 如上，可下载只包含run_accession和fastq_aspera条目的TSV，提取下载地址：
```bash
cat filereport_read_run_PRJNA171289_tsv.txt | cut -f 2 | \
    sed 's/fasp.sra.ebi.ac.uk://g' | \
    sed 's/;/\n/' | \
    awk '!/fastq_aspera/{print}' > ENA-Aspera-FASTQ.txt
# 或：
cat filereport_read_run_PRJNA171289_tsv.txt | cut -f 2 | \
    sed 's/fasp.sra.ebi.ac.uk://g' | \
    sed 's/;/\n/' | \
    sed '/fastq_aspera/d' > ENA-Aspera-FASTQ.txt
# 或：
cat filereport_read_run_PRJNA171289_tsv.txt | cut -f 2 | \
    sed 's/fasp.sra.ebi.ac.uk://g; s/;/\n/; /fastq_aspera/d' \
    > ENA-Aspera-FASTQ.txt
```
* 运行如下代码，进行下载：
```bash
ascp -QT -l 200m -k 1 -P 33001 \
    -i ~/miniconda3/pkgs/aspera-cli-3.7.7-0/etc/asperaweb_id_dsa.openssh \
    --mode recv --host fasp.sra.ebi.ac.uk --user era-fasp \
    --file-list ENA-Aspera-FASTQ.txt \
    ~/data/fastq/
```


## bgzip-tabix

`tabix`可以对NGS分析中常见格式的文件建立索引，从而加快访问速度，不仅支持VCF文件，还支持BED, GFF，SAM等格式。

对于VCF文件，由于SNP位点多，其文件也非常大，需要进行压缩。`bgzip`可以压缩VCF文件，用法如下：
```bash
bgzip example.vcf
```

压缩之后，原本的`example.vcf`文件就变成了`example.vcf.gz`文件。压缩后缀为`.gz`, 若要解压缩，可以：
```bash
bgzip -d example.vcf.gz
gunzip example.vcf.gz
```

**注意：**在对VCF文件**压缩**时，不可以使用gzip来代替bgzip。

对于大型的VCF文件而言，如何快速访问其中的记录也是个难点。`tabix`可以对VCF文件构建索引，如下：
```bash
tabix -p vcf example.vcf.gz
```

注意输入的VCF文件必须是使用`bgzip`压缩之后的VCF文件，生成的索引文件为`example.vcf.gz.tbi`。

构建好索引之后，可以快速的获取指定区域的记录，示例如下：
```bash
# 获取位于11号染色体的SNP位点
tabix example.vcf.gz 11
# 获取位于11号染色体上突变位置大于或者等于2343545的SNP位点
tabix example.vcf.gz 11:2343545
# 获取位于11号染色体上突变位置介于2343540到2343596的SNP位点
tabix example.vcf.gz 11:2343540-2343596
```


## snpEff-SnpSift

在进行基因组学研究中，需要对一些SNP和Indel进行分析，而注释它们就需要用到SnpEff，SnpEff是一个变异注释和预测变异影响(氨基酸和核酸改变)的工具。输入文件通常是VCF(variant call format)文件，也接受BED文件(如来自ChIP-Seq peaker)。运行后会输出变异的注释并预测对已知基因的影响。详见[官网帮助文档](https://pcingola.github.io/SnpEff/)。

**示例1:** 先用SnpSift选择非低质量位点，再用snpEff进行注释
```bash
#!/bin/bash

SnpSift filter \
    "(na FILTER) | (FILTER = 'PASS')" \
    01_Nipponbare_B001.snp.vcf.filtered.gz | \
    snpEff ann -v \
        -s B001.snpEff.summary.html \
        Oryza_sativa \
        > B001.snpEff.vcf
```

**示例2:** 注释完成后再用SnpSift进一步筛选
```bash
#!/bin/bash

SnpSift filter \
    "(ANN[*].IMPACT = 'HIGH') | (ANN[*].IMPACT = 'MODERATE')" \
    B001.snpEff.vcf \
    > B001.snpEff.HIGH-MODERATE.vcf

# IMPACT = {HIGH, MODERATE, LOW, MODIFIER}
# Other example: "ANN[*].EFFECT has 'missense_variant'"

SnpSift extractFields \
    B001.snpEff.HIGH-MODERATE.vcf \
    CHROM POS REF ALT "ANN[0].EFFECT" "ANN[0].GENEID"\
    > B001.snpEff.HIGH-MODERATE.gene.txt

cat B001.snpEff.summary.genes.txt | sed 's/\t/,/g' > B001.snpEff.summary.genes.csv
```
​
