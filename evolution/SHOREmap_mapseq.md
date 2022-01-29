# 使用SHOREmap做mapping-by-sequencing

> 参考：  
> http://bioinfo.mpipz.mpg.de/shoremap/  
> https://www.jianshu.com/p/9f4de8573b6c


## 简介

SHOREmap可以用来分析传统作图群体（自然系natural strains和分化系,diverged accession杂交，或outcrossing)或近等作图群体（isogenic mapping population, 诱变后代与未诱变亲本进行杂交，即会交,backcrossing)所产生的重测序数据。根据作图群体构建方式的不同，SHOREmap的outcross或backcross采用不同基于滑窗(sliding)方式对等位基因频率进行分析。

SHOREmap的backcross和outcross都需要从突变重组库中获得的一致的碱基识别信息

## 安装

### 前置安装

SHOREmap需要[DISLIN](http://www.mps.mpg.de/dislin/)科学库进行数据可视化

但是在安装DISLIN之前还需要保证存在`/usr/lib/libXm.so*`和`/usr/lib/libXm.so*`，这两者的安全需要root权限，所以要么联系管理员，要么想办法绕开(这个办法，我还没有想到).

```bash
sudo apt-get update
sudo apt-get install libmotif4
sudo apt-get install libxt-dev
```

开始安装dislin库

```bash
cd /path/to/src
## 下载
wget ftp://ftp.gwdg.de/pub/grafik/dislin/linux/i586_64/dislin-11.0.linux.i586_64.tar.gz
## 解压缩
tar -zxvf dislin-11.0.linux.i586_64.tar.gz
cd dislin-11.0
## 加入系统路径
mkdir -p $HOME/biosoft/dislin
DISLIN=$HOME/biosoft/dislin
export DISLIN
## 安装
./INSTALL
## 复制dislin_d.h 到dislin的文件下
cp ./example/dislin_d.h $DISLIN
## 删除安装文件(可选)
rm -rf dislin-11.0
```

### 安装SHOREmap v3.x

我这次安装的是当前最新的3.4版本，其他版本估计换汤不换药。

```bash
cd $HOME/biosoft
wget http://bioinfo.mpipz.mpg.de/shoremap/SHOREmap_v3.4.tar.gz
## 替换SHOREmap下的dislin的一些文件
tar -zxvf SHOREmap_v3,4
rm dislin/*dislin_d.*
cp $DISLIN/*dislin_d.* dislin
## 编辑/etc/profile或.bashrc
vi .bashrc
export LD_LIBRARY_PATH=$HOME/src/SHOREmap_v3.4/dislin
## 退出保存.bashrc: Esc+:wq
source .bashrc
## 到之前安装的文件夹下
cd & cd src/SHOREmap_v3.4
(可选，如果没有g++)sudo apt-get install build-essential
make
## 将编译文件拷贝到习惯的文件夹中，然后添加执行路径
cp SHOREmap ../../biosoft/SHOREmap_v3.4
echo "export $HOME/bisoft/SHOREmap_v3.4" >> ~/.bashrc
```

最后，可以重新启动一下bash验证

### 官方网站提供的两个常见问题的解答

Note 1: if the compilation complains like "/usr/bin/ld: cannot find -lXt" (or "/usr/bin/ld: cannot find -ldislin_d"), please open the makefile with the command

```bash
vi makefile
```

Press keys 'esc' and 'i' on the keyboard to edit makefile; move the cursor with arrow keys to the position before -lXt, and edit -L/path/to/libXt.so/; if '-ldislin_d' is not found, edit -L/path/to/dislin_d/ before -ldislin_d). After that, press keys 'esc', type in **:wq**, and press enter to save editing and quit vi. ('-L' tells the linker where to find the library given by -l)

Note 2: if '/usr/lib/ld: warning: libXm.so.3, needed by ./dislin/libdislin_d.so, not found (try using -rpath or -rpath-link)' occurs, and you have installed libmotif4, do the following:

```bash
cp /usr/lib/libXm.so.4 /usr/lib/libXm.so.3
```

We can make SHOREmap avaiable for general use by inserting the following command into /etc/profile

```ruby
export PATH=$PATH:/path/to/SHOREmap_v3.x
```

and

```bash
source /etc/profile
```

## 总体流程

### OUTCROSS

| outcross的基本步骤 | 描述 |
| --- | --- |
| SHOREmap extract | 提取与SNP突变相关的重测序的一致的识别 |
| SHOREmap create | 根据背景/亲本系的重测序质量创建SNP标记列表 |
| SHOREmap outcross | 进行等位基因频率分析并定义mapping interval（也就是找到突变所在的大致区域) |
| SHOREmap annotate | 对mapping interval中的突变基因效应进行注释 |

### BACKCROSS

| backcross的基本步骤 | 描述 |
| --- | --- |
| SHOREmap extract | 提取与SNP突变相关的重测序的一致的识别 |
| SHOREmap backcross | 进行等位基因分析 |
| SHOREmap annotate | 对mapping interval中的突变基因效应进行注 |

## 具体步骤

### 下载数据

只安装软件，却没有数据，我们也只能干瞪眼。

#### oucross分析所需数据

```bash
## OCF2 
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/OC.fg.reads1.fq.gz &
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/OC.fg.reads2.fq.gz &
## Ler 
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/OC.bg.reads1.fq.gz &
wget -4 -qh ttp://bioinfo.mpipz.mpg.de/shoremap/data/software/OC.bg.reads2.fq.gz &
```

#### backcross分析所需数据

```bash
## BCF2
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/BC.fg.reads1.fq.gz &
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/BC.fg.reads2.fq.gz &
## mir159a (Col)
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/BC.bg.reads1.fq.gz
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/BC.bg.reads2.fq.gz
```

#### 其他数据

除了最基本的测序数据外，我们可能还需要参考基因组，已有的注释数据等

```bash
## 参考基因组
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/TAIR10_chr_all.fas &
## 基因注释
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/TAIR10_GFF3_genes.gff &
## SHORE操作的结果数据
wget -4 -q http://bioinfo.mpipz.mpg.de/shoremap/data/software/scoring_matrix_het.txt &
```

### 重测序

首先使用`bwa`,`bowtie2`等read比对工具将得到的数据比对到参考基因组上。  
假设你当前处在MBS文件夹下，该文件下有如下文件

```
## 混池测序结果，双端
BC.fg.reads1.fq.gz
BC.fg.reads2.fq.gz
## 背景信息测序结果
BC.bg.reads1.fq.gz
BC.bg.reads2.fa.gz
## 拟南芥参考基因组
TAIR10_chr_all.fas
## 拟南芥注释信息
TAIR10_GFF3_genes.gff
```

以下操作都是基于上述文件进行。

#### 第一步：序列比对，产生SAM文件

```bash
mkdir index
## 创建比对所需索引
bowtie2-build TAIR10_chr_all.fas index/TAIR10
## 序列比对
bowtie2 -x index/TAIR10 -1 BC.fg.reads1.fq.gz -2 BC.fg.reads2.fq.gz -S FG.sam
bowtie2 -x index/TAIR10 -1 BC.bg.reads1.fq.gz -2 BC.bg.reads2.fq.gz -S BG.sam
```

#### 第二步：SAMtools预测突变位点

为了加快运算速度，可以先转换格式，并排序

```bash
samtools view -b -o BG.bam BG.sam
samtools view -b -o FG.bam FG.sam
samtools sort -o BG.sorted.bam BG.bam 
samtools sort -o FG.sorted.bam FG.bam 
samtools index BG.sorted.bam
samtools index FG.sorted.bam
```

consensus-calling program 寻找可能的变异位点

```bash
samtools mpileup -u -t DP \
    -f ../../../index/TAIR10_chr_all.fa \
    ../../align/bwa/default/BG.sorted.bam | \
    bcftools call -vm -Ov > BG.vcf
samtools mpileup -u -t DP \
    -f ../../../index/TAIR10_chr_all.fa \
    ../../align/bwa/default/FG.sorted.bam | \
    bcftools call -vm -Ov > FG.vcf
```

#### 额外步骤：VCF格式转换

由于bcftools工具版本，所以最后的文件版本是4.2，而SHOREmap要求4.1。通过biostar找到高人写的降级工具（其实就是把一些字符替换一下，但是不了解vcf不同版本的差异话，是不知道怎么写）

把下面的代码存为`vcf_dowgrade.sh`

```bash
## If you are trying to view VCF 4.2 files in IGV - you may run into issues. This function might help you.
## This script will:
## 1. Rename the file as version 4.1
## 2. Replace parentheses in the INFO lines (IGV doesn't like these!)

function vcf_downgrade() {
  outfile=${1/.bcf/}
  outfile=${outfile/.gz/}
  outfile=${outfile/.vcf/}
  bcftools view --max-alleles 2 -O v $1 | \
  sed "s/##fileformat=VCFv4.2/##fileformat=VCFv4.1/" | \
  sed "s/(https://" | \
  sed "s/)//" | \
  sed "s/,Version=\"3\">/>/" | \
  bcftools view -O z > ${outfile}.dg.vcf.gz
  tabix ${outfile}.dg.vcf.gz
}
```

其实对于单个文件而言，可以直接用以下命令

```bash
infile=BG.vcf
outfile=BG.vcf
bcftools  view --max-alleles 2 -O v ${infile} | \
sed "s/##fileformat=VCFv4.2/##fileformat=VCFv4.1/" | \
  sed "s/(https://" | \
  sed "s/)//" | \
  sed "s/,Version=\"3\">/>/" | \
  bcftools view -O z > ${outfile}.dg.vcf.gz
```

### 使用SHOREmap寻找突变所在区

#### 第一步：需要把bcf文件通过SHOREmap convert转换成SHOREmap能认识的格式

```bash
SHOREmap convert \
    --marker myfile.vcf \
    --folder ./convert \
    -runid 1

## 会生成三个文件:
1_converted_consen.txt
1_converted_variant.txt
1_converted_reference.txt
```

#### 第二步：提取候选分子标记的consensus information(mapping pool)

```bash
SHOREmap extract \
    --chrsizes chromsize.txt \
    --folder ./SHOREmap_analysis \
    --marker 1_converted_variant.txt \
    --consen 1_converted_consen.txt \
    -verbose
```

#### 第三步：使用SHOREmap backcross分析

SHOREmap backcross可用来分析回交作图群体所得到重组后代混池数据。相对于传统作图群体，只有诱变剂产生的突变会分离，也只有这些才会用于突变定位。

SHOREmap backcross会尝试过滤出所有参考基因组和测序池之间不同部分用于找到突变点特异部分。为了保证不是自然变异或者是测序错误，测序池选择的部分要多次出现在亲本或背景中。然后根据前景和/或背景的(识别碱基,base calls,质量/覆盖率/等位基因)信息，确定是否把保留的SNP位点作为分子标记。在正确的筛选后（拟南芥大概有上百个标记），SHOREmap backcross就能在分析marker的AF后识别大致的峰。进一步对变异注释后，就能找到目标性状的候选基因了。

SHOREmap backcross所需的输入文件如下：

- 染色体大小文件，--chrsizes。分为两行，一行是染色体位置，一行是染色体大小。scaffold同理
- 候选marker文件。也就是使用SHOREmap convert通过vcf生成的converted_variant.txt，每一列的含义如下。  
    1 Project name  
    2 Identity of chromosome  
    3 Position of the SNP-marker  
    4 Reference base  
    5 Alternative base (or mutant base)  
    6 Quality of the alternative base (ranging from 0 to 40)  
    7 Number of reads supporting the predicted base  
    8 Ratio of reads supporting the predicted base to total coverage

```bash
SHOREmap backcross \
    --chrsizes chromsize.txt \
    --marker ./convert/1_converted_variant.txt \
    --consen ./SHOREmap_analysis/extracted_consensus_0.txt \
    --folder ./BC_analysis \
    -plot-bc \
    --marker-score 40 \
    --marker-freq 0.0 \
    --min-converage 10 \
    --max-coverage 80 \
    -bg ./convert/2_converted_variant.txt \
    --bg-cov 1 \
    --bg-freq 0.0 \
    --bg-score 1 \
    -non-EMS \
    --cluster 1 \
    --marker-hit 1 \
    -verbose
```

#### 第四步：对结果进行注释

```bash
SHOREmap annotate \
    --chrsizes chromsize.txt \
    --folder ./BC_analysis/ann \
    --snp ./convert/1_converted_variant.txt \
    --chrom 2 \
    --start 1 \
    --end 4000000 \
    --genome TAIR10_chr_all.fas \
    --gff TAIR10_GFF3_genes.gff
```

### 示例代码

#### snp_comp.sh
```bash
file1=$1
file2=$2
pre=`basename $file1 | cut -d '.' -f 1`;
index="~/DBs/ath/bowtie2_index/ath";
genome="~/DBs/ath/bowtie2_index/ath10.fa";

bowtie2 -x $index -1 $file1 -2 $file2 -p 4 -S comp/$pre.sam

samtools view -bS -o comp/$pre.bam comp/$pre.sam
samtools sort comp/$pre.bam -@ 4 -O bam -o comp/$pre.sorted.bam

# use samtools 0.1.19 here
~/software/samtools-0.1.19/samtools mpileup -uD  -f $genome comp/$pre.sorted.bam | ~/software/samtools-0.1.19/bcftools/bcftools view -cg - > comp/$pre.raw.vcf

SHOREmap convert --marker comp/$pre.raw.vcf --folder comp/$pre\_marker_new -runid 581
```

#### extract_marker.sh
```bash
file1=$1
pre=`basename $file1 | cut -d '_' -f 1`;
mark="filter_"$pre"_input.txt";

SHOREmap extract --chrsizes chrsizes.txt --folder ./$pre\_result --marker $mark --consen $pre\_consen.txt -verbose
```

#### backcross.sh
```bash
file1=$1
pre=`basename $file1 | cut -d '_' -f 1`;
con=$pre"_result/extracted_consensus_0.txt";
mark="filter_"$pre"_input.txt";

SHOREmap backcross --chrsizes chrsizes.txt --marker $mark --consen $con --folder ./$pre -plot-bc -plot-win -plot-scale --min-coverage 4 --max-coverage 100 --cluster 1 --marker-score 20 -non-EMS -verbose
```