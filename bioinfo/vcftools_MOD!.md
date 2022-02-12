# VCFtools用来处理VCF文件

1. 筛选特定突变
2. 比较文件
3. 总结突变
4. 转化文件格式
5. 验证并合并文件
6. 取突变交集和差集

### Get basic file statistics

input可以为VCF或BCF格式（`--vcf --gvcf or --bcf`）。

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf</code></div><div class="line number2 index1 alt1"><code class="r plain">less test.vcf | vcftools --vcf -</code></div></div></td></tr></tbody></table>

### Applying a filter

可以把筛选的突变写入一个新文件。`--recode` 表示输出筛选的内容，`--recode-INFO-all` 保留所有的INFO fields的内容。default情况下，INFO fields不写，因为筛选会改变文件里的突变情况。染色体名字要注意，比如是chr1还是1，要写全。\--out输出到特定的位置和特定的文件名

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf --chr chr1 --from-bp 1000000 --to-bp 2000000 --recode --recode-INFO-all --out result</code></div></div></td></tr></tbody></table>

### Writing out to screen

用`--stdout or -c` 可以重定位所有输出到标准输出，可以通过管道输出给别的软件或者直接输入到某个输出文件。

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf --chr chr1 --from-bp 1000000 --to-bp 2000000 --recode --stdout | less</code></div><div class="line number2 index1 alt1"><code class="r plain">vcftools --vcf test.vcf --chr chr1 --from-bp 1000000 --to-bp 2000000 --recode -c &gt; ../subset.vcf</code></div><div class="line number3 index2 alt2"><code class="r plain">vcftools --vcf test.vcf --chr chr1 --from-bp 1000000 --to-bp 2000000 --recode -c |gzip -c &gt; ../subset.vcf.gz</code></div></div></td></tr></tbody></table>

### comparing two files

比较2个VCF文件，看哪些个体或者位点是2个文件共享的。指定第二个文件要用`--diff --gzdiff or --diff-bcf`。

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test1.vcf --diff test2.vcf --out compare</code></div></div></td></tr></tbody></table>

### Getting allele frequency

使用\--freq计算等位基因频率

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf --freq --out output</code></div></div></td></tr></tbody></table>

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div><div class="line number2 index1 alt1">2</div><div class="line number3 index2 alt2">3</div><div class="line number4 index3 alt1">4</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">CHROM POS N_ALLELES N_CHR {ALLELE:FREQ}</code></div><div class="line number2 index1 alt1"><code class="r plain">chr1 861315 2 604 G:0.996689 A:0.00331126</code></div><div class="line number3 index2 alt2"><code class="r plain">chr1 865655 2 604 T:0.998344 A:0.00165563</code></div><div class="line number4 index3 alt1"><code class="r plain">chr1 865664 2 604 C:0.998344 T:0.00165563</code></div></div></td></tr></tbody></table>

### Getting sequencing depth information

使用\--depth计算测序深度。

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf --depth -c &gt; depth_summary.txt</code></div></div></td></tr></tbody></table>

### Getting linkage disequilibrium statistics

计算连锁不平衡，500kb为一个窗口。可以用`--hap-r2 --geno-r2 or --geno-chisq`。因为是两两比较，比较耗费时间，建议用`--ld-windows --ld-windows-bp or --min-r2` 去减少比较的次数。

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf --hap-r2 --ld-window-bp 500000 --out ld_windows_500000</code></div></div></td></tr></tbody></table>

### Getting Fst population statistics

必须提供text文件，每行一个个体（同一个population）。使用\--weir-fst-pop进行Fst计算。

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf --weir-fst-pop population1.txt --weir-fst-pop population2.txt --out pop1VSpop2</code></div></div></td></tr></tbody></table>

### Converting VCF files to PLINK format

使用\--plink进行文件转换

[?](https://www.cnblogs.com/triple-y/p/10331449.html#)

<table border="0" cellpadding="0" cellspacing="0"><tbody><tr><td class="gutter"><div class="line number1 index0 alt2">1</div></td><td class="code"><div class="container"><div class="line number1 index0 alt2"><code class="r plain">vcftools --vcf test.vcf --plink --chr 1 --out output_in_plink</code></div></div></td></tr></tbody></table>

-

-

## vcftools使用说明

vcftools是一种可以对VCF文件和BCF文件进行格式转换及过滤的工具，其中很多过滤及计算功能我们可以自己使用perl或者python编写脚本实现，但都不如这个工具的运算速度快。目前官网的版本是vcftools v0.1.13。

中文版本：[https://www.jianshu.com/p/badd24cbc538](https://www.jianshu.com/p/badd24cbc538)

# 基本参数

## 输入参数

- --vcf <input\_filename> 支持v4.0、v4.1或者v4.2版本的VCF文件
- --gzvcf <input\_filename> 通过gzipped压缩过的VCF文件
- --bcf <input\_filename>

## 输出参数

- --out <output\_prefix> 输出文件，后面直接对输出文件命名
- --stdout 可接管道符对输出结果进行重新定向
- --temp <temporary\_directory> 指定结果的输出目录

## 过滤参数

### 根据位置过滤

- --chr 指令可多次使用
- --not-chr 指令可以多次使用
- --from-bp
- --to-bp 这两个参数需要和--chr一起使用
- --positions
- --exclude-positions Include or exclude a set of sites on the basis of a list of positions in a file. Each line of the input file should contain a (tab-separated) chromosome and position. The file can have comment lines that start with a “#”, they will be ignored.自己理解下就好

### 根据位点过滤

- --snp 字符串的名称可以匹配dbSNP的数据，适合人类基因组，该指令可多次使用
- --snps
- \-exclude Include or exclude a list of SNPs given in a file. The file should contain a list of SNP IDs (e.g. dbSNP rsIDs), with one ID per line. No header line is expected.自己理解下就好

### 变异类型过滤

- --keep-only-indels 只保留indel标记
- --remove-indels 删除indel标记

### 根据flag过滤

- --remove-filtered-all Removes all sites with a FILTER flag other than PASS.
- --keep-filtered
- --remove-filtered

ncludes or excludes all sites marked with a specific FILTER flag. These options may be used more than once to specify multiple FILTER flags.自己理解下就好

### 根据INFO过滤

- --keep-INFO
- --remove-INFO

Includes or excludes all sites with a specific INFO flag. These options only filter on the presence of the flag and not its value. These options can be used multiple times to specify multiple INFO flags.自己理解下就好

### 根据ALLELE过滤

- --maf MAF最小值过滤
- --max-maf MAF最大值过滤

此处省去很多参数，具体参见[vcftools官网](http://vcftools.sourceforge.net/man_latest.html)

### 根据基因型数值过滤

- --min-meanDP
- --max-meanDP   
    根据测序深度进行过滤
- --hwe

Assesses sites for Hardy-Weinberg Equilibrium using an exact test, as defined by Wigginton, Cutler and Abecasis (2005). Sites with a p-value below the threshold defined by this option are taken to be out of HWE, and therefore excluded.

- --max-missing 完整度，该参数介于0，1之间

### 根据材料过滤

- --indv
- --remove-indv

Specify an individual to be kept or removed from the analysis. This option can be used multiple times to specify multiple individuals. If both options are specified, then the “--indv” option is executed before the “--remove-indv option”.

- --keep
- --remove

Provide files containing a list of individuals to either include or exclude in subsequent analysis. Each individual ID (as defined in the VCF headerline) should be included on a separate line. If both options are used, then the “--keep” option is executed before the “--remove” option. When multiple files are provided, the union of individuals from all keep files subtracted by the union of individuals from all remove files are kept. No header line is expected.

- --max-indv

Randomly thins individuals so that only the specified number are retained.

### 基因型过滤参数

- --remove-filtered-geno-all 排除flag不为’.’和’PASS’的基因型
- --remove-filtered-geno 排除flag为string的基因型
- --minGQ 排除GQ低于这个参数的基因型
- --minDP
- --maxDP

Includes only genotypes greater than or equal to the “--minDP” value and less than or equal to the “--maxDP” value. This option requires that the “DP” FORMAT tag is specified for all sites.

## 计算统计

### 核算多样性统计

- --site-pi 计算所有SNP
- --window-pi
- --window-pi-step

Measures the nucleotide diversity in windows, with the number provided as the window size. The output file has the suffix “.windowed.pi”. The latter is an optional argument used to specify the step size in between windows.滑动窗口计算sliding windows，参考A reference genome for common bean and genome-wide analysis of dual domestications

### FST计算

- --weir-fst-pop

This option is used to calculate an Fst estimate from Weir and Cockerham’s 1984 paper. This is the preferred calculation of Fst. The provided file must contain a list of individuals (one individual per line) from the VCF file that correspond to one population. This option can be used multiple times to calculate Fst for more than two populations. These files will also be included as “--keep” options. By default, calculations are done on a per-site basis. The output file has the suffix “.weir.fst”.

- --fst-window-size
- --fst-window-step

These options can be used with “--weir-fst-pop” to do the Fst calculations on a windowed basis instead of a per-site basis. These arguments specify the desired window size and the desired step size between windows.滑动窗口计算,重测序可以设置10-kb/2-kb sliding windows，参考A reference genome for common bean and genome-wide analysis of dual domestications

### 其它计算

- --het

Calculates a measure of heterozygosity on a per-individual basis. Specfically, the inbreeding coefficient, F, is estimated for each individual using a method of moments. The resulting file has the suffix “.het”.

- --hardy

Reports a p-value for each site from a Hardy-Weinberg Equilibrium test (as defined by Wigginton, Cutler and Abecasis (2005)). The resulting file (with suffix “.hwe”) also contains the Observed numbers of Homozygotes and Heterozygotes and the corresponding Expected numbers under HWE.

- --TajimaD

Outputs Tajima’s D statistic in bins with size of the specified number. The output file has the suffix “.Tajima.D”.

- --indv-freq-burden

This option calculates the number of variants within each individual of a specific frequency. The resulting file has the suffix “.ifreqburden”.

- --LROH

This option will identify and output Long Runs of Homozygosity. The output file has the suffix “.LROH”. This function is experimental, and will use a lot of memory if applied to large datasets.

- --relatedness

This option is used to calculate and output a relatedness statistic based on the method of Yang et al, Nature Genetics 2010 (doi:10.1038/ng.608). Specifically, calculate the unadjusted Ajk statistic. Expectation of Ajk is zero for individuals within a populations, and one for an individual with themselves. The output file has the suffix “.relatedness”.

- --relatedness2

This option is used to calculate and output a relatedness statistic based on the method of Manichaikul et al., BIOINFORMATICS 2010 (doi:10.1093/bioinformatics/btq559). The output file has the suffix “.relatedness2”.

- --site-quality 主要用于提取VCF文件中每个位点的QUAL信息

<table><tbody><tr><td class="gutter"><pre><span class="line">1<br><span class="line">2<br><span class="line">3<br><span class="line">4<br><span class="line">5<br><span class="line">6<br><span class="line">7<br><span class="line">8<br><span class="line">9<br></span></span></span></span></span></span></span></span></span></pre></td><td class="code"><pre><span class="line">vcftools --vcf test.vcf --site-quality  --out quality<br><span class="line">```  <br><span class="line">  <br><span class="line">- --missing-indv<br><span class="line">      <br><span class="line">- --missing-site 计算每个位点的缺失率      <br><span class="line"><br><span class="line">``` bash<br><span class="line">vcftools --vcf test.recode.vcf --missing-site  --out ms<br></span></span></span></span></span></span></span></span></span></pre></td></tr></tbody></table>

- --SNPdensity 计算SNP在设定bin内的密度

<table><tbody><tr><td class="gutter"><pre><span class="line">1<br></span></pre></td><td class="code"><pre><span class="line">vcftools --vcf test.vcf --SNPdensity 1000000 --out SNPden<br></span></pre></td></tr></tbody></table>

- --kept-sites

Creates a file listing all sites that have been kept after filtering. The file has the suffix “.kept.sites”.

- --removed-sites

Creates a file listing all sites that have been removed after filtering. The file has the suffix “.removed.sites”.

- --singletons

This option will generate a file detailing the location of singletons, and the individual they occur in. The file reports both true singletons, and private doubletons (i.e. SNPs where the minor allele only occurs in a single individual and that individual is homozygotic for that allele). The output file has the suffix “.singletons”.

- --hist-indel-len

This option will generate a histogram file of the length of all indels (including SNPs). It shows both the count and the percentage of all indels for indel lengths that occur at least once in the input file. SNPs are considered indels with length zero. The output file has the suffix “.indel.hist”.

- --hapcount

This option will output the number of unique haplotypes within user specified bins, as defined by the BED file. The output file has the suffix “.hapcount”.

- --mendel

This option is use to report mendel errors identified in trios. The command requires a PLINK-style PED file, with the first four columns specifying a family ID, the child ID, the father ID, and the mother ID. The output of this command has the suffix “.mendel”.

- --extract-FORMAT-info

Extract information from the genotype fields in the VCF file relating to a specfied FORMAT identifier. The resulting output file has the suffix “.<FORMAT\_ID>.FORMAT”. For example, the following command would extract the all of the GT (i.e. Genotype) entries:

vcftools --vcf file1.vcf --extract-FORMAT-info GT

- --get-INFO

This option is used to extract information from the INFO field in the VCF file. The argument specifies the INFO tag to be extracted, and the option can be used multiple times in order to extract multiple INFO entries. The resulting file, with suffix “.INFO”, contains the required INFO information in a tab-separated table. For example, to extract the NS and DB flags, one would use the command:

vcftools --vcf file1.vcf --get-INFO NS --get-INFO DB

## 输出格式

- --recode
- --recode-bcf

These options are used to generate a new file in either VCF or BCF from the input VCF or BCF file after applying the filtering options specified by the user. The output file has the suffix “.recode.vcf” or “.recode.bcf”. By default, the INFO fields are removed from the output file, as the INFO values may be invalidated by the recoding (e.g. the total depth may need to be recalculated if individuals are removed). This behavior may be overriden by the following options. By default, BCF files are written out as BGZF compressed files.

- --recode-INFO
- --recode-INFO-all

These options can be used with the above recode options to define an INFO key name to keep in the output file. This option can be used multiple times to keep more of the INFO fields. The second option is used to keep all INFO values in the original file.

- --contigs

This option can be used in conjuction with the --recode-bcf when the input file does not have any contig declarations. This option expects a file name with one contig header per line. These lines are included in the output file.

## 格式转换

- --012

This option outputs the genotypes as a large matrix. Three files are produced. The first, with suffix “.012”, contains the genotypes of each individual on a separate line. Genotypes are represented as 0, 1 and 2, where the number represent that number of non-reference alleles. Missing genotypes are represented by -1. The second file, with suffix “.012.indv” details the individuals included in the main file. The third file, with suffix “.012.pos” details the site locations included in the main file.

- --IMPUTE

This option outputs phased haplotypes in IMPUTE reference-panel format. As IMPUTE requires phased data, using this option also implies --phased. Unphased individuals and genotypes are therefore excluded. Only bi-allelic sites are included in the output. Using this option generates three files. The IMPUTE haplotype file has the suffix “.impute.hap”, and the IMPUTE legend file has the suffix “.impute.hap.legend”. The third file, with suffix “.impute.hap.indv”, details the individuals included in the haplotype file, although this file is not needed by IMPUTE.

- --ldhat
- --ldhat-geno

These options output data in LDhat format. This option requires the “--chr” filter option to also be used. The first option outputs phased data only, and therefore also implies “--phased” be used, leading to unphased individuals and genotypes being excluded. The second option treats all of the data as unphased, and therefore outputs LDhat files in genotype/unphased format. Two output files are generated with the suffixes “.ldhat.sites” and “.ldhat.locs”, which correspond to the LDhat “sites” and “locs” input files respectively.

- --BEAGLE-GL
- --BEAGLE-PL

These options output genotype likelihood information for input into the BEAGLE program. The VCF file is required to contain FORMAT fields with “GL” or “PL” tags, which can generally be output by SNP callers such as the GATK. Use of this option requires a chromosome to be specified via the “--chr” option. The resulting output file has the suffix “.BEAGLE.GL” or “.BEAGLE.PL” and contains genotype likelihoods for biallelic sites. This file is suitable for input into BEAGLE via the “like=” argument.

- --plink
- --plink-tped
- --chrom-map

These options output the genotype data in PLINK PED format. With the first option, two files are generated, with suffixes “.ped” and “.map”. Note that only bi-allelic loci will be output. Further details of these files can be found in the PLINK documentation.  
Note: The first option can be very slow on large datasets. Using the --chr option to divide up the dataset is advised, or alternatively use the --plink-tped option which outputs the files in the PLINK transposed format with suffixes “.tped” and “.tfam”.  
For usage with variant sites in species other than humans, the --chrom-map option may be used to specify a file name that has a tab-delimited mapping of chromosome name to a desired integer value with one line per chromosome. This file must contain a mapping for every chromosome value found in the file.

## 比较选线

- DIFF VCF FILE
    
- --diff
    
- --gzdiff
- --diff-bcf

These options compare the original input file to this specified VCF, gzipped VCF, or BCF file. These options must be specified with one additional option described below in order to specify what type of comparison is to be performed. See the examples section for typical usage.

- --diff-site

Outputs the sites that are common / unique to each file. The output file has the suffix “.diff.sites\_in\_files”.

- --diff-indv

Outputs the individuals that are common / unique to each file. The output file has the suffix “.diff.indv\_in\_files”.

- --diff-site-discordance

This option calculates discordance on a site by site basis. The resulting output file has the suffix “.diff.sites”.

- --diff-indv-discordance

This option calculates discordance on a per-individual basis. The resulting output file has the suffix “.diff.indv”.

- --diff-indv-map

This option allows the user to specify a mapping of individual IDs in the second file to those in the first file. The program expects the file to contain a tab-delimited line containing an individual’s name in file one followed by that same individual’s name in file two with one mapping per line.

- --diff-discordance-matrix

This option calculates a discordance matrix. This option only works with bi-allelic loci with matching alleles that are present in both files. The resulting output file has the suffix “.diff.discordance.matrix”.

- --diff-switch-error

This option calculates phasing errors (specifically “switch errors”). This option creates an output file describing switch errors found between sites, with suffix “.diff.switch”.