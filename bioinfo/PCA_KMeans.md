# PCA降维与KMeans聚类
> https://zhuanlan.zhihu.com/p/433947355  
> https://zhuanlan.zhihu.com/p/342052190  
> https://blog.csdn.net/qq_19672707/article/details/106857918  
> https://juejin.cn/post/7000772832934232100

- [PCA降维与KMeans聚类](#pca降维与kmeans聚类)
  - [PCA主成分分析](#pca主成分分析)
    - [一、原理](#一原理)
    - [二、分析流程](#二分析流程)
    - [三、相关说明](#三相关说明)
    - [四、R语言实现主成分分析](#四r语言实现主成分分析)
  - [KMeans聚类分析](#kmeans聚类分析)
    - [KMeans](#kmeans)
    - [算法原理](#算法原理)
    - [优化目标](#优化目标)
    - [k-means++聚类算法](#k-means聚类算法)
    - [聚类效果衡量指标](#聚类效果衡量指标)
      - [衡量簇内差异来衡量聚类的效果](#衡量簇内差异来衡量聚类的效果)
      - [轮廓系数（Silhouette Coefficient）](#轮廓系数silhouette-coefficient)
    - [**K-Means 算法的优缺点**](#k-means-算法的优缺点)
  - [机器学习可视化利器`Yellowbrick`](#机器学习可视化利器yellowbrick)
    - [特征分析可视化](#特征分析可视化)
      - [Rank1D/Rank2D](#rank1drank2d)
      - [平行坐标（Parallel Coordinates）](#平行坐标parallel-coordinates)
      - [径向坐标可视化（Radial Visualization）](#径向坐标可视化radial-visualization)
      - [PCA](#pca)
      - [流形（Manifold）](#流形manifold)
    - [分类模型可视化](#分类模型可视化)
      - [类别预测误差（Class Prediction Error）](#类别预测误差class-prediction-error)
      - [分类报告（Classification Report）](#分类报告classification-report)
      - [混淆矩阵（Confusion Matrix）](#混淆矩阵confusion-matrix)
      - [精确率/召回率曲线（Precision/Recall）](#精确率召回率曲线precisionrecall)
      - [ROC/AUC](#rocauc)
      - [判别阈值（Discrimination Threshold）](#判别阈值discrimination-threshold)
    - [回归模型可视化](#回归模型可视化)
      - [残差图（Residuals Plot）](#残差图residuals-plot)
      - [预测误差图（Prediction Error）](#预测误差图prediction-error)
      - [库克距离（Cooks Distance）](#库克距离cooks-distance)
    - [聚类模型可视化](#聚类模型可视化)
      - [轮廓系数（Silhouette Scores）](#轮廓系数silhouette-scores)
      - [簇间距离图（Intercluster Distance）](#簇间距离图intercluster-distance)
    - [目标值可视化](#目标值可视化)
      - [类平衡（ClassBalance）](#类平衡classbalance)
  - [R语言代码示例](#r语言代码示例)

## PCA主成分分析

### 一、原理

主成分分析（Principal Component Analysis，PCA）是一种降维方法，通常用于降低大型数据集的维数。即将大型数据集转换为较小的变量集，该变量集仍包含大型数据集中的大部分信息。

减少数据集的变量数量会在一定程度上降低准确性，但降维的技巧是为了简单而牺牲一点准确性，因为较小的数据集更容易探索和可视化，并使机器学习算法分析数据变得更容易、更快，而无需处理无关变量。

综上，PCA的思想大概为：减少数据集的变量数量，同时保留尽可能多的信息。

### 二、分析流程

1. 标准化数据
2. 计算协方差矩阵
3. 计算协方差矩阵的特征向量和特征值
4. 创建一个特征向量来决定保留哪些主成分
5. 沿着主成分重铸数据

### 三、相关说明

1、标准化（standardization）

这一步的目的是标准化连续初始变量的范围，使得每个变量对分析的贡献相等。因为如果初始变量的范围之间存在很大差异，则范围较大的变量将支配范围较小的变量，这会导致结果有偏差。将数据转换为可进行比较的规模则可解决该问题。

可以通过减去平均值并除以每个变量每个值的标准偏差（standard deviation）来实现。

2、计算协方差矩阵（covariance matrix）

这一步的目的是了解输入数据集的变量如何相对于彼此的均值而变化，或者换句话说，查看它们之间是否存在任何关系。 因为有时，变量以包含冗余信息的方式高度相关。 因此，为了识别这些相关性，计算协方差矩阵。

3、计算协方差矩阵的特征向量和特征值，识别主成分

特征向量（eigenvector）和特征值（eigenvalue）是线性代数概念，需要从协方差矩阵中计算，以便确定主成分数据。特征向量与特征值总是成对出现，故每个特征向量都有一个特征值，它们的数量等于数据的维数。例如，对于一个三维数据集，有3个变量，因此有三个特征向量，对应3个特征值。协方差矩阵的特征向量实际上是方差最大（信息最多）的轴的方向，称之为主成分。 特征值只是附加到特征向量的系数，它给出了每个主成分中携带的方差量。

主成分是由初始变量的线性组合或混合构成的新变量。 这些组合是以这样一种方式完成的，即新变量（即主成分）是不相关的，并且初始变量中的大部分信息被挤压或压缩到第一个成分中。 所以，这个想法是n维数据给你n个主成分，但PCA试图将最大可能的信息放在第一个组件中，然后在第二个组件中放置最大的剩余信息，依此类推。

以这种方式在主成分中组织信息，将允许在不丢失太多信息的情况下降低维度，这是通过丢弃信息较少的成分并将剩余的成分视为新变量来实现的。

这里要意识到的重要一点是，主成分的可解释性较差并且没有任何实际意义，因为它们是由初始变量的线性组合构成的。

从几何上讲，主成分表示解释最大方差的数据的方向，也就是说，捕获数据的大部分信息的线。 这里方差和信息的关系是，一条线携带的方差越大，沿线的数据点的离散度就越大，沿线的离散度越大，它所包含的信息就越多。 简而言之，只需把主成分看作是提供了观察和评估数据的最佳角度的新轴即可，这样能更好地观察结果之间的差异。

4、创建一个特征向量来决定保留哪些主成分

计算特征向量并按特征值降序对它们进行排序，可以按重要性顺序找到主成分。 在这一步中，要做的是选择是保留所有这些组件还是丢弃那些重要性较低（低特征值）的组件，并与剩余的组件形成一个称之为特征向量的向量矩阵。

因此，特征向量只是一个矩阵，其列是我们决定保留的组件的特征向量。 这使它成为降维的第一步，因为如果我们选择只保留 n 个中的 p 个特征向量（分量），则最终数据集将只有 p 个维度。

5、沿着主成分重铸数据

在前面的步骤中，除了标准化之外，不对数据做任何更改，只是选择主成分并形成特征向量，但输入数据集始终保持在原始轴方面（即在初始变量方面）。

在这一步中，目的是使用由协方差矩阵的特征向量形成的特征向量，将数据从原始轴重新定向到由主成分表示的轴（因此得名主成分分析）。 这可以通过将原始数据集的转置乘以特征向量的转置来完成。

### 四、R语言实现主成分分析

使用`prcomp`函数
```r
library(ggplot2)
library(ggrepel)

iris_input <- iris # 使用R自带iris数据集（150*5，行为样本，列为特征）
rownames(iris_input) <- paste("sample",1:nrow(iris_input),sep = "") # 设置样本名

# 进行PCA（scale. = TRUE表示分析前对数据进行归一化）
pca1 <- prcomp(iris_input[,-ncol(iris_input)],center = TRUE,scale. = TRUE)

df1 <- pca1$x # 提取PC score
df1 <- as.data.frame(df1) # 注意：如果不转成数据框形式后续绘图时会报错
#               PC1        PC2         PC3          PC4
# sample1 -2.257141 -0.4784238  0.12727962  0.024087508
# sample2 -2.074013  0.6718827  0.23382552  0.102662845
# sample3 -2.356335  0.3407664 -0.04405390  0.028282305

summ1 <- summary(pca1)
summ1
# Importance of components:
#                           PC1    PC2     PC3     PC4
# Standard deviation     1.7084 0.9560 0.38309 0.14393
# Proportion of Variance 0.7296 0.2285 0.03669 0.00518
# Cumulative Proportion  0.7296 0.9581 0.99482 1.00000

# 提取主成分的方差贡献率,生成坐标轴标题
summ1 <- summary(pca1)
xlab1 <- paste0("PC1(",round(summ1$importance[2,1]*100,2),"%)")
ylab1 <- paste0("PC2(",round(summ1$importance[2,2]*100,2),"%)")

# 绘制PCA得分图
library(ggplot2)
p.pca1 <- ggplot(data = df1,aes(x = PC1,y = PC2,color = iris_input$Species))+
  stat_ellipse(aes(fill = iris_input$Species),
               type = "norm",geom = "polygon",alpha = 0.25,color = NA)+ # 添加置信椭圆
  geom_point(size = 3.5)+
  labs(x = xlab1,y = ylab1,color = "Condition",title = "PCA Scores Plot")+
  guides(fill = "none")+
  theme_bw()+
  scale_fill_manual(values = c("purple","orange","pink"))+
  scale_colour_manual(values = c("purple","orange","pink"))+
  theme(plot.title = element_text(hjust = 0.5,size = 15),
        axis.text = element_text(size = 11),axis.title = element_text(size = 13),
        legend.text = element_text(size = 11),legend.title = element_text(size = 13),
        plot.margin = unit(c(0.4,0.4,0.4,0.4),'cm'))
ggsave(p.pca1,filename = "PCA.pdf")
```

自写代码
```r
library(ggplot2) 
library(ggrepel) 

iris_input <- iris # 使用R自带iris数据集（150*5，行为样本，列为特征） 
rownames(iris_input) <- paste("sample",1:nrow(iris_input),sep = "") # 设置样本名 

iris_scale <- scale(iris_input[,-ncol(iris_input)]) # 标准化原始数据
cor_mat <- cor(iris_scale) # 计算相关系数矩阵

rs_mat <- eigen(cor_mat) # 特征分解
# rs_mat
# eigen() decomposition
# $values
# [1] 2.91849782 0.91403047 0.14675688 0.02071484
# 
# $vectors
# [,1]        [,2]       [,3]       [,4]
# [1,]  0.5210659 -0.37741762  0.7195664  0.2612863
# [2,] -0.2693474 -0.92329566 -0.2443818 -0.1235096
# [3,]  0.5804131 -0.02449161 -0.1421264 -0.8014492
# [4,]  0.5648565 -0.06694199 -0.6342727  0.5235971

val <- rs_mat$values # 提取特征值,即各主成分的方差
standard_deviation <- sqrt(val) # 换算成标准差
# [1] 1.7083611 0.9560494 0.3830886 0.1439265

proportion_of_variance <- val/sum(val) # 计算方差贡献率
# [1] 0.729624454 0.228507618 0.036689219 0.005178709

cumulative_proportion <- cumsum(proportion_of_variance) # 计算累积贡献率
# [1] 0.7296245 0.9581321 0.9948213 1.0000000

load_mat <- as.matrix(rs_mat$vectors) # 提取特征向量,即载荷矩阵(loadings)
PC <- iris_scale %*% load_mat # 计算主成分得分
colnames(PC) <- paste("PC",1:ncol(PC),sep = "")
df2 <- as.data.frame(PC) # 转换成数据框，否则直接用于绘图会报错
head(df2)
#               PC1        PC2         PC3          PC4
# sample1 -2.257141 -0.4784238  0.12727962  0.024087508
# sample2 -2.074013  0.6718827  0.23382552  0.102662845
# sample3 -2.356335  0.3407664 -0.04405390  0.028282305
```

## KMeans聚类分析

大量数据中具有"相似"特征的数据点或样本划分为一个类别。聚类分析提供了样本集在非监督模式下的类别划分。聚类的基本思想是"物以类聚、人以群分"，将大量数据集中相似的数据样本区分出来，并发现不同类的特征。  

聚类模型可以建立在无类标记的数据上，是一种非监督的学习算法。尽管全球每日新增数据量以PB或EB级别增长，但是大部分数据属于无标注甚至非结构化。所以相对于监督学习，不需要标注的无监督学习蕴含了巨大的潜力与价值。聚类根据数据自身的距离或相似度将他们划分为若干组，划分原则是组内样本最小化而组间距离最大化。

- 常用于数据探索或挖掘前期
- 没有先验经验做探索性分析
- 样本量较大时做预处理
- 解决问题
  - 数据集可以分几类
  - 每个类别有多少样本量
  - 不同类别中各个变量的强弱关系如何
  - 不同类型的典型特征是什么
- 应用场景
  - 群类别间的差异性特征分析
  - 群类别内的关键特征提取
  - 图像压缩、分割、图像理解
  - 异常检测
  - 数据离散化
- 缺点
  - 无法提供明确的行动指向
  - 数据异常对结果有影响

本文将从算法原理、优化目标、sklearn聚类算法、算法优缺点、算法优化、算法重要参数、衡量指标以及案例等方面详细介绍KMeans算法。

### KMeans

K 均值（`KMeans`）是聚类中最常用的方法之一，基于点与点之间的距离的相似度来计算最佳类别归属。

`KMeans`算法通过试着将样本分离到 nnn个方差相等的组中来对数据进行聚类，从而最小化目标函数 (见下文)。该算法要求指定集群的数量。它可以很好地扩展到大量的样本，并且已经在许多不同领域的广泛应用领域中使用。

被分在同一个簇中的数据是有相似性的，而不同簇中的数据是不同的，当聚类完毕之后，我们就要分别去研究每个簇中的样本都有什么样的性质，从而根据业务需求制定不同的商业或者科技策略。常用于客户分群、用户画像、精确营销、基于聚类的推荐系统。

### 算法原理

- 从 nnn个样本数据中随机选取 k 个质心作为初始的聚类中心。质心记为  
    $\mu_1^{(0)},\mu_2^{(0)},\cdots,\mu_k^{(0)},$
- 定义优化目标  
    $J(c,\mu)=\min\sum_{i=1}^M||x_i-\mu_{c_i}||^2$
- 开始循环，计算每个样本点到那个质心到距离，样本离哪个近就将该样本分配到哪个质心，得到 K 个簇  
    $c_i^t<-arg\,\min_k||x_i-\mu_{k}^t||^2$
- 对于每个簇，计算所有被分到该簇的样本点的平均距离作为新的质心  
    $\mu_{k}^{(t+1)}<-arg\,\min_u\sum^b_{i:c_i^t=k}||x_i-\mu||^2$
- 直到 J 收敛，即所有簇不再发生变化。

### 优化目标

`KMeans` 在进行类别划分过程及最终结果，始终追求"簇内差异小，簇间差异大"，其中差异由样本点到其所在簇的质心的距离衡量。在 KNN 算法学习中，我们学习到多种常见的距离 ---- 欧几里得距离、曼哈段距离、余弦距离。

在 sklearn 中的`KMeans`使用欧几里得距离：

$d(x,\mu)=\sqrt{\sum_{i=1}^{n}(x_i-\mu_i)^2}$

则一个簇中所有样本点到质心的距离的平方和为：

$Cluster Sum of Square(CSS)=\sum_{j=0}^{m}\sum_{i=1}^{n}(x_i-\mu_i)^2\\ Total Cluster Sum of Square=\sum_{l}^{k}CSS_l$

其中，m 为一个簇中样本的个数，j 是每个样本的编号。这个公式被称为簇内平方和`(cluster Sum of Square)`， 又叫做`Inertia`。

而将一个数据集中的所有簇的簇内平方和相加，就得到了整体平方和`(Total Cluster Sum of Square)`，又叫做`Total Inertia`。Total Inertia 越小，代表着每个簇内样本越相似，聚类的效果就越好。因此 `KMeans` 追求的是，求解能够让`Inertia`最小化的质心。

> K-means 有损失函数吗？  
> 损失函数本质是用来衡量模型的拟合效果的，只有有着求解参数需求的算法，才会有损失函数。Kmeans 不求解什么参数，它的模型本质也没有在拟合数据，而是在对数据进行一 种探索。  
> 另外，在决策树中有衡量分类效果的指标准确度`accuracy`，准确度所对应的损失叫做泛化误差，但不能通过最小化泛化误差来求解某个模型中需要的信息，我们只是希望模型的效果上表现出来的泛化误差 很小。因此决策树，KNN 等算法，是绝对没有损失函数的。

虽然在 sklearn 中只能被动选用欧式距离，但其他距离度量方式同样可以用来衡量簇内外差异。不同距离所对应的质心选择方法和`Inertia`如下表所示, 在`K-means`中，只要使用了正确的质心和距离组合，无论使用什么样的距离，都可以达到不错的聚类效果。

| 距离度量 | 质心 | Inertia |
| --- | --- | --- |
| 欧几里得距离 | 均值 | 最小化每个样本点到质心的欧式距离之和 |
| 曼哈顿距离 | 中位数 | 最小化每个样本点到质心的曼哈顿距离之和 |
| 余弦距离 | 均值 | 最小化每个样本点到质心的余弦距离之和 |

```python
from sklearn.cluster import KMeans
import numpy as np
X = np.array([[1, 2], [1, 4], [1, 0],
              [10, 2], [10, 4], [10, 0]])
kmeans = KMeans(n_clusters=2, random_state=0).fit(X)
kmeans.labels_
array([1, 1, 1, 0, 0, 0], dtype=int32)
kmeans.predict([[0, 0], [12, 3]])
array([1, 0], dtype=int32)
kmeans.cluster_centers_
array([[10.,  2.],
       [ 1.,  2.]])
```

### k-means++聚类算法

由`KMeans`算法原来可知，`KMeans`在聚类之前首先需要初始化 k 个簇中心，因此 `KMeans` 算法对初值敏感，对于不同的初始值，可能会导致不同的聚类结果。若初始化是个随机过程，很有可能 k 个簇中心都在同一个簇中，这种情况 `KMeans` 聚类算法很大程度上都不会收敛到全局最小。

为了优化选择初始质心的方法，2007 年 Arthur, David, and Sergei Vassilvitskii 三人发表了论文“k-means++: `The advantages of careful seeding`[http://ilpubs.stanford.edu:8090/778/1/2006-13.pdf](http://ilpubs.stanford.edu:8090/778/1/2006-13.pdf)，`sklearn.cluster.KMeans` 中默认参数为 `init='k-means++'` ，其算法原理为在初始化簇中心时，逐个选取 k 个簇中心，且离其他簇中心越远的样本越有可能被选为下个簇中心。

算法步骤：
- 从数据即 $\chi$ 中随机（均匀分布）选取一个样本点作为第一个初始聚类中心 $c_1$
- 计算每个样本与当前已有聚类中心之间的最短距离 $D(x)$；再计算每个样本点被选为下个聚类中心的概率 $P(x)$，最后选择最大概率值所对应的样本点作为下一个簇中心  
    $P(X)=\frac{D(x)^2}{\sum_{x\in\chi}D(x)^2}$
- 重复上步骤，直到选择 k 个聚类中心

`k-means++`算法初始化的簇中心彼此相距都十分的远，从而不可能再发生初始簇中心在同一个簇中的情况。

```python
def InitialCentroid(x, K):
    c1_idx = int(np.random.uniform(0, len(x))) # Draw samples from a uniform distribution.
    centroid = x[c1_idx].reshape(1, -1)  # choice the first center for cluster.
    k = 1
    n = x.shape[0] # number of samples
    while k < K:
        d2 = []
        for i in range(n):
            subs = centroid - x[i, :]  # D(x) = (x_1, y_1) - (x, y)
            dimension2 = np.power(subs, 2) # D(x)^2
            dimension_s = np.sum(dimension2, axis=1)  # sum of each row
            d2.append(np.min(dimension_s))
        new_c_idx = np.argmax(d2)
        centroid = np.vstack([centroid, x[new_c_idx]])
        k += 1
    return centroid
```

### 聚类效果衡量指标

聚类模型的结果不是某种标签输出，并且聚类的结果是不确定的，其优劣由业务需求或者算法需求来决定，并且没有永远的正确答案。那么如何衡量聚类的效果呢?

#### 衡量簇内差异来衡量聚类的效果

簇内平方和：`Total_Inertia`

肘部法（手肘法）认为图上的拐点就是 K 的最佳值

```python
# 应用肘部法则确定 kmeans方法中的k
from scipy.spatial.distance import cdist # 计算两个输入集合的每对之间的距离。
from sklearn.cluster import KMeans
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd
from sklearn.datasets import make_blobs

plt.style.use('seaborn')
plt.rcParams['font.sans-serif']=['Simhei'] #显示中文
plt.rcParams['axes.unicode_minus']=False #显示负号
X, y = make_blobs(n_samples=5000, centers=4, cluster_std = 2.5, n_features=2,random_state=42)
K=range(1,10)

# 直接计算sse
sse_result=[]
for k in K:
    kmeans=KMeans(n_clusters=k, random_state=666)
    kmeans.fit(X)
    sse_result.append(sum(np.min(cdist(X,kmeans.cluster_centers_,'euclidean'),axis=1))/X.shape[0])
plt.figure(figsize=(16,5))
plt.subplot(1,2,1)
plt.plot(K,sse_result,'gp-')
plt.xlabel('k',fontsize=20)
plt.ylabel(u'平均畸变程度')
plt.title(u'肘部法则确定最佳的K值(1)',fontdict={'fontsize':15})

# 第二种，使用inertia_
L = []
for k in K:
    kmeans = KMeans(n_clusters=k, random_state=666)
    kmeans.fit(X)
    L.append((k, kmeans.inertia_))
a = pd.DataFrame(L)
a.columns = ['k', 'inertia']
plt.subplot(1,2,2)
plt.plot(a.k, a.inertia,'gp-')
plt.xlabel('k',fontsize=20)
plt.ylabel('inertia')
plt.title(u'肘部法则确定最佳的K值(2)',fontdict={'fontsize':15})
plt.xticks(a.k)
plt.show()
```

但其有很大的局限：
- 它的计算太容易受到特征数目的影响。
- 它不是有界的，`Inertia`是越小越好，但并不知道合适达到模型的极限，能否继续提高。
- 它会受到超参数 K 的影响，随着`K`越大，`Inertia`注定会越来越小，但并不代表模型效果越来越好。
- Inertia 对数据的分布有假设，它假设数据满足凸分布，并且它假设数据是各向同性的 (`isotropic`)，所以使用`Inertia`作为评估指标，会让聚类算法在一些细长簇，环形簇，或者不规则形状的流形时表现不佳。

#### 轮廓系数（Silhouette Coefficient）

对没有真实标签的数据进行探索，常用轮廓系数评价聚类算法模型效果。轮廓系数可以理解为描述聚类后各个类别的轮廓清晰度的指标。其包含有两种因素——内聚度和分离度。

- 内聚度可以理解为反映一个样本点与类内元素的紧密程度：样本与其自身所在的簇中的其他样本的相似度`a`（组内差异），等于样本与同一簇中所有其他点之间的平均距离。
- 分离度可以理解为反映一个样本点与类外元素的紧密程度：样本与其他簇中的样本的相似度`b`（组间差异），等于样本与下一个最近的簇中的所有点之间的平均距离。

根据聚类的要求"簇内差异小，簇外差异大"，我们希望`b`永远大于`a`，并且大得越多越好。

单个样本的轮廓系数计算公式为: $s = \frac{b-a}{max(a,b)}$ 取值范围(-1,1)

- 越接近 1 表示样本与自己所在的簇中的样本很相似，并且与其他簇中的样本不相似，当样本点与簇外的样本更相似的时候，轮廓系数就为负。
- 当轮廓系数为 0 时，则代表两个簇中的样本相似度一致，两个簇本应该是一个簇。
- 对于簇结构为凸的数据轮廓系数较高，对于簇结构非凸的轮廓系数较低。因此，轮廓系数不能在不同的算法之间比较优劣，如统一数据下，可能KMeans的结果就比DBSCAN要好。

```python
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

# 定义KMeans，以及K值
kmeans = KMeans(n_clusters=n_clusters)	
# 根据数据data进行聚类，结果存放于result_list中
result_list = kmeans.fit_predict(data)
# 将原始的数据data和聚类结果result_list
# 传入对应的函数计算出该结果下的轮廓系数
score = silhouette_score(data, result_list)
```

### **K-Means 算法的优缺点**

- 优点
  - k-means 算法是解决聚类问题的一种经典算法，算法简单、快速 。
  - 算法尝试找出使平方误差函数值最小的 k 个划分。当簇是密集的、球状或团状的，且簇与簇之间区别明显时，聚类效果较好。
- 缺点
  - k-平均方法只有在簇的平均值被定义的情况下才能使用，且对有些分类属性的数据不适合。
  - 要求用户必须事先给出要生成的簇的数目 k 。
  - 对初值敏感，对于不同的初始值，可能会导致不同的聚类结果。
  - 不适合于发现非凸面形状的簇，或者大小差别很大的簇。
  - KMeans 本质上是一种基于欧式距离度量的数据划分方法，均值和方差大的维度将对数据的聚类结果产生决定性影响。所以在聚类前对数据（具体的说是每一个维度的特征）做归一化和单位统一至关重要。此外，异常值会对均值计算产生较大影响，导致」中心偏移，因此对于"噪声"和孤立点数据最好能提前过滤。

## 机器学习可视化利器`Yellowbrick`

`Yellowbrick`是一款用于促进机器学习模型选择的可视化分析和诊断工具。它在`scikit-learn`的api基础上做了扩展，能让我们更容易的驾驭模型优化阶段。简而言之，`yellowbrick`将`scikit-learn`和`matplotlib`有机结合起来，通过可视化方式帮助我们优化模型。

```bash
# install
pip install -U yellowbrick
```

### 特征分析可视化

#### Rank1D/Rank2D

默认情况下，Rank1D使用Shapiro-Wilk算法来评估特征分布的正态性。然后绘制条形图，显示每个特征的相对等级。

> **Shapiro-Wilk算法**从统计学意义上将样本分布与正态分布进行比较，以确定数据是否显示出与正态性的偏离或符合。

```python
from yellowbrick.features import rank1d
from yellowbrick.datasets import load_energy

X, _ = load_energy()
visualizer = rank1d(X, color="r")
```

默认情况下，Rank2D可视化工具使用皮尔逊相关系数检测两个特征之间的相关性。

```python
from yellowbrick.features import rank2d
from yellowbrick.datasets import load_credit

X, _ = load_credit()
print(X[['limit','sex', 'edu']].head())
visualizer = rank2d(X)
```

#### 平行坐标（Parallel Coordinates）

平行坐标是多维特征可视化技术，其中垂直轴（Y轴）为每个特征的值，Y轴表示特征。折线的颜色表示目标值。这允许一次性可视化多个维度；事实上，给定无限的水平空间（例如滚动窗口），技术上可以显示无限数量的维度！

```python
from sklearn.datasets import load_wine
from yellowbrick.features import parallel_coordinates

X, y = load_wine(return_X_y=True)
print(X.shape,y.shape) #(178, 13) (178,)
visualizer = parallel_coordinates(X, y, normalize="standard")
```

#### 径向坐标可视化（Radial Visualization）

RadViz 是一种多元数据可视化算法，它围绕圆的圆周均匀地绘制每个特征维度，然后在圆的内部绘制点，以便该点在从中心到每个弧的轴上对其值进行归一化。 这种机制允许尽可能多的维度可以很容易地放在一个圆上，大大扩展了可视化的维度。

数据科学家使用这种方法来检测类之间的可分离性。例如 是否有机会从特征集中学习，还是噪音太大？

如果您的数据包含具有缺失值的行 (`numpy.nan`)，则不会绘制这些缺失值。 换句话说，您可能无法获得数据的全貌。 RadViz 将发出 `DataWarning` 警告，告诉您丢失的百分比。

如果您收到此警告，则可能需要查看插补策略。 具体如何填充缺失值请参考：[scikit-learn Imputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html)。

```python
from yellowbrick.features import radviz
from yellowbrick.datasets import load_occupancy

X, y = load_occupancy()
visualizer = radviz(X, y, colors=["maroon", "gold"])
```

#### PCA

PCA 分解可视化器利用主成分分析将高维数据分解为两个或三个维度，以便可以将每个实例绘制在散点图中。 PCA 的使用意味着这些`投影的数据集`可以沿着主要变化的轴进行分析，并且可以被解释，以确定是否可以利用球面距离度量。

```python
from yellowbrick.datasets import load_spam
from yellowbrick.features import pca_decomposition

X, y = load_spam()
print(X.shape,y.shape) # (4600, 57) (4600,)
visualizer = pca_decomposition(X, y)
```

#### 流形（Manifold）

流形可视化工具使用流形学习将多维度描述的实例嵌入到2维，提供高维可视化。从而允许创建散点图来显示数据中的潜在结构。

与 PCA 和 SVD 等分解方法不同，流形通常使用最近邻方法进行嵌入，允许他们捕获会丢失的非线性结构。然后可以分析生成的投影的噪声或可分离性，以确定是否可以在数据中创建决策空间。

```python
from sklearn.datasets import load_iris
from yellowbrick.features import manifold_embedding

X, y = load_iris(return_X_y=True)
visualizer = manifold_embedding(X, y)
```

### 分类模型可视化

#### 类别预测误差（Class Prediction Error）

`ClassPredictionError`图将拟合分类模型中每个类的支持度（训练样本数），并显示为堆叠条形图。 每个条形都被分段以显示每个类别的预测比例（包括假阴性和假阳性，如混淆矩阵）。

您可以使用 `ClassPredictionError` 来可视化您的分类器遇到特别困难的类别，

更重要的是，它在每个类的基础上给出了哪些不正确的答案。 这通常可以让您更好地了解不同模型的优缺点以及数据集特有的特定挑战。

类别预测误差图提供了一种方法来快速了解分类器在预测正确类别方面的效果。

```python
from yellowbrick.datasets import load_game
from sklearn.preprocessing import OneHotEncoder
from sklearn.ensemble import RandomForestClassifier
from yellowbrick.classifier import class_prediction_error

X, y = load_game()
X = OneHotEncoder().fit_transform(X)
visualizer = class_prediction_error(
    RandomForestClassifier(n_estimators=10), X, y
)
```

#### 分类报告（Classification Report）

分类报告可视化器显示模型的准确率、召回率、F1分数。 为了更容易的解释和问题检测，该报告将数字分数与颜色编码的热图相结合。 所有热图都在 `(0.0, 1.0)` 范围内，以便于在不同分类报告之间轻松比较分类模型。

```python
from yellowbrick.datasets import load_credit
from sklearn.ensemble import RandomForestClassifier
from yellowbrick.classifier import classification_report

X, y = load_credit()
visualizer = classification_report(
    RandomForestClassifier(n_estimators=10), X, y
)
```

#### 混淆矩阵（Confusion Matrix）

ConfusionMatrix 可视化器是一个 ScoreVisualizer，它使用拟合的 scikit-learn 分类器和一组测试 X 和 y 值，然后返回一个报告，显示每个测试值预测的类别与其实际类别的比较情况。

数据科学家使用混淆矩阵来了解哪些类最容易混淆。

```python
from yellowbrick.datasets import load_game
from sklearn.preprocessing import OneHotEncoder
from sklearn.linear_model import RidgeClassifier
from yellowbrick.classifier import confusion_matrix

X, y = load_game()
X = OneHotEncoder().fit_transform(X)
visualizer = confusion_matrix(RidgeClassifier(), X, y, cmap="Greens")
```

#### 精确率/召回率曲线（Precision/Recall）

PrecisionRecallCurve 显示了分类器精度（结果相关性的度量）和召回率（完整性的度量）之间的权衡。

对于每个类别，精度定义为真阳性与真假阳性总和的比率，召回率是真阳性与真阳性和假阴性总和的比率。

```python
from sklearn.naive_bayes import GaussianNB
from yellowbrick.datasets import load_occupancy
from yellowbrick.classifier import precision_recall_curve

X, y = load_occupancy()
visualizer = precision_recall_curve(GaussianNB(), X, y)
```

#### ROC/AUC

ROC/AUC图是用户可视化分类器的灵敏度和特异性之间的权衡。

ROC是分类器预测质量的度量，它比较可视化模型的灵敏度和特异性之间的权衡。绘制时，ROC 曲线在 Y 轴上显示真阳性率，在 X 轴上显示每个类别的假阳性率。因此，理想点是图的左上角：假阳性为零，真阳性为 1。

另一个度量，即曲线下面积 (AUC)，它是对假阳性和真阳性之间关系的计算。 AUC 越高，模型通常越好。然而，检查曲线的“陡度”也很重要，因为这描述了真阳性率的最大化，同时，假阳性率的最小化。

```python
from yellowbrick.classifier import roc_auc
from yellowbrick.datasets import load_spam
from sklearn.linear_model import LogisticRegression

X, y = load_spam()
visualizer = roc_auc(LogisticRegression(), X, y)
```

#### 判别阈值（Discrimination Threshold）

精度、召回率、f1 分数等指标是对二元分类器的判别阈值的可视化。

判别阈值是选择正类而不是负类的概率或分数。 通常，这设置为 50%，但可以调整阈值以增加或减少对误报或其他应用程序因素的敏感度。

```python
from yellowbrick.classifier import discrimination_threshold
from sklearn.linear_model import LogisticRegression
from yellowbrick.datasets import load_spam

X, y = load_spam()
visualizer = discrimination_threshold(
    LogisticRegression(multi_class="auto", solver="liblinear"), X, y
)
```

### 回归模型可视化

#### 残差图（Residuals Plot）

在回归模型的上下文中，残差是目标变量的观测值 (y) 与预测值 (ŷ) 之间的差异，即预测的误差。

残差图显示了纵轴上的残差和横轴上的因变量之间的差异，使您可以检测目标内可能容易受到或多或少误差影响的区域。

```python
from sklearn.linear_model import Ridge
from yellowbrick.datasets import load_concrete
from yellowbrick.regressor import residuals_plot

X, y = load_concrete()
visualizer = residuals_plot(
    Ridge(), X, y, train_color="maroon", test_color="gold"
)
```

#### 预测误差图（Prediction Error）

预测误差图显示了数据集中的实际目标与我们的模型生成的预测值。 这使我们能够看到模型中有多少方差。

数据科学家可以使用此图通过与45度线进行比较来诊断回归模型，那个地方预测结果完全的匹配模型。

```python
from sklearn.linear_model import Lasso
from yellowbrick.datasets import load_bikeshare
from yellowbrick.regressor import prediction_error

X, y = load_bikeshare()
visualizer = prediction_error(Lasso(), X, y)
```

#### 库克距离（Cooks Distance）

Cook距离是统计分析中一种常见的距离，用于诊断各种回归分析中是否存在异常数据。

具有较大影响的实例可能是异常值，并且具有大量高影响点的数据集可能不适合线性回归，而无需进一步处理，例如去除异常值或插补。

CooksDistance 可视化器按索引显示所有实例的茎图及其相关距离分数，以及启发式阈值，以快速显示数据集的百分比可能会影响 OLS 回归模型。

```python
from sklearn.datasets import load_diabetes
from yellowbrick.regressor import cooks_distance

X, y = load_diabetes(return_X_y=True)
visualizer = cooks_distance(X, y)
```

### 聚类模型可视化

#### 轮廓系数（Silhouette Scores）

当关于数据集的真实情况未知时，使用轮廓系数，并对模型聚类密度的估算进行计算。 该分数是通过对每个样本的轮廓系数求平均值来计算的，计算为每个样本的平均簇内距离和平均最近簇距离之间的差异，并对最大值归一化。这会产生介于 1 和 -1 之间的分数，其中 1 是高度密集的聚类，而 -1 是完全不正确的聚类。

Silhouette可视化器在每个簇的基础上显示每个样本的轮廓系数，可视化哪些簇是密集的，哪些不是。这对于确定聚类不平衡或通过比较多个可视化工具为选择值特别有用。

```python
from sklearn.cluster import KMeans
from yellowbrick.datasets import load_nfl
from yellowbrick.cluster import silhouette_visualizer

X, y = load_nfl()
visualizer = silhouette_visualizer(KMeans(5, random_state=42), X)
```

#### 簇间距离图（Intercluster Distance）

簇间距离图显示了簇中心在二维中的嵌入，并保留了与其他中心的距离。

例如，在可视化中越靠近中心，它们就越靠近原始特征空间。根据评分指标来确定簇的大小。

默认情况下，它们按成员身份确定大小，例如，属于每个中心的实例数。这给出了簇的相对重要性的理解。

> 注意: 因为两个簇在 2D 空间中重叠，所以并不意味着它们在原始特征空间中重叠。

```python
from yellowbrick.datasets import load_nfl
from sklearn.cluster import MiniBatchKMeans
from yellowbrick.cluster import intercluster_distance

X, y = load_nfl()
visualizer = intercluster_distance(MiniBatchKMeans(5, random_state=777), X)
```

### 目标值可视化

#### 类平衡（ClassBalance）

分类模型的最大挑战之一是训练数据中的类不平衡。严重的类不平衡可能会被相对较好的 F1 和准确度分数所掩盖。

分类器只是猜测多数类，而不是对代表性不足的类进行任何评估。

有几种处理类别不平衡的技术，例如分层抽样、对多数类别进行下采样、加权等。

但在采取这些行动之前，了解训练数据中的类别平衡是什么很重要。

ClassBalance 可视化工具通过为每个类创建一个条形图来了解类别的平衡，即类在数据集中的表示频率。

```python
from yellowbrick.datasets import load_game
from yellowbrick.target import class_balance

X, y = load_game()
visualizer = class_balance(y, labels=["draw", "loss", "win"])
```

## R语言代码示例
```r
library(magrittr)
library(tinyfuncr)

file <- commandArgs(TRUE)[[1]]
maxK <- 50L
filebase <- sub(".xls", "", file)

# read file
data <- read_tcsv(file)
data_mat <- data[6:ncol(data)]
rownames(data_mat) <- data[[4]]

######## Run PCA ########
pca <- prcomp(data_mat, center = TRUE, scale. = TRUE)
cat(paste0(filebase, "\n"))
summary(pca)

# select PC num
pc_num <- which(summary(pca)$importance[3, ] > 0.9)[[1]]
data_pc <- as.data.frame(pca$x)[1:pc_num]
cat(paste0(
  "\nSelect the first ",
  pc_num,
  " of all PCs to explain >= 90% variance\n"
))

######## Elbow Method to Select K ########
sklearn <- reticulate::import("sklearn.cluster")
ybce <- reticulate::import("yellowbrick.cluster.elbow")

el <-
  ybce$kelbow_visualizer(sklearn$KMeans(random_state = 4L, n_init = 1L),
                         data_pc,
                         k = maxK,
                         show = F)
k <- el$elbow_value_
score <- el$elbow_score_

cat(paste0(
  "Elbow at K = ",
  k,
  ", Score = ",
  score,
  " (testing with maxK=",
  maxK,
  ")\n"
))

el$fig$savefig(
  paste0(
    filebase,
    ".PC",
    pc_num,
    ".maxK",
    maxK,
    ".KMeansDistortionScore.png"
  ),
  dpi = 300,
  bbox_inches = "tight"
)

######## Draw Silhouette Plot ########
sklearn <- reticulate::import("sklearn.cluster")
ybc <- reticulate::import("yellowbrick.cluster")

sp <-
  ybc$silhouette_visualizer(sklearn$KMeans(as.integer(k), random_state = 4L, n_init = 1L),
                            data_pc,
                            show = F)

sp$fig$savefig(
  paste0(filebase,
         ".PC",
         pc_num,
         ".KMeans",
         K,
         ".SilhouettePlot.png"),
  dpi = 300,
  bbox_inches = "tight"
)

######## Silhouette Score Peak ########
sklearn <- reticulate::import("sklearn.cluster")
ybce <- reticulate::import("yellowbrick.cluster.elbow")

el <-
  ybce$kelbow_visualizer(
    sklearn$KMeans(random_state = 4L, n_init = 1L),
    data_pc,
    k = maxK,
    metric = "silhouette",
    show = F
  )

k <- el$elbow_value_
score <- el$elbow_score_

cat(
  paste0(
    "Peak of Silhouette Score at K = ",
    k,
    ", Score = ",
    score,
    " (testing with maxK=",
    maxK,
    ")\n"
  )
)

el$fig$savefig(
  paste0(filebase,
         ".PC",
         pc_num,
         ".maxK",
         maxK,
         ".SilhouetteScore.png"),
  dpi = 300,
  bbox_inches = "tight"
)

######## KMeans Clustering ########
kmeans <- stats::kmeans(data_pc, centers = k, iter.max = 30)
cluster <- data.frame(kmeans[["cluster"]])
cluster$Name <- rownames(cluster)
colnames(cluster) <- c("Cluster", "Name")

data %<>% dplyr::left_join(cluster, by = "Name")
data <- data[c(1:5, ncol(data), 6:(ncol(data) - 1))]

write_tcsv(data, paste0(filebase, ".PC", pc_num, ".KMeans", k, ".xls"))
```
