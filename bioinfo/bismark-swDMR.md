# 全基因组重亚硫酸盐测序（Whole Genome Bisulfite Sequencing）甲基化分析

> 参考：  
> https://github.com/FelixKrueger/Bismark  
> https://github.com/xflicsu/swDMR  
> https://www.jianshu.com/p/0aa453ba9476

表观遗传学研究已经证实了特定基因区域的DNA甲基化修饰对于染色体构象、基因表达调控机制、转录因子结合和抑制转座子元件等有着重要影响，而全基因组DNA甲基化研究将是表观基因组学最为关注的内容之一。

目前，DNA甲基化检测的技术已经比较成熟，例如高通量的WGBS、RRBS、MeDIP-seq、MBD-seq，低通量的BSP、MSP等，其中以WGBS（Whole genome bisulfite sequencing）最为经典。Bisulfite处理能够将基因组中**未发生甲基化的C碱基转换成U**，进行PCR扩增后变成T，与原本具有甲基化修饰的C碱基区分开来，再结合高通量测序技术，可绘制单碱基分辨率的全基因组DNA甲基化图谱。

## Bismark

### Bismark Genome Preparation（建立索引）

```bash
bismark_genome_preparation \
    --bowtie2 \
    --path_to_aligner ~/Software/bowtie2-2.3.3/ \
    --verbose \
    ~/bismark_example/01index/

# 重要参数说明
bismark_genome_preparation
--bowtie2/--hisat2：调用bowtie2/hisat2建立基因组索引
--path_to_aligner：比对软件所在文件夹的全路径
--verbose：输出详情
--parallel：设置线程，索引建立是并行运行，因此实际线程要×2
--large-index：大基因组索引建立
--yes：如果有安全类问题则自动选择yes，比如覆盖某个已存在的文件
<path_to_genome_folder>：基因组所在文件夹路径，即~/bismark_example/01index/
```

### Bismark（进行比对）

```bash
bismark -N 0 -L 20 \
    --un --ambiguous --sam \
    --bowtie2 \
    --path_to_bowtie2 ~/Software/bowtie2-2.3.3/ \
    -o ~/bismark_example/02compare \
    --fastq \
    --prefix test.file \
    --genome ~/bismark_example/01index/ \
    -1 ~/bismark_example/R1.fq.gz \
    -2 ~/bismark_example/R2.fq.gz

# 重要参数说明
bismark
-N：设置seed中允许的最大错配数，可取0或1，默认为0。值越高，对齐速度越慢，灵敏度越高。
-L：设置seed长度，最大值为32，默认为20。值越高，对齐速度越快，灵敏度越低。
--quiet：不输出比对流程信息
--un：过滤多处匹配的reads
--ambiguous：多处匹配reads信息独立记录
--sam/--bam：输出SAM格式，与--parallel不兼容/输出BAM格式，可调整--parallel
--bowtie2/--hisat2 ：调用bowtie2/hisat2，默认bowtie2
--path_to_bowtie2/--path_to_hisat2 ：bowtie2/hisat2所在文件夹的全路径
-o/--output_dir ：输出文件的全路径
--samtools_path：samtools所在文件夹的全路径
--prefix：指定输出文件的前缀
--q/--fastq：输入文件为FastQ
-f/--fasta：输入文件为FastA
--phred33-quals/--phred64-quals：指定FastQ文件的质量分数格式，默认为phred33
--genome：包含未修改的参考基因组和bismark_genome_preparation创建的子文件夹(CT_conversion/和GA_conversion/）的文件夹的路径，即~/bismark_example/01index/
-1/-2：双端测序文件

# 输出文件
(a) test.file.R1_bismark_bt2_pe.sam 所有对齐和甲基化的信息
(b) test.file.R1_bismark_bt2_PE_report.txt 对齐和甲基化的主要信息概括
```

### Duplicate Bismark（过滤重复）

```bash
deduplicate_bismark \
    -sam -p \
    ~/bismark_example/02compare/test.file.R1_bismark_bt2_pe.bam \
    --output_dir ~/bismark_example/03duplicate/

# 重要参数说明
deduplicate_bismark
--sam/--bam：删除 Bismark 对齐产生的SAM/BAM 文件中的重复数据，建议用于WGBS，但不建议应用于RRS (reduced representation shotgun)，如 RRBS、amplicon or target enrichment libraries。
-p/--paired ：前一步双端数据产生的结果文件
-s/--single：前一步单端数据产生的结果文件
--samtools_path：samtools所在文件夹的全路径
--output_dir：输出文件夹路径
--multiple：指定输入文件都作为一个样本处理，连接在一起进行重复数据删除。
对SAM文件使用Unix“cat”，对BAM文件使用“samtools cat”。所有输入文件的格式必须相同。默认情况下，标头取自要连接的第一个文件。

# 输出文件
(a) test.file.R1_bismark_bt2_pe.deduplicated.bam
(b) test.file.R1_bismark_bt2_pe.deduplication_report.txt
```

### Bismark Methylation Extractor（甲基化信息提取）

```bash
bismark_methylation_extractor \
    -p --gzip --no_overlap \
    --comprehensive \
    --bedGraph \
    --count \
    --CX_context \
    --cytosine_report \
    --buffer_size 20G \
    --genome_folder ~/bismark_example/01index/ \
    -o testfile_report
    ~/bismark_example/03duplicate/test.file.R1_bismark_bt2_pe.deduplicated.bam

# 重要参数说明
bismark_methylation_extractor
--comprehensive ：合并所有四个可能的特定链，将甲基化信息转换为context-dependent的输出文件
--no_overlap：避免因双端读取read_1和read_2理论上的重叠，导致甲基化重复计算。可能会删去相当大部分的数据，对于双端数据的处理，默认情况下此选项处于启用状态，可以使用--include_overlap禁用。
-p/--paired-end：前一步双端数据产生的结果文件
--bedGraph：指将产生一个BedGraph文件存储CpG的甲基化信息（不与--CX联用，仅产生CpG信息）
--CX/--CX_context：与--bedGraph联用，产生一个包含三种类型甲基化信息的BedGraph文件;与--cytosine_report联用，产生一个包含三种类型甲基化信息的全基因组甲基化报告
--cytosine_report：指将产生一个存储CpG的甲基化信息的报告（不与--CX联用时，仅产生CpG信息），默认情况坐标系基于 1
--buffer_size：甲基化信息进行排序时指定内存，默认为2G
--zero_based：使用基于 0 的基因组坐标而不是基于 1 的坐标，默认值：OFF
--split_by_chromosome：分染色体输出
--genome_folder：包含未修改的参考基因组和bismark_genome_preparation创建的子文件夹(CT_conversion/和GA_conversion/）的文件夹的路径，即~/bismark_example/01index/
<filenames> :BAM 格式的 Bismark 结果文件
```

#### 输出文件

1. `CHG/CHH/CpG_context_test.file.R1_bismark_bt2_pe.deduplicated.txt`

```
col1 : 比对上的序列ID
col2 : 基因组正负链：+ -
col3 : 染色体编号
col4 : 染色体位置
col5 : 甲基化C的状态（XxHhZzUu）

X 代表CHG中甲基化的C
x  代笔CHG中非甲基化的C
H 代表CHH中甲基化的C
h  代表CHH中非甲基化的C
Z  代表CpG中甲基化的C
z  代表CpG中非甲基化的C
U 代表其他情况的甲基化C(CN或者CHN)
u  代表其他情况的非甲基化C (CN或者CHN)

CpG：甲基化C下游是个G碱基
CHH：甲基化C下游的2个碱基都是H（A、C、T）
CHG：甲基化的C下游的2个碱基是H和G
```

2. `test.file.R1_bismark_bt2_pe.deduplicated.bedGraph.gz`

```
col1 : 染色体编号
col2 : 胞嘧啶（c）位置信息
col3 : 胞嘧啶（c）位置信息
col4 : 甲基化率
```

3. `test.file.R1_bismark_bt2_pe.deduplicated.bismark.cov.gz`

```
col1 : 染色体编号
col2 : 起始位置
col3 : 终止位置
col4 : 甲基化率 （col5/col5+col6）
col5 : 甲基化数目
col6 : 非甲基化数目 
```

4. `test.file.R1_bismark_bt2_pe.deduplicated.CX_report.txt`

```
col1 : 染色体编号
col2 : 位置
col3 : 正负链信息
col4 : 甲基化碱基数目
col5 : 非甲基化碱基数目
col6 : 类型
col7 : 具体背景 
```

5. `test.file.R1_bismark_bt2_pe.deduplicated.M-bias.txt/...R1.png/...R2.png`

```
#reads中每个可能位置的甲基化比例
CpG context (R1)
col1 : read position
col2 : 甲基化count（count methylated）
col3 : 非甲基化count（count unmethylated）
col4 : 甲基化率（beta）
col5 : coverage
```

M-bias plot 通过Perl模块`GD:：Graph`产生，没有模块的只产生M-bias.txt文件，可用`--ignore`参数忽略

![test.file.R1_bismark_bt2_pe.deduplicated.M-bias_R1.png](https://upload-images.jianshu.io/upload_images/26617878-a9702f851d7e7f9b.png)

![test.file.R1_bismark_bt2_pe.deduplicated.M-bias_R2.png](https://upload-images.jianshu.io/upload_images/26617878-8845827dce6bb755.png)

6. `test.file.R1_bismark_bt2_pe.deduplicated_splitting_report.txt` 甲基化总体报告

![report.png](https://upload-images.jianshu.io/upload_images/26617878-0f8b6ff7410c9dee.png)


## swDMR
