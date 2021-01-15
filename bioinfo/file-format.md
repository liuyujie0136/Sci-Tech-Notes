# 生物信息学常见文件格式

## BED文件

BED文件(Browser Extensible Data)格式是UCSC Genome Browser的一个格式，提供了一种灵活的方式来定义的数据行，以用来描述注释信息。BED行有3个必须的列和9个额外可选的列，每行的数据格式要求一致。

**必须有以下3列**
* chrom：即染色体号
* chromStart：即feature在染色体上起始位置，在染色体上最左端坐标是0
* chromEnd：即feature在染色体上的终止位置。例如一个染色体前100个碱基定义为chromStart=0, chromEnd=100, 跨度为0-99

**可选的9列**
* name：feature的名字，在基因组浏览器左边显示
* score：在基因组浏览器中显示的灰度设定，值介于0-1000
* strand：定义链的方向，`+`或者`-`
* thickStart：起始位置(例如基因起始编码位置）
* thickEnd：终止位置（例如基因终止编码位置）　
* itemRGB：是一个RGB值的形式, R, G, B (eg. 255, 0,0), 如果itemRgb设置为`On`, 这个RBG值将决定数据的显示的颜色
* blockCount：BED行中的block数目，也就是外显子数目
* blockSize：用逗号分割的外显子的大小, 这个item的数目对应于BlockCount的数目
* blockStarts：用逗号分割的列表, 所有外显子的起始位置，数目也与blockCount数目对应


## GTF/GFF文件

GFF全称为General Feature Format；GTF全称为Gene Transfer Format。

GTF文件以及GFF文件都由9列数据组成，这两种文件的前8列都是相同的（一些小的差别），第9列信息展现方式有所不同:

* `seqid` - name of the chromosome or scaffold; chromosome names can be given with or without the 'chr' prefix. Important note: the seq ID must be one used within Ensembl, i.e. a standard chromosome name or an Ensembl identifier such as a scaffold ID, without any additional content such as species or assembly. See the example GFF output below. (一般为chr或者scanfold编号) 
* `source` - name of the program that generated this feature, or the data source (database or project name) (注释的来源，如果未知用.代替) 
* `type` - type of feature. Must be a term or accession from the SOFA sequence ontology (注释信息的类型，比如Gene、cDNA、mRNA、CDS等) 
* `start` - Start position of the feature, with sequence numbering starting at 1.
* `end` - End position of the feature, with sequence numbering starting at 1.
* `score` - A floating point value. (序列相似性比对时的E-values值或者基因预测是的P-values值，“.”表示为空) 
* `strand` - defined as + (forward) or - (reverse).(正反义链) 
* `phase` - One of '0', '1' or '2'. '0' indicates that the first base of the feature is the first base of a codon, '1' that the second base is the first base of a codon, and so on. (CDS类型中指出该值，值为CDS的起始位置，除以3得到的余数) 
* `attributes` - A semicolon-separated list of tag-value pairs, providing additional information about each feature. Some of these tags are predefined, e.g. ID, Name, Alias, Parent. 
  * **GFF**：以多个键值对组成的注释信息描述，键与值之间用`=`，不同的键值用`;`
  * **GTF**：以`gene_id`或`transcript_id`开头，但标签与值之间以空格分开，值常加引号，每个特征之后都要有分号间隔（包括最后一个特征）


## FASTQ文件

二代测序平台获得的原始数据为fastq（或为压缩文件fq.gz）格式，包含双末端测序所得的正向和反向两个文件（通常用“1”和“2”来区分），其内容如下：

* 每一个read包含**四行**内容
* 第一行以`@`开头，后面是reads的属性信息，也即read名称，中间用“:”隔开。
* 第二行为read序列信息。一般条件下read1里面最前面为特异性Barcode和反向引物的序列，read2里面最前面为正向引物的序列。
* 第三行以`+`开头，一般与`@`后面的内容相同，常省略，但`+`一定不能省。
* 第四行代表read每个碱基的测序质量。每个碱基对应的字符在ASCII码中对应的十进制数字减去33即为该碱基质量（也即Phred33体系）。如某碱基的质量为D，对应的十进制数字为68，则碱基质量为68-33=35。碱基质量Q=-10*logP，P为碱基被测错的概率。也即Q为30代表被测错的概率为0.001，碱基质量越高，则被测错的概率越低。

### 附：Phred体系

如果直接把Q值直接对应ASCII码，应该挺方便的，但是Q值有时会有负值，再者，看ASCII码的0-31位都是控制字符，没法打印和保存，能打印的从要从32位的空格开始，所以就可以给实际的Q值加上一个固定值，这样就可以打印出来而保存在fastq文件中了。

所以下面就是固定值加多少的问题。Phred33，就是加了33，也就是10变成43，查表是`+`。而当时的Solexa加的是64，这就是Phred64。数据处理时，有些软件会根据碱基质量得分的不同做不同处理，所以需要指定正确的编码方式。一般来说，软件会自动进行判断和处理。

![不同版本质量得分和质量字符ASCII的关系](https://liuyujie0136.github.io/Sci-Tech-Notes/bioinfo/phred.png)

由上图可以看出，Phred+33的字符使用33-73，而+64使用59-104之间的ASCII码。所以，只要ASCII小于59的仅仅在Phred+33中使用，而+64的都大于等于59。对于Illumina 1.8+版本，其使用33-74字符，即大于74的只在Phred+64中使用。

### 如何判断是Phred33还是Phred64

默认读取1000条序列，在这1000条序列中：
* 如果有2个以上的质量字符ASCII值小于等于58（即有两个碱基的得分小于等于25），同时没有任何质量字符的ASCII值大于等于75，即判断是Phred+33。
* 如果有2个以上的质量字符ASCII值大于等于75（即有两个碱基的得分大于等于10），同时没有任何质量字符的ASCII值小于等于58，即判断是Phred+64。
* 如果所有质量字符的ASCII值介于59到74之间，即判断可能是Phred+33，但建议使用更多的序列做进一步测试。出现这种结果可能有两种情况：(1) Phred+33编码，所有碱基质量得分介于26到42之间；(2)Phred+64编码，所有碱基质量得分介于-5到10。是前者的可能性大。
* 如果出现上述3种以外的情况，建议打印出质量字符的ASCII值人工判断，脚本如下：
```bash
cat test.fq | head -n 1000 | awk '{if(NR%4==0) printf("%s",$0);}' \
| od -A n -t u1 -v \
| awk 'BEGIN{min=100;max=0;} \
{for(i=1;i<=NF;i++) {if($i>max) max=$i; if($i<min) min=$i;}}END \
{if(max<=126 && min<59) print "Phred33"; \
else if(max>73 && min>=64) print "Phred64"; \
else if(min>=59 && min<64 && max>73) print "Solexa64"; \
else print "Unknown score encoding"; \
```


## VCF文件

VCF是用于描述SNP，INDEL和SV结果的文本文件。VCF文件分为两部分内容：以"#"开头的注释部分；没有"#"开头的主体部分。注释部分有很多对VCF的介绍信息，仔细查看注释部分就可以完全明白VCF各行各列的意义。主体部分中每一行代表一个Variant的信息。

* **CHROM 和 POS**：代表参考序列名和variant的位置；如果是INDEL的话，位置是INDEL的第一个碱基位置。
* **ID**：variant的ID。比如在dbSNP中有该SNP的id，则会在此行给出；若没有，则用"."表示其为一个novel variant。
* **REF 和 ALT**：参考序列的碱基 和 Variant的碱基。
* **QUAL**：Phred格式(Phred_scaled)的质量值，表示在该位点存在variant的可能性；该值越高，则variant的可能性越大；计算方法：`Phred = -10 * log (1-p)` ，p为variant存在的概率; 通过计算公式可以看出值为10的表示错误概率为0.1，该位点为variant的概率为90%。
* **FILTER**：使用上一个QUAL值来进行过滤的话，是不够的。GATK能使用其它的方法来进行过滤，过滤结果中通过则该值为"PASS"或"."。
* **INFO**：这一行是variant的详细信息，以"TAG=Value"，并使用";"分隔的形式。其中很多的注释信息在VCF文件的头部注释中给出：
  * **AC，AF 和 AN**：AC(Allele Count) 表示该Allele的数目；AF(Allele Frequency) 表示Allele的频率； AN(Allele Number) 表示Allele的总数目。对于1个diploid sample而言：则基因型 0/1 表示sample为杂合子，Allele数为1(双倍体的sample在该位点只有1个等位基因发生了突变)，Allele的频率为0.5(双倍体的sample在该位点只有50%的等位基因发生了突变)，总的Allele为1； 基因型 1/1 则表示sample为纯合的，Allele数为2，Allele的频率为1，总的Allele为2。
  * **DP**：reads覆盖度。是一些reads被过滤掉后的覆盖度。
  * **Dels**：Fraction of Reads Containing Spanning Deletions。进行SNP和INDEL calling的结果中，有该TAG并且值为0表示该位点为SNP，没有则为INDEL。
  * **FS**：使用Fisher's精确检验来检测strand bias而得到的Fhred格式的p值。该值越小越好。一般进行filter的时候，可以设置 FS < 10～20。
  * **HaplotypeScore**：Consistency of the site with at most two segregating haplotypes
  * **InbreedingCoeff**：Inbreeding coefficient as estimated from the genotype likelihoods per-sample when compared against the Hard-Weinberg expectation
  * **MLEAC**：Maximum likelihood expectation (MLE) for the allele counts (not necessarily the same as the AC), for each ALT allele, in the same order as listed
  * **MLEAF**：Maximum likelihood expectation (MLE) for the allele frequency (not necessarily the same as the AF), for each ALT alle in the same order as listed
  * **MQ**：RMS Mapping Quality
  * **MQ0**：Total Mapping Quality Zero Reads
  * **MQRankSum**：Z-score From Wilcoxon rank sum test of Alt vs. Ref read mapping qualities
  * **QD**：Variant Confidence/Quality by Depth
  * **RPA**：Number of times tandem repeat unit is repeated, for each allele (including reference)
  * **RU**：Tandem repeat unit (bases)
  * **ReadPosRankSum**：Z-score from Wilcoxon rank sum test of Alt vs. Ref read position bias
  * **STR**：Variant is a short tandem repeat
* **FORMAT 和 B001**：这两行合起来提供了"B001"这个sample的基因型的信息。"B001"代表这该名称的样品，是由BAM文件中的@RG下的SM标签决定的。
  * **GT**：样品的基因型（genotype）。两个数字中间用’/'分开，这两个数字表示双倍体的sample的基因型。0 表示样品中有ref的allele； 1 表示样品中variant的allele； 2表示有第二个variant的allele。因此： 0/0 表示sample中该位点为纯合的，和ref一致； 0/1 表示sample中该位点为杂合的，有ref和variant两个基因型； 1/1 表示sample中该位点为纯合的，和variant一致。
  * **AD 和 DP**：AD(Allele Depth)为sample中每一种allele的reads覆盖度,在diploid（二倍体）中则是用逗号分割的两个值，前者对应ref基因型，后者对应variant基因型； DP（Depth）为sample中该位点的覆盖度。
  * **GQ**：基因型的质量值(Genotype Quality)。Phred格式(Phred_scaled)的质量值，表示在该位点该基因型存在的可能性；该值越高，则Genotype的可能性越大；计算方法：`Phred = -10 * log (1-p)`，p为基因型存在的概率。
  * **PL**：指定的三种基因型的质量值(provieds the likelihoods of the given genotypes)。这三种指定的基因型为(0/0,0/1,1/1)，这三种基因型的概率总和为1。和之前不一致，该值越大，表明为该种基因型的可能性越小。 `Phred = -10 * log (p)`，p为基因型存在的概率。



