# 关于readsCount、RPKM/FPKM、RPM(CPM)、TPM的理解
> https://www.jianshu.com/p/c25e84383ae3

## 0. 背景

### feature

- 定义：基因组上对具有不同性质区域的定义 (例如：gene/exon/intron/miRNA等)。
- 优点：利于分类整理结果。

### 两类测序bias

- 长度bias：相同表达丰度的转录本，往往会由于其基因长度上的差异，导致测序获得的Read（Fregment）数不同。总的来说，越长的转录本，测得的Read（Fregment）数越多。
- 测序深度bias：由测序文库的不同大小而引来的差异。即同一个转录本，其测序深度越深，通过测序获得的Read（Fregment）数就越多。
- 本文提到的RPKM/FPKM/RPM等方法都是为了消除这两类bias而进行的标准化。例如：RPM主要应用于sRNA（sRNA长度变化不大），来消除测序深度bias；RPKM/FPKM应用范围会更广泛，用于同时消除长度及测序深度这两种bias。
- **下面的计算以exon为例。**

## 1. 几种丰度计算方法

### reads Count

- 定义：高通量测序中比对到exon上的reads数。可使用featureCount等软件进行计算。
- 优点：可有效说明该区域是否真的有表达及真实的表达丰度。能够近似呈现真实的表达情况。有利于实验验证。
- 缺点：由于exon长度不同，难以进行不同exon丰度比较；由于测序总数不同，难以对不同测序样本间进行比较。

### RPKM/FPKM

定义：
- **RPKM**: Reads Per Kilobase of exon model per Million mapped reads (每千个碱基的转录每百万映射读取的reads)；
- **FPKM**: Fragments Per Kilobase of exon model per Million mapped fragments(每千个碱基的转录每百万映射读取的fragments)  

![公式(1):RPKM=\frac{ExonMappedReads * 10^9}{TotalMappedReads * ExonLength}](https://math.jianshu.com/math?formula=%E5%85%AC%E5%BC%8F(1)%3ARPKM%3D%5Cfrac%7BExonMappedReads%20*%2010%5E9%7D%7BTotalMappedReads%20*%20ExonLength%7D)  

![公式(2):FPKM=\frac{ExonMappedFragments * 10^9}{TotalMappedFragments * ExonLength}](https://math.jianshu.com/math?formula=%E5%85%AC%E5%BC%8F(2)%3AFPKM%3D%5Cfrac%7BExonMappedFragments%20*%2010%5E9%7D%7BTotalMappedFragments%20*%20ExonLength%7D)  

上述公式1 RPKM可从下面公式推导而出：  

![公式(3): RPKM=\frac{(ExonMappedReads / TotalMappedReads * 10^6)}{ExonLength}*10^3](https://math.jianshu.com/math?formula=%E5%85%AC%E5%BC%8F(3)%3A%20RPKM%3D%5Cfrac%7B(ExonMappedReads%20%2F%20TotalMappedReads%20*%2010%5E6)%7D%7BExonLength%7D*10%5E3)  

![等价于：RPKM = (RPM / ExonLength) * 10^3](https://math.jianshu.com/math?formula=%E7%AD%89%E4%BB%B7%E4%BA%8E%EF%BC%9ARPKM%20%3D%20(RPM%20%2F%20ExonLength)%20*%2010%5E3)  

![公式(4): RPKM=\frac{(ExonMappedReads * ReadLength)/ ExonLength * 10^9}{(TotalMappedReads * ReadLength/GenomeLength)}](https://math.jianshu.com/math?formula=%E5%85%AC%E5%BC%8F(4)%3A%20RPKM%3D%5Cfrac%7B(ExonMappedReads%20*%20ReadLength)%2F%20ExonLength%20*%2010%5E9%7D%7B(TotalMappedReads%20*%20ReadLength%2FGenomeLength)%7D)

解释：ExonMappedReads即为比对到该exon上的reads count；ReadLength是测序的Read长度； TotalMappedReads即为比对到基因组上所有reads count的总和；ExonLength 为该Exon的长度；ReadLength可消除；GenomeLength即为基因组全长，因为是相同基因组，所以该数值也可消除。

公式4中，TotalMappedReads \* ReadLength/ GenomeLength为基因组上每个碱基的测序深度，ExonMappedReads \* ReadLength / ExonLength可以简单的认为是该Exon上每个碱基的“测序深度”。两者相除，就得出该Exon上每个碱基相对基因组进行标准化的'测序深度'。因一般是相同物种，基因组一般相同，且测序长度相同，所以公式4换算并消去ReadLength，GenomeLength，就成为公式1的形式了。那么因Exon长短、测序深度造成的样本间造成的偏差，都可以消除。个人认为RPKM/FPKM应当能够消除两种类型的bias。

- 优点：tophat-cufflinks流程固定，应用范围广。理论上，可弥补reads Count的缺点，消除样本间和基因间差异。
- 讨论：有人说RPKM/FPKM标准化不合理，好像说的很有道理，也没太多时间整理了。[黄树嘉](https://www.jianshu.com/p/35e861b76486)的描述与RPKM/FPKM的差异：他的文章主要是说一个Exon整体转录了几次？主要描述为一个Exon整体转录情况。但是公式1RPKM主要计算的是Exon上单个碱基的转录情况，一个Exon上所有碱基转录情况的均值代表了该Exon转录情况。所以RPKM/FPKM可能与真实表达情况是有偏差的，但是应该能够大致反映真实的情况。
- FPKM：与RPKM计算过程类似。只有一点差异：RPKM计算的是reads，FPKM计算的是fragments。single-end/paired-end测序数据均可计算reads count，fragments count只能通过paired-end测序数据计算。paired-end测序数据时，两端的reads比对到相同区域，且方向相反，即计数1个fragments；如果只有单端reads比对到该区域，则一个reads即计数1个fragments。所以reads count接近且小于2 \*fragments count。

### RPM

- 定义：_RPM/CPM_: Reads/Counts of exon model per Million mapped reads (每百万映射读取的reads)
- 公式：_RPM_ = ExonMappedReads \* 10^6 /TotalMappedReads  
    ![RPM=\frac{ExonMappedReads * 10^6}{TotalMappedReads}](https://math.jianshu.com/math?formula=RPM%3D%5Cfrac%7BExonMappedReads%20*%2010%5E6%7D%7BTotalMappedReads%7D)
- 优点：利于进行样本间比较。根据比对到基因组上的总reads count，进行标准化。即：不论比对到基因组上的总reads count是多少，都将总reads count标准化为10^6。sRNA\_seq等测序长度较短的高通量测序经常采用RPM进行标准化，因为sRNA长度差异较小，18-35 nt较多，所以长度对不同的small RNAs相互比较影响较小 (优点：计算简单、方便。)。
- 缺点：未消除exon长度造成的表达差异，难以进行样本内exon差异表达的比较。

### TPM

- 定义：_TPM_: Transcripts Per Kilobase of exon model per Million mapped reads (每千个碱基的转录每百万映射读取的Transcripts)
- 公式：![TPM=\frac{Ni/Li * 10^6}{sum(N1/L1+N2/L2 + ... + Nn/Ln)}](https://math.jianshu.com/math?formula=TPM%3D%5Cfrac%7BNi%2FLi%20*%2010%5E6%7D%7Bsum(N1%2FL1%2BN2%2FL2%20%2B%20...%20%2B%20Nn%2FLn)%7D)  
    解释：Ni为比对到第i个exon的reads数； Li为第i个exon的长度；sum(N1/L1+N2/L2 + ... + Nn/Ln)为所有 (n个)exon按长度进行标准化之后数值的和。
- 计算过程：首先对每个exon计算Pi=Ni/Li，即按长度对reads count进行标准化；随后计算过程类似RPM (将Pi作为正常的ExonMappedReads，然后以RPM的公式计算TPM)。
- 优点：首先消除exon长度造成的差异，随后消除样本间测序总reads count不同造成的差异。
- 缺点：因为不是采用比对到基因组上的总reads count，所以特殊情况下不够准确。例如：某突变体对exon造成整体影响时，难以找出差异。

## 2. 相互关系

- 评价：以上几种计算exon表达丰度的方法，差异不是非常大。如果结果是显著的，那么采用上面任一计算方法大多均可找出显著结果。但是当表达丰度差异不是那么显著时，不易区分不同类别，需要根据实际需要选择对应的标准化方法。通常情况下首先推荐FPKM值；计算样本差异时(edgeR/DESeq2等)，采用raw read count；计算sRNA丰度时，通常采用RPM值。
- 注意：以上TotalMappedReads推荐首选比对到基因组上的总reads数，而不是比对到exon或者gene上总reads数。而且需要注意可能存在某一区域产生大量序列的情况，如：rRNA/tRNA及一些未注释区域等。如果该区域可能影响最终结果，TotalMappedReads需要减去这些产生过量序列区域产生的序列或进行Deduplicate处理（Deduplicate很多时候都是很有必要的）。这同样需要根据实际情况而定。


## 3. Use TPM !
> https://www.rna-seqblog.com/rpkm-fpkm-and-tpm-clearly-explained/

It used to be when you did RNA-seq, you reported your results in RPKM (Reads Per Kilobase Million) or FPKM (Fragments Per Kilobase Million). However, TPM (Transcripts Per Kilobase Million) is now becoming quite popular. Since there seems to be a lot of confusion about these terms, I thought I’d use a StatQuest to clear everything up.

These three metrics attempt to normalize for sequencing depth and gene length. Here’s how you do it for RPKM:
1. Count up the total reads in a sample and divide that number by 1,000,000 – this is our “per million” scaling factor.
2. Divide the read counts by the “per million” scaling factor. This normalizes for sequencing depth, giving you reads per million (RPM)
3. Divide the RPM values by the length of the gene, in kilobases. This gives you RPKM.

FPKM is very similar to RPKM. RPKM was made for single-end RNA-seq, where every read corresponded to a single fragment that was sequenced. FPKM was made for paired-end RNA-seq. With paired-end RNA-seq, two reads can correspond to a single fragment, or, if one read in the pair did not map, one read can correspond to a single fragment. The only difference between RPKM and FPKM is that FPKM takes into account that two reads can map to one fragment (and so it doesn’t count this fragment twice).

TPM is very similar to RPKM and FPKM. The only difference is the order of operations. Here’s how you calculate TPM:
1. Divide the read counts by the length of each gene in kilobases. This gives you reads per kilobase (RPK).
2. Count up all the RPK values in a sample and divide this number by 1,000,000. This is your “per million” scaling factor.
3. Divide the RPK values by the “per million” scaling factor. This gives you TPM.

So you see, when calculating TPM, the only difference is that you normalize for gene length first, and then normalize for sequencing depth second. However, the effects of this difference are quite profound.

When you use TPM, the sum of all TPMs in each sample are the same. This makes it easier to compare the proportion of reads that mapped to a gene in each sample. In contrast, with RPKM and FPKM, the sum of the normalized reads in each sample may be different, and this makes it harder to compare samples directly.

Here’s an example. If the TPM for gene A in Sample 1 is 3.33 and the TPM in sample B is 3.33, then I know that the exact same proportion of total reads mapped to gene A in both samples. This is because the sum of the TPMs in both samples always add up to the same number (so the denominator required to calculate the proportions is the same, regardless of what sample you are looking at.)

With RPKM or FPKM, the sum of normalized reads in each sample can be different. Thus, if the RPKM for gene A in Sample 1 is 3.33 and the RPKM in Sample 2 is 3.33, I would not know if the same proportion of reads in Sample 1 mapped to gene A as in Sample 2. This is because the denominator required to calculate the proportion could be different for the two samples.
