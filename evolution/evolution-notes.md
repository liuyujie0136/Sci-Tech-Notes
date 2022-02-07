# 分子演化与群体遗传笔记

- [分子演化与群体遗传笔记](#分子演化与群体遗传笔记)
  - [群体遗传演化名词解释](#群体遗传演化名词解释)
    - [1. 等位基因频率](#1-等位基因频率)
    - [2. 基因型频率](#2-基因型频率)
    - [3. 群体](#3-群体)
    - [4. 遗传平衡定律(又称哈迪.温伯格定律)](#4-遗传平衡定律又称哈迪温伯格定律)
    - [5. 适合度](#5-适合度)
    - [6. 突变压力](#6-突变压力)
    - [7. 选择压力](#7-选择压力)
    - [8. 选择系数](#8-选择系数)
    - [9. 群体分层](#9-群体分层)
    - [10. 核苷酸多样性(π)](#10-核苷酸多样性π)
    - [11. 群体间固定指数(Fst)](#11-群体间固定指数fst)
    - [12. θw](#12-θw)
    - [13. 连锁不平衡](#13-连锁不平衡)
    - [14. 瓶颈效应](#14-瓶颈效应)
    - [15. 适合度](#15-适合度)
    - [16. 随机遗传漂变](#16-随机遗传漂变)
    - [17. 奠基者效应](#17-奠基者效应)
    - [18. 迁移压力(又叫基因流)](#18-迁移压力又叫基因流)
    - [19. 有效群体大小](#19-有效群体大小)
    - [20. 中性学说](#20-中性学说)
    - [21. Tajima’s D值检验](#21-tajimas-d值检验)
    - [22. 定向选择](#22-定向选择)
    - [23. 选择性清除](#23-选择性清除)
    - [24. 微进化](#24-微进化)
    - [25. 大进化](#25-大进化)
    - [26. 趋同进化](#26-趋同进化)
    - [27. 遗传负荷](#27-遗传负荷)
  - [Population Genetics Glossary](#population-genetics-glossary)
    - [Glossary and Bibliography of terms in population and molecular genetics, systematics etc.](#glossary-and-bibliography-of-terms-in-population-and-molecular-genetics-systematics-etc)
    - [Useful reference texts](#useful-reference-texts)
    - [Literature cited](#literature-cited)
  - [Determination of Haplotypes from Genotype information](#determination-of-haplotypes-from-genotype-information)
  - [群体遗传学与重测序分析](#群体遗传学与重测序分析)
- [0 几种变异类型](#0-几种变异类型)
- [1 重测序和从头组装](#1-重测序和从头组装)
- [2 重测序分析流程](#2-重测序分析流程)
- [3 群体进化选择](#3-群体进化选择)
  - [3.1 正选择](#31-正选择)
  - [3.2 负选择](#32-负选择)
  - [3.3 平衡选择](#33-平衡选择)
- [4 群体遗传学中的统计指标](#4-群体遗传学中的统计指标)
  - [4.1 群体多态性参数](#41-群体多态性参数)
  - [4.2 分离位点数目](#42-分离位点数目)
  - [4.3 核苷酸多样性!\pi](#43-核苷酸多样性)
  - [4.4 群体内选择检验：Tajima's D](#44-群体内选择检验tajimas-d)
  - [4.5 群体间分歧度检验：_!F_{st}_](#45-群体间分歧度检验)
  - [4.6 群体分歧度检验：ROD](#46-群体分歧度检验rod)
- [5 群体结构分析](#5-群体结构分析)
  - [5.1 进化树](#51-进化树)
    - [5.1.1 进化树算法](#511-进化树算法)
      - [5.1.1.1 基于距离](#5111-基于距离)
      - [5.1.1.2 基于特征](#5112-基于特征)
    - [5.1.2 进化树类型](#512-进化树类型)
    - [5.1.3 进化树软件](#513-进化树软件)
  - [5.2 PCA图](#52-pca图)
  - [5.3 群体分层图](#53-群体分层图)
  - [5.4 其他](#54-其他)
- [6 连锁不平衡分析](#6-连锁不平衡分析)
  - [6.1 LD衰减分析](#61-ld衰减分析)
- [7 GWAS](#7-gwas)
  - [7.1 GWAS流程](#71-gwas流程)
  - [7.2 GWAS数学模型](#72-gwas数学模型)
  - [7.3 GWAS结果](#73-gwas结果)
- [7 其他统计指标和算法](#7-其他统计指标和算法)
  - [7.1 MSMC](#71-msmc)
  - [7.2 LAMP](#72-lamp)
  - [7.3 Treemix](#73-treemix)
- [8 群体重测序方案推荐](#8-群体重测序方案推荐)
- [参考文献](#参考文献)
  - [群体演化模型与π的计算](#群体演化模型与π的计算)
- [基础的序列术语](#基础的序列术语)
- [【群体遗传学】1.1进化模型](#群体遗传学11进化模型)
- [Wright-Fisher模型](#wright-fisher模型)
- [Moran模型](#moran模型)
- [【群体遗传学】 π （pi）的计算](#群体遗传学-π-pi的计算)
- [杂合度 _heterozygosity_](#杂合度-heterozygosity)
- [!\pi](#)
- [!\theta_W](#-1)
- [群体遗传学统计指标——种群核苷酸多样性（π值）](#群体遗传学统计指标种群核苷酸多样性π值)
- [理论](#理论)
- [使用VCFTOOLS计算种群核苷酸多样性](#使用vcftools计算种群核苷酸多样性)
  - [VCF文件处理](#vcf文件处理)
      - [给VCF文件添加ID](#给vcf文件添加id)
      - [SNP位点过滤](#snp位点过滤)
  - [准备样本ID文件](#准备样本id文件)
  - [计算核苷酸多样性（π）](#计算核苷酸多样性π)
  - [数据可视化](#数据可视化)
      - [那如用R何画折线图嘞？](#那如用r何画折线图嘞)
- [记录一个关于计算核酸多样性（pi）的经历（附计算pi的perl脚本）](#记录一个关于计算核酸多样性pi的经历附计算pi的perl脚本)


## 群体遗传演化名词解释

### 1. 等位基因频率

等位基因频率是群体遗传学的术语，用来显示一个种群中基因的多样性，或者说是基因库的丰富程度。在一个群体中，等位基因频率即某类等位基因占该基因位点上全部等位基因数的比率。如：在某种群中一个等位基因的基因频率为20%，那么在种群的所有成员中，1/5的染色体带有那个等位基因，而其他4/5的染色体带有该等位基因的其他对应变种--可以是一种也可以是很多种。

计算方法：
- 通过基因型个数计算基因频率：某种基因的基因频率=此种基因的个数/（此种基因的个数+其等位基因的个数）
- 通过基因型频率计算基因频率：某种基因的基因频率＝某种基因的纯合体频率+1/2杂合体频率
- 根据遗传平衡定律计算基因频率：一个群体在符合一定条件的情况下，群体中各个体的比例可从一代到另一代维持不变。

### 2. 基因型频率

群体中某一基因型个体占群体总个数的比例。可以反映某一基因型个体在群体中的相对数量。在群体遗传学中基因型频率指在一个种群中某种基因型的所占的百分比。

### 3. 群体

是指生活在一定空间范围内，能够相互交配并生育具有正常生殖能力后代的同种个体群。群体与个体相对，是个体的共同体，不同个体按某种特征结合在一起，进行共同活动、相互交往，就形成了群体。

### 4. 遗传平衡定律(又称哈迪.温伯格定律)

一个不发生突变、迁移和选择的无限大的相互交配的群体中，群体的基因频率和基因型频率将逐代保持不变。

在理想状态下，各等位基因的频率和等位基因的基因型频率在遗传中是稳定不变的，即保持着基因平衡。该定律运用在生物学、生态学、遗传学。例：当等位基因只有一对（Aa）时，设基因A的频率为p，基因a的频率为q，则A+a=p+q=1，AA+Aa+aa=p2+2pq+q2=1。哈迪-温伯格平衡定律（Hardy-Weinberg equilibrium） 对于一个大且随机交配的种群，基因频率和基因型频率在没有迁移、突变和选择的条件下会保持不变。

### 5. 适合度

指一个个体能够生存并将其基因传给下一代的能力，可用相同环境中不同个体的相对生育率来衡量。

### 6. 突变压力

一定条件下，一个群体的突变率可明显增高，形成突变压力，使某个基因频率增高。

### 7. 选择压力

又称为进化压力，指外界施与一个生物进化过程的压力，从而改变该过程的前进方向，所谓达尔文的自然选择，或者物竞天择、适者生存，即是指自然界施与生物体选择压力从而使得适应自然环境者得以存活和繁衍。

分类：
- **负向选择**（纯化选择）：若某个群体内DNA突变对于生物是有害的，对这个突变的选择就是负向的(Negative selection)或纯化选择(Purifying selection)。理论上，纯化选择将消灭群体中的有害突变。但是轻微有害突变的命运则没有那么明确。
- **正向选择**：若某个群体内某DNA突变对于生物是有益的，对这个突变的选择就是正向的(Positive selection)。根据有益优势水平的不同，这个有益突变在正面选择下在种群中广泛存在需要不同的时间，短的可以是几代，长的可以上成千上万代。
- **平衡选择**：是一种关于自然选择保持种群内遗传多样性的学说，是在一些等位基因上杂合的基因型的系列，这些等位基因的纯合体仅在正常的杂交群体的少数个体中存在，并且在适合度上低于杂合体，然后将会出现有利于在许多座位上发展复等位基因系列的选择压力。

### 8. 选择系数

指在选择作用下降低的适合度。

### 9. 群体分层

群体分层是指群体内存在亚群的现象，亚群内部个体间的相互关系大于整个群体内部个体间的平均亲缘关系。

### 10. 核苷酸多样性(π)

衡量特定群体多样性高低的参数，是指在同一群体中随机挑选的两条DNA序列在各个核苷酸位点上核苷酸差异的均值。π值越大，说明其对应的亚群多样性越高。

### 11. 群体间固定指数(Fst)

衡量群体中等位基因频率是否偏离遗传平衡论比例的指标，用来研究不同群体间的分化程度。其取值为0到1，0代表两个群体未分化，其成员间是完全随机交配的；1代表两个群体完全分化，形成物种隔离，且无共同的多样性存在。

### 12. θw

Watterson's 多态性估值，从理论上说，在中性条件下，应当有θw=4Neμ的平衡状态，Ne表示有效群体大小，μ表示每一代的序列突变率。

### 13. 连锁不平衡

相邻位点之间的非随机关联，当一个位点上的某一等位基因与另一位点上的等位基因共同出现的概率大于随机组合的假设，则这两个位点之间存在连锁不平衡。

### 14. 瓶颈效应

由于环境骤变(如火灾、地震、洪水等)或人类活动(如人工选择、驯化)，使得某一生物种群的规模迅速减少，仅有一少部分个体能够顺利通过瓶颈事件，在之后的恢复期内产生大量后代。

### 15. 适合度

是指某种基因型的个体在一定环境条件下，能生存并传递其基因于下一代的能力。

是指生物体或生物群体对环境适应的量化特征，是分析估计生物所具有的各种特征的适应性，以及在进化过程中继续往后代传递的能力的指标。达尔文的《物种起源》中指出：适合度是衡量一个个体存活和繁殖成功机会的尺度。适合度越大，个体成活的机会和繁殖成功的机会也越大，反之则相反（因此义项与广义适合度相对应，故亦可称之为狭义适合度）。达尔文的适者生存的个体选择观点就是建立在适合度基础上的，但用个体选择的观点无法解释动物的利他行为。因为利他行为所增进的是其他个体的适合度，而不是自己的适合度。

计算方式：适合度可以用数据计算出来：W=ml。其中，W代表适合度，m表示基因型个体生育力，l表示基因型个体存活率。

### 16. 随机遗传漂变

当一个族群中的生物个体的数量较少时，下一代的个体容易因为有的个体没有产生后代，或是有的等位基因没有传给后代，而和上一代有不同的等位基因频率。一个等位基因可能(在经过一个以上的世代后)因此在这个族群中消失，或固定成为唯一的等位基因。这种现象就叫“遗传漂变”。

### 17. 奠基者效应

有少数个体的基因频率决定了他们后代中的基因频率的效应，是一种极端的遗传漂变作用。

### 18. 迁移压力(又叫基因流)

由于某种原因，具有某一基因频率的群体的一部分移入基因频率与其不同的另一群体，并杂交定居，就会引起迁入群体的基因频率发生改变。

### 19. 有效群体大小

指与实际群体有相同基因频率方差或相同杂合度衰减率的理想群体含量，通常小于绝对的群体大小。

### 20. 中性学说

认为分子水平上的大多数突变是中性或近中性的，自然选择对它们不起作用，这些突变靠一代又一代的随机漂变而被保存或趋于消失，从而形成分子水平上的进化性变化或种内变异。

### 21. Tajima’s D值检验

在原有的平衡状态中θT=θW=4Neμ，所以D值为0。但是，如果群体中存在许多低频率的等位基因(稀有等位基因)，使θT<θW，则D<0。相反，当群体中是中等频率的等位基因占主导时，θT>θW，D>0。Tajima把过多低频率等位基因的存在归咎为定向选择时，选择性清除会削弱原有等位基因的在群体中的频率，而使新等位基因以低频率补充进来成为稀有等位基因。相反，如果是中等频率的等位基因占主导，则可能是平衡选择的结果，或者是种群大小在经历瓶颈时使稀有等位基因丢失。因此，当Tajima’s D显著大于0时，可用于推断瓶颈效应和平衡选择；当Tajima’s D显著小于0时，可用于推断群体规模放大和定向选择。由于平衡选择与定向选择都属于正选择的范畴，因此，只要D值显著背离0，就可能是自然选择的结果；而当D值不显著背离0时，则中性零假说则不能被排除。

### 22. 定向选择

是指生存环境的方向性选择（自然选择）或品种的人工定向选择。

### 23. 选择性清除

自然选择会促使有利变异更容易在群体中被保留下来，其两侧序列往往由于连锁效应同时被保留；而非有利变异则被选择清除（selective sweep）。简单的说就是基因组某区域由于受到了选择而消除多态性，即遗传多样性降低的现象。

### 24. 微进化

群体在世代过程中等位基因频率的变化，成为微进化，即发生在物种内的遗传变化。

### 25. 大进化

从现有物种中产生新物种的过程，是微进化的扩展、累积的结果。

### 26. 趋同进化

在突变和选择的作用下，不同物种间具有趋同进化的趋势，这种现象称协同进化。

### 27. 遗传负荷

如果一个群体的突变不断积累，并且这些突变是有害的，就会出现适合度下降。这种现象称为遗传负荷。


## Population Genetics Glossary
> [Population Genetics Glossary, PopEcol, UWyo](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html)  
> [Population Ecology, University of Wyoming](http://www.uwyo.edu/dbmcd/popecol/index.html)

### Glossary and Bibliography of terms in population and molecular genetics, systematics etc.

- **Allele**: a variant segment of the genetic material. Diploid organisms will have two potential alleles for any particular stretch (gene, _sensu_ _latu_) of DNA (e.g., a 'normal' and a 'mutant' allele for _Drosophila_ trait such as eye color). If the alleles are the same (or indistinguishable) on both chromosomes, the individual is a homozygote, if the alleles differ, a heterozygote. Bateson and Saunders (1902) originally coined the term for traits alternative to one another in Mendelian inheritance (Gk. _Allelon_, one another; _morphe_, form). Now used for alternative forms at a genetic [locus](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Locus). [Codominant](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Codominant) alleles are particularly useful as [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker).
- Allopatric: having non-overlapping geographic ranges. Cf. [sympatric](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Sympatric),
- **Allozymes**: [Codominant](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Codominant) protein variants ([alleles](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allele)) that can be [visualized](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Visualization) by appropriate staining and starch-gel [electrophoresis](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Electrophoresis). These were the first major molecular [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker), developed in the late 1960’s.
- Assignment (test):  A method of assigning individuals to the populations from which they were most likely to have originated (regardless of where they dispersed to or were sampled).  A web-based assignment calculator is at: http://www.biology.ualberta.ca/jbrzusto/Doh.html.  \[See also Davies, N., F.X. Villablanca, and G.K. Roderick. 1999. Determining the source of individuals: multilocus genotyping in nonequilibrium population genetics. Trends Ecol. Evol. 14: 17-21; Waser, P.M., and C. Strobeck. 1998. Genetic signatures of interpopulation dispersal. Trends Ecol. Evol. 13: 43-44\].   J.M. Cornuet's GeneClass does Bayesian and other assignment tests: http://www.ensam.inra.fr/urlb/
- Assortative mating: Nonrandom mating systems in which like pairs with like. Cf. [Disassortative mating](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Disassortative), [Random mating](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#RandomMating).
- Assumptions: A critical portion of any model of the genetic structure of populations or taxa. Most models make simplifying assumptions concerning drift, mutation or linearity that will be violated to some degree by almost every actual data set. The key point is whether the violations are sufficient to invalidate the conclusions of the model. A _robust_ analysis is one whose conclusions are insensitive to violations of the assumptions.
- Autosome: chromosome other than a sex chromosome.
- ‘Beanbag’ genetics: An initially derogatory term for the classical basis of population genetics founded by Sewall Wright, J.B.S. Haldane, and R.A. Fisher. Manipulation of counts of gene and [genotype](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Genotype) frequencies based on the forces of mutation, drift, [migration](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Migration), selection and non-[random mating](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#RandomMating) provide the basis for a theoretical understanding of evolution.
- **Bottleneck**: Reduction in population size that can have major influence on genetic variation because of the relationship between genetic drift and population size.
- bp: Abbreviation of 'base pairs' (nucleotides).
- Clade: [Monophyletic](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Monophyletic) group of taxa.
- Cladistics: School of phylogenetic analysis emphasizing the branching patterns of [monophyletic](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Monophyletic) taxa relying on synapomorphies (vs. symplesiomorphies) to unite sister taxa. \[See Avise, pp. 34-39, 121-122\].
- Cladogram: A diagram, in the form of a stylized tree, showing inferred historical branching patterns among taxa.
- **Codominant**: expression of heterozygote phenotypes that differ from either homozygote phenotype. [Microsatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite) are codominant [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker), because one can distinguish a heterozygote (two bands) from each of the homozygotes (single band).
- Coefficient of relatedness (_r_): A measure of the degree of relatedness between individuals, ranging from -1.0 (no genes in common, at least over the [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker) assayed) to +1.0 (identical twins or clones). In an outbred diploid population, siblings should have _r_ = 0.5, individuals chosen at random should have _r_ = 0.0. This measure is the foundation of Hamilton's (1964) theory of kin selection, which sparked a revolution in the study of animal behavior, behavioral ecology and the analysis of fitness. \[See Avise p. 191; Queller and Goodnight, 1989\].
- Congruence: Agreement among or within phylogenetic data sets.
- **Diploid**:  Having a double complement of chromosomes (generally a paternal and a maternal set).  Many genetic analyses are conducted on taxa whose cells are usually diploid.  Exceptions to diploidy include haploid gametes, haplo-diploid males in hymenoptera, polyploid species (particularly in plants, but a recent mammalian example exists!), and haploid stages in some complex life cycles.
- Disassortative mating: Nonrandom mating system in which unlike individuals pair.  Cf. [Assortative mating](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Assortative), [Random mating.](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#RandomMating)
- **Effective population size**: see _[Ne.](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Ne)_
- **Electrophoresis**: polarized acetate, agarose or acrylamide gel through which one runs proteins or DNA. The material then separates by weight or polarity and allows one to distinguish variants (e.g., [alleles](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allele), enzyme variants). \[-_phoresis_; from the Greek for ‘to carry’\]. \[See Avise, Fig. 3.2, p. 48, and Fig. 3.3, p. 50\]. [Allozymes](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allozymes) refer to enzyme variants used as [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker).
- Endemism:  occurring in only one restricted locality.  Island species are often endemic (not found on adjacent mainland).  High levels of endemism (e.g., plants and invertebrates in Florida sand-pine scrub habitats) suggest a history of geographical isolation.  South American mountain ranges, for example, have very high rates of endemism for plants and animals.
- **Evolutionary forces**:  Five major forces can cause evolutionary change: **Natural selection**, **Genetic drift (or population size)**, **Mutation**, **Non-random mating**, **Migration (in the genetic sense of permanent movement of genes from one location to another)**
- Exon: Section of the DNA that codes for amino acids. See [intron](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Intron).
- Fitness: Easiest to encapsulate in its population genetics sense as the relative rate of increase of a [genotype](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Genotype) under viability selection alone. Metz et al. (1992) discuss the concept in a TREE article, Grafen (1982) discusses inclusive fitness, Danchin et al. (1995) and McGraw and Caswell (1996) discuss measuring fitness from real-world data. For many cases, the matrix population parameter l can be taken as a measure of fitness (Caswell, 1989, p. pp. 163-171). \[If l > 1 then the genotype increases, if l < 1 then it decreases\].
- Flanking region: for [microsatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite), the flanking regions are the stretches of DNA outside the simple sequence tandem repeat. These sequences are used as [primer](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Primer) pairs. The flanking regions are usually invariant across a population or species, but mutations in the flanking region can be a cause of null alleles as well as a potentially serious source of [homoplasy](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Homoplasy) (see Pemberton et al. 1995).
- Forensic: Of or relating to courts or legal matters. Molecular markers are increasingly common in the context of forensics, both in wildlife and human cases involving identity or relatedness.
- **_F_-statistics**: a measure of genetic structure developed by Sewall Wright (1969, 1978). Related to statistical analysis of variance  (_ANOVA_). _F<sub>ST</sub>_ is the proportion of the total genetic variance contained in a subpopulation (the _S_ subscript) relative to the total genetic variance (the _T_ subscript). Values can range from 0 to 1. High _F<sub>ST</sub>_ implies a considerable degree of differentiation among populations. _F<sub>IS</sub>_ (inbreeding coefficient) is the proportion of the variance in the subpopulation contained in an individual. High _F<sub>IS</sub>_ implies a considerable degree of inbreeding. _Related measures:_ q (theta) of Weir and Cockerham (1984) and _G<sub>ST</sub>_ of Nei (1973, 1978). \[See Weir, 1996; Avise, Box 6.3, p. 206\].
- **Gene diversity** (expected heterozygosity): A measure of genetic variation in a population. It is calculated from the squared gene (= allele) frequencies. See Weir (1996) p. 124 for the formula.
- **Gene flow**:  movement of genes from one population to another, causing them to become more similar.  Genetic migration is the primary agent of gene flow.
- **Gene frequencies**: The term used in population genetics for [allele](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allele) frequencies.
- Genetic distance: various statistics for measuring the 'genetic distance' between subgroups or populations. Major distance measures include Nei's distance (1972, 1978), Reynold's distance (Reynolds et al. 1983) and new distance measures that incorporate the [stepwise mutation](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Stepwise) process in [microsatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite) (_R<sub>ST</sub>_ of Slatkin 1995a, b; _D_ of Shriver et al., _delta mu_ of Goldstein et al. 1995).
- **Genetic drift**: a force that reduces heterozygosity by the random loss of alleles.  Drift is inversely related to population size.  Infinitely large populations (an assumption of the Hardy-Weinberg equilibirum) will not experience drift, whereas small populations will experience major effects of drift.  Drift is one of the major forces of evolutionary change (along with natural selection, mutation, genetic migration, and non-random mating).  The equilibrium/balance between drift and mutation is a major focus of much of population genetics.
- **Genetic markers**: any trait used as a marker of genetic variation with in and among individuals and taxa. Traits used include phenotypic traits (eye color), protein products ([allozymes](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allozymes), albumin), and segments of the DNA. One might use a particular genetic marker as a diagnostic trait (is this meat a legal elk or Rancher Smith's prize bull?; does this person have a heritable genetic disorder?), as a tool for management (how different are trout in Wyoming from trout in Colorado?), as an aid to systematic analyses, or in a huge variety of ways in basic evolutionary biology. Different genetic markers (e.g., [microsatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite), [mtDNA](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#MtDNA), allozymes, RAPD's) have different scopes (fine-grained vs. coarse-grained analyses), and different advantages and disadvantages (e.g., specificity, cost, ease of analytical interpretation of the resulting data).
- Genome size: The genome is the collective term for all the complement of hereditary material found in an organism (e.g., all the DNA in the set of chromosomes in eukaryotes). Genome size ranges from approximately 10<sup>4</sup> base pairs ([bp](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#bp)) in some viruses to approximately 10<sup>10</sup> in many angiosperm plants, to > 10<sup>10</sup> in some salamanders and fishes. Mammals have approximately 2-3 x 10<sup>9</sup> bp. Although polyploidy can increase genome size, most increase seems to be due to relatively small duplication events (because genome sizes within taxa tend to be approximately normally distributed around an intermediate modal size. \[See Ayala, 1982, pp. 219-22\].
- **Genotype**: The set of DNA variants found at one or more [loci](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Locus) in an individual. The information from which genotypes are developed could include [allozyme](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allozymes) [alleles](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allele), microsatellite alleles, or sequence information (we then usually refer to haplotypes).  Cf. [phenotype](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Phenotype).
- Haldane’s rule: "When in the F1 offspring of two different animal races one sex is absent, rare, or sterile, that sex is the heterozygous \[[heterogametic](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Heterogametic)\] sex." \[See Avise, p. 289\].
- Haploid: having a single complement of chromosomes. See [diploid](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Diploid).
- **Hardy-Weinberg** principle: (Hardy-Weinberg Equilibrium is abbreviated HWE)   Given certain simplifying assumptions such as no genetic drift (= infinite population size), [random mating](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#RandomMating), non-overlapping generations, no selection and no (genetic) [migration](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Migration), the [genotype](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Genotype) frequencies in an infinite population can be predicted from the gene frequencies, _p_ and _q_ by the formula: _p<sup>2</sup>+2pq+q<sup>2</sup>_ . A population will achieve Hardy-Weinberg equilibrium (HWE) in a single generation (unless one of the assumptions listed above is violated). We test for HWE by comparing observed and expected genotype frequencies.  An amazing proportion of the subject matter of population genetics is centered on how/why populations deviate from HWE.
- Heritability: _h_<sup>2</sup> = narrow-sense heritability in quantitative genetics = _V_<sub>A</sub>/_V_<sub>P</sub>, where _V_<sub>A</sub> is the additive genetic variance and _V_<sub>P</sub> is the phenotypic variance.  Heritability (in the narrow sense) enters into the response to selection _R_, where _R_ =_h_<sup>2</sup>_S_, and _S_ is the intensity of selection.  See Gillespie (1998) p. 129, Hartl (2000) pp. 166-167.
- Heterogametic sex: the sex whose sex chromosomes are different from each other. In mammals, most other vertebrates and most insects, males are the heterogametic sex (XY), whereas in birds, lepidopterans, and some fish it is females (WZ). Chromosomal sex determination is not universal (alternatives are phenotypic and [allelic](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allele) sex determination).
- **Heterozygosity (expected)**: An individaul or population-level parameter. The proportion of [loci](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Locus) expected to be heterozygous in an individual (ranging from 0 to 1.0). _H_<sub>O</sub> (observed heterozygosity) is the observed proportion of heterozygotes, averaged over loci. _H_<sub>E</sub> (expected heterozygosity) is also known as [gene diversity](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneDiversity) (= _D_; preferred, less ambiguous term) and is calculated as 1.0 minus the sum of the squared [gene frequencies](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneFreq). \[See Weir, 1996, p. 124 for the multi-locus, multi-allele formula\].
- Homology: having the same origin (used for genes or characters deriving from a common ancestor).
- Homoplasy: similarity of traits or genes for reasons other than coancestry (e.g., convergent evolution, parallelism, evolutionary reversals, horizontal gene transfer, gene duplications). Homoplasy violates a basic assumption of the analysis of [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker) \-- variants of similar phenotype (e.g., base pair size) are assumed to derive from a common ancestor. \[See Sanderson, M., and Hufford. 1996. Homoplasy: The Recurrence of Similarity in Evolution. Academic Press, NY ISBN 618030-X\].
- HWE: see [Hardy-Weinberg](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#HardyWeinberg).
- Hypervariability: High degree of variation among individuals within local populations at a given [genetic marker](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker). Examples of hypervariable markers include [minisatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Minisatellite) and [microsatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite).
- Independent assortment: During gamete formation segregating pairs of unit factors (e.g., genes controlling color or shape traits) assort independently of each other.  As a result, one can use multiplicative probabilities to compute multi-trait or multigene phenotypes or genotypes. Linkage disequilibrium can prevent the expected probabilities from being realized.
- Individualization: buzzword (largely restricted to forensics applications) to embrace the idea that molecular markers can facilitate distinguishing individuals.
- Intron: DNA sequences within the protein-coding sequences of a gene; introns are transcribed into mRNA but are cut out of the message before it is translated into protein. Introns may contain sequences involved in regulating expression of a gene. See [exon](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Exon).
- Isolate breaking: Excess heterozygosity (over Hardy-Weinberg expectation) observed when divergent populations or subpopulations establish secondary contact.  The opposite of the [Wahlund effect](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Wahlund).
- Isozymes: Enzyme variants with the same functional role, but differing in 1°, 2°, 3° or 4° structure. In some cases, isozymes may be multimers produced by multiple genes. They may, therefore, not qualify as [codominant](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Codominant) [allozymes](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allozymes) for use as [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker).
- Karyotype:  the complement of chromosomes (e.g., 2 _n_ = 46 in humans) that constitute the genetic material of a eukaryote.
- Ladder: A series of known-size fragments run in a gel to allow sizing of fragments of target DNA run in other lanes. One commonly used ladder is [phage](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Phage) [lambda](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Lambda) cut with a restriction enzyme \[yields fragments of 216, 211, 200, 164 and 150 [bp](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#bp)\].
- Lambda: Lambda (l) phage DNA is a useful tool in molecular biology. Because its entire sequence is known (= 50Kb double-stranded), it is often used to create a [ladder](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Ladder) of known-size fragments for sizing bands on gels. It is also a useful cloning vector.
- **Locus**: from the Latin for 'place'. A stretch of DNA at a particular place on a particular chromosome — often used for a 'gene' in the broad sense, meaning a stretch of DNA being analyzed for variability (e.g., a [microsatellite](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite) locus).
- Marker: see [Genetic marker](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker).
- **Microsatellites**: Short tandem repeats (e.g., AC _n_ , where _n_ > 8) of nucleotide sequences -- the tandem units can be dinucleotides, trinucleotides or tetranucleotides. The apparent mutation process is by [slippage replication](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Slippage) errors, where the repeats allow matching via excision or addition of repeats. Because this sort of slippage replication is more likely than point mutations, microsatellite [loci](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Locus) tend to be [hypervariable](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Hypervariable). The usual procedure is to use an oligo (e.g., AC10) as a [probe](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Probe), screen a genomic library and then sequence positive clones to develop [primer](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Primer) pairs that can be used to amplify the target DNA with the [PCR](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#PCR).  Alternative name is SSTR (simple sequence tandem repeat). \[See also McDonald and Potts (1997), or 1-page intro. at http://www.uwyo.edu/zoology/McDONALD.HTM\].
- **Migration**: In population genetics, migration means the (permanent) movement of genes into or out of a population. Thus, a 'migrating' warbler does not cause any migration (in the genetic sense) by moving from breeding grounds in Wyoming to wintering grounds in Mexico and then returning to breed in the same Wyoming locale.  We refer to the process of genes moving among populations as [gene flow](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Geneflow).
- Minimum viable population size:  See Nunney, L., and K.A. Campbell. 1993. Assessing minimum viable population size: demography meets population genetics. Trends Ecol. Evol. 8: 234-239.
- Minisatellites: \[see VNTR\]. Segments of repeated DNA often used as [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker) for individual identification ([forensic](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Forensic) DNA 'fingerprinting') or analyses of relatedness. Can be either single- or multi-[locus](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Locus). Minisatellite technology relies on [probe](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Probe)\-based hybridization. Advantages include lack of need for specific [primers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Primer) and [hypervariability](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Hypervariable). Disadvantages include inability to use [PCR](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#PCR) amplification, the need for Southern blotting, and, for multi-locus minisatellites, the lack of locus-specificity (making population genetic analyses difficult). \[See Avise, Fig. 3.16, p. 80\].
- Molecular clock hypothesis: Hypothesis that molecular change is linear with time, and constant over different taxa and in different places. If that is so, then the sequence difference between homologs in different taxa can be used to estimate time since divergence. \[See Avise text, pp. 100-109\].
- Monophyletic group ([clade](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Clade)): Evolutionary assemblage of taxa that includes a common ancestor and all of its descendants. \[See Avise, p. 36\].
- MtDNA: mitochondrial DNA. Sequencing of mtDNA is a widely used technique in systematics. The mostly maternal, clonal transmission of mt-DNA provides both opportunities and problems for phylogenetic analysis. \[See Avise, p. 63\].
- **_Ne_**: **Effective population size**. Many factors include fluctuating population size, sex ratio (_N_<sub>e</sub> = (4 _N_<sub>m</sub>-_N_<sub>f</sub>)/(_N_<sub>m</sub>+_N_<sub>f</sub>), age of reproduction (overlapping generations), the spatial dispersion of the population ( _N_<sub>e</sub> = 4πσ<sup>2</sup>δ) and family size can affect _N_<sub>e</sub>. Usually, _N_<sub>e</sub> will be less than _N_ (the census population size) in natural populations. If, however, the distribution of family sizes is more uniform than Poisson, then _N_<sub>e</sub> can be > _N_. _N_<sub>e</sub> is a fundamental component of many population genetics formulations.   Often, however, it is found in the term 4 _N_<sub>m</sub> or 4 _N_<sub>m</sub> (mutation or migration respectively) and hence cannot be estimated by itself.  See Crow and Kimura (1970) for an overview; Ewens (1982), Harris and Allendorf (1989), Caballero and Hill (1992), and Nunney and Elam (1994) also discuss the concept.  Hartl's (2000) primer of population genetics has a useful summary on pp. 96-98.
- Non-[synonymous](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Synonymous) substitution: A nucleotide substitution (mutation) that results in a different amino acid. More likely for first and second position codons.
- **Nucleotides**: the building blocks of DNA (and RNA).  DNA nucleotides consist of a nitrogenous base, a deoxyribose sugar and a phosphate group.
- OTU: Operational taxonomic unit. Examples include populations, species, genera, and families. For phylogenetic analyses, the OTUs will be terminal taxa (i.e., occur at the branch tips of the tree).
- Outgroup: Taxon phylogenetically outside the [clade](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Clade) of interest (the ingroup). When one uses an outgroup in phylogenetic inference, the ingroup is implicitly assumed to be [monophyletic](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Monophyletic).  Best reference point for determining polarity (direction of character change/whether a character is or isn't ancestral). \[See Avise, p. 36; p. 416 of Molecular Systematics, 2nd edn.\].
- Panmixia: Absence of any differentiation among subpopulations (because of high levels of [gene flow](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Geneflow), creating effectively one single large population with no internal structure).  The adjective is **panmictic**.
- **PCR**: polymerase chain reaction. Technique for amplifying nucleic acids in a [thermal cycler](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#ThermalCycler). Involves use of forward and reverse [primer](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Primer) pairs that start off the reaction. End yield is many orders of magnitude more DNA of the target sequence than one started with. The resulting amplified DNA can then be [visualized](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Visualization) with stains or radioactive labeling, or sized with fluorescent markers in a sequencer. \[See Avise, p. 84, Fig. 3.18, p. 85\].
- **Phenotype**: the outward expression of a genotype.  This "visible" variation might be expressed as coat color in a mouse, as the odor of a secondary compound (mint or sagebrush), or as the length of a DNA fragment on an electrphoretic gel.  Cf. [genotype](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Genotype).
- **Phylogeography**: Study of the patterns of genetic differentiation across landscapes, often involving intraspecific variation and the comparison of patterns across a range of different taxa in the same region (phylogenetic biogeography). Pioneered by John Avise.
- Polymerase chain reaction: See [PCR](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#PCR).
- Polyploid: having more than two sets of homologous chromosomes.  A common route to speciation in plants.  Recently, researchers discovered a polyploid South American rodent.
- **Primer**: Short, preexisting single-stranded polynucleotide chain to which new deoxyribonucleotides can be added by DNA polymerase (to 'prime' [PCR](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#PCR) amplification). The primer anneals to a nucleic acid template (DNA of the organism of interest) and promotes copying of the template, starting from the primer site. To amplify [microsatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite) one uses a forward and reverse primer pair: \[_agctcagtccctagtcagtact_\]acacacacacacacacacacac\[_ggtacttcggagctatccgaattccct_\] In this example the italicized [bp](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#bp) are the forward and reverse primers (should not differ among individuals), whereas the unitalicized 'ac' repeat is the microsatellite. By running back and forth across the repeat one can amplify a few copies of the microsatellite region by orders of magnitude, yielding sufficient DNA to allow [visualization](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Visualization) of the amplified product on an acrylamide gel by staining with ethidium bromide. Some primer sequences may be conserved across wide taxonomic gaps (e.g., across families), while others may differ even among congeners.
- Probe : Single-stranded DNA or RNA molecules of specific base sequence, labeled either radioactively, immunologically, or by other means, that are used to detect the complementary base sequence by hybridization. Some [genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker) (e.g., [minisatellites](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Minisatellite)) depend on probe-based techniques.
- _r: See_ [Coefficient of relatedness](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Coefficient) .
- **Random mating**: A fundamental simplifying assumption for many population genetics models. Non-random mating may be [assortative](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Assortative) (birds of a feather), [disassortative](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Disassortative) (opposites attract) or skewed (hotshots). For example, for [Hardy-Weinberg](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#HardyWeinberg) equilibrium, random mating is required.
- Recombination: Exchange of gene segments by crossing over at chiasmata (exchange of material between non-sister chromatids). The exchanged sections are usually homologous. The likelihood of recombination increases with increasing physical distance.
- Relatedness:  See [coefficient of relatedness](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Coefficient).
- Sequencing: Molecular techniques for deducing the nucleotide composition of the DNA. The two major alternatives are Maxam-Gilbert sequencing, and Sanger/dideoxy sequencing. \[See Avise, 1996, Fig. 3.17, p. 83; Russell, 1992, pp. 458-462; Miyamoto and Cracraft, 1991\].
- Silent substitution: mutation in a coding/expressed region of the DNA that produces no change in the amino acid coded for (because of the redundancy of the genetic code).  Also known as [synonymous substitution](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Synonymous).
- Simple sequence tandem repeat: See [microsatellite](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite).
- Slippage replication:  A mutation process whereby a simple sequence tandem ([microsatellite](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite)) repeat grows by addition or subtraction of the "beads" of simple units that make up the "necklace".  A dinucleotide AC repeat would grow by addition or subtraction of AC units.
- Stepwise mutation: [Microsatellite](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Microsatellite) variation appears to result from [slippage](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Slippage) in replication, which is most likely to add or delete a single repeat unit (steps of one). As a result, [alleles](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allele) more similar in size will presumably be more closely related. This additional 'phylogenetic' information can be used in assessing genetic differentiation or [genetic distance](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticDistance).
- Sympatric: occurring in the same geographic area. Cf. [allopatric](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Allopatric).
- Synonymous substitution: A nucleotide substitution that does not result in a different amino acid (e.g., any codon beginning CC will code for proline, regardless of the codon in the third position). Also known as a "silent" substitution.  Synonymous substitutions result from the degeneracy (redundancy) of the genetic code at the third codon position. A non-synonymous substitution changes the amino acid coding. \[See Avise, Fig. 4.2, p. 102\].
- Taxon (plural taxa): Group of organisms linked by common ancenstry.  Taxa can range in scale from populations to kingdoms.
- Thermal cycler: the 'engine' or [PCR](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#PCR) machine, in which the PCR is performed.
- Transition: a point mutation in the DNA in which replacement is by a similar nucleotide. I.e., a purine (A and G) by a purine or a pyrimidine (C or T) by a pyrimidine. Transitions happen more often than [transversions](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Transversion).  The dissimilar rates of mutation can be incorporated in phylogenetic inference by various weighting schemes.  \[See pp. 432-438 of Mol. Syst., 2nd edn.\].
- Transversion: a point mutation in the DNA in which replacement is by a dissimilar nucleotide. I.e., a purine (A or G) is replaced by a pyrimidine (C or T) or vice versa. Cf. [transition](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Transition).
- Visualization: technique for assessing variation among DNA segments ([genetic markers](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#GeneticMarker)). Methods include radiolabeling (exposure of gels to x-ray film) and various stains (ethidium bromide, silver stains etc.).
- **Wahlund effect**: Reduction in homozygosity (increase in [heterozygosity](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#Heterozygosity)) when distinct taxa are analyzed jointly, or when they hybridize.  Whenever subpopulations vary in gene frequency, the population as a whole will show a Wahlund effect.  The opposite effect, known as [isolate breaking](https://www.uwyo.edu/dbmcd/popecol/maylects/popgengloss.html#IsolBreak), occurs when divergent populations intermix.  In that case, the interbreeds will show an increase in heterozygosity over the Hardy-Weinberg expectation.

### Useful reference texts

- **Avise, J.C. 1994. Molecular Markers, Natural History and Evolution. Chapman and Hall, New York.**
- Brooks, D.R., and D.A. McLennan. 1991. Phylogeny, Ecology, and Behavior. Chicago Univ. Press, Chicago.
- Ferraris, J.D., and S.R. Palumbi (eds.). 1996. Molecular Zoology: Advances, Strategies, and Protocols. Wiley-Liss, NY. 580 pp.
- **Gillespie, J. H. 1998. Population Genetics: A Concise Guide. The Johns Hopkins University Press, Baltimore, Md. On Reserve**
- **Hartl, D.L. 2000. A Primer of Population Genetics (3rd ed.). Sinauer Associates, Sunderland, MA.**
- Hartl, D.L., and A.G. Clark. 1989. Principles of Population Genetics. Sinauer Associates, Sunderland, MA.
- Hillis, D.M., C. Moritz, and B.K. Mable (eds.). 1996. Molecular Systematics (2nd ed.). Sinauer Associates, Sunderland, MA.
- Hoelzel, A.R., and G.A. Dover. 1991. Molecular Genetic Ecology. IRL Press, Oxford U. Press, Oxford
- Kendrew, J.C. 1994. The Encyclopedia of Molecular Biology . Blackwell Science, Oxford
- Li, W. 1997. Molecular Evolution. Sinauer Associates, Sunderland, MA.
- Maddison, W.P., and D.R. Maddison. 1992. MacClade: Analysis of Phylogeny and Character Evolution. Sinauer Associates, Sunderland, MA. V. 3.
- Martins, E.P., and T.F. Hansen. 1997. Phylogenies and the comparative method: a general approach to incorporating phylogenetic information into the analysis of interspecific data. Am. Nat. 149: 646-667.
- Rieger, R, A. Michaelis, and M.M. Green. 1991. Glossary of Genetics : Classical and Molecular, 5th edn. Springer-Verlag, Berlin
- Russell, P.J. Genetics, 3rd Edition. Harper, Collins, New York. UW Science library call
- Weir, B.S. 1996. Genetic Data Analysis II: Methods for discrete population genetic data (2nd ed.). Sinauer Assoc., Sunderland, MA.

### Literature cited

- Ayala, F.J. 1982. Population and Evolutionary Genetics: A Primer. Benjamin/Cummings, Menlo Park, CA.
- Caballero, A., and W.G. Hill. 1992. A note on the inbreeding effective population size. Evol. 46: 1969-1972.
- Caswell, H. 1989. Matrix Population Models. Sinauer Associates, Sunderland, Mass.
- Danchin, E., G. Gonzalez Davila, and J.D. Lebreton. 1995. Estimating bird fitness correctly by using demographic-models. J Avian Biol. 26: 67-75.
- Ewens, W.J. 1982. On the concept of effective population size. Theor. Pop. Biol. 21: 373-378.
- Freifelder, D.M. 1987. Molecular Biology, 2nd edn. Boston: Jones and Bartlett  
- Goldstein, D.B., A.R. Linares, L.L. CavalliSforza, and M.W. Feldman. 1995. An evaluation of genetic distances for use with microsatellite loci. Genetics 139: 463-471.
- Grafen, A. 1982. How not to measure inclusive fitness. Nature 298: 425-426
- Harris, R.B., and F.W. Allendorf. 1989. Genetically effective population size of large mammals: an assessment of estimators. Conserv. Biol. 3: 181-191.
- Hartl, D.L. 2000. A Primer of Population Genetics (3rd ed.). Sinauer Associates, Sunderland, MA.
- McDonald, D.B., and W.K. Potts. 1997. Microsatellite DNA as a genetic marker at several scales. pp. 29-49 In Avian Molecular Evolution and Systematics (D. Mindell, ed.). Academic Press, New York.
- McGraw, J.B., and H. Caswell. 1996. Estimation of individual fitness from life-history data. Am. Nat. 147: 47-64.
- Metz, J.A.J., R.M. Nisbet, and S.A.H. Geritz. 1992. How should we define 'fitness' for general ecological scenarios? TREE 7: 198-202
- Miyamoto, M.M., and J. Cracraft. 1991. Phylogenetic Analysis of DNA Sequences. Oxford University Press, Oxford.
- Nei, M. 1972. Genetic distance between populations. Am. Nat. 106: 283-292.
- Nei, M. 1973. Analysis of gene diversity in subdivided populations. P.N.A.S., USA 70: 3321-3323.
- Nei, M. 1977. F-statistics and the analysis of gene diversity in subdivided populations. Ann. Hum. Genet. 41: 225-233.
- Nei, M. 1978. Estimation of average heterozygosity and genetic distance from a small number of individuals. Genetics 76: 379-390.
- Nunney, L., and D.R. Elam. 1994. Estimating the effective population size of conserved populations. Conserv. Biol. 8: 175-184.
- Pemberton, J.M., Slate, J., Bancroft, D.R., and Barrett, J.A. (1995). Nonamplifying alleles at microsatellite loci: a caution for parentage and population studies. Mol. Ecol. 4, 49-52.
- Queller, D.C., and K.F. Goodnight. 1989. Estimating relatedness using genetic markers. Evol. 43: 258-275.
- Reynolds, J., B.S. Weir, and C.C. Cockerham. 1983. Estimation of the coancestry coefficient: Basis for a short-term genetic distance. Genetics 105: 767-779.
- Shriver, M.D., L. Jin, E. Boerwinkle, R. Deka, R.E. Ferrell, and R. Chakraborty. 1995. A novel measure of genetic distance for highly polymorphic tandem repeat loci. Mol. Biol. Evol. 12: 914-920.
- Slatkin, M. 1995a. A measure of population subdivision based on microsatellite allele frequencies. Genetics 139: 457-462  
- Genetic structure; stepwise mutation model. CORRECTION NEXT
- Slatkin, M. 1995b. A measure of population subdivision based on microsatellite allele frequencies (vol. 139, pg. 457, 1995). Genetics 139: 1463
- Swofford, D.L. 1996. PAUP: Phylogenetic Analysis Using Parsimony (and Other Methods), version 4.0. Sinauer Associates, Sunderland MA.
- Swofford, D.L., G.J. Olsen, P.J. Waddell, and D.M. Hillis. 1996. Phylogenetic inference. Chapter 11, pp. 407-514 _In_: Molecular Systematics, 2nd ed. (D.M.. Hillis, C. Moritz, and B.K. Mable, eds.). Sinauer Associates, Sunderland, MA.
- Weir, B.S. 1996. Genetic Data Analysis II: Methods for discrete population genetic data (2nd ed.). Sinauer Assoc., Sunderland, MA.
- Weir, B.S., and C.C. Cockerham. 1984. Estimating F\-statistics for the analysis of population structure. Evol. 38(6): 1358-1370.
- Wright, S. 1969. Evolution and the Genetics of Populations, Vol. 2. University of Chicago Press, Chicago.
- Wright, S. 1978. Evolution and the Genetics of Populations, Vol. 4. University of Chicago Press, Chicago.


## Determination of Haplotypes from Genotype information
> [http://www.biorecipes.com/Haplotypes/code.html](http://www.biorecipes.com/Haplotypes/code.html)


## 群体遗传学与重测序分析
> https://www.jianshu.com/p/807e54278539

分子层面对生物的研究，在个体水平上主要是看单个基因的变化以及全转录本的变化（RNA-seq）；在对个体的研究的基础上，开始了群体水平的研究。如果说常规的遗传学主要的研究对象是个体或者个体家系的话，那么群体遗传学则是主要研究由不同个体组成的群体的遗传规律。

在测序技术大力发展之前，对群体主要是依靠表型进行研究，如加拉巴哥群岛的13中鸟雀有着不同的喙，达尔文认为这是自然选择造成的后果\[1\]。达尔文的进化论对应的观点可以简单概括为“物竞天择，适者生存”，这也是最为大众所接受的一种进化学说。直到1968年，日本遗传学家提出了中性进化理论\[2\]，也叫中性演化理论。中性理论的提出很大程度上是基于分子生物化学的发展。可以这样理解中性理论：一群人抽奖，在没有内幕的情况下，每个人抽到一等奖的概率是相等的，这个可能性和参与抽奖的人的身高、年龄、爱好等因素都没有关系。中性理论常作为群体遗传研究中的假设理论（CK）来计算其他各种统计指标。

群体遗传学，研究的单位是群体，比如粳稻、籼稻、野生稻，就能够构成不同的群体；我们国内的各省份的水稻也可以作为一个个群体。 群体遗传学大概可以分为群体内的研究和群体间的研究。比如研究云南元阳的水稻的遗传多样性；如果研究是的云南元阳的水稻和东北的水稻，那就可以算成是群体间的研究。群体间和群体内的研究是相互的。  
测序价格的急剧下降\[3\]使得大规模的群体测序得以实现。  

![](https://upload-images.jianshu.io/upload_images/3454743-02d29b77d6521d9e.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

测序价格变化趋势

-

# 0 几种变异类型

常见的变异类型有SNP、IdDel、SV、CNV等。重测序中最关注的是SNP，其次是InDel。其他的几种结构变异的研究不是太多。

  

![](https://upload-images.jianshu.io/upload_images/3454743-dfe2430b2ac46900.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

常见的变异类型

-

# 1 重测序和从头组装

有参考基因组的物种的全基因组测序叫做重测序，没有参考基因组的物种的全基因组测序则需要从头组装。随着测序价格的降低，越来越多物种的参考基因组都已经测序组装完成。_plant genomes_\[4\]网站实时显示全基因组测序已经完成的植物，其中2012年以后爆发式增长。在群体遗传学研究中更多的是有参考基因组的物种，尤其是模式物种，植物中常见的是拟南芥、水稻和玉米。  

![](https://upload-images.jianshu.io/upload_images/3454743-78721f386100d0e0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1032/format/webp)

重测序和从头组装

  

![](https://upload-images.jianshu.io/upload_images/3454743-e3b8f76aa16d1ee3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1199/format/webp)

plant genomes

# 2 重测序分析流程

主要的分析流程见下图。现在的测序公司基本上都会帮客户完成整个的分析流程，因为主要耗费的资源是计算资源。我认为在整个分析的流程中最重要的是Linux目录的构建，混乱的目录会导致后续的分析频频出问题，重测序分析会生成很多的中间文件，良好的目录管理会使得项目分析流程井然有序。  
该部分涉及到的软件的安装和基础的Linux基础知识就不详细说明了。

  

![](https://upload-images.jianshu.io/upload_images/3454743-e98c49e44ff29419.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

重测序分析流程

  

![](https://upload-images.jianshu.io/upload_images/3454743-1d07b4fc2f205c25.png?imageMogr2/auto-orient/strip|imageView2/2/w/780/format/webp)

Nature Genetics\[5\]

-

# 3 群体进化选择

## 3.1 正选择

正选择似乎可以更好地用自然选择来解释。就是一个基因or位点能够使个体有着更强的生存力或者是育性，这样就会使得这个个体的后代更多，如此一来，这个基因or位点在群体中就越来越多。

  

![](https://upload-images.jianshu.io/upload_images/3454743-02bb9e581874e15b.png?imageMogr2/auto-orient/strip|imageView2/2/w/854/format/webp)

正选择\[6\]

  

正选择能够使有利的突变基因or位点在群体中得到传播，但是与此同时却降低了群体的多态性水平。也就是说原先该位点周围的核苷酸组成是多样性的，在经过正选择之后，这个位点周围核苷酸的多样性就渐渐的趋于同质化了。这就好比一块田，里面本来有水稻和稗草及其他杂草，由于稗草的适应性增强，稗草在逐渐增多，水稻慢慢变少，最后甚至是只剩下了稗草。  
我们将这种选择之后多态性降低的情况叫做选择扫荡（Selective Sweep)。检测选择扫荡的软件有SweeD\[7\]。选择扫荡有可能是人工选择的结果，如2014年 Nature Genetics关于非洲栽培稻的文章就使用了SweeD来检测非洲栽培稻基因组上受人工选择的区域\[8\]。

  

![](https://upload-images.jianshu.io/upload_images/3454743-d9bfbe2c851d51a3.png?imageMogr2/auto-orient/strip|imageView2/2/w/578/format/webp)

SweeD在非洲栽培稻上的应用\[8\]

## 3.2 负选择

负选择和正选择刚好是相反的。简单理解成群体中的某个个体出现了一个致命的突变，从而自己或者是后代从群体中被淘汰。这也导致群体中该位点的多态性的降低。就好比我有10株水稻，其中一株在成长过程中突然不见了，那么对我的这个小的水稻群体来说，这个消失的水稻的独有的位点在群体中就不见了，整体的多态性就降低了。

  

![](https://upload-images.jianshu.io/upload_images/3454743-f492625143673aa4.png?imageMogr2/auto-orient/strip|imageView2/2/w/960/format/webp)

图片出处暂时不详

## 3.3 平衡选择

平衡选择指多个等位基因在一个群体的基因库中以高于遗传漂变预期的频率被保留，如杂合子优势。

  

![](https://upload-images.jianshu.io/upload_images/3454743-3e7a9b34a09e2a41.png?imageMogr2/auto-orient/strip|imageView2/2/w/1115/format/webp)

平衡选择\[9\]

  

平衡选择检测的算法有BetaScan2\[10\]，这是个Python脚本，输入文件只需要过滤好的SNP数据即可。

-

# 4 群体遗传学中的统计指标

## 4.1 群体多态性参数

计算公式为：  
![\theta = 4N_e\mu](https://math.jianshu.com/math?formula=%5Ctheta%20%3D%204N_e%5Cmu)  
其中![N_e](https://math.jianshu.com/math?formula=N_e)是有效群体大小，![\mu](https://math.jianshu.com/math?formula=%5Cmu)是每个位点的突变速率。_但是群体大小往往是无法精确知道的，需要对其进行估计。_

## 4.2 分离位点数目

分离位点数![\theta_w](https://math.jianshu.com/math?formula=%5Ctheta_w)是![\theta](https://math.jianshu.com/math?formula=%5Ctheta)的估计值，表示相关基因在多序列比对中表现出多态性的位置。计算公式为：  
![\theta_w = \frac{K}{a_n}](https://math.jianshu.com/math?formula=%5Ctheta_w%20%3D%20%5Cfrac%7BK%7D%7Ba_n%7D)  
其中![K](https://math.jianshu.com/math?formula=K)为分离位点数量，比如SNP数量。  
![a_n](https://math.jianshu.com/math?formula=a_n)为个体数量的倒数和：  
![a_n = \sum^{n-1}_{i = 1}\frac{1}{i}](https://math.jianshu.com/math?formula=a_n%20%3D%20%5Csum%5E%7Bn-1%7D_%7Bi%20%3D%201%7D%5Cfrac%7B1%7D%7Bi%7D)

## 4.3 核苷酸多样性![\pi](https://math.jianshu.com/math?formula=%5Cpi)

![\pi](https://math.jianshu.com/math?formula=%5Cpi)指的是核苷酸多样性，值越大说明核苷酸多样性越高。通常用于衡量群体内的核苷酸多样性，也可以用来推演进化关系\[11\]。计算公式为：  
![\pi = \sum_{ij}x_ix_j\pi_{ij}=2*\sum_{i = 2}^{n}\sum_{j=1}^{i-1}x_ix_j\pi{ij}](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Csum_%7Bij%7Dx_ix_j%5Cpi_%7Bij%7D%3D2*%5Csum_%7Bi%20%3D%202%7D%5E%7Bn%7D%5Csum_%7Bj%3D1%7D%5E%7Bi-1%7Dx_ix_j%5Cpi%7Bij%7D)  
可以理解成现在群体内两两求![\pi](https://math.jianshu.com/math?formula=%5Cpi)，再计算群体的均值。计算的软件最常见的是_vcftools_，也有对应的R包_PopGenome_。通常是选定有一定的基因组区域，设定好窗口大小，然后滑动窗口进行计算。  
3KRGP文章就计算了水稻不同亚群间4号染色体部分区域上的![\pi](https://math.jianshu.com/math?formula=%5Cpi)值\[12\]，能够看出控制水稻籽粒落粒性的基因_Sh4_![^{[13]}](https://math.jianshu.com/math?formula=%5E%7B%5B13%5D%7D)位置多态性在所有的亚群中都降低了。说明这个基因在所有的亚群中都是受到选择的，这可能是人工选择的结果。  

![](https://upload-images.jianshu.io/upload_images/3454743-7307a3f11fe0beb8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

3KRGP不同亚群核苷酸多态性

## 4.4 群体内选择检验：Tajima's D

Tajima's D是日本学者Tajima Fumio 1989年提出的一种统计检验方法，用于检验DNA序列在演化过程中是否遵循中性演化模型\[14\]。计算公式为：  
![D=\frac{\pi-\theta_w}{\sqrt{V(\pi-\theta_w)}}](https://math.jianshu.com/math?formula=D%3D%5Cfrac%7B%5Cpi-%5Ctheta_w%7D%7B%5Csqrt%7BV(%5Cpi-%5Ctheta_w)%7D%7D)  
D值大小有如下三种生物学意义：  

![](https://upload-images.jianshu.io/upload_images/3454743-f3928e677d27d6de.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

D值生物学意义

## 4.5 群体间分歧度检验：_![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)_

![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)叫固定分化指数，用于估计亚群间平均多态性大小与整个种群平均多态性大小的差异，反映的是群体结构的变化。其简单估计的计算公式为：  
![F_{st}=\frac{\pi_{Between}-\pi_{Within}}{\pi_{Between}}](https://math.jianshu.com/math?formula=F_%7Bst%7D%3D%5Cfrac%7B%5Cpi_%7BBetween%7D-%5Cpi_%7BWithin%7D%7D%7B%5Cpi_%7BBetween%7D%7D)  
![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)的取值范围是\[0,1\]。当![F_{st}=1](https://math.jianshu.com/math?formula=F_%7Bst%7D%3D1)时，表明亚群间有着明显的种群分化。  
在中性进化条件下，![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)的大小主要取决于遗传漂变和迁移等因素的影响。假设种群中的某个等位基因因为对特定的生境的适应度较高而经历适应性选择，那该基因的频率在种群中会升高，种群的分化水平增大，使得种群有着较高的![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)值。  
![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)值可以和GWAS的结果一起进行分析，![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)超过一定阈值的区域往往和GWAS筛选到的位点是一致的，如2018年棉花重测序的文章\[15\]：  

![](https://upload-images.jianshu.io/upload_images/3454743-2c2429bf66f92fd1.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

棉花重测序文章图

## 4.6 群体分歧度检验：ROD

ROD可以基于野生群体和驯化群体间核苷酸多态性参数![\pi](https://math.jianshu.com/math?formula=%5Cpi)的差异识别选择型号，也可以测量驯化群体和野生型群体相比损失的多态性。计算公式为：  
![ROD=1-\frac{\pi_{驯化群体}}{\pi_{野生群体}}](https://math.jianshu.com/math?formula=ROD%3D1-%5Cfrac%7B%5Cpi_%7B%E9%A9%AF%E5%8C%96%E7%BE%A4%E4%BD%93%7D%7D%7B%5Cpi_%7B%E9%87%8E%E7%94%9F%E7%BE%A4%E4%BD%93%7D%7D)  
和![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)一样，ROD也可以和GWAS结合起来：  

![](https://upload-images.jianshu.io/upload_images/3454743-490a5c26e67b5011.png?imageMogr2/auto-orient/strip|imageView2/2/w/1050/format/webp)

2019年油菜重测序文章图\[16\]

-

# 5 群体结构分析

群体结构分析可以简单理解成采样测序的这些个体可以分成几个小组，以及给每个个体之间的远近关系是怎么样的。群体结构分析三剑客， 分别是_进化树_、_PCA_和_群体结构图_。

## 5.1 进化树

进化树就是将个体按照远近关系分别连接起来的图。

### 5.1.1 进化树算法

#### 5.1.1.1 基于距离

- 非加权算术平均对群法UPGMA
- 邻接法Neighbor-joining

#### 5.1.1.2 基于特征

- 最大简约法—最小变化数（祖先状态最小化）
- 最大似然法—所有枝长和模型参数最优化
- 贝叶斯推断—基于后验概率

### 5.1.2 进化树类型

- 有根树  
    有根树就是所有的个体都有一个共同的祖先。就像这样的：
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-c8b00e65d8330c6d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1073/format/webp)
    
    油菜重测序文章图\[16\]
    
- 无根树  
    无根树只展示个体间的距离，无共同祖先，就像这样的：
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-4d54f671089b4120.png?imageMogr2/auto-orient/strip|imageView2/2/w/657/format/webp)
    
    水稻3K文章图\[12\]
    

### 5.1.3 进化树软件

常用的绘图软件是_Phylip_和_Snpphylo_。进化树修饰的软件有_MEGA_，_ggtree_等，推荐网页版工具[iTOL](https://links.jianshu.com/go?to=https%3A%2F%2Fitol.embl.de%2F)，无比强大。  
外群定根法：当群体的个体的差异很小时，可以引入其他物种作为根。如在对三叶草建树时可以引入水稻的序列作为根进行建树。

## 5.2 PCA图

PCA是很常见的降维方法，如微生物研究中常用来检验样品分群情况。PCA计算的软件很多，plink可以直接用vcf文件计算PCA，R语言也可以进行PCA计算。

  

![](https://upload-images.jianshu.io/upload_images/3454743-48270cbed26c8a3f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1009/format/webp)

油菜重测序文章图\[16\]

  

PCA图在群体重测序中有如下几种作用：

- 查看分群信息，就是测序的样品大概分成几个群。如2015年大豆重测序文章的图\[17\]:
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-9868c48699f60931.png)
    
    大豆重测序文章图
    
- 检测离群样本  
    离群样本就是在PCA图看起来和其他样本差异很大的样本，有可能是这个样本的遗传背景和其他样本本来就很大，也有可能是样本混淆了，比如了将野生型的样本标记成了驯化种进行测序。如果有离群样本，那在后续的类似于GWAS的分析中就需要将离群样本进行剔除。当然如果样本本来就是个很特别的，那就另当别论。
- 推断亚群进化关系  
    可以从PCA图可以看出群体的进化关系，尤其是地理位置的进化关系。
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-b01e7cc9fe50f634.png?imageMogr2/auto-orient/strip|imageView2/2/w/884/format/webp)
    
    葡萄群体重测序文章图\[18\]
    

## 5.3 群体分层图

进化树和PCA能够看出来群体是不是分层的，但是无法知道群体分成几个群合适，也无法看出群体间的基因交流，更无法看出个体的混血程度。这时候就需要群体分层图了。  

![](https://upload-images.jianshu.io/upload_images/3454743-5ae4841c712c9e33.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

葡萄群体重测序文章图\[18\]

  
群体分层图的本质是堆叠的柱状图，和微生物研究中的物种组成柱状图类似。每个柱子是一个样本，可以看出一个样本的血缘组成，有几种颜色就说明该样本由几个祖先而来，如果只有一个色，那就说明这个个体很纯。  
常用的软件有_structure_和_ADMIXTURE_\[19\]。两款软件给出的结果都是值。一般选择最低的点为最终的值。  

![](https://upload-images.jianshu.io/upload_images/3454743-8fecfb03e6398e91.png?imageMogr2/auto-orient/strip|imageView2/2/w/282/format/webp)

K值选择(图片来自维基百科)

  
群体分层图的可视化有个极强大的R包：Pophelper\[20\]。  

![](https://upload-images.jianshu.io/upload_images/3454743-9fb0e4c502e6acb6.png?imageMogr2/auto-orient/strip|imageView2/2/w/529/format/webp)

pophelper绘图参数\[21\]

## 5.4 其他

可以将进化树和群体分层图结合进行展示，如下图：

  

![](https://upload-images.jianshu.io/upload_images/3454743-202de262aef0323b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

棉花重测序文章\[15\]

-

# 6 连锁不平衡分析

先了解下概念，此处借鉴基迪奥生物网站的解释\[22\]。  
要理解 LD 衰减图，我们就必须先理解连锁不平衡（Linkage disequilibrium，LD）的概念。连锁不平衡是由两个名词构成，连锁 + 不平衡。前者，很容易让我们产生概念混淆；后者，让这个概念变得愈加晦涩。因此从一个类似的概念入手，大家可能更容易理解 LD 的概念，那就是基因的共表达。  
基因的共表达，通常指的是两个基因的表达量呈现相关性。比较常见的例子就是：转录组因子和靶基因间的关系。因为转录因子对它的靶基因有正调控作用，所以转录因子的表达量提高会导致靶基因的表达量也上调，两者往往存在正相关关系。这个正相关关系，可以使用相关系数 ![r^2](https://math.jianshu.com/math?formula=r%5E2) 来度量，这个数值在 - 1~1 之间。总而言之，相关性可以理解为两个元素共同变化，步调一致。  
类似的，连锁不平衡（LD）就是度量两个分子标记的基因型变化是否步调一致，存在相关性的指标。如果两个 SNP 标记位置相邻，那么在群体中也会呈现基因型步调一致的情况。比如有两个基因座，分别对应 A/a 和 B/b 两种等位基因。如果两个基因座是相关的，我们将会看到某些基因型往往共同遗传，即某些单倍型的频率会高于期望值。  
参照王荣焕等\[23\]的方法进行LD参数计算：  

![](https://upload-images.jianshu.io/upload_images/3454743-ab19cdf467a8adfb.png?imageMogr2/auto-orient/strip|imageView2/2/w/540/format/webp)

LD参数计算\[23\]

## 6.1 LD衰减分析

随着标记间的距离增加，平均的LD程度将降低，呈现出衰减状态，这种情况叫LD衰减。LD衰减分析的作用：

- 判断群体的多样性差异，一般野生型群体的LD衰减快于驯化群体；
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-47e7cd4e2573df24.png?imageMogr2/auto-orient/strip|imageView2/2/w/730/format/webp)
    
    2015年大豆重测序文章图\[17\]
    
- 估计GWAS中标记的覆盖度，通过比较LD衰减距离(0.1)和标记间的平均距离来判断标记是否足够。
    

-

# 7 GWAS

GWAS(genome-wide association study)，全基因组关联分析，常用在医学和农学领域。简单理解成将SNP等遗传标记和表型数据进行关联分析，检测和表型相关的位点，然后再倒回去找到对应的基因，研究其对表型的影响。这些被研究的表型在医学上常常是疾病的表型；在农学上常常是受关注的农艺性状，比如水稻的株高、产量、穗粒数等。GWAS思想首次提出是在心肌梗塞的治疗上\[24\]，首次应用是在2005年的文章上\[25\]。

  

![](https://upload-images.jianshu.io/upload_images/3454743-2077437b93988fca.png?imageMogr2/auto-orient/strip|imageView2/2/w/1199/format/webp)

2005年文章图\[25\]

## 7.1 GWAS流程

![](https://upload-images.jianshu.io/upload_images/3454743-12602ab32931525e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

GWAS流程

- 样品准备就是要收集不同的个体，比如3KRGP就3000多个水稻材料\[12\]，然后对这些材料进行全基因组测序，还需要表型数据，比如水稻的株高、产量等。
- 基因型的检测就是前面的变异检测，只是变异检测完的SNP数据还需要过滤才能进行后续的关联分析。
- 关联分析这一步只需要将基因型数据和表型数据丢给软件就行了。

## 7.2 GWAS数学模型

目前使用最广泛的模型是混合线性模型\[26\]：

  

![](https://upload-images.jianshu.io/upload_images/3454743-f9ae365c31c4b8bc.png?imageMogr2/auto-orient/strip|imageView2/2/w/960/format/webp)

混合线性模型公式

  

所有的参数软件（如Emmax）会自动完成计算。

## 7.3 GWAS结果

GWAS结果文件通常只有两个图，一个是曼哈顿图，另外一个是Q-Q图。一般是先看Q-Q图，如果Q-Q正常，曼哈顿图的结果才有意义。

- Q-Q图  
    用于推断关联分析使用的模型是否正确，如下图：
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-039b6ea9aa2154e3.png?imageMogr2/auto-orient/strip|imageView2/2/w/435/format/webp)
    
    模型正确时的Q-Q图
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-1d9d2124b9d508d5.png?imageMogr2/auto-orient/strip|imageView2/2/w/431/format/webp)
    
    模型不适合时的Q-Q图
    
      
    
    如果模型不正确，那就只能换算法或者软件。
    
- 曼哈顿图  
    之所以叫曼哈顿图，是由于这种图长得像曼哈顿：  
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-3c5e93210df9d2a3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1100/format/webp)
    
    曼哈顿下城（来自维基百科）
    
      
    
    ![](https://upload-images.jianshu.io/upload_images/3454743-47663f28bf7fb316.png?imageMogr2/auto-orient/strip|imageView2/2/w/830/format/webp)
    
    棉花重测序文章图\[15\]
    
      
    图中横着的虚线通常是研究者设定的，最严格的的阈值线是Bonfferonin(![\frac{0.05}{total{SNPs}}](https://math.jianshu.com/math?formula=%5Cfrac%7B0.05%7D%7Btotal%7BSNPs%7D%7D))。阈值线以上的点就是很值得关注的位点。  
    后续就是验证实验了，比如验证不同的单倍型的生物学功能。

-

# 7 其他统计指标和算法

## 7.1 MSMC

MSMC（multiple sequentially Markovian coalescent）\[27\]，底层算法很复杂，类似于PSMC。MSMC的主要功能是推断有效群体大小和群体分离历史。

  

![](https://upload-images.jianshu.io/upload_images/3454743-265e3538047e99d2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

MSMC推断群体大小和分离历史\[27\]

  

这样看起来更直观：

  

![](https://upload-images.jianshu.io/upload_images/3454743-d113dc3e89f55776.png?imageMogr2/auto-orient/strip|imageView2/2/w/893/format/webp)

MSMV推断群体大小和群体分离历史\[27\]

![](https://upload-images.jianshu.io/upload_images/3454743-828f0e470b34583c.png?imageMogr2/auto-orient/strip|imageView2/2/w/954/format/webp)

MSM在葡萄上的应用\[18\]

## 7.2 LAMP

LAMP(Local Ancestry in Admixed Populations，混杂群体的局部族源推断)，用于推断采用聚类的方法假设同时检测的位点间不存在重组情况，对每组相邻的 SNP 进行检测分析\[28\]，在运算速度和推断准确度上都有了质的飞跃。

## 7.3 Treemix

用于推断群体分离和混合\[29\]。图是这样的：  

![](https://upload-images.jianshu.io/upload_images/3454743-8c3453937b45cd92.png?imageMogr2/auto-orient/strip|imageView2/2/w/737/format/webp)

Treemix结果图\[28\]

  
这种图和进化树长得特别相似，可以将得到的结果和进化树进行比较。如2019年NC上关于_Cushion willow_的文章中就用到了这种算法根据。图是这样的：  

![](https://upload-images.jianshu.io/upload_images/3454743-9731f03bf82344d2.png?imageMogr2/auto-orient/strip|imageView2/2/w/690/format/webp)

NC文献\[30\]

  

![](https://upload-images.jianshu.io/upload_images/3454743-780ed667c90f99a6.png?imageMogr2/auto-orient/strip|imageView2/2/w/599/format/webp)

NC文章图\[30\]

  
前文提到的很多软件和算法都是用来推断群体进化的，也就是找到群体的祖先。都可以看成族源推断。具体的差异可以参考综述_法医族源推断的分子生物学进展_\[31\]。

-

-

# 8 群体重测序方案推荐

测序方案关系到后续的分析，不同的样本量对应不同的测序方法和分析方法。

  

![](https://upload-images.jianshu.io/upload_images/3454743-62c66d4990f5270f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

图片来自genek.tv\[32\]

-

# 参考文献

\[1\]. [自然选择(维基百科)](https://links.jianshu.com/go?to=%255Bhttps%3A%2F%2Fzh.wikipedia.org%2Fwiki%2F%25E8%2587%25AA%25E7%2584%25B6%25E9%2580%2589%25E6%258B%25A)  
\[2\]. Kimura, Motoo. "Evolutionary rate at the molecular level." **_Nature_**. 217.5129 (1968): 624-626 .  
\[3\]. [测序价格变化趋势](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.genome.gov%2Fabout-genomics%2Ffact-sheets%2FDNA-Sequencing-Costs-Data)  
\[4\]. [plant genomes](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.plabipd.de%2Ftimeline_view.ep)  
\[5\]. DePristo, Mark A., et al. "A framework for variation discovery and genotyping using next-generation DNA sequencing data." **_Nature Genetics_**. 43.5 (2011): 491.  
\[6\]. Biswas, Shameek, and Joshua M. Akey. "Genomic insights into positive selection." \*\*_TRENDS in Genetics_ __. 22.8 (2006): 437-446.  
\[7\]. Pavlidis, Pavlos, et al. "Sweed: likelihood-based detection of selective sweeps in thousands of genomes." **_Molecular biology and evolution_** 30.9 (2013): 2224-2234.  
\[8\]. Wang, Muhua, et al. "The genome sequence of African rice (_Oryza glaberrima_) and evidence for independent domestication." **_Nature Genetics_** 46.9 (2014): 982.  
\[9\]. Bamshad, Michael, and Stephen P. Wooding. "Signatures of natural selection in the human genome." **_Nature Reviews Genetics_** 4.2 (2003): 99.  
\[10\]. Siewert, Katherine M., and Benjamin F. Voight. "BetaScan2: Standardized statistics to detect balancing selection utilizing substitution data." **_BioRxiv_** (2018): 497255.  
\[11\]. Yu, N.; Jensen-Seaman MI; Chemnick L; Ryder O; Li WH (March 2004). **_Genetics_**. 166 (3): 1375–83.  
\[12\]. Wang, Wensheng, et al. "Genomic variation in 3,010 diverse accessions of Asian cultivated rice." **_Nature_** 557.7703 (2018): 43.  
\[13\]. Li, C., Zhou, A. & Sang, T. Rice domestication by reducing shattering. **_Science_** 311, 1936–1939 (2006).  
\[14\]. Tajima, Fumio. "Statistical method for testing the neutral mutation hypothesis by DNA polymorphism." **_Genetics_** 123.3 (1989): 585-595.  
\[15\]. Du, Xiongming, et al. "Resequencing of 243 diploid cotton accessions based on an updated A genome identifies the genetic basis of key agronomic traits." **_Nature Genetics_** 50.6 (2018): 796.  
\[16\]. Lu, Kun, et al. "Whole-genome resequencing reveals Brassica napus origin and genetic loci involved in its improvement." **_Nature communications_**. 10.1 (2019): 1154.  
\[17\]. Zhou, Z., Jiang, Y., Wang, Z. et al. Resequencing 302 wild and cultivated accessions identifies genes related to domestication and improvement in soybean. **_Nat Biotechnol_** 33, 408–414 (2015).  
\[18\]. Liang, Z., Duan, S., Sheng, J. et al. Whole-genome resequencing of 472 Vitis accessions for grapevine diversity and demographic history analyses. **_Nat Commun_** 10, 1190 (2019).  
\[19\]. Alexander, D.H., Lange, K. Enhancements to the ADMIXTURE algorithm for individual ancestry estimation. **_BMC Bioinformatics_** 12, 246 (2011).  
\[20\]. Francis, Roy M. "pophelper: an R package and web app to analyse and visualize population structure." **_Molecular ecology resources_** 17.1 (2017): 27-32.  
\[21\]. [http://www.royfrancis.com/pophelper/articles/index.html](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.royfrancis.com%2Fpophelper%2Farticles%2Findex.html).  
\[22\]. [https://www.omicshare.com/forum/thread-878-1-1.html](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.omicshare.com%2Fforum%2Fthread-878-1-1.html).  
\[23\]. WANG Rong-Huan, WANG Tian-Yu, LI Yu. Linkage disequilibrium in plant genomes\[J\]. **_HEREDITAS_**, 2007, 29(11): 1317-1323.  
\[24\]. Ozaki, K., Ohnishi, Y., Iida, A. et al. Functional SNPs in the lymphotoxin-α gene that are associated with susceptibility to myocardial infarction. **_Nat Genet_** 32, 650–654 (2002).  
\[25\]. Klein, Robert J., et al. "Complement factor H polymorphism in age-related macular degeneration." **_Science_** 308.5720 (2005): 385-389.  
\[26\]. Yu, Jianming, et al. "A unified mixed-model method for association mapping that accounts for multiple levels of relatedness." **_Nature genetics_** 38.2 (2006): 203.  
\[27\]. Schiffels, Stephan, and Richard Durbin. "Inferring human population size and separation history from multiple genome sequences." **_Nature genetics_** 46.8 (2014): 919.  
\[28\]. Sankararaman, Sriram, et al. "Estimating local ancestry in admixed populations." **_The American Journal of Human Genetics_** 82.2 (2008): 290-303.  
\[29\]. Pickrell, Joseph K., and Jonathan K. Pritchard. "Inference of population splits and mixtures from genome-wide allele frequency data." **_PLoS genetics_** 8.11 (2012): e1002967.  
\[30\]. Chen, Jia-hui, et al. "Genome-wide analysis of Cushion willow provides insights into alpine plant divergence in a biodiversity hotspot." **_Nature communications_** 10.1 (2019): 1-12.  
\[31\]. 孙宽，侯一平。法医族源推断的分子生物学进展 \[J\]. **_法医学杂志_**，2018,34 (03):286-293.  
\[32\]. [genek.tv](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.genek.tv%2F)


## 群体演化模型与π的计算
> https://www.jianshu.com/p/e676b21ac1a2  
> https://www.jianshu.com/p/333330bf7ef5  
> https://www.jianshu.com/p/53727b4f646b


进化开始于一个个体的一条染色体上的一个突变。分子群体遗传学研究的是这些突变在群体中频率的升高或降低。许多进化力量能够通过群体来加速或减缓这些突变的传递。通过个体间分子突变的模式能够推断出具体是哪些进化力量在起作用。

遗传标记的使用最早是1990年ABO血型的发现，而“分子”遗传学则可以追溯到Harris(1966)、Lewontin及Hubby(1966)等人开创性的研究。这些研究者的开创性研究发现个体间在分子水平上的突变数量远超过之前从形态学研究中观察到的数量。这些研究使用同工酶（allozymes）来揭示分子变异。这种方法只能观察到所有变异中的一部分—这些突变能够通过改变电荷使得蛋白以不同的速度通过凝胶。直到1983年才出现了第一个关于核酸分子变异的研究（Aquadro and Greenberg 1983; Kreitman 1983）。这些研究通过对每个核苷酸进行测序，让我们能够全面观察到自然群体中的遗传变异。

分子群体遗传学研究更广泛地关注进化过程对自然群体的影响。鉴于此，通常使用少量个体样本的DNA序列来探究那些作用于整个群体的进化力量。哪怕是一个位点上的遗传变异模式都可以用于进化、重组和自然选择等力量的推断，还可以对群体历史进行推断（如相对大小和迁移史）。基于过去100多年大量群体遗传理论的建立和发展，这样的一些推断是可行的。这些理论告诉我们当每种进化力量发生作用时，我们应该（期望）观察到什么。关于群体遗传学早期的理论研究并没有利用分子数据，但是分子方法的快速发展崛起极大地促进了相关研究的开展，这些研究工作对分子进化过程进行了建模。

要想从DNA序列中推断出正确的推论，那分子群体遗传学理论是至关重要必不可少的。因此，了解主要的模型和它们对应的假设是很重要的。

**在本章中会对这些模型进行简单的介绍，但是不会把群体遗传学的基础的都覆盖到。我们假设读者是有一定基础的。**

本章主要着重于最相关的理论和模型，并将这些理论和模型应用到群体遗传数据上。理解用于序列推断的模型的结构对于理解这些推断是如何实现的是至关重要的。此外，本章节尝试阐明在群体遗传中经常被混淆的术语，并定义它们在本书中的表示和用法。最后讨论的是分子进化的中性理论，尝试在解释这个概念的同时将其易混淆的地方也作简单说明。

# 基础的序列术语

分子群体遗传研究中获取的DNA序列通常如图1那样排列比对在一块。图1所示的是4调序列排列比对的结果。每条序列有15个核苷酸；4条序列来自染色体的同一位点。

![](//upload-images.jianshu.io/upload_images/3454743-2e71c3f33228b3a3.png?imageMogr2/auto-orient/strip|imageView2/2/w/542/format/webp)

因为这4条序列来在4条单独的同源染色体，所以“我”将这4条同源DNA链称为序列（`sequence`）或者是染色体（`chromosomes`）（不管这4条序列是否是独特的）。在本书中我们将使用这个术语，但是在文献中对这4条序列还可以用其他的术语来表述，如基因（`gene`）、`alleles`、`samples`、`cistrons`以及`allele copies`。和20年前一样，用`gene`来描述来自一个单一位点的多条序列并不是常见的，尤其是现在个别研究者会从一个物种的多个基因中采集多条序列。但是许多的研究还是使用`allele`来表示每个染色体，实际上是使用“等位基因”的“不同来源”进行定义。“我”只有在描述个体的某个位点上核苷酸（或氨基酸）不同时才使用`allele`。这种是根据等位基因状态的差异进行定义的。因此，对于图1，我们可以说染色体为_n_\=4。需要注意的是，这个术语并不取决于这4条序列是否随机来自2个二倍体个体，或四倍体个体，或4个独立的自交二倍体。在所有的例子中，我们都是从自然界中采集4条染色体。

在这个比对图中，我们能够看到某些位点是不同的，但我们主要关注的是双等位位点（因为它们是最常见的变异类型，尽管在一个位点上可能有2个以上的变异）。有许多的术语用于描述这种DNA序列上的差异。我们可以看到在我们的样品中有6个多态性（`polymorphism`），或者是分隔点（`segregating site`），或者是突变（`mutations`），或者是单核苷酸多态性（`single nucleotide polymorphisms`，SNPs）。虽然之前多态性和分隔点是使用最多的术语，但是现在更常用的说法是SNP（发音是`snip`，最早是1994年）。一个单一序列上所有等位基因的集合叫做单倍型（`haplotype`）。

突变（`mutations`）在不同的领域有着完全不同的意思。突变可以用来表示DNA发生变化的过程或该过程中产生的新的等位基因。有时候突变是多态性的同义词；在更注重医学的人体群体遗传学中，仅仅是指稀有的多态性（发生的次数<1-5%，或者仅仅是单一序列）。所有的多态性最初都是以突变为表现形式出现的。在本书中，“我”用突变来表示变异产生的过程以及在这个过程中新突变的出现。最后，“替换”（`substitution`）表示的是那些在物种间观察到的DNA差异，以区别于物种内的变异。

通常，我们认为`indel`(insertion/deletion)不是分离位点（`segregating sites`）（虽然有时候插入1bp的碱基也算作分离位点）。这样的划分的原因是当两段序列有多个核苷酸插入时，很难区分真真正正的差异碱基数目。比如，2bp的`indel`算1个多态性位点还是两个？这个答案取决于我们是把这个2bp的`indel`看作是一个单独的突变还是2个分离的长度为1bp的突变？通常不将`indel`等类似的数据加入到分析中。



# 【群体遗传学】1.1进化模型

[![](https://upload.jianshu.io/users/upload_avatars/3454743/374bf3a9-1563-42b3-b332-e93f5cc4949a.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/ef4cfe3d9189)

[研究僧小蓝哥](https://www.jianshu.com/u/ef4cfe3d9189)关注

12020.10.05 09:08:43字数 1,996阅读 1,451

# Wright-Fisher模型

在所有群体中，就多态性（`polymorphisms`）来说，遗传漂变（`genetic drift`）能够改变等位基因的频率。由于群体的有限性加上在每代新个体中某些染色体比其他的染色体有更多的机会传到下一代，所以漂变对等位基因频率的影响是随机的。遗传漂变不同于自然选择（因为在后代中，等位基因或基因型之间并没有稳定持续的差异，因此每个个体的等位基因在频率上并没有持续升高或降低）。

遗传漂变模型能够解释群体中的个体是如何一代代进行更替的。最常见的模型是`Wright-Fisher 模型`。这个模型假设一个群体的大小是固定的，_N_ 倍性雌雄同体。我们把他们理解成雌雄同体的，这样一来群体中的一个个体就能另外一个进行结合。但是这个群体是可以推广到具有不同性别的群体的。因为群体中的个体是二倍体，因此在该群体的每一代中就有_2N_条染色体（常染色体）。如果我们把性染色体加入到该模型中，那就有_1.5N_条`X`染色体或`Z`染色体，0.5`N`条`Y`染色体或`W`染色体，以及_0.5N_条线粒体或叶绿体基因组（这些数量取决于我们研究的生物）。为了形成下一代个体，我们假设个体间是随机结合的并统一对染色体近行采样并分配给下一代。没有个体存活到下一代，相反，整个群体都被新一代个体所取代。这个模型最适用于一年生植物和昆虫（只存活一年这一类）等没有世代重叠的群体（一年生脊椎动物很少但是确实是存在的）。

现在来看遗传漂变对`Wright-Fisher模型`中等位基因频率的影响。假设某个核苷酸位点上有两种等位基因（`Allele`）：A

![_1](https://math.jianshu.com/math?formula=_1)

和A

![_2](https://math.jianshu.com/math?formula=_2)

。在第_t_代中，有_i_条染色条携带了A

![_1](https://math.jianshu.com/math?formula=_1)

，则频率为：

![P_t = \frac{i}{2N}](https://math.jianshu.com/math?formula=P_t%20%3D%20%5Cfrac%7Bi%7D%7B2N%7D)

也就是说有_2N-i_ 条染色体携带了A

![_2](https://math.jianshu.com/math?formula=_2)

，频率为：

![q_t = 1 - p_t](https://math.jianshu.com/math?formula=q_t%20%3D%201%20-%20p_t)

下一代染色体采样就相当于从参数为_2N_ 和

![\frac{i}{2N}](https://math.jianshu.com/math?formula=%5Cfrac%7Bi%7D%7B2N%7D)

的二项分布中进行抽样。因此，`Wright-Fisheries模型`中下一代的_p_的均值和方差为：

![E(p_{t+1}) = p_t](https://math.jianshu.com/math?formula=E(p_%7Bt%2B1%7D)%20%3D%20p_t)

![Var(p_{t+1}) = \frac{p_{t}q_{t}}{2N}](https://math.jianshu.com/math?formula=Var(p_%7Bt%2B1%7D)%20%3D%20%5Cfrac%7Bp_%7Bt%7Dq_%7Bt%7D%7D%7B2N%7D)

![E(.)](https://math.jianshu.com/math?formula=E(.))

表示一个随机变量的期望（均值），

![Var(.)](https://math.jianshu.com/math?formula=Var(.))

表示的是方差。这些式子表示当只有遗传漂变在发挥作用时（没有突变没有选择），也就意味着随着时间的改变，等位基因的频率在期望上是不变的。因为我们的期望中，等位基因频率是不会改变的，所以我们并不能对任何一个等位基因进行预测。另外，在这个过程中，方差是和群体大小直接相关的。因此，在小群体中，等位基因频率会有更大的改变。更重要的是，即使我们预计不会发生重大的变化，以相同的等位基因频率开始的独立群体将不可避免地在平均等位基因频率上产生差异，这样一来就形成了 进化趋异。等位基因通常是朝着0或1进行漂变的。如果某个等位基因的在群体中的频率是1，那我们就称其为`fixed`。如果一旦发生“固定”，那就不会又其他的变化发生，因为两个alleles中的其中一个已经从群体中消失了。

当遗传漂变是唯一的进化力量时，对一个群体来说，遗传变异的水平是会下降的。如果我们将杂合度（`heterozygosity`）定义为随机选择的两个染色体具有不同等位基因的概率的话，那在一个随机交配的群体中一个双等位基因的杂合度就是_`2pq`_。如果其中一个allele比另外一个更常见的话，杂合度就会降低。在`Wright-Fisher模型`中，期望在每代中杂合度降低的速率是

![\frac{1}{2N}](https://math.jianshu.com/math?formula=%5Cfrac%7B1%7D%7B2N%7D)

。虽然杂合度下降并不能用于衡量等位基因频率的变化，但是上述的这些结果表明当遗传漂变是唯一的进化力量时，等位基因变化的速率是极低的。

# Moran模型

`Moran模型`在某些方面比`Wright-Fisher模型`更接近真实情况，也更容易在数学上进行某些处理。在`Moran模型`中，不同年龄的个体是可以共存的，也就不用像`Wright-Fisher模型`那样新的一代会完全替代上一代。严格来说，`Moran模型`只能用于单倍体群体，但是为了和`Wright-Fisher模型`进行比较，我们假设一个固定大小的群体中有_2N_个单倍体个体。在一个给定的时间点，一个个体随机被选择然后进行繁衍，另外一个个体被随机选择后面对死亡。如果我们将这个过程重复_2N_遍，我们将得到和`Wright-Fisher模型`一样大小的一代群体。可以这样理解：平均来说，每个个体会被下一代取代；但是某些个体存活的时间少于1代，而有的个体存活的时间超过一代（编者注：就像有的人超过人类平均年龄后才去世，但是有的人在平均年龄之前就去世了）。

在这个模型中，当携带一个等位基因的个体进行繁殖而另外一个个体面临死亡时，等位基因的频率才会发生改变。当让，也可能是携带相同等位基因的两个个体都死亡了，那现在的情况是没有等位基因频率发生变化。在经过_2N_次的出生-死亡迭代后，我们能够知道下一代中等位基因频率的均值和方差。还是考虑一个和`Wright-Fisher模型`中相同的双等位基因位点。在`Moran模型`的下一代中，等位基因频率_p_的均值和方差分别是：

![E(p_{t+1}) = p_t](https://math.jianshu.com/math?formula=E(p_%7Bt%2B1%7D)%20%3D%20p_t)

![Var(p_{t+1}) = \frac{2p_{t}q_{t}}{2N}](https://math.jianshu.com/math?formula=Var(p_%7Bt%2B1%7D)%20%3D%20%5Cfrac%7B2p_%7Bt%7Dq_%7Bt%7D%7D%7B2N%7D)

和`Wright-fisher模型`一样，平均等位基因频率的均值是不变的，但是此时的方差是`Wright-Fisher模型`中的两倍。这是因为在`Moran模型`中，每个个体后代的数量是`Wright-Fisher模型`的两倍。这种差异的结果就是在`Moran`模型中，遗传漂变的次数是`Wright-Fisher模型`的两倍，杂合度也就降低了一半：_1/N_。有遗传漂变引起的进化在`Moran模型`中任然是很慢的，但是速度比`Wright-Fisher模型`快了两倍。

此处的两种模型对大多数物种来说都是“不真实的”。在某些应用场景下需要的是更接近真实情况的模型，`Cannings模型`就是其中之一。在这个模型中，子代数可以有任意的方差。这个模型属于`Wright-Fisher模型`的推广模型。但`Wright-Fisher模型`不仅直观，还能从中推到出一些重要的进化结论。`Wright-fisher模型`还可以对其他模型的结果进行验证，相当于一个可以用于验证其他模型的模型。

# 【群体遗传学】 π （pi）的计算

[![](https://upload.jianshu.io/users/upload_avatars/3454743/374bf3a9-1563-42b3-b332-e93f5cc4949a.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/ef4cfe3d9189)

[研究僧小蓝哥](https://www.jianshu.com/u/ef4cfe3d9189)关注

12020.10.21 17:05:25字数 1,277阅读 3,701

# 杂合度 _heterozygosity_

某个位点的第![i](https://math.jianshu.com/math?formula=i)个等位基因的样本频率为![p_i](https://math.jianshu.com/math?formula=p_i)，那么该位点所有等位基因的频率和应该是1。先考虑二倍体的双等位基因，那就是![p_1 + p_2 = 1](https://math.jianshu.com/math?formula=p_1%20%2B%20p_2%20%3D%201)。衡量单个多态位点变异（variation）的一个方法是计算样本杂合度（_heterozygosity_），公式如下：

![h = \frac{n}{n-1}(1-\sum{p_i^2})](https://math.jianshu.com/math?formula=h%20%3D%20%5Cfrac%7Bn%7D%7Bn-1%7D(1-%5Csum%7Bp_i%5E2%7D))

在公式中，![n](https://math.jianshu.com/math?formula=n)代表的是样本中序列的数量。

# ![\pi](https://math.jianshu.com/math?formula=%5Cpi)

上面这个公式是针对一个位点的，如果是正对一条序列的话，那其实就就是将整条序列的杂合度加起来即可。

![\pi = \sum_{j=1}^{S}h_j](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Csum_%7Bj%3D1%7D%5E%7BS%7Dh_j)

其中![S](https://math.jianshu.com/math?formula=S)表示的是分离位点的数量，![h_j](https://math.jianshu.com/math?formula=h_j)表示的是第![j](https://math.jianshu.com/math?formula=j)个分离位点的杂合度。在Wright-Fisher模型（无限位点的二倍体）下，![E(\pi) = \theta](https://math.jianshu.com/math?formula=E(%5Cpi)%20%3D%20%5Ctheta)，因此有时这个统计量也叫![\theta_{\pi}](https://math.jianshu.com/math?formula=%5Ctheta_%7B%5Cpi%7D)。我们需要注意的是在单态位点（monomorphic site）时杂合度是0。

先看这样一个例子：

![](//upload-images.jianshu.io/upload_images/3454743-234c3f45cc4d613a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

假设现在有4个样本，15个位点，但是只有6个位点是分离位点，我们先计算每个分离位点的杂合度：

根据公式可知，对分离位点1（图中的第二列序列），有两个等为位点，分别是T和C，其中T有3个，C有1个，那么对T来说，它的频率就是0.75，对C来说它的频率就是0.25。根据公式可得：

![h = \frac{4}{3}\sum[1-(0.75^2 + 0.25^2) ]= 0.50](https://math.jianshu.com/math?formula=h%20%3D%20%5Cfrac%7B4%7D%7B3%7D%5Csum%5B1-(0.75%5E2%20%2B%200.25%5E2)%20%5D%3D%200.50)

我们以此计算就能得到其他5个分离位点的杂合度分别为：0.667，0.5，0.667，0.5，0.5。

那么就能计算![\pi](https://math.jianshu.com/math?formula=%5Cpi)值了：

![\pi = 0.5 + 0.667 + 0.5 +0.667 + 0.5 + 0.5 = 3.33](https://math.jianshu.com/math?formula=%5Cpi%20%3D%200.5%20%2B%200.667%20%2B%200.5%20%2B0.667%20%2B%200.5%20%2B%200.5%20%3D%203.33)

但是我们通常关注的是每个位点![\pi](https://math.jianshu.com/math?formula=%5Cpi)的均值：

![\pi = \frac{3.33}{15} = 0.222](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Cfrac%7B3.33%7D%7B15%7D%20%3D%200.222)

我们将![\pi](https://math.jianshu.com/math?formula=%5Cpi)的计算进行推广就能得到下面这个公式：

![\pi = \frac{\sum_{i < j}k_{ij}}{n(n-1)/2}](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Cfrac%7B%5Csum_%7Bi%20%3C%20j%7Dk_%7Bij%7D%7D%7Bn(n-1)%2F2%7D)

其中![k_{ij}](https://math.jianshu.com/math?formula=k_%7Bij%7D)表示的是第![i](https://math.jianshu.com/math?formula=i)条序列和第![j](https://math.jianshu.com/math?formula=j)条序列之间不同核苷酸的数量，分母表示的是![n](https://math.jianshu.com/math?formula=n)个序列之间进行比较的唯一次数（非重复比较）。现在我们将这个公式应用到上面的序列中。

![](//upload-images.jianshu.io/upload_images/3454743-cb0de4959c872bc0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

现在是有4条序列，所以![n = 4](https://math.jianshu.com/math?formula=n%20%3D%204). 然后以此进行比较:

第一条VS第二条:3个不同的核苷酸

第一条VS第三条:4个不同的核苷酸

第一条VS第四条:3个不同的核苷酸

第二条VS第三条:5个不同的核苷酸

第二条VS第四条:0个不同的核苷酸

第三条VS第四条:5个不同的核苷酸

所以,![\pi = \frac{3+4+3+5+0+5}{4(4-1)/2} = 3.33](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Cfrac%7B3%2B4%2B3%2B5%2B0%2B5%7D%7B4(4-1)%2F2%7D%20%3D%203.33)

**需要注意的是当数据量很大的时候,使用公式![\pi = \sum_{j=1}^{S}h_j](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Csum_%7Bj%3D1%7D%5E%7BS%7Dh_j)计算更快**。

正如前面说到的，我们在计算序列之间的差异时通常是省略`indel`将其变成缺失值进行处理的。当使用公式![\pi = \sum_{j=1}^{S}h_j](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Csum_%7Bj%3D1%7D%5E%7BS%7Dh_j)并且将`indel`变成缺失值时，针对不同位点![n](https://math.jianshu.com/math?formula=n)是不同的。使用公式![\pi = \frac{\sum_{i < j}k_{ij}}{n(n-1)/2}](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Cfrac%7B%5Csum_%7Bi%20%3C%20j%7Dk_%7Bij%7D%7D%7Bn(n-1)%2F2%7D)的话，通常会省略gap位置。

比如这个例子：

![](//upload-images.jianshu.io/upload_images/3454743-cf3950b3fa0d0dbe.png?imageMogr2/auto-orient/strip|imageView2/2/w/592/format/webp)

如果用第一个公式，那么![\pi = 3.49](https://math.jianshu.com/math?formula=%5Cpi%20%3D%203.49)，但是如果用第二个公式的话，![\pi = 2.83](https://math.jianshu.com/math?formula=%5Cpi%20%3D%202.83)。原因是第一个公式将`indel`当作缺失值进行处理，而第二个公式将`indel`当作gap直接省略了这些位点（哪怕是在这些位点并不是分离位点）。不同的公式给出的结果也不一样，尤其是正对平均的每个位点时。因此，在处理基因组这种大数据时，通常使用![\pi = \sum_{j=1}^{S}h_j](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Csum_%7Bj%3D1%7D%5E%7BS%7Dh_j)这个公式。

我们可以把![\pi](https://math.jianshu.com/math?formula=%5Cpi)的期望方差表示成参数为![\theta](https://math.jianshu.com/math?formula=%5Ctheta)的函数。虽然在中性进化模型下，这个参数没啥用😄。

如果没有重组发生的话：

![Var(\pi) = \frac{n + 1}{3(n + 1)}\theta + \frac{n(n^2 + n + 3)}{9n(n - 1)}\theta^2](https://math.jianshu.com/math?formula=Var(%5Cpi)%20%3D%20%5Cfrac%7Bn%20%2B%201%7D%7B3(n%20%2B%201)%7D%5Ctheta%20%2B%20%5Cfrac%7Bn(n%5E2%20%2B%20n%20%2B%203)%7D%7B9n(n%20-%201)%7D%5Ctheta%5E2)

从公式可以看出，和![\pi](https://math.jianshu.com/math?formula=%5Cpi)相关的方差很大，即使样本很大时，方差也不接近于0。

# ![\theta_W](https://math.jianshu.com/math?formula=%5Ctheta_W)

![\theta_W](https://math.jianshu.com/math?formula=%5Ctheta_W)通常叫![Watterson‘s \theta。用](https://math.jianshu.com/math?formula=Watterson%E2%80%98s%20%5Ctheta%E3%80%82%E7%94%A8)\\pi![表示核苷酸变异的另外一种方法是利用样品中所有分离位点的数量](https://math.jianshu.com/math?formula=%E8%A1%A8%E7%A4%BA%E6%A0%B8%E8%8B%B7%E9%85%B8%E5%8F%98%E5%BC%82%E7%9A%84%E5%8F%A6%E5%A4%96%E4%B8%80%E7%A7%8D%E6%96%B9%E6%B3%95%E6%98%AF%E5%88%A9%E7%94%A8%E6%A0%B7%E5%93%81%E4%B8%AD%E6%89%80%E6%9C%89%E5%88%86%E7%A6%BB%E4%BD%8D%E7%82%B9%E7%9A%84%E6%95%B0%E9%87%8F)S![进行衡量，但是需要注意的是样本量太大时会得到很大的](https://math.jianshu.com/math?formula=%E8%BF%9B%E8%A1%8C%E8%A1%A1%E9%87%8F%EF%BC%8C%E4%BD%86%E6%98%AF%E9%9C%80%E8%A6%81%E6%B3%A8%E6%84%8F%E7%9A%84%E6%98%AF%E6%A0%B7%E6%9C%AC%E9%87%8F%E5%A4%AA%E5%A4%A7%E6%97%B6%E4%BC%9A%E5%BE%97%E5%88%B0%E5%BE%88%E5%A4%A7%E7%9A%84)S![，因此需要对](https://math.jianshu.com/math?formula=%EF%BC%8C%E5%9B%A0%E6%AD%A4%E9%9C%80%E8%A6%81%E5%AF%B9)S$进行校正：

![\theta_W = \frac{S}{a}](https://math.jianshu.com/math?formula=%5Ctheta_W%20%3D%20%5Cfrac%7BS%7D%7Ba%7D)

![a = \sum_{i = 1}^{n-1}\frac{1}{i}](https://math.jianshu.com/math?formula=a%20%3D%20%5Csum_%7Bi%20%3D%201%7D%5E%7Bn-1%7D%5Cfrac%7B1%7D%7Bi%7D)

对类似于Wright-Fisher模型处于平衡状态且有无限突变位点的群体，![\theta_W](https://math.jianshu.com/math?formula=%5Ctheta_W)也是![\theta](https://math.jianshu.com/math?formula=%5Ctheta)的估计量。

那么综上：

![\theta:E(S) = \theta{a}](https://math.jianshu.com/math?formula=%5Ctheta%3AE(S)%20%3D%20%5Ctheta%7Ba%7D)

将这个公式应用到这个例子上：

![](//upload-images.jianshu.io/upload_images/3454743-a3534a6106957e19.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

![\theta_W = \frac{6}{1/1+1/2+1/3} = 3.28](https://math.jianshu.com/math?formula=%5Ctheta_W%20%3D%20%5Cfrac%7B6%7D%7B1%2F1%2B1%2F2%2B1%2F3%7D%20%3D%203.28)

可以看到这个公式得到的结果和前面公式计算得到的3.33很接近。

还是和前面说的一样，遇到`indel`不同的处理方式得到的结果不一样：

![](//upload-images.jianshu.io/upload_images/3454743-de679ef9827ceb2e.png?imageMogr2/auto-orient/strip|imageView2/2/w/592/format/webp)

1. 如果将`indel`当作缺失值进行处理，那![\theta_W = 5/(1/1+1/2+1/3) = 2.73](https://math.jianshu.com/math?formula=%5Ctheta_W%20%3D%205%2F(1%2F1%2B1%2F2%2B1%2F3)%20%3D%202.73)
2. 如果将`indel`当作gap进行处理，那![\theta_W = 1/(1/1+1/2) = 0.667](https://math.jianshu.com/math?formula=%5Ctheta_W%20%3D%201%2F(1%2F1%2B1%2F2)%20%3D%200.667)

将这两种不同方法得到的结果相加：

![\theta_W = 2.73 + 0.667 = 3.40](https://math.jianshu.com/math?formula=%5Ctheta_W%20%3D%202.73%20%2B%200.667%20%3D%203.40)

同样，我们可以用参数为![\theta](https://math.jianshu.com/math?formula=%5Ctheta)的函数来表示![S](https://math.jianshu.com/math?formula=S)的期望方差（Wright-Fisher模型，没有重组发生）：

![Var(s) = \sum_{i=1}^{n-1}\frac{1}{i}\theta + \sum_{i=1}^{n-1}\frac{1}{i^2}\theta^{2}](https://math.jianshu.com/math?formula=Var(s)%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn-1%7D%5Cfrac%7B1%7D%7Bi%7D%5Ctheta%20%2B%20%5Csum_%7Bi%3D1%7D%5E%7Bn-1%7D%5Cfrac%7B1%7D%7Bi%5E2%7D%5Ctheta%5E%7B2%7D)

如果是自由重组的话，就只是前半部分。

还可以从这个公式推断出：

![Var(\theta_W) = \frac{Var(S)}{a^2}](https://math.jianshu.com/math?formula=Var(%5Ctheta_W)%20%3D%20%5Cfrac%7BVar(S)%7D%7Ba%5E2%7D)

我们通常会看到关于![\theta](https://math.jianshu.com/math?formula=%5Ctheta)的两种估计值：![\pi](https://math.jianshu.com/math?formula=%5Cpi)和![\theta_W](https://math.jianshu.com/math?formula=%5Ctheta_W)，测序错误等会造成不同的影响，因此通常需要两个值都看，还有更多的统计参数可以使用（如Tajima's D）。



# 群体遗传学统计指标——种群核苷酸多样性（π值）

[![](https://upload.jianshu.io/users/upload_avatars/23339616/11cf55e6-8aa3-411c-a1df-23af4b1163ec.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)](https://www.jianshu.com/u/92f34db5d264)

[EwanH](https://www.jianshu.com/u/92f34db5d264)关注

12021.02.04 10:23:47字数 1,308阅读 7,532

# 理论

种群核苷酸多样性，顾名思义指的就是核苷酸多样性，值越大说明核苷酸多样性越高。通常用于衡量群体内的核苷酸多样性，也可以用来推演进化关系。计算公式为：

  

![](//upload-images.jianshu.io/upload_images/23339616-c4fddb33de803081.png?imageMogr2/auto-orient/strip|imageView2/2/w/331/format/webp)

核苷酸多样性计算公式

计算群体的π值，可以理解成先把群体内每个样本两两求解，再求得群体的均值。

计算的软件最常见的是vcftools，也有对应的R包PopGenome。通常是选定某一基因组区域，设定好窗口大小，然后滑动窗口进行计算。

# 使用VCFTOOLS计算种群核苷酸多样性

## VCF文件处理

#### 给VCF文件添加ID

SNP data通常都是以VCF格式文件呈现，老规矩，拿到VCF文件的第一件事情就是添加各个SNP位点的ID。

先看一下最开始生成的VCF文件：

![](//upload-images.jianshu.io/upload_images/23339616-0fc9995176a79920.png?imageMogr2/auto-orient/strip|imageView2/2/w/1081/format/webp)

原始VCF文件

可以看到，ID列都是"."，需要我们自己加上去。我用的是某不知名大神写好的perl脚本，可以去我的github上下载(https://github.com/Wanyi-Huang/VCF\_add\_id)，用法：

> perl path2file/VCF\_add\_id.pl YourDataName.vcf YourDataNameWithId.vcf

当然也可以用excel手工添加。添加后的文件如下图所示（格式：CHROMID\_\_POS）：

![](//upload-images.jianshu.io/upload_images/23339616-ff6260360c9e838a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1040/format/webp)

添加ID后的VCF文件

#### SNP位点过滤

原始Call出来的SNP实在是太多了，而且有一些低频位点会影响后续的分析（软件有时候还会报错），不仅会影响速度，也会影响最后结果的准确性，因此我们去掉他们。此处用到强大的plink软件，用法：

> plink --vcf YourDataNameWithId.vcf --maf 0.05 --geno 0.2 --recode vcf-iid -out YourDataNameWithId-maf0.05 --allow-extra-chr

参数解释：--maf 0.05：过滤掉次等位基因频率低于0.05的位点；--geno 0.2：过滤掉有20%的样品缺失的SNP位点；--allow-extra-chr：我的参考数据是Contig级别的，个数比常见分析所用的染色体多太多，所以需要加上此参数。

## 准备样本ID文件

非常简单，把某群体的全部样本ID放在一个TXT文件里就行，注意要每一行一个ID。

## 计算核苷酸多样性（π）

> vcftools --vcf YourDataNameWithId.vcf --keep YourDataName.txt --window-pi 10000 --out YourDataName\_pi

\--window-pi 指定窗口的大小，这里我设置了10000，具体大小根据基因组大小选择

\--out 指定输出文件的前缀名

## 数据可视化

数据可视化就是~花式~展示你的结果。在多样性分析中，π值越大表明群体中该位点的核苷酸多样性越大，反之亦然。那么我们所画的图，应该要展示基因组各个区域π值的大小。因此，我们可以选择散点图or折线图。

  

![](//upload-images.jianshu.io/upload_images/23339616-a500749932084de0.png?imageMogr2/auto-orient/strip|imageView2/2/w/699/format/webp)

某文章描述群体核苷酸多样性使用的散点图

画散点图的方法，之前在[群体分歧度检验](https://www.jianshu.com/p/bb0beec0ed63)中已经分享过啦，各位移步参考。

#### 那如用R何画折线图嘞？

我的数据集如下图所示：

  

![](//upload-images.jianshu.io/upload_images/23339616-1c6a8b8bb4928664.png?imageMogr2/auto-orient/strip|imageView2/2/w/293/format/webp)

用于画图的数据表pi.txt

注：第一列为位置信息；第二列为对应位置的pi值；第三列为需要的颜色；最后一列为染色体位置信息（非必要）。

分享一下我写得一个R流程：（大家需要自己根据自己的数据就行调整，但是万变不离其中，你们可以的！）

> #读入数据；
> 
> dt1<- read.delim("pi.txt",sep="\\t", header = T, check.names = F)
> 
> \# 加载ggplot2包；
> 
> library(ggplot2)
> 
> #定义染色体位置。
> 
> #我分析的物种有8条染色体，我将所有的染色体都串起来作图，因此需要标出每条染色体的中间位置在哪。
> 
> br = c(440000, 1370000, 2410000, 3515000, 4610000,5800000,7095000,8399791)
> 
> la = c("1","2","3","4","5","6","7","8")
> 
> #自定义图表主题，对图表做精细调整；
> 
> mytheme<-theme(panel.grid.major =element\_blank(),
> 
>                 panel.grid.minor = element\_blank(),
> 
>                 panel.background = element\_blank(),
> 
>                 panel.border = element\_blank(),
> 
>                 axis.line.y = element\_line(color = "black"),
> 
>                 axis.line.x = element\_line(color = "black"),
> 
>                 #axis.title.x = element\_text(size = rel(1.2)),
> 
>                 axis.title.y = element\_text(size = rel(1.2)),
> 
>                 axis.text.y = element\_text(size=rel(1.2),color="black"),
> 
>                 #axis.text.x = element\_text(size=rel(1.2),color="black"),
> 
>                 plot.margin=unit(x=c(top.mar,right.mar,bottom.mar,left.mar),units="inches"))
> 
> #绘制
> 
> pa<-ggplot(data=dt1, mapping = aes(x=No,y=pop1))+geom\_line(color=dt1$Color1,size=1)
> 
> #设置x轴范围，避免点的溢出绘图区；
> 
> pa<-pa+scale\_x\_continuous(limits = c(-1000, 9230000),breaks = br,labels = la)
> 
> #设置y轴范围
> 
> pa<-pa+scale\_y\_continuous(limits = c(-0.0005,0.01),breaks = c(0,0.002,0.004,0.006,0.008,0.010),labels = c("0.0","2.0","4.0","6.0","8.0","10.0"))
> 
> #设置图例、坐标轴、图表的标题；
> 
> pa<-pa+labs(y="Pi (10e-3)",x=NULL)
> 
> #自定义图表主题，对图表做精细调整；
> 
> pa<-pa+mytheme
> 
> #出图
> 
> pa

  

参考：

[Vcftools Manual](https://links.jianshu.com/go?to=http%3A%2F%2Fvcftools.sourceforge.net%2Fman_latest.html)

[Comparative genomics revealed adaptive admixture in Cryptosporidium hominis in Africa](https://links.jianshu.com/go?to=https%3A%2F%2Fpubmed.ncbi.nlm.nih.gov%2F33355530%2F)

21人点赞

[群体遗传学实战](https://www.jianshu.com/nb/47222028)



# 记录一个关于计算核酸多样性（pi）的经历（附计算pi的perl脚本）

![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[Xulei0737](https://blog.csdn.net/weixin_43362619) ![](https://csdnimg.cn/release/blogv2/dist/pc/img/newCurrentTime.png) 于 2020-07-09 23:46:48 发布 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 3214 ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) ![](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollectionActive.png) 收藏  3 

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

本文链接：[https://blog.csdn.net/weixin\_43362619/article/details/107239012](https://blog.csdn.net/weixin_43362619/article/details/107239012)

版权

  之前在做叶绿体基因组核酸多样的时候，先是用全基因组做，选择窗口和步长使用dnasp5来求pi值。后来发现文章里放的都是编码序列和非编码序列，或者每个基因的pi值。叶绿体的编码序列一般在80个多个，如果再加上非编码区，那么有一百多个序列需要计算，手动算的话挺费时费劲的，一分钟算三个的话得近一个小时。。。作为懒人的我肯定是不愿意每天花一个小时做这样的重复工作，于是……  
  我找了文章里的计算方法，发现使用的竟然全部是dnasp（难道都是一个个算的么？dnasp是windows的一款GUI软件，似乎并不能批量计算），后来发现vcftools好像可以计算pi，而且是[linux](https://so.csdn.net/so/search?q=linux&spm=1001.2101.3001.7020)版本的，通过脚本很容易实现批量计算，于是抓紧试了试。但是vcftools只支持vcf格式的文件，没关系，把比对的序列转成vcf格式就行了，最终实现批量计算多个基因的pi值，小小开心下。但是有个问题，同样的序列用vcftools计算得到的pi和用dnasp计算得到的pi不一样。开始是觉得我转换得到vcf的时候不对。于是我找到了另一款计算pi的工具：perl中有个Bio::PopGen::Statistics模块，可以计算pi，而且输入的是比对后的序列。但是！结果仍然和dnasp计算的不一样！令人惊讶的是，popgene和vcftools的计算结果是相同的，我百思不得其解。难道是dnasp算的有问题。于是找到了计算pi的公式，手动算了下，结果和dnasp的值相同。难道。。。另外两个算错了？？？于是研究了Bio::PopGen::Statistics模块中计算pi值的代码，终于找到了原因。  
   Bio::PopGen::Statistics在计算pi的时候和vcftools类似，先把比对结果转换成vcf格式，然后计算。但是不同的是，如果两个序列比如AAA,AAT，直接计算的话差异碱基数为1。当转成vcf的时候是用 3 A T 0/0 1/1，来表示(3号位点其中一个是A，另一个是T)。这时候的差异碱基数就变成了4（把0/0，1/1分别看成两个位点，如果还原成序列的话就是A，A，T，T，及从原来的2条序列变成了4条。。。。），实际计算的时候是把样品数当成4而不是2。也就是说，差异碱基数变成了原先的四倍，样品（序列）数变成了原先的两倍，所以最终算的值和dnasp的结果不同。  
   考虑到vcftools是用于vcf文件的计算，如果有个位点的信息是0/1，那么确实要表示成两个碱基，所以样品数量要变成原先的两倍，计算的也没毛病。dnasp是直接计算序列的pi，没有考虑太多，和直接套公式计算的结果相同，结果没得说。Bio::PopGen::Statistics呢，我给的是比对的序列，然后你按照vcf的来算，似乎并不友好（没详细看它的文档），按理说我给它的序列和给dnasp的序列是相同的，结果应该也是相同的，但是实际情况并不是。  
   综上所述，如果想计算的是比对后序列的pi，dnasp首选，在这里也是唯一的选择，因为即使你把序列给Bio::PopGen::Statistics算，它算的结果依旧不是你想要的。。。如果你的结果是群体的变异位点信息，那么就选择vcftools把~（当然，不怕麻烦的话也可以把变异位点还原成序列，然后用dnasp来做……）。  
   到这里就结束了？？怎么可能，我还没找到批量计算对齐后序列的pi（和dnasp计算的结果相同）方法呢。于是自己根据公式造了小轮子。这里注意到有的对齐序列是有indel的（用“-”字符表示），计算的时候是不把含有indel的位点算进去的。这也就是为什么在设置步长和窗口的时候，dnasp给的结果中，窗口的数值有的不是我们设置的倍数，如下图，设置的窗口时200，最后竟然出现了706，这就是由于501到706中间有6个位点是“-”，这个计算的时候要把它去掉，窗口200再加上6个“-”，所以出现了706。此外，最后一个窗口往往要小于设置的窗口，程序计算的时候也需要注意下。计算脚本用perl完成（只会perl…），支持步长和窗口的设置，计算结果和dnasp一样，但是比其灵活：便于做批量的计算。  
![dnasp计算pi的结果](https://img-blog.csdnimg.cn/20200709231650805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzM2MjYxOQ==,size_16,color_FFFFFF,t_70)  
   最后，代码就不放到这了，有需要的话可以加群：936427018。