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


