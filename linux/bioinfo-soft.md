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

注意：在对VCF文件**压缩**时，不可以使用gzip来代替bgzip。

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
    Sample1.snp.vcf.gz | \
    snpEff ann -v \
        -s Sample1.snpEff.summary.html \
        Oryza_sativa \
        > Sample1.snpEff.vcf
```

**示例2:** 注释完成后再用SnpSift进一步筛选
```bash
#!/bin/bash

SnpSift filter \
    "(ANN[*].IMPACT = 'HIGH') | (ANN[*].IMPACT = 'MODERATE')" \
    Sample1.snpEff.vcf \
    > Sample1.snpEff.HIGH-MODERATE.vcf

# IMPACT = {HIGH, MODERATE, LOW, MODIFIER}
# Other example: "ANN[*].EFFECT has 'missense_variant'"

SnpSift extractFields \
    Sample1.snpEff.HIGH-MODERATE.vcf \
    CHROM POS REF ALT "ANN[0].EFFECT" "ANN[0].GENEID"\
    > Sample1.snpEff.HIGH-MODERATE.gene.txt

cat Sample1.snpEff.summary.genes.txt | sed 's/\t/,/g' > Sample1.snpEff.summary.genes.csv
```


## bedtools

BEDTools是可用于genomic features的比较，相关操作及进行注释的工具。而genomic features通常使用Browser Extensible Data (BED) 或者 General Feature Format (GFF)文件表示，用UCSC Genome Browser进行可视化比较。bedtools总共有二三十个工具/命令来处理基因组数据。
比较典型而且常用的功能举例如下：

* 格式转换: bamToBed, bedToBam, bedToIgv
* 对基因组坐标的逻辑运算: 交集（intersectBed，windowBed），邻集（closestBed），补集（complementBed），并集（mergeBed），差集（subtractBed）
* 计算覆盖度(coverage): coverageBed, genomeCoverageBed

### `bedtools genomecov`

`genomecov`可以把alignment的结果文件转为bedgraph格式文件，[参考](http://bedtools.readthedocs.io/en/latest/content/tools/genomecov.html)。

有两种方法可以调用之：

```bash
bedtools genomecov [OPTIONS] [-i|-ibam] -g (iff. -i)
genomeCoverageBed  [OPTIONS] [-i|-ibam] -g (iff. -i)
```

这个命令本身并不是设计来做格式转换的，bam2bedgraph也只是其中的一个小功能而已，需要加上`-bg`参数，就可以report depth in BedGraph format.

示例：
```bash
bedtools genomecov -bg -i Sample1.tagAlign -g genome.len > Sample1.bedGraph
bedtools genomecov -bg -ibam Sample2.unique.sorted.bam > Sample2.bedGraph
```

注意：alignment的文件必须是sorted的，如果是bed格式的比对文件，用`-i`参数来指定输入文件，需要加入参考基因组的染色体大小记录文件(genome.len)，如果是bam格式的比对文件，用`-ibam`指定输入文件，而且不需要参考基因组的染色体大小记录文件。

### `bedtools multicov`

`multicov`可以对RNA-seq比对到各个基因的reads进行计数。

示例：
```bash
bedtools multicov -bams aln1.bam aln2.bam aln3.bam -bed locus.bed
```

注意，locus.bed文件是必须的，示例如下：
```bash
chr1    10000   20000   gene1
chr1    20000   30000   gene2
chr1    30000   40000   gene3
```

### `bedtools getfasta`

`getfasta`可以根据坐标区域来从基因组里面提取fasta序列

示例：
```bash
bedtools getfasta -fi genome.fa -bed HighQual_locus.bed -fo HighQual.fa
```

注意：bed格式用来记录坐标区域，参考基因组用`-fi`参数指定，输出的fasta序列文件用`-fo`参数指定

### `bedtools intersect`

`intersect`可以分析特定位置上有哪些基因，接下来就还可以对基因进行各种下游分析。需要提供基因的坐标信息和需要处理的bed文件。

示例：
```bash
bedtools intersect -a features.bed -b locus.bed -wa -wb -F 1 | bedtools groupby -g 1-3 -c 7 -o collapse
```

注意：`-f`和`-F`可以用来指定覆盖比例(Minimum overlap required as a fraction of A/B)，详见下文或`bedtools intersect -h`。

#### 常用`bedtools intersect`案例

用来求两个BED或者BAM文件中的overlap，overlap可以进行自定义是整个genome features的overlap还是局部。 加`-wa`参数可以报告出原始的在A文件中的feature，加`-wb`参数可以报告出原始的在B文件中的feature, 加`-c`参数可以报告出两个文件中的overlap的feature的数量，参数`-s`可以得到忽略strand的overlap，具体案例如下：

* 包含着染色体位置的两个文件，分别记为A文件和B文件。求交集: `bedtools intersect -a A.bed -b B.bed`
* A文件中哪些染色体位置是与文件B中的染色体位置有overlap: `bedtools intersect -a A.bed -b B.bed -wa`
* A文件中染色体位置与文件B中染色体位置的交集，以及对应的文件B中的染色体位置: `bedtools intersect -a A.bed -b B.bed -wb`
* 求对于A文件的染色体位置是否与文件B中的染色体位置有交集。如果有交集，分别输入A文件的染色体位置和B文件的染色体位置；如果没有交集，输入A文件的染色体位置并以". -1 -1"补齐: `bedtools intersect -a A.bed -b B.bed -loj`
* 对于A文件中染色体位置，如果和B文件中染色体位置有overlap，则输出在A文件中染色体位置和在B文件中染色体位置，以及overlap的长度: `bedtools intersect -a A.bed -b B.bed -wo`
* 对于A文件中染色体位置，如果和B文件中染色体位置有overlap，则输出在A文件中染色体位置和在B文件中染色体位置，以及overlap的长度；如果和B文件中染色体位置都没有overlap，则用". -1 -1"补齐: `bedtools intersect -a A.bed -b B.bed -wao`
* 对于A文件中染色体位置，输出在A文件中染色体位置和有多少B文件染色体位置与之有overlap: `bedtools intersect -a A.bed -b B.bed -c`
* 对于A文件中染色体位置，输出在A文件中染色体位置和与B文件染色体位置至少有90%的overlap的记录: `bedtools intersect -a A.bed -b B.bed -f 0.90 -wa -wb`
* 对于A文件中染色体位置，输出在B文件中染色体位置和与A文件染色体位置至少有90%的overlap的记录: `bedtools intersect -a A.bed -b B.bed -F 0.90 -wa -wb`

### `bedtools merge`

`merge`用于合并位于同一个bed/gff/vcf文件中的重叠区域。

```bash
bedtools merge [OPTION] –i <bed/gff/vcf>

-s      必须相同(正负)链的区域才合并（软件默认不考虑正负链特征）
-n      报告合并的区域数量，没有被合并则1
-d      两个独立区域间距小于（等于）该值时将被合并为一个区域
-nms    报告被合并区域的名称
-scores 报告几个被合并特征区域的scores
```

### 其他小功能

* `pairToPair`: 比较BEDPE文件搜索overlaps, 类似于`pairToBed`。
* `bamToBed`: 将BAM文件转换为BED文件或者BEDPE文件，如: `bamToBed -i reads.bam`。
* `windowBed`: 类似于`intersectBed`, 但是可以指定一个数字，让A中的genome feature增加上下游去和B中的genome features进行overlap。默认情况这个值为1000，可以使用`-w`加定义，如: `windowBed -a A.bed -b B.bed -w 5000`。还可以用`-l`指定是上游，用`-r`指定下游，如: `windowBed -a A.bed -b B.bed -l 200 -r 20000`。
* `subtractBed`: 在A中去除掉B中有的genome features。
* `coverageBed`: 计算深度和覆盖度，如计算基因组任意1Kb的测序read的覆盖度。
* `genomeCoverageBed`: 计算给定bam文件在基因组上的覆盖度及每个碱基的深度。

### 参考资料

* 软件论文：Quinlan, A.R. & Hall, I.M. BEDTools: a flexible suite of utilities for comparing genomic features. Bioinformatics 26, 841-842 (2010).
* 帮助文档：
  * [bedtools: a powerful toolset for genome arithmetic](http://bedtools.readthedocs.io/en/latest/index.html)
  * [bedtools Tutorial](http://quinlanlab.org/tutorials/bedtools/bedtools.html)
  * [Bedtools Documentation](https://media.readthedocs.org/pdf/bedtools/latest/bedtools.pdf)


## bcftools

### 下载与安装
```bash
wget https://github.com/samtools/bcftools/releases/download/1.11/bcftools-1.11.tar.bz2
tar xjvf bcftools-1.11.tar.bz2
cd bcftools-1.11/
./configure --prefix=~/biosoft/bcftools
make
make install
```

### `bcftools annotate`

`annotate`命令有两个用途:
* 注释VCF文件: `bcftools annotate -a db.vcf -c ID,QUAL,+TAG sample1.vcf -o sample1_annotated.vcf`
  * `-a`: 指定注释用的数据库文件，格式可以是VCF, BED, 或者是Tab分隔的自定义文件。在Tab分隔的自定义文件中，必须包含CHROM, POS字段。
  * `-c`: 指定将数据库的哪些信息添加到输出文件中。
  * `-o`: 指定输出文件，也可用重定向`>`或管道操作`|`。
* 编辑VCF文件，如去除其中的某些注释信息，或去除某些样本: `bcftools annotate -x ID,INFO/DP,FORMAT/DP sample1.vcf -o sample1_removeID.vcf`
  * `-x`: 表示去除VCF文件中的注释信息，可以是其中的某一列，比如ID, 也可以是某些字段，比如INFO/DP，多个字段的信息用逗号分隔。去除之后，这些信息所在的列并不会去除，而是用.填充。

### `bcftools concat`

`concat`命令可以将多个VCF文件合并为一个VCF文件，要求输入的VCF文件必须是排序之后的，如果包含多个样本的信息，样本的顺序也必须一致。经典的应用场景包括合并不同染色体上的VCF文件，合并SNP和INDEL两种类型的VCF文件，用法如下:
```bash
bcftools concat a.vcf.gz b.vcf.gz -o concat.vcf
```

注意：输入文件必须是经过bgzip压缩的文件，而且还需要有tabix的索引。

### `bcftools merge`

`merge`命令也是用于合并VCF文件，主要用于将单个样本的VCF文件合并成一个多个样本的VCF文件。用法如下:

```bash
bcftools merge a.vcf.gz b.vcf.gz -o merge.vcf
```

注意：
* `concat`可以进行VCF的纵向合并，比如不同染色体的vcf文件，或者SNP和INDEL进行的合并。但是样品顺序必须保持一致。
* `merge`可以进行VCF的横向合并，比如单个样本的VCF文件的合并。
* `concat`和`merge`的共同点是输入文件必须是bgzip压缩，且有索引。

### `bcftools isec`

`isec`用于在多个VCF文件之间取交集，差集，并集等操作，默认参数为取交集。经典的应用场景是对多种软件的SNP Calling结果进行Venn分析。用法如下, 其中`-p`参数指定输出结果的目录:

```bash
bcftools isec a.vcf.gz b.vcf.gz -p dir
```

### `bcftools stats`

`stats`命令用于统计VCF文件的基本信息。如突变个数、突变类型的个数、转换颠换个数、测序深度、INDEL长度。还可以利用`plot-vcfstats`进行可视化处理。用法如下:

```bash
bcftools stats sample1.vcf > sample1.stats
```

输出文件可以用于plot-vcfstats命令进行可视化: `plot-vcfstats sample1.stats -p output`，这个脚本位于bcftools安装目录的misc目录下，依赖latex生成pdf文件。

### `bcftools index`

`index`命令用于对VCF文件建立索引，要求输入的VCF文件必须是使用bgzip压缩之后的文件，支持`.csi`和`.tbi`两种索引，用法如下:

```bash
bcftools index sample1.vcf.gz       # csi格式
bcftools index -t sample1.vcf.gz    # tbi格式
```

### `bcftools view`

* `view`命令可以用于VCF文件和BCF(binary format of VCF)文件之间的转换: `bcftools view sample1.vcf.gz -O u -o sample1.bcf`
  * `-O`: 参数指定输出文件的类型 (`u`-未压缩BCF, `b`-压缩后BCF, `v`-未压缩VCF, `z`-压缩后VCF)
  * `-o`: 参数指定输出文件的名字
* `view`命令还可以根据样本筛选VCF文件: `bcftools view samples.vcf.gz -s sample1,sample2 -o subset.vcf`
  * `-s`参数指定想要保留的样本信息，多个样本用逗号分隔。如果样本名称添加了`^`前缀，代表去除这些样本，如`-s ^sample1,sample2`，这个写法表示从VCF文件中去除sample1,sample2这两个样本的信息。
* `view`命令还可以过滤突变位点，过滤的条件非常多，可以根据突变位点的类型，基因型类型等条件进行过滤，详细的参数可以参考软件的帮助文档，这里只做一个基本示例: `bcftools view sample1.vcf.gz -k -o sample1_known.vcf`
  * `-k`参数表示筛选已知的突变位点，即ID那一列值不是.的突变位点。

### `bcftools query`

`query`命令也是用于格式转换，和view命令不同，query通过表达式来指定输出格式，可定制化程度更高。用法如下:

```bash
bcftools query -f '%CHROM\t%POS\t%REF\t%ALT[\t%SAMPLE=%GT]\n' sample1.vcf.gz
```

`-f`参数通过一个表达式来指定输出格式，其中变量的写法如下:
* `%CHROM`: 代表VCF文件中染色体那一列，其他的列，比如POS, ID, REF, ALT, QUAL, FILTER也是类似的写法
* `[]`: 对于FORMAT字段的信息，必须要中括号括起来
* `%SAMPLE`: 代表样本名称
* `%GT`: 代表FORMAT字段中genotype的信息
* `\t` 制表符分隔，`\n` 换行

### `bcftools sort`

`sort`命令用于对VCF文件排序，按照染色体位置进行排序: `bcftools sort sample1.vcf.gz -o sample1.sorted.vcf`

### `bcftools reheader`

`reheader`命令有两个用途:
* 编辑VCF文件的头部: `bcftools reheader -h header.file samples.vcf -o samples.newheader.vcf`
  * `-h`参数指定新的header文件，内容如下:
  ```
  ##fileformat=VCFv4.3
  ##reference=file:///seq/references/1000Genomes-NCBI37.fasta
  ##contig=<ID=11,length=135006516>
  ##contig=<ID=20,length=63025520>
  ....
  ```
* 替换样本信息: `bcftools reheader -s sname.file samples.vcf -o samples.newsname.vcf`
  * `-s`参数指定需要替换的样本名，第一列代表VCF文件中原始的样本名称，第二列代表替换后的样本名称，两类之间用空格分隔，样本名不允许有空格。内容如下:
  ```
  sample1 S1
  sample2 S2
  sample3 S3
  ```

### 常用例子：详见[此文](https://www.jianshu.com/p/266c55c87978)


## BWA

### 简介

BWA，即Burrows-Wheeler-Alignment Tool，是一种能够将差异度较小的序列比对到一个较大的参考基因组上的软件包。它有三个不同的算法：

- **BWA-backtrack:** 是用来比对 Illumina 的序列的，reads 长度最长能到 100bp。
- **BWA-SW:** 用于比对 long-read ，支持的长度为 70bp-1Mbp；同时支持剪接性比对。
- **BWA-MEM:** 推荐使用的算法，支持较长的read长度，同时支持剪接性比对（split alignments)，但是BWA-MEM是更新的算法，也更快，更准确，且 BWA-MEM 对于 70bp-100bp 的 Illumina 数据来说，效果也更好些。

对于上述三种算法，首先需要使用索引命令构建参考基因组的索引，用于后面的比对。所以，使用BWA整个比对过程主要分为两步，第一步建索引，第二步使用`BWA MEM`进行比对。

BWA的使用需要两中输入文件：

- Reference genome data（fasta格式 **.fa, .fasta, .fna**）
- Short reads data (fastaq格式 **.fastaq, .fq**)

### 安装

```bash
git clone https://github.com/lh3/bwa.git
cd bwa
make

# quick start
./bwa index ref.fa
./bwa mem ref.fa read-se.fq.gz | gzip -3 > aln-se.sam.gz
./bwa mem ref.fa read1.fq read2.fq | gzip -3 > aln-pe.sam.gz
```

安装好之后，会出现一个名为`bwa`的可执行文件，输入下面命令可以查看帮助信息

```bash
./bwa

Program: bwa (alignment via Burrows-Wheeler transformation)
Version: 0.7.17-r1188
Contact: Heng Li <<a href="mailto:lh3@sanger.ac.uk" _href="mailto:lh3@sanger.ac.uk">lh3@sanger.ac.uk</a>>
Usage:   bwa <command> [options]
Command: index         index sequences in the FASTA format
         mem           BWA-MEM algorithm
         fastmap       identify super-maximal exact matches
         pemerge       merge overlapping paired ends (EXPERIMENTAL)
         aln           gapped/ungapped alignment
         samse         generate alignment (single ended)
         sampe         generate alignment (paired ended)
         bwasw         BWA-SW for long queries
         shm           manage indices in shared memory
         fa2pac        convert FASTA to PAC format
         pac2bwt       generate BWT from PAC
         pac2bwtgen    alternative algorithm for generating BWT
         bwtupdate     update .bwt to the new format
         bwt2sa        generate SA from BWT and Occ
Note: To use BWA, you need to first index the genome with `bwa index'.
      There are three alignment algorithms in BWA: `mem', `bwasw', and
      `aln/samse/sampe'. If you are not sure which to use, try `bwa mem'
      first. Please `man ./bwa.1' for the manual.
```

### 建立索引 index

bwa软件的作用是将 reads 比对到参考基因组上，在比对之前，首先需要对参考基因组建立索引，即对 fasta 文件构建 FM-index。

Usage:
```bash
bwa index [ –p prefix ] [ –a algoType ] <in.db.fasta>

# Example:
bwa index IRGSP-1.0_genome.fasta
```

Options:
* `-p` 输出数据库的前缀，默认和输入的文件名一致，输出的数据库在其输入文件所在的文件夹，并以该文件名为前缀。
* `-a` 构建index的算法，有以下两个选项:
  * `-a is` 是默认的算法，虽然相对较快，但是需要较大的内存，当构建的数据库大于2GB的时候就不能正常工作了;
  * `-a bwtsw` 对于短的参考序列式不工作的，必须要大于等于10MB, 但能用于较大的基因组数据，比如人的全基因组。

### BWA-MEM 算法

该算法先使用 MEM (maximal exact matches) 进行 seeding alignments，再使用 SW(affine-gap Smith-Waterman) 算法进行 seeds 的延伸。BWA–MEM 算法执行局部比对和剪接性。可能会出现 query 序列的多个不同的部位出现各自的最优匹配，导致 reads 有多个最佳匹配位点。有些软件如 Picard’s markDuplicates 跟 mem 的这种剪接性比对不兼容，在这种情况下，可以使用 –M 选项来将 shorter split hits 标记为次优。

Usage:
```bash
bwa mem [options] ref.fa reads_1.fq.gz [reads_2.fq.gz] > aln.sam
```

Options:
* `-t` 线程数，默认是1，增加线程数，会减少运行时间。
* `-M` 将 shorter split hits 标记为次优，以兼容 Picard’s markDuplicates 软件。
* `-p` 若无此参数：输入文件只有1个，则进行单端比对；若输入文件有2个，则作为paired reads进行比对。若加入此参数：则仅以第1个文件作为输入(会忽略第二个输入序列文件，把第一个文件当做单端测序的数据进行比对)，该文件必须是read1.fq和read2.fq进行reads交叉的数据。
* `-R` 完整的read group的头部，可以用 '\t' 作为分隔符， 在输出的SAM文件中被解释为制表符TAB. read group 的 ID，会被添加到输出文件的每一个read的头部。
* `-T` 当比对的分值低于该值时，不输出该比对结果，这个参数只影响输出的结果，不影响比对的过程。
* `-a` 将所有的比对结果都输出，包括 single-end 和 unpaired paired-end 的 reads，但是这些比对的结果会被标记为次优。
* `-Y` 对数据进行soft clipping, 当错配或者gap数过多比对不上时，会对序列进行切除，这里的切除并只是在比对时去掉这部分序列，最终输出结果中序列还是存在的，所以称为soft clipping。

另：对于超长读长的reads,比如PacBio和Nanopore测序仪产生的reads, 用法如下:
```bash
bwa mem -x pacbio ref.fa reads.fq > aln.sam
bwa mem -x ont2d ref.fa reads.fq > aln.sam
```

### BWA-backtrack 算法

对应的子命令为`aln/samse/sample`，先使用 `aln` 命令将单独的 reads 比对到参考序列，再使用 `samse/sampe` 生成 sam 文件。

Usage:
```bash
# single-end reads
bwa aln [options] ref.fa read.fq > aln_sa.sai
bwa samse [options] ref.fa aln_sa.sai read.fq > aln-se.sam

# paired-end reads
bwa aln [options] ref.fa read1.fq > aln_sa1.sai
bwa aln [options] ref.fa read2.fq > aln_sa2.sai
bwa sampe [options] ref.fa aln_sa1.sai aln_sa2.sai read1.fq read2.fq > aln-pe.sam
```

Options:
* `-o` 允许出现的最大gap数。
* `-e` 每个gap允许的最大长度。
* `-d` 不允许在3’端出现大于多少bp的deletion。
* `-i` 不允许在reads两端出现大于多少bp的indel。
* `-l` Read前多少个碱基作为seed，如果设置的seed大于read长度，将无法继续，最好设置在25-35，与-k 2 配合使用。
* `-k` 在seed中的最大编辑距离，使用默认2，与-l配合使用。
* `-t` 要使用的线程数。
* `-R` 此参数只应用于pair end中，当没有出现大于此值的最佳比对结果时，将会降低标准再次进行比对。增加这个值可以提高配对比对的准确率，但是同时会消耗更长的时间，默认是32。
* `-I` 表示输入的文件格式为Illumina 1.3+数据格式。
* `-B` 设置标记序列。从5’端开始多少个碱基作为标记序列，当-B为正值时，在比对之前会将每个read的标记序列剪切，并将此标记序列表示在BC SAM 标签里，对于pair end数据，两端的标记序列会被连接。
* `-b` 指定输入格式为bam格式。
* `-f` 指定输出文件，默认为标准输出。

### BWA-SW 算法

Usage:
```bash
bwa bwasw -t <threads> ref.fa reads_1.fq.gz [reads_2.fq.gz] > aln.sam
```

有 2 个输入文件时，进行 paired-end 比对，此模式仅对 Illumina 的 short-insert 数据进行比对。在 Paired-end 模式下，`BWA-SW` 依然输出剪接性比对结果，但是这些结果会标记为 not properly paired; 同时如果有多个匹配位点，则不会写入 mate 的匹配位置。

### 常用示例

BWA works with a variety types of DNA sequence data, though the optimal algorithm and setting may vary. The following list gives the recommended settings:

- Illumina/454/IonTorrent single-end reads longer than ~70bp or assembly contigs up to a few megabases mapped to a closely related reference genome:
  ```bash
  bwa mem ref.fa reads.fq > aln.sam
  ```
- Illumina single-end reads shorter than ~70bp:
  ```bash
  bwa aln ref.fa reads.fq > reads.sai; bwa samse ref.fa reads.sai reads.fq > aln-se.sam
  ```
- Illumina/454/IonTorrent paired-end reads longer than ~70bp:
  ```bash
  bwa mem ref.fa read1.fq read2.fq > aln-pe.sam
  ```
- Illumina paired-end reads shorter than ~70bp:
  ```bash
  bwa aln ref.fa read1.fq > read1.sai; bwa aln ref.fa read2.fq > read2.sai
  bwa sampe ref.fa read1.sai read2.sai read1.fq read2.fq > aln-pe.sam
  ```
- PacBio subreads or Oxford Nanopore reads to a reference genome:
  ```bash
  bwa mem -x pacbio ref.fa reads.fq > aln.sam
  bwa mem -x ont2d ref.fa reads.fq > aln.sam
  ```

BWA-MEM is recommended for query sequences longer than ~70bp for a variety of error rates (or sequence divergence). Generally, BWA-MEM is more tolerant with errors given longer query sequences as the chance of missing all seeds is small. As is shown above, with non-default settings, BWA-MEM works with Oxford Nanopore reads with a sequencing error rate over 20%.


## samtools

samtools是一个用于操作sam和bam文件的工具合集。

### `samtools view`

`view`命令的主要功能是：将输入文件转换成输出文件，通常是将比对后的sam文件转换为bam文件。；然后对bam文件进行各种操作，比如数据的排序(不属于本命令的功能)和提取(这些操作是对bam文件进行的，因而当输入为sam文件的时候，不能进行该操作)。

bam文件优点：bam文件为二进制文件，占用的磁盘空间比sam文本文件小；利用bam二进制文件的运算速度快。

`view`命令中，对sam文件头部的输入(-t或-T）和输出(-h)是单独的一些参数来控制的。

Usage:
```bash
samtools view [options] <in.bam>|<in.sam> [region1 [...]]
```

默认情况下不加 region，则是输出所有的 region

Options:  
* `-b` 默认下输出是 SAM 格式文件，该参数设置输出 BAM 格式
* `-h` 默认下输出的 sam 格式文件不带 header，该参数设定输出sam文件时带 header 信息
* `-H` 仅输出header，无比对信息
* `-S` 默认下输入是 BAM 文件，若是输入是 SAM 文件，则最好加该参数，否则有时候会报错。 
* `-c` Instead of printing the alignments, only count them and print the total number. All filter options, such as ‘-f’, ‘-F’ and ‘-q’ , are taken into account.
* `-L` output alignments overlapping the input BED FILE
* `-o` 指定输出文件
* `-F` 对flag进行过滤
  * 数字4代表该序列没有比对到参考序列上
  * 数字8代表该序列的mate序列没有比对到参考序列上
* `-q` minimum mapping quality
* `-@` Number of additional threads to use

Examples:

（1）将sam文件和bam文件相互转换。注意：没有header的sam文件不能转换成bam文件。

```bash
samtools view -b -S lane.bam -o lane.sam
samtools view -h sample.bam > sample.sam
```

（2）提取比对到参考序列上的比对结果: `-F`

```bash
samtools view -bF 4 lane.bam > lane.F4.bam
```

（3）提取paired reads中两条reads都比对到参考序列上的比对结果，只需要把两个4+8的值12作为过滤参数即可

```bash
samtools view -bF 12 lane.bam > lane.F12.bam
```

（4）提取没有比对到参考序列上的比对结果: `-f`

```bash
samtools view -bf 4 lane.bam > lane.f4.bam
```

（5）提取paired reads中比对到参考基因组上的数据

```bash
samtools view -b -F 4 -f 8 lane.sam > only.read1.mapped.bam
samtools view -b -F 8 -f 4 lane.sam > only.read2.mapped.bam
samtools view -b -F 12 lane.sam > all.mapped.read1.read2.bam
```

（6）提取bam文件中比对到scaffold1上的比对结果，并保存到sam文件格式

```bash
samtools view lane.bam scaffold1 > scaffold1.sam
```

（7）提取scaffold1上能比对到30k到100k区域的比对结果

```bash
samtools view lane.bam scaffold1:30000-100000 > scaffold1_30k-100k.sam
```

（8）根据fasta文件，将 header 加入到 sam 或 bam 文件中

```bash
samtools view -T genome.fasta -h scaffold1.sam > scaffold1.h.sam
```

（9）sam 和 bam 格式转换

```bash
# BAM转换为SAM
samtools view -h 1.bam > out.sam

# SAM转换为BAM
samtools view -bS 1.sam > out.bam

# 如果无@SQ，要写-t，所以如果没有@SQ，需要：
samtools faidx ref.fa
samtools view -bt ref.fa.fai out.sam > out.bam
```

### `samtools sort`

对bam文件进行排序。

Usage: 
```bash
samtools sort [-l level] [-m maxMem] [-o out.bam] [-O format] [-n] [-T tmpprefix] [-@ threads] [in.sam|in.bam|in.cram]
```

Options:  
* `-m` 设置每个线程运行时的内存大小，可以使用K，M和G表示内存大小。默认下是 500,000,000 即500M。对于处理大数据时，如果内存够用，则设置大点的值，以节约时间。
* `-n` 设置按照read名称进行排序。默认下是按序列在fasta文件中的顺序（即header）和序列从左往右的位点排序。
* `-l` 设置输出文件压缩等级。0-9，0是不压缩，9是压缩等级最高。不设置此参数时，使用默认压缩等级
* `-o` 设置最终排序后的输出文件名，默认标准输出
* `-O` 设置最终输出的文件格式，可以是bam，sam或者cram，默认为bam
* `-T` 设置临时文件的前缀
* `-@` 设置排序和压缩的线程数量，默认是单线程。

### `samtools merge`

当有多个样本的bam文件时，可以使用`merge`命令将这些bam文件进行合并为一个bam文件。`merge`命令将多个已经排序后的bam文件合并成为一个保持所有输入记录并保持现有排序顺序的bam文件。

Usage:
```bash
samtools merge [-nurlf] [-h inh.sam] [-R reg] [-b <list>] <out.bam> <in1.bam> [<in2.bam> ... <inN.bam>]
```

Options:  
* `-l` 指定压缩等级
* `-b` 输入文件列表，一个文件一行
* `-f` 强制覆盖同名输出文件
* `-h` 指定inh.sam内的header复制到输出bam文件中并替换输出文件的文件头。否则，输出文件的文件头将从第一个输入文件复制过来
* `-n` 设定输入比对文件是以read名进行排序的而不是以染色体坐标排序的
* `-R` 合并输入文件的指定区域
* `-r` 使RG标签添加到每一个比对文件上，标签值来自文件名
* `-u` 输出的bam文件不压缩
* `-c` 当多个输入文件包含相同的@RG ID时，只保留第一个到合并后输出的文件。当合并多个相同样本的不同文件时，非常有用
* `-p` 与`-c`参数类似，对于要合并的每一个文件中的@PG ID只保留第一个文件中的@PG 

Examples:

```bash
#合并test_L1.bam和test_L2.bam文件
samtools merge -h test.sam \
    test_L1_L2.bam \
    test_L1.sorted.bam \
    test_L2.sroted.bam

#合并test_L1.bam和test_L2.bam文件中的指定区域chr7
samtools merge -h test.sam \
    -R chr7
    test_L1_L2.bam \
    test_L1.sorted.bam \
    test_L2.sroted.bam
```

### `samtools index`

为了能够快速访问bam文件，可以为已经基于坐标排序后bam或者cram的文件创建索引，生成以.bai或者.crai为后缀的索引文件。必须使用排序后的文件，否则可能会报错。另外，不能对sam文件使用此命令。如果想对sam文件建立索引，那么可以使用tabix命令创建。

Usage:
```bash
samtools index [-bc] [-m INT] <aln.bam|cram> [out.index]
```

Options:
* `-b` 创建bai索引文件，未指定输出格式时，此参数为默认参数
* `-c` 创建csi索引文件，默认情况下，索引的最小间隔值为2^14，与bai格式一致
* `-m` 创建csi索引文件，最小间隔值2^INT
* `-@` 线程数

### `samtools faidx`

对fasta文件建立索引，生成的索引文件以.fai后缀结尾。该命令也能依据索引文件快速提取fasta文件中的某一条（子）序列．如果没有指定区域，faidx命令就创建文件索引并生成后缀为.fai的索引文件。如果指定区域，便显示该区域序列。输入文件可以为bgzip压缩格式的文件。

Usage:
```bash
samtools faidx <ref.fa|ref.fa.gz>
samtools faidx <ref.fa|ref.fa.gz> <region1, ...> > selected.fa
```

生成的索引文件.fai是一个文本文件，共有5列：

- 第１列是子序列的名称，每个序列">"之后到第一个空格之前的所有字符
- 第２列是序列长度，单位为bp
- 第３列是OFFSET，即第一个碱基的偏移量，以0为起始值
- 第４列是LINEBASES，除了最后一行以外，其他代表序列的碱基数，单位为bp
- 第５列是LINEWIDTH，除了最后一行以外，其他代表序列的长度，包括换行符。注：windows系统中换行符为"\r\n"，要在序列长度的基础上加2。

### `samtools tview`

tview能直观的显示出reads比对基因组的情况，和IGV有点类似。

Usage:
```bash
samtools tview <aln.bam> <ref.fa>
```

* 按下 g ，则提示输入要到达基因组的某一个位点。如"chr01:1000"表示到达chr01的第1000个碱基位点处。
* 使用H(左)J(上)K(下)L(右)移动显示界面。大写字母移动快，小写字母移动慢。
* 使用空格建向左快速移动（和 L 类似），使用Backspace键向左快速移动（和 H 类似）。
* Ctrl+H 向左移动1kb碱基距离； Ctrl+L 向右移动1kb碱基距离
* 可以用颜色标注比对质量，碱基质量，核苷酸等。30～40的碱基质量或比对质量使用白色表示；20～30黄色；10～20绿色；0～10蓝色。
* 使用点号'.'切换显示碱基和点号；使用r切换显示read name等
* 还有很多其它的使用说明，具体按 ？键来查看

### `samtools flagstat <in.bam>`

统计输入文件的相关数据并将这些数据输出至屏幕显示。每一项统计数据都由两部分组成，分别是QC pass和QC failed，表示通过QC的reads数据量和未通过QC的reads数量。以"PASS + FAILED"格式显示。

### `samtools depth`

得到每个碱基位点或者区域的测序深度，并输出到标准输出。需要先`index`。

Usage:
```bash
samtools depth [options] [in1.bam|in1.sam|in1.cram[in2.bam|in2.sam|in2.cram]…]
```

Options:  
* `-a` 输出所有位点，包括零深度的位点
* `-aa` 完全输出所有位点，包括未使用到的参考序列
* `-b` 计算BED文件中指定位置或区域的深度
* `-q` 只计算碱基质量值大于此值的reads
* `-Q` 只计算比对质量值大于此值的reads
* `-r` 只计算指定区域的reads，格式为CHR:FROM–TO

### `samtools mpileup`

该命令用于生成bcf文件，再使用bcftools进行SNP和Indel的分析。

Usage:
```bash
samtools mpileup [-EBug] [-C capQcoef] [-r reg] [-f in.fa] [-l list] [-M capMapQ] [-Q minBaseQ] [-q minMapQ] in.bam [in2.bam [...]]
```

Options:  
* `-f` 输入有索引文件的fasta参考序列
* `-F` 含有间隔reads的最小片段
* `-l` BED文件，包含区域位点的位置列表文件
* `-r` 只在指定区域产生pileup，需要已建立索引的bam文件。通常和`-l`参数一起使用
* `-o` 生成pileup格式文件或者VCF、BCF文件，默认标准输出
* `-g` 计算基因型的似然值，输出文件格式为BCF
* `-v` 计算基因型的似然值，输出文件格式为VCF
* `-A` 在检测变异中，不忽略异常的reads对
* `-D` 输出每个样本的reads深度
* `-V` 输出每个样本未比对到参考基因组的reads数量
* `-t` 设置FORMAT和INFO的列表内容，以逗号分割
* `-u` 生成未压缩的VCF和BCF文件
* `-I` 不检测INDEL
* `-m` 候选INDEL的最小间隔的reads。

Examples:

```bash
samtools mpileup -f genome.fasta abc.bam > abc.txt
samtools mpileup -gSDf genome.fasta abc.bam > abc.bcf
samtools mpileup -guSDf genome.fasta abc.bam | bcftools view -cvNg - > abc.vcf
# complex one
samtools mpileup -l locus.bed -r 7 -q 1 -C 50 -t DP,DV -m 2 -F 0.002 -uvf human.fasta test.bam -o test.chr7.raw.vcf
```

`mpileup`不使用`-u`或`-g`参数时，则不生成二进制的bcf文件，而生成一个文本文件(输出到标准输出)。该文本文件统计了参考序列中每个碱基位点的比对情况。该文件每一行代表了参考序列中某一个碱基位点的比对结果，包含6列：参考序列名，位置，参考碱基，比对上的reads数，比对情况，比对上的碱基的质量。

其中第5列内容解释如下：

- `.` 代表与参考序列正链匹配
- `,` 代表与参考序列负链匹配
- `ATCGN` 代表在正链上的不匹配
- `atcgn` 代表在负链上的不匹配
- `*` 代表模糊碱基
- `^` 代表匹配的碱基是一个read的开始；`^`后面紧跟的ASCII码减去33代表比对质量，这两个符号修饰的是后面的碱基，其后紧跟的碱基(.,ATCGatcgNn)代表该read的第一个碱基
- `$` 代表一个read的结束，该符号修饰的是其前面的碱基
- `\+[0-9]+[ACGTNacgtn]+` 正则式，以"+"(`\+`转义)开头，代表在该位点后插入的碱基
- `-[0-9]+[ACGTNacgtn]+` 正则式，以"-"开头，代表在该位点后缺失的碱基

### `samtools rmdup`

NGS上机测序前需要进行PCR一步，使一个模板扩增出一簇，从而在上机测序的时候表现出为1个点，即一个reads。若一个模板扩增出了多簇，结果得到了多个reads，这些reads的坐标(coordinates)是相近的。在进行了reads比对后需要将这些由PCR duplicates获得的reads去掉，并只保留最高比对质量的reads。使用`rmdup`命令即可完成。

Usage:
```bash
samtools rmdup [-sS] <input.sorted.bam> <output.bam>
```

Options:
* `-s` 对single-end reads做处理。默认情况下，只对paired-end reads做处理。
* `-S` 将paired-end reads作为single-end reads处理。

### `samtools idxstats <in.bam>`

检索和打印已建立索引的bam文件的统计数据，包括参考序列名称、序列长度、比对上的reads数量和未比对上的read数量。输出结果显示在屏幕上，以制表符分割。

### `samtools bedcov [-Q minQual] <region.bed> <in1.bam> [...]`

计算由BED文件指定的基因组区域内的总碱基数量(read depth)。

### `samtools reheader <in.header.sam> <in.bam> > <out.bam>`

替换bam文件的头

### `samtools cat [-h header.sam] [-o out.bam] <in1.bam> [...<inN.bam>]`

连接多个bam文件，适用于非sorted的bam文件

### 将bam文件转换为fastq文件

有时候，我们需要提取出比对到一段参考序列的reads，进行小范围的分析，以利于debug等。这时需要将bam或sam文件转换为fastq格式。可以使用 `bedtools bamtofastq -i <BAM> -fq <FQ1> -fq2 <FQ2>` 完成。


## GATK4

[GATK4.0和全基因组数据分析实践（上）](https://mp.weixin.qq.com/s?__biz=MzAxOTUxOTM0Nw==&mid=2649798425&idx=1&sn=ae355ed362848578e5c853413f23dfd7&chksm=83c1d505b4b65c13124c9acd210356c4364ec9f5498bbd16fa4475be29811213abb64ea9720f&scene=21#wechat_redirect)

[GATK4.0和全基因组数据分析实践（下）](https://mp.weixin.qq.com/s?__biz=MzAxOTUxOTM0Nw==&mid=2649798455&idx=1&sn=67a7407980a57ce138948eb46992b603&chksm=83c1d52bb4b65c3dde31df94e9686654bf616166c7311b531213ebf0010f67a32ce827e677b1&scene=21#wechat_redirect)

