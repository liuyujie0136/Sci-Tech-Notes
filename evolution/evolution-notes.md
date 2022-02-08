# 分子演化与群体遗传笔记

- [分子演化与群体遗传笔记](#分子演化与群体遗传笔记)
  - [群体遗传演化名词解释](#群体遗传演化名词解释)
  - [Population Genetics Glossary](#population-genetics-glossary)
    - [Glossary and Bibliography of terms in population and molecular genetics, systematics etc.](#glossary-and-bibliography-of-terms-in-population-and-molecular-genetics-systematics-etc)
    - [Useful reference texts](#useful-reference-texts)
    - [Literature cited](#literature-cited)
  - [群体遗传学与重测序分析](#群体遗传学与重测序分析)
    - [0. 几种变异类型](#0-几种变异类型)
    - [1. 重测序和从头组装](#1-重测序和从头组装)
    - [2. 重测序分析流程\[5\]](#2-重测序分析流程5)
    - [3. 群体进化选择](#3-群体进化选择)
      - [3.1 正选择\[6\]](#31-正选择6)
      - [3.2 负选择](#32-负选择)
      - [3.3 平衡选择\[9\]](#33-平衡选择9)
    - [4. 群体遗传学中的统计指标](#4-群体遗传学中的统计指标)
      - [4.1 群体多态性参数](#41-群体多态性参数)
      - [4.2 分离位点数目](#42-分离位点数目)
      - [4.3 核苷酸多样性](#43-核苷酸多样性)
      - [4.3.1 利用杂合度(heterozygosity)计算核苷酸多样性](#431-利用杂合度heterozygosity计算核苷酸多样性)
      - [4.4 群体内选择检验: Tajima's D](#44-群体内选择检验-tajimas-d)
      - [4.5 群体间分歧度检验: Fst](#45-群体间分歧度检验-fst)
      - [4.6 群体分歧度检验: ROD](#46-群体分歧度检验-rod)
    - [5. 群体结构分析](#5-群体结构分析)
      - [5.1 进化树](#51-进化树)
      - [5.2 PCA图](#52-pca图)
      - [5.3 群体分层图](#53-群体分层图)
    - [6. 连锁不平衡分析](#6-连锁不平衡分析)
      - [6.1 LD衰减分析](#61-ld衰减分析)
    - [7. GWAS](#7-gwas)
      - [7.1 GWAS流程](#71-gwas流程)
      - [7.2 GWAS数学模型](#72-gwas数学模型)
      - [7.3 GWAS结果](#73-gwas结果)
    - [8 其他统计指标和算法](#8-其他统计指标和算法)
      - [8.1 MSMC](#81-msmc)
      - [8.2 LAMP](#82-lamp)
      - [8.3 Treemix](#83-treemix)
  - [Determination of Haplotypes from Genotype information](#determination-of-haplotypes-from-genotype-information)


## 群体遗传演化名词解释

**1\. 等位基因频率**

等位基因频率是群体遗传学的术语，用来显示一个种群中基因的多样性，或者说是基因库的丰富程度。在一个群体中，等位基因频率即某类等位基因占该基因位点上全部等位基因数的比率。如：在某种群中一个等位基因的基因频率为20%，那么在种群的所有成员中，1/5的染色体带有那个等位基因，而其他4/5的染色体带有该等位基因的其他对应变种--可以是一种也可以是很多种。

计算方法：
- 通过基因型个数计算基因频率：某种基因的基因频率=此种基因的个数/（此种基因的个数+其等位基因的个数）
- 通过基因型频率计算基因频率：某种基因的基因频率＝某种基因的纯合体频率+1/2杂合体频率
- 根据遗传平衡定律计算基因频率：一个群体在符合一定条件的情况下，群体中各个体的比例可从一代到另一代维持不变。

**2\. 基因型频率**

群体中某一基因型个体占群体总个数的比例。可以反映某一基因型个体在群体中的相对数量。在群体遗传学中基因型频率指在一个种群中某种基因型的所占的百分比。

**3\. 群体** 

是指生活在一定空间范围内，能够相互交配并生育具有正常生殖能力后代的同种个体群。群体与个体相对，是个体的共同体，不同个体按某种特征结合在一起，进行共同活动、相互交往，就形成了群体。

**4\. 遗传平衡定律(又称哈迪.温伯格定律)**

一个不发生突变、迁移和选择的无限大的相互交配的群体中，群体的基因频率和基因型频率将逐代保持不变。

在理想状态下，各等位基因的频率和等位基因的基因型频率在遗传中是稳定不变的，即保持着基因平衡。该定律运用在生物学、生态学、遗传学。例：当等位基因只有一对（Aa）时，设基因A的频率为p，基因a的频率为q，则A+a=p+q=1，AA+Aa+aa=p2+2pq+q2=1。哈迪-温伯格平衡定律（Hardy-Weinberg equilibrium） 对于一个大且随机交配的种群，基因频率和基因型频率在没有迁移、突变和选择的条件下会保持不变。

**5\. 适合度**

指一个个体能够生存并将其基因传给下一代的能力，可用相同环境中不同个体的相对生育率来衡量。

**6\. 突变压力**

一定条件下，一个群体的突变率可明显增高，形成突变压力，使某个基因频率增高。

**7\. 选择压力**

又称为进化压力，指外界施与一个生物进化过程的压力，从而改变该过程的前进方向，所谓达尔文的自然选择，或者物竞天择、适者生存，即是指自然界施与生物体选择压力从而使得适应自然环境者得以存活和繁衍。

分类：
- **负向选择**（纯化选择）：若某个群体内DNA突变对于生物是有害的，对这个突变的选择就是负向的(Negative selection)或纯化选择(Purifying selection)。理论上，纯化选择将消灭群体中的有害突变。但是轻微有害突变的命运则没有那么明确。
- **正向选择**：若某个群体内某DNA突变对于生物是有益的，对这个突变的选择就是正向的(Positive selection)。根据有益优势水平的不同，这个有益突变在正面选择下在种群中广泛存在需要不同的时间，短的可以是几代，长的可以上成千上万代。
- **平衡选择**：是一种关于自然选择保持种群内遗传多样性的学说，是在一些等位基因上杂合的基因型的系列，这些等位基因的纯合体仅在正常的杂交群体的少数个体中存在，并且在适合度上低于杂合体，然后将会出现有利于在许多座位上发展复等位基因系列的选择压力。

**8\. 选择系数**

指在选择作用下降低的适合度。

**9\. 群体分层**

群体分层是指群体内存在亚群的现象，亚群内部个体间的相互关系大于整个群体内部个体间的平均亲缘关系。

**10\. 核苷酸多样性(π)**

衡量特定群体多样性高低的参数，是指在同一群体中随机挑选的两条DNA序列在各个核苷酸位点上核苷酸差异的均值。π值越大，说明其对应的亚群多样性越高。

**11\. 群体间固定指数(Fst)**

衡量群体中等位基因频率是否偏离遗传平衡论比例的指标，用来研究不同群体间的分化程度。其取值为0到1，0代表两个群体未分化，其成员间是完全随机交配的；1代表两个群体完全分化，形成物种隔离，且无共同的多样性存在。

**12\. θw**

Watterson's 多态性估值，从理论上说，在中性条件下，应当有θw=4Neμ的平衡状态，Ne表示有效群体大小，μ表示每一代的序列突变率。

**13\. 连锁不平衡**

相邻位点之间的非随机关联，当一个位点上的某一等位基因与另一位点上的等位基因共同出现的概率大于随机组合的假设，则这两个位点之间存在连锁不平衡。

**14\. 瓶颈效应**

由于环境骤变(如火灾、地震、洪水等)或人类活动(如人工选择、驯化)，使得某一生物种群的规模迅速减少，仅有一少部分个体能够顺利通过瓶颈事件，在之后的恢复期内产生大量后代。

**15\. 适合度**

是指某种基因型的个体在一定环境条件下，能生存并传递其基因于下一代的能力。

是指生物体或生物群体对环境适应的量化特征，是分析估计生物所具有的各种特征的适应性，以及在进化过程中继续往后代传递的能力的指标。达尔文的《物种起源》中指出：适合度是衡量一个个体存活和繁殖成功机会的尺度。适合度越大，个体成活的机会和繁殖成功的机会也越大，反之则相反（因此义项与广义适合度相对应，故亦可称之为狭义适合度）。达尔文的适者生存的个体选择观点就是建立在适合度基础上的，但用个体选择的观点无法解释动物的利他行为。因为利他行为所增进的是其他个体的适合度，而不是自己的适合度。

计算方式：适合度可以用数据计算出来：W=ml。其中，W代表适合度，m表示基因型个体生育力，l表示基因型个体存活率。

**16\. 随机遗传漂变**

当一个族群中的生物个体的数量较少时，下一代的个体容易因为有的个体没有产生后代，或是有的等位基因没有传给后代，而和上一代有不同的等位基因频率。一个等位基因可能(在经过一个以上的世代后)因此在这个族群中消失，或固定成为唯一的等位基因。这种现象就叫“遗传漂变”。

**17\. 奠基者效应**

有少数个体的基因频率决定了他们后代中的基因频率的效应，是一种极端的遗传漂变作用。

**18\. 迁移压力(又叫基因流)**

由于某种原因，具有某一基因频率的群体的一部分移入基因频率与其不同的另一群体，并杂交定居，就会引起迁入群体的基因频率发生改变。

**19\.  有效群体大小**

指与实际群体有相同基因频率方差或相同杂合度衰减率的理想群体含量，通常小于绝对的群体大小。

**20\. 中性学说**

认为分子水平上的大多数突变是中性或近中性的，自然选择对它们不起作用，这些突变靠一代又一代的随机漂变而被保存或趋于消失，从而形成分子水平上的进化性变化或种内变异。

**21\. Tajima’s D值检验**

在原有的平衡状态中θT=θW=4Neμ，所以D值为0。但是，如果群体中存在许多低频率的等位基因(稀有等位基因)，使θT<θW，则D<0。相反，当群体中是中等频率的等位基因占主导时，θT>θW，D>0。Tajima把过多低频率等位基因的存在归咎为定向选择时，选择性清除会削弱原有等位基因的在群体中的频率，而使新等位基因以低频率补充进来成为稀有等位基因。相反，如果是中等频率的等位基因占主导，则可能是平衡选择的结果，或者是种群大小在经历瓶颈时使稀有等位基因丢失。因此，当Tajima’s D显著大于0时，可用于推断瓶颈效应和平衡选择；当Tajima’s D显著小于0时，可用于推断群体规模放大和定向选择。由于平衡选择与定向选择都属于正选择的范畴，因此，只要D值显著背离0，就可能是自然选择的结果；而当D值不显著背离0时，则中性零假说则不能被排除。

**22\. 定向选择**

是指生存环境的方向性选择（自然选择）或品种的人工定向选择。

**23\. 选择性清除**

自然选择会促使有利变异更容易在群体中被保留下来，其两侧序列往往由于连锁效应同时被保留；而非有利变异则被选择清除（selective sweep）。简单的说就是基因组某区域由于受到了选择而消除多态性，即遗传多样性降低的现象。

**24\. 微进化**

群体在世代过程中等位基因频率的变化，成为微进化，即发生在物种内的遗传变化。

**25\. 大进化**

从现有物种中产生新物种的过程，是微进化的扩展、累积的结果。

**26\. 趋同进化**

在突变和选择的作用下，不同物种间具有趋同进化的趋势，这种现象称协同进化。

**27\. 遗传负荷**

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


## 群体遗传学与重测序分析
> https://www.jianshu.com/p/807e54278539

分子层面对生物的研究，在个体水平上主要是看单个基因的变化以及全转录本的变化（RNA-seq）；在对个体的研究的基础上，开始了群体水平的研究。如果说常规的遗传学主要的研究对象是个体或者个体家系的话，那么群体遗传学则是主要研究由不同个体组成的群体的遗传规律。

在测序技术大力发展之前，对群体主要是依靠表型进行研究，如加拉巴哥群岛的13中鸟雀有着不同的喙，达尔文认为这是自然选择造成的后果\[1\]。达尔文的进化论对应的观点可以简单概括为“物竞天择，适者生存”，这也是最为大众所接受的一种进化学说。直到1968年，日本遗传学家提出了中性进化理论\[2\]，也叫中性演化理论。中性理论的提出很大程度上是基于分子生物化学的发展。可以这样理解中性理论：一群人抽奖，在没有内幕的情况下，每个人抽到一等奖的概率是相等的，这个可能性和参与抽奖的人的身高、年龄、爱好等因素都没有关系。中性理论常作为群体遗传研究中的假设理论（CK）来计算其他各种统计指标。

群体遗传学，研究的单位是群体，比如粳稻、籼稻、野生稻，就能够构成不同的群体；我们国内的各省份的水稻也可以作为一个个群体。群体遗传学大概可以分为群体内的研究和群体间的研究。比如研究云南元阳的水稻的遗传多样性；如果研究是的云南元阳的水稻和东北的水稻，那就可以算成是群体间的研究。群体间和群体内的研究是相互的。

测序价格的急剧下降\[3\]使得大规模的群体测序得以实现。

![测序价格变化趋势](figure/evol_seq_cost.jpg)

### 0. 几种变异类型

常见的变异类型有SNP、InDel、SV、CNV等。重测序中最关注的是SNP，其次是(small) InDel。其他的几种结构变异的研究不是太多。

![常见的变异类型](figure/evol_var_type.png)

### 1. 重测序和从头组装

有参考基因组的物种的全基因组测序叫做重测序，没有参考基因组的物种的全基因组测序则需要从头组装。随着测序价格的降低，越来越多物种的参考基因组都已经测序组装完成。_plant genomes_ \[4\]网站实时显示全基因组测序已经完成的植物，其中2012年以后爆发式增长。在群体遗传学研究中更多的是有参考基因组的物种，尤其是模式物种，植物中常见的是拟南芥、水稻和玉米。

![重测序和从头组装](figure/evol_reseq_assemb.png)

**群体重测序方案推荐\[32\]**  
![群体重测序方案推荐](figure/evol_reseq_method.png)

### 2. 重测序分析流程\[5\]

![重测序分析流程](figure/evol_NGS_analysis.png)

### 3. 群体进化选择

#### 3.1 正选择\[6\]

正选择似乎可以更好地用自然选择来解释。就是一个基因或位点能够使个体有着更强的生存力或者是育性，这样就会使得这个个体的后代更多，如此一来，这个基因或位点在群体中就越来越多。

![正选择](figure/evol_positive_sel.png)

正选择能够使有利的突变基因或位点在群体中得到传播，但是与此同时却降低了群体的多态性水平。也就是说原先该位点周围的核苷酸组成是多样性的，在经过正选择之后，这个位点周围核苷酸的多样性就渐渐的趋于同质化了。这就好比一块田，里面本来有水稻和稗草及其他杂草，由于稗草的适应性增强，稗草在逐渐增多，水稻慢慢变少，最后甚至是只剩下了稗草。

我们将这种选择之后多态性降低的情况叫做选择扫荡（Selective Sweep)。检测选择扫荡的软件有SweeD\[7\]。选择扫荡有可能是人工选择的结果，如2014年 Nature Genetics关于非洲栽培稻的文章就使用了SweeD来检测非洲栽培稻基因组上受人工选择的区域\[8\]。

#### 3.2 负选择

负选择和正选择刚好是相反的。简单理解成群体中的某个个体出现了一个致命的突变，从而自己或者是后代从群体中被淘汰。这也导致群体中该位点的多态性的降低。就好比我有10株水稻，其中一株在成长过程中突然不见了，那么对我的这个小的水稻群体来说，这个消失的水稻的独有的位点在群体中就不见了，整体的多态性就降低了。

![负选择](figure/evol_negative_sel.png)

#### 3.3 平衡选择\[9\]

平衡选择指多个等位基因在一个群体的基因库中以高于遗传漂变预期的频率被保留，如杂合子优势。

![平衡选择](figure/evol_balance_sel.png)

平衡选择检测的算法有BetaScan2\[10\]，这是个Python脚本，输入文件只需要过滤好的SNP数据即可。

### 4. 群体遗传学中的统计指标

#### 4.1 群体多态性参数

计算公式为：  
![\theta = 4N_e\mu](https://math.jianshu.com/math?formula=%5Ctheta%20%3D%204N_e%5Cmu)  
其中![N_e](https://math.jianshu.com/math?formula=N_e)是有效群体大小，![\mu](https://math.jianshu.com/math?formula=%5Cmu)是每个位点的突变速率。但是群体大小往往是无法精确知道的，需要对其进行估计。

#### 4.2 分离位点数目

分离位点数![\theta_w](https://math.jianshu.com/math?formula=%5Ctheta_w)是![\theta](https://math.jianshu.com/math?formula=%5Ctheta)的估计值，表示相关基因在多序列比对中表现出多态性的位置。计算公式为：  
![\theta_w = \frac{K}{a_n}](https://math.jianshu.com/math?formula=%5Ctheta_w%20%3D%20%5Cfrac%7BK%7D%7Ba_n%7D)  
其中![K](https://math.jianshu.com/math?formula=K)为分离位点数量，比如SNP数量。  
![a_n](https://math.jianshu.com/math?formula=a_n)为个体数量的倒数和：  
![a_n = \sum^{n-1}_{i = 1}\frac{1}{i}](https://math.jianshu.com/math?formula=a_n%20%3D%20%5Csum%5E%7Bn-1%7D_%7Bi%20%3D%201%7D%5Cfrac%7B1%7D%7Bi%7D)

#### 4.3 核苷酸多样性

![\pi](https://math.jianshu.com/math?formula=%5Cpi)指的是核苷酸多样性，值越大说明核苷酸多样性越高。通常用于衡量群体内的核苷酸多样性，也可以用来推演进化关系\[11\]。计算公式为：  
![\pi = \sum_{ij}x_ix_j\pi_{ij}=2*\sum_{i = 2}^{n}\sum_{j=1}^{i-1}x_ix_j\pi{ij}](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Csum_%7Bij%7Dx_ix_j%5Cpi_%7Bij%7D%3D2*%5Csum_%7Bi%20%3D%202%7D%5E%7Bn%7D%5Csum_%7Bj%3D1%7D%5E%7Bi-1%7Dx_ix_j%5Cpi%7Bij%7D)  
可以理解成现在群体内两两求![\pi](https://math.jianshu.com/math?formula=%5Cpi)，再计算群体的均值。计算的软件最常见的是`vcftools`，也有对应的R包`PopGenome`。通常是选定有一定的基因组区域，设定好窗口大小，然后滑动窗口进行计算。

3KRG文章就计算了水稻不同亚群间4号染色体部分区域上的![\pi](https://math.jianshu.com/math?formula=%5Cpi)值\[12\]，能够看出控制水稻籽粒落粒性的基因 _Sh4_\[13\] 位置多态性在所有的亚群中都降低了。说明这个基因在所有的亚群中都是受到选择的，这可能是人工选择的结果。

#### 4.3.1 利用杂合度(heterozygosity)计算核苷酸多样性
> https://www.jianshu.com/p/53727b4f646b

某个位点的第![i](https://math.jianshu.com/math?formula=i)个等位基因的样本频率为![p_i](https://math.jianshu.com/math?formula=p_i)，那么该位点所有等位基因的频率和应该是1。先考虑二倍体的双等位基因，那就是![p_1 + p_2 = 1](https://math.jianshu.com/math?formula=p_1%20%2B%20p_2%20%3D%201)。衡量单个多态位点变异 (variation) 的一个方法是计算样本**杂合度 (heterozygosity)**，公式如下（ ![n](https://math.jianshu.com/math?formula=n) 表示样本中序列的数量）：

![h = \frac{n}{n-1}(1-\sum{p_i^2})](https://math.jianshu.com/math?formula=h%20%3D%20%5Cfrac%7Bn%7D%7Bn-1%7D(1-%5Csum%7Bp_i%5E2%7D))

上面这个公式是针对一个位点的，如果是针对一条序列的话，那其实就是将整条序列的杂合度加起来即可，即：

![\pi = \sum_{j=1}^{S}h_j](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Csum_%7Bj%3D1%7D%5E%7BS%7Dh_j)

其中![S](https://math.jianshu.com/math?formula=S)表示的是分离位点的数量，![h_j](https://math.jianshu.com/math?formula=h_j)表示的是第![j](https://math.jianshu.com/math?formula=j)个分离位点的杂合度。在**Wright-Fisher模型**（无限位点的二倍体）下，![E(\pi) = \theta](https://math.jianshu.com/math?formula=E(%5Cpi)%20%3D%20%5Ctheta)，因此有时这个统计量也叫![\theta_{\pi}](https://math.jianshu.com/math?formula=%5Ctheta_%7B%5Cpi%7D)。此外，在单态位点（monomorphic site）的杂合度是0。

**一个例子：**  
![heter_pi_example](figure/evol_heter_pi_example.png)

假设现在有4个样本，15个位点，但是只有6个位点是分离位点，我们先计算每个分离位点的杂合度：根据公式可知，对分离位点1（图中的第二列序列），有两个等为位点，分别是T和C，其中T有3个，C有1个，那么对T来说，它的频率就是0.75，对C来说它的频率就是0.25。根据公式可得：

![h = \frac{4}{3}\sum[1-(0.75^2 + 0.25^2) ]= 0.50](https://math.jianshu.com/math?formula=h%20%3D%20%5Cfrac%7B4%7D%7B3%7D%5Csum%5B1-(0.75%5E2%20%2B%200.25%5E2)%20%5D%3D%200.50)

我们以此计算就能得到其他5个分离位点的杂合度分别为：0.667，0.5，0.667，0.5，0.5；那么就能计算![\pi](https://math.jianshu.com/math?formula=%5Cpi)值了：

![\pi = 0.5 + 0.667 + 0.5 +0.667 + 0.5 + 0.5 = 3.33](https://math.jianshu.com/math?formula=%5Cpi%20%3D%200.5%20%2B%200.667%20%2B%200.5%20%2B0.667%20%2B%200.5%20%2B%200.5%20%3D%203.33)

但是我们通常关注的是每个位点![\pi](https://math.jianshu.com/math?formula=%5Cpi)的均值：

![\pi = \frac{3.33}{15} = 0.222](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Cfrac%7B3.33%7D%7B15%7D%20%3D%200.222)

我们将![\pi](https://math.jianshu.com/math?formula=%5Cpi)的计算进行推广就能得到下面这个公式：

![\pi = \frac{\sum_{i < j}k_{ij}}{n(n-1)/2}](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Cfrac%7B%5Csum_%7Bi%20%3C%20j%7Dk_%7Bij%7D%7D%7Bn(n-1)%2F2%7D)

其中![k_{ij}](https://math.jianshu.com/math?formula=k_%7Bij%7D)表示的是第![i](https://math.jianshu.com/math?formula=i)条序列和第![j](https://math.jianshu.com/math?formula=j)条序列之间不同核苷酸的数量，分母表示的是![n](https://math.jianshu.com/math?formula=n)个序列之间进行比较的唯一次数（非重复比较）。

应用该公式，对于4条序列，![n = 4](https://math.jianshu.com/math?formula=n%20%3D%204)，然后以此进行比较：第一条vs第二条=3个不同的核苷酸；第一条vs第三条=4个不同的核苷酸；第一条vs第四条=3个不同的核苷酸；第二条vs第三条=5个不同的核苷酸；第二条vs第四条=0个不同的核苷酸；第三条vs第四条=5个不同的核苷酸。所以：

![\pi = \frac{3+4+3+5+0+5}{4(4-1)/2} = 3.33](https://math.jianshu.com/math?formula=%5Cpi%20%3D%20%5Cfrac%7B3%2B4%2B3%2B5%2B0%2B5%7D%7B4(4-1)%2F2%7D%20%3D%203.33)

> 需要注意的是，我们在计算序列之间的差异时应当去除InDel。当使用第一个公式并将InDel当作缺失值时，计算杂合度不考虑这些缺失值，所以针对不同位点其 ![n](https://math.jianshu.com/math?formula=n) 是不同的；使用第二个公式，则在序列的两两比较之中会排除所有含有InDel等gap的位置。不同的公式给出的结果具有一定差异（往往是第一种的结果更大），因此，在处理基因组这种大数据时，通常使用第一个公式（各杂合度加和），且其计算速度更快。

#### 4.4 群体内选择检验: Tajima's D

Tajima's D是日本学者Tajima Fumio 1989年提出的一种统计检验方法，用于检验DNA序列在演化过程中是否遵循中性演化模型\[14\]。计算公式为：  
![D=\frac{\pi-\theta_w}{\sqrt{V(\pi-\theta_w)}}](https://math.jianshu.com/math?formula=D%3D%5Cfrac%7B%5Cpi-%5Ctheta_w%7D%7B%5Csqrt%7BV(%5Cpi-%5Ctheta_w)%7D%7D)

D值大小有如下三种生物学意义：  
![D值生物学意义](figure/evol_tajD_exp.png)

#### 4.5 群体间分歧度检验: Fst

![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)叫固定分化指数，用于估计亚群间平均多态性大小与整个种群平均多态性大小的差异，反映的是群体结构的变化。其简单估计的计算公式为：  
![F_{st}=\frac{\pi_{Between}-\pi_{Within}}{\pi_{Between}}](https://math.jianshu.com/math?formula=F_%7Bst%7D%3D%5Cfrac%7B%5Cpi_%7BBetween%7D-%5Cpi_%7BWithin%7D%7D%7B%5Cpi_%7BBetween%7D%7D)  
![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)的取值范围是\[0,1\]。当![F_{st}=1](https://math.jianshu.com/math?formula=F_%7Bst%7D%3D1)时，表明亚群间有着明显的种群分化。  
在中性进化条件下，![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)的大小主要取决于遗传漂变和迁移等因素的影响。假设种群中的某个等位基因因为对特定的生境的适应度较高而经历适应性选择，那该基因的频率在种群中会升高，种群的分化水平增大，使得种群有着较高的![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)值。  
![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)值可以和GWAS的结果一起进行分析，![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)超过一定阈值的区域往往和GWAS筛选到的位点是一致的，如2018年棉花重测序的文章\[15\]：

![2018棉花重测序文章图](figure/evol_Fst_GWAS.png)

#### 4.6 群体分歧度检验: ROD

ROD可以基于野生群体和驯化群体间核苷酸多态性参数![\pi](https://math.jianshu.com/math?formula=%5Cpi)的差异识别选择型号，也可以测量驯化群体和野生型群体相比损失的多态性。计算公式为：  
![ROD=1-\frac{\pi_{驯化群体}}{\pi_{野生群体}}](https://math.jianshu.com/math?formula=ROD%3D1-%5Cfrac%7B%5Cpi_%7B%E9%A9%AF%E5%8C%96%E7%BE%A4%E4%BD%93%7D%7D%7B%5Cpi_%7B%E9%87%8E%E7%94%9F%E7%BE%A4%E4%BD%93%7D%7D)  
和![F_{st}](https://math.jianshu.com/math?formula=F_%7Bst%7D)一样，ROD也可以和GWAS结合起来，如2019年油菜重测序文章\[16\]：

![2019油菜重测序文章图](figure/evol_ROD_GWAS.png)

### 5. 群体结构分析

群体结构分析可以简单理解成采样测序的这些个体可以分成几个小组，以及给每个个体之间的远近关系是怎么样的。群体结构分析三剑客， 分别是 **进化树** 、 **PCA** 和 **群体结构图** 。

#### 5.1 进化树

进化树就是将个体按照远近关系分别连接起来的图。

- 进化树算法
  - 基于距离
    - 非加权算术平均对群法UPGMA
    - 邻接法Neighbor-joining
  - 基于特征
    - 最大简约法—最小变化数（祖先状态最小化）
    - 最大似然法—所有枝长和模型参数最优化
    - 贝叶斯推断—基于后验概率
- 进化树类型
  - 有根树：有根树就是所有的个体都有一个共同的祖先
  - 无根树：无根树只展示个体间的距离，无共同祖先

#### 5.2 PCA图

PCA是很常见的降维方法，如微生物研究中常用来检验样品分群情况。PCA计算的软件很多，`plink`可以直接用vcf文件计算PCA，R语言也可以进行PCA计算。

PCA图在群体重测序中有如下几种作用：

- 查看分群信息：就是测序的样品大概分成几个群    
- 检测离群样本：离群样本就是在PCA图看起来和其他样本差异很大的样本，有可能是这个样本的遗传背景和其他样本本来就很大，也有可能是样本混淆了，比如了将野生型的样本标记成了驯化种进行测序。如果有离群样本，那在后续的类似于GWAS的分析中就需要将离群样本进行剔除。当然如果样本本来就是个很特别的，那就另当别论。
- 推断亚群进化关系：可以从PCA图可以看出群体的进化关系，尤其是地理位置的进化关系    

#### 5.3 群体分层图

进化树和PCA能够看出来群体是不是分层的，但是无法知道群体分成几个群合适，也无法看出群体间的基因交流，更无法看出个体的混血程度。这时候就需要群体分层图了，如葡萄群体重测序文章图\[18\]。  

![葡萄群体重测序文章图](figure/evol_grape_ADMIXTURE.png)

群体分层图的本质是堆叠的柱状图，和微生物研究中的物种组成柱状图类似。每个柱子是一个样本，可以看出一个样本的血缘组成，有几种颜色就说明该样本由几个祖先而来，如果只有一个色，那就说明这个个体很纯。

常用的软件有 `structure` 和 `ADMIXTURE` \[19\]。两款软件给出的结果都是K值。一般选择最低的点为最终的K值。群体分层图的可视化有个极强大的R包：`Pophelper`\[20\]。

### 6. 连锁不平衡分析

要理解 LD 衰减图，我们就必须先理解连锁不平衡（Linkage disequilibrium，LD）的概念\[22\]。连锁不平衡是由两个名词构成，连锁 + 不平衡。前者，很容易让我们产生概念混淆；后者，让这个概念变得愈加晦涩。因此从一个类似的概念入手，大家可能更容易理解 LD 的概念，那就是基因的共表达。

基因的共表达，通常指的是两个基因的表达量呈现相关性。比较常见的例子就是：转录组因子和靶基因间的关系。因为转录因子对它的靶基因有正调控作用，所以转录因子的表达量提高会导致靶基因的表达量也上调，两者往往存在正相关关系。这个正相关关系，可以使用相关系数 ![r^2](https://math.jianshu.com/math?formula=r%5E2) 来度量，这个数值在 -1~1 之间。总而言之，相关性可以理解为两个元素共同变化，步调一致。

类似的，连锁不平衡（LD）就是度量两个分子标记的基因型变化是否步调一致，存在相关性的指标。如果两个 SNP 标记位置相邻，那么在群体中也会呈现基因型步调一致的情况。比如有两个基因座，分别对应 A/a 和 B/b 两种等位基因。如果两个基因座是相关的，我们将会看到某些基因型往往共同遗传，即某些单倍型的频率会高于期望值。

参照王荣焕等\[23\]的方法进行LD参数计算：  
![LD参数计算](figure/evol_LD_calc.png)

#### 6.1 LD衰减分析

随着标记间的距离增加，平均的LD程度将降低，呈现出衰减状态，这种情况叫LD衰减。LD衰减分析的作用：

- 判断群体的多样性差异，一般野生型群体的LD衰减快于驯化群体
- 估计GWAS中标记的覆盖度，通过比较LD衰减距离(0.1)和标记间的平均距离来判断标记是否足够。

### 7. GWAS

GWAS(genome-wide association study)，全基因组关联分析，常用在医学和农学领域。简单理解成将SNP等遗传标记和表型数据进行关联分析，检测和表型相关的位点，然后再倒回去找到对应的基因，研究其对表型的影响。这些被研究的表型在医学上常常是疾病的表型；在农学上常常是受关注的农艺性状，比如水稻的株高、产量、穗粒数等。

#### 7.1 GWAS流程

- 样品准备就是要收集不同的个体，比如3KRG就3000多个水稻材料\[12\]，然后对这些材料进行全基因组测序，还需要表型数据，比如水稻的株高、产量等。
- 基因型的检测就是前面的变异检测，只是变异检测完的SNP数据还需要过滤才能进行后续的关联分析。
- 关联分析这一步只需要将基因型数据和表型数据丢给软件就行了。

#### 7.2 GWAS数学模型

目前使用最广泛的模型是混合线性模型\[26\]，所有的参数软件（如Emmax）会自动完成计算：  
![混合线性模型公式](figure/evol_GWAS_linearmix.png)

#### 7.3 GWAS结果

GWAS结果文件通常只有两个图，一个是曼哈顿图，另外一个是Q-Q图。一般是先看Q-Q图，如果Q-Q正常，曼哈顿图的结果才有意义。

- Q-Q图：用于推断关联分析使用的模型是否正确
- 曼哈顿图：图中横着的虚线通常是研究者设定的，最严格的的阈值线是Bonfferonin(![\frac{0.05}{total{SNPs}}](https://math.jianshu.com/math?formula=%5Cfrac%7B0.05%7D%7Btotal%7BSNPs%7D%7D))。阈值线以上的点就是很值得关注的位点。后续就是验证实验了，比如验证不同的单倍型的生物学功能。

### 8 其他统计指标和算法

#### 8.1 MSMC

MSMC（multiple sequentially Markovian coalescent）\[27\]，底层算法很复杂，类似于PSMC。MSMC的主要功能是推断有效群体大小和群体分离历史。

#### 8.2 LAMP

LAMP(Local Ancestry in Admixed Populations，混杂群体的局部族源推断)，用于推断采用聚类的方法假设同时检测的位点间不存在重组情况，对每组相邻的 SNP 进行检测分析\[28\]，在运算速度和推断准确度上都有了质的飞跃。

#### 8.3 Treemix

用于推断群体分离和混合\[29\]。其结果图和进化树长得特别相似，可以将得到的结果和进化树进行比较。

前文提到的很多软件和算法都是用来推断群体进化的，也就是找到群体的祖先。都可以看成族源推断。具体的差异可以参考综述\[31\]。

**参考文献**

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
\[32\]. genek.tv


## Determination of Haplotypes from Genotype information
> [http://www.biorecipes.com/Haplotypes/code.html](http://www.biorecipes.com/Haplotypes/code.html)

