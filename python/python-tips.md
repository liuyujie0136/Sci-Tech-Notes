# Python使用技巧

- [Python使用技巧](#python使用技巧)
  - [VS Code 相关技巧](#vs-code-相关技巧)
    - [缩进快捷键](#缩进快捷键)
    - [同时编辑多行](#同时编辑多行)
  - [Python更换pip源(pypi镜像) - 清华大学开源软件镜像站](#python更换pip源pypi镜像---清华大学开源软件镜像站)
  - [Python计算排列数与组合数](#python计算排列数与组合数)
    - [编写函数计算组合数$C_n^i$](#编写函数计算组合数c_ni)
    - [使用第三方模块scipy计算排列组合的具体数值](#使用第三方模块scipy计算排列组合的具体数值)
    - [使用阶乘的方式求组合数](#使用阶乘的方式求组合数)
    - [使用itertools列出排列组合的全部情况](#使用itertools列出排列组合的全部情况)
    - [附：MATLAB计算排列组合数](#附matlab计算排列组合数)
  - [Python中`if __name__ == '__main__':`的作用和原理](#python中if-__name__--__main__的作用和原理)
    - [作用](#作用)
    - [原理](#原理)
  - [修改Python IDLE初始默认文件打开/保存路径的方法](#修改python-idle初始默认文件打开保存路径的方法)
  - [Biopython教程与手册](#biopython教程与手册)
  - [Python教程系列-王的机器](#python教程系列-王的机器)
  - [Python基础教程-C语言中文网](#python基础教程-c语言中文网)
  - [Python中统计列表元素的出现次数并降序排序](#python中统计列表元素的出现次数并降序排序)
  - [Python有序字典(OrderedDict)与普通字典(dict)](#python有序字典ordereddict与普通字典dict)
    - [无序字典（普通字典）](#无序字典普通字典)
    - [有序字典](#有序字典)
  - [Python `gzip`模块](#python-gzip模块)
    - [编写压缩文件](#编写压缩文件)
    - [读取压缩文件](#读取压缩文件)
  - [17 Statistical Hypothesis Tests in Python](#17-statistical-hypothesis-tests-in-python)
    - [Normality Tests](#normality-tests)
      - [Shapiro-Wilk Test](#shapiro-wilk-test)
      - [D’Agostino’s K^2 Test](#dagostinos-k2-test)
      - [Anderson-Darling Test](#anderson-darling-test)
    - [Correlation Tests](#correlation-tests)
      - [Pearson’s Correlation Coefficient](#pearsons-correlation-coefficient)
      - [Spearman’s Rank Correlation](#spearmans-rank-correlation)
      - [Kendall’s Rank Correlation](#kendalls-rank-correlation)
      - [Chi-Squared Test](#chi-squared-test)
    - [Stationary Tests](#stationary-tests)
      - [Augmented Dickey-Fuller Unit Root Test](#augmented-dickey-fuller-unit-root-test)
      - [Kwiatkowski-Phillips-Schmidt-Shin](#kwiatkowski-phillips-schmidt-shin)
    - [Parametric Statistical Hypothesis Tests](#parametric-statistical-hypothesis-tests)
      - [Student’s t-test](#students-t-test)
      - [Paired Student’s t-test](#paired-students-t-test)
      - [Analysis of Variance Test (ANOVA)](#analysis-of-variance-test-anova)
      - [Repeated Measures ANOVA Test](#repeated-measures-anova-test)
    - [Nonparametric Statistical Hypothesis Tests](#nonparametric-statistical-hypothesis-tests)
      - [Mann-Whitney U Test](#mann-whitney-u-test)
      - [Wilcoxon Signed-Rank Test](#wilcoxon-signed-rank-test)
      - [Kruskal-Wallis H Test](#kruskal-wallis-h-test)
      - [Friedman Test](#friedman-test)
    - [Further Reading](#further-reading)


## VS Code 相关技巧

### 缩进快捷键
1. 向左缩进: `Ctrl + [`
2. 向右缩进: `Ctrl + ]`

### 同时编辑多行
1. `Alt + Shift`: 竖列选择这种模式下只可以选择竖列，不可以随意插入光标。所以只限制于同一列且不间隔的情况下。
2. `Shift + Ctrl`: 竖列选择 `Ctrl+Click`，选择多个编辑位点。这种模式下不仅可以选择竖列，同时还可以在多个地方插入光标。

## Python更换pip源(pypi镜像) - [清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)

1. 临时使用（注意，simple 不能少, 是 https 而不是 http）
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```
2. 设为默认（升级 pip 到最新版本后进行配置）
```
pip install pip -U
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```
3. 如果您到 pip 默认源的网络连接较差，可临时使用本镜像站来升级 pip
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U
```


## Python计算排列数与组合数

### 编写函数计算组合数$C_n^i$
```python
def Combinatorial(n,i):
    '''设计组合数'''
    #n>=i
    Min=min(i,n-i)
    result=1
    for j in range(0,Min):
        #由于浮点数精度问题不能用//
        result=result*(n-j)/(Min-j)
    return  result
if __name__ == '__main__':
    print(int(Combinatorial(45,2)))
```

### 使用第三方模块scipy计算排列组合的具体数值
```python
from scipy.special import comb, perm
#计算排列数
A=perm(3,2)
#计算组合数
C=comb(45,2)
print(A,C)
```

### 使用阶乘的方式求组合数
```python
import math
def factorial_me(n):
    '''建立求阶乘的函数'''
    result = 1
    for i in range(2, n + 1):
        result = result * i
    return result
def comb_1(n,m):
    # 直接使用math里的阶乘函数计算组合数
    return math.factorial(n)//(math.factorial(n-m)*math.factorial(m))
def comb_2(n,m):
    # 使用自己的阶乘函数计算组合数
    return factorial_me(n)//(factorial_me(n-m)*factorial_me(m))
def perm_1(n,m):
    # 直接使用math里的阶乘函数计算排列数
    return math.factorial(n)//math.factorial(n-m)
def perm_2(n,m):
    # 使用自己的阶乘函数计算排列数
    return factorial_me(n)//factorial_me(n-m)

if __name__ == '__main__':
    print(factorial_me(6))
    print(comb_1(45,2))
    print(comb_2(45,2))
    print(perm_1(45,2))
    print(perm_2(45,2))
```

### 使用itertools列出排列组合的全部情况
```python
from itertools import combinations, permutations
# 列举排列结果[(1, 2), (1, 3), (2, 1), (2, 3), (3, 1), (3, 2)]
print(list(permutations([i for i in range(1,4)],2)))
#列举组合结果[(1, 2), (1, 3), (2, 3)]
print(list(combinations([1,2,3],2)))
```

### 附：MATLAB计算排列组合数

1. 求n的阶乘
```matlab
factorial(n)
gamma(n+1)
v='n!'; vpa(v)
```

2. 求组合数
```matlab
combntns(x,m)	#列举出从n个元素中取出m个元素的组合，其中x是含有n个元素的向量
nchoosek(n,k)	#从n个元素中取k个元素的所有组合数
nchoosek(x,m)	#从向量x中取m个元素的组合
```

3. 求排列数
```matlab
perms(x)	#给出向量x的所有排列 
prod(n:m)	#求排列数：m*(m-1)*(m-2)*…*(n+1)*n (m>n)
prod(1:2:2n-1)	#求(2n-1)!! 
prod(2:2:2n)	#求(2n)!! 
prod(A)		#对矩阵A的各列求积 
prod(A,dim)	#dim=1(默认); dim=2: 对矩阵A的各行求积（等价于(prod(A'))'） 
```

4. 累积求积函数cumprod()
```matlab
cumprod(n:m)	#输出一个向量[n n*(n+1) n(n+1)(n+2) … n(n+1)(n+2)…(m-1)m]
cumprod(A)	#A为矩阵, 输出同维数的矩阵，按列累积求积
cumprod(A,dim)	#A为矩阵, dim=1(默认, 同上); dim=2: 按行累积求积
```

5. 使用 `format rat` 命令即可使输出结果转化为分数形式


## Python中`if __name__ == '__main__':`的作用和原理

### 作用

一个python文件通常有两种使用方法，第一是作为脚本直接执行，第二是 `import` 到其他的 python 脚本中被调用（模块重用）执行。因此 `if __name__ == 'main':` 的作用就是控制这两种情况执行代码的过程，在 `if __name__ == 'main':` 下的代码只有在第一种情况下（即文件作为脚本直接执行）才会被执行，而 import 到其他脚本中是不会被执行的。举例说明：新建 `test.py` ，内容如下：

```python
# test.py
print('this is one')
if __name__ == '__main__':
    print('this is two')
```

直接执行 `test.py`，结果如下，可见 `if __name__=="__main__":` 语句之前和之后的代码都被执行。

```python
this is one
this is two
```

下面尝试 `import` 执行。在同一文件夹新建名称为 `import-test.py` 的脚本，内容如下。执行之，结果仅为`this is one`。

```python
# import-test.py
import test
```

### 原理

每个python模块（python文件，也就是此处的 test.py 和 import-test.py）都包含内置的变量 `__name__`，当该模块被直接执行的时候，`__name__` 等于文件名（包含后缀`.py` ）；如果该模块 `import` 到其他模块中，则该模块的 `__name__` 等于模块名称（不包含后缀`.py`）。
而 `__main__` 始终指当前执行模块的名称（包含后缀`.py`）。进而当模块被直接执行时，`__name__ == '__main__'` 结果为真。

为了进一步说明，我们在 `test.py` 脚本的 `if __name__=="__main__":` 之前加入 `print(__name__)`，即将 `__name__` 打印出来。结果如下：直接执行时输出为`__main__`，`import` 执行时输出为`test`。


## 修改Python IDLE初始默认文件打开/保存路径的方法

找到桌面或者开始菜单里的Python IDLE快捷方式，或者直接打开安装目录下的pythonw.exe。右击之，选择“属性”，在属性窗口中可对“起始位置”进行修改，即可更改默认文件打开/保存路径。


## Biopython教程与手册
> [https://biopython-cn.readthedocs.io/zh_CN/latest/](https://biopython-cn.readthedocs.io/zh_CN/latest/)

## Python教程系列-王的机器
> [https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&__biz=MzIzMjY0MjE1MA==&scene=1&album_id=1352817590674194433&count=3&uin=NTM4NTg2OTA5&key=aa37eea3a2616ab374acae3a140a6236676d3bd8daacbafb519fe3e44010e8becd1c8204d6577ef30fa031062646595b3c09548f08216a5aa1fb2adc07e68fc5e5f42a004a77efa52721a9cf7bce99e1d392ef5cc5fad0c7d7df72b25453ca10c28b3ebeb4e4eb204a3e644db23bbf307fb4cc4f4f0840a0eaa7e63280e09289&devicetype=Windows+10+x64&version=6300002f&lang=zh_CN&ascene=1&pass_ticket=zHXxNRWO9vaAdMHWYrW7Si%2FjFsybBIzRU11g9UIV9Ps42Y4mLOvCrvw4DPWir0Pr](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&__biz=MzIzMjY0MjE1MA==&scene=1&album_id=1352817590674194433&count=3&uin=NTM4NTg2OTA5&key=aa37eea3a2616ab374acae3a140a6236676d3bd8daacbafb519fe3e44010e8becd1c8204d6577ef30fa031062646595b3c09548f08216a5aa1fb2adc07e68fc5e5f42a004a77efa52721a9cf7bce99e1d392ef5cc5fad0c7d7df72b25453ca10c28b3ebeb4e4eb204a3e644db23bbf307fb4cc4f4f0840a0eaa7e63280e09289&devicetype=Windows+10+x64&version=6300002f&lang=zh_CN&ascene=1&pass_ticket=zHXxNRWO9vaAdMHWYrW7Si%2FjFsybBIzRU11g9UIV9Ps42Y4mLOvCrvw4DPWir0Pr)

## Python基础教程-C语言中文网
> [http://c.biancheng.net/python/](http://c.biancheng.net/python/)

`print()`函数详细语法: `print(value, ..., sep='', end='\n', file=sys.stdout, flush=False)`


## Python中统计列表元素的出现次数并降序排序

```python
from collections import Counter

ls = ['a', 'b', 'c', 'c', 'd', 'b', 'a', 'a', 'c', 'c']

counted_ls = Counter(ls)
sorted_ls_with_count = sorted(counted_ls.items(), key=lambda x: x[1], reverse=True)
```


## Python有序字典(OrderedDict)与普通字典(dict)

### 无序字典（普通字典）

```python
my_dict = dict()
my_dict["name"] = "lowman"
my_dict["age"] = 26
my_dict["girl"] = "Tailand"
my_dict["money"] = 80
my_dict["hourse"] = None
for key, value in my_dict.items():
    print(key, value)
```

输出：
```
money 80
girl Tailand
age 26
hourse None
name lowman
```

可以看见，遍历一个普通字典，返回的数据和定义字典时的字段顺序是不一致的。**注意**: Python3.6改写了dict的内部算法，Python3.6版本以后的dict是有序的，所以也就无须再关注dict顺序性的问题
 
### 有序字典

```python
import collections

my_order_dict = collections.OrderedDict()
my_order_dict["name"] = "lowman"
my_order_dict["age"] = 45
my_order_dict["money"] = 998
my_order_dict["hourse"] = None

for key, value in my_order_dict.items():
    print(key, value)
```

输出：
```
name lowman
age 45
money 998
hourse None
```

有序字典可以按字典中元素的插入顺序来输出。**注意**: 有序字典的作用只是记住元素插入顺序并按顺序输出。如果有序字典中的元素一开始就定义好了，后面没有插入元素这一动作，那么遍历有序字典，其输出结果仍然是无序的，因为缺少了有序插入这一条件，所以此时有序字典就失去了作用，所以有序字典一般用于动态添加并需要按添加顺序输出的时候。


## Python `gzip`模块

> Python gzip module provides a very simple way to compress and decompress files and work in a similar manner to GNU programs gzip and gunzip.

### 编写压缩文件

```python
import gzip
import io
import os

output_file_name = 'jd_example.txt.gz'
file_mode = 'wb'

with gzip.open(output_file_name, file_mode) as output:
    with io.TextIOWrapper(output, encoding='utf-8') as encode:
        encode.write('We can write anything in the file here.\n')

print(output_file_name, 
        'contains', os.stat(output_file_name).st_size, 'bytes')
os.system('file -b --mime {}'.format(output_file_name))
```

### 读取压缩文件

```python
import gzip
import io

read_file_name = 'jd_example.txt.gz'
file_mode = 'rb'

with gzip.open(read_file_name, file_mode) as input_file:
    with io.TextIOWrapper(input_file, encoding='utf-8') as dec:
        print(dec.read())

# i = io.TextIOWrapper(gzip.open(input_gz, "rb"), encoding='utf-8')
# use this code will return an object "i" just like commom "open" does,
# so you can use "i.readline()" or "for line in i" and so on
```


## 17 Statistical Hypothesis Tests in Python
> https://machinelearningmastery.com/statistical-hypothesis-tests-in-python-cheat-sheet/  
> By [Jason Brownlee](https://machinelearningmastery.com/author/jasonb/) on August 15, 2018 in [Statistics](https://machinelearningmastery.com/category/statistical-methods/)

### Normality Tests

This section lists statistical tests that you can use to check if your data has a Gaussian distribution.

#### Shapiro-Wilk Test

Tests whether a data sample has a Gaussian distribution.

Assumptions

- Observations in each sample are independent and identically distributed (iid).

Interpretation

- H0: the sample has a Gaussian distribution.
- H1: the sample does not have a Gaussian distribution.

Python Code

```python
# Example of the Shapiro-Wilk Normality Test
from scipy.stats import shapiro
data = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
stat, p = shapiro(data)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably Gaussian')
else:
	print('Probably not Gaussian')
```

More Information

- [A Gentle Introduction to Normality Tests in Python](https://machinelearningmastery.com/a-gentle-introduction-to-normality-tests-in-python/)
- [scipy.stats.shapiro](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.shapiro.html)
- [Shapiro-Wilk test on Wikipedia](https://en.wikipedia.org/wiki/Shapiro%E2%80%93Wilk_test)

#### D’Agostino’s K^2 Test

Tests whether a data sample has a Gaussian distribution.

Assumptions

- Observations in each sample are independent and identically distributed (iid).

Interpretation

- H0: the sample has a Gaussian distribution.
- H1: the sample does not have a Gaussian distribution.

Python Code

```python
# Example of the D'Agostino's K^2 Normality Test
from scipy.stats import normaltest
data = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
stat, p = normaltest(data)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably Gaussian')
else:
	print('Probably not Gaussian')
```

More Information

- [A Gentle Introduction to Normality Tests in Python](https://machinelearningmastery.com/a-gentle-introduction-to-normality-tests-in-python/)
- [scipy.stats.normaltest](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.normaltest.html)
- [D’Agostino’s K-squared test on Wikipedia](https://en.wikipedia.org/wiki/D%27Agostino%27s_K-squared_test)

#### Anderson-Darling Test

Tests whether a data sample has a Gaussian distribution.

Assumptions

- Observations in each sample are independent and identically distributed (iid).

Interpretation

- H0: the sample has a Gaussian distribution.
- H1: the sample does not have a Gaussian distribution.

Python Code

```python
# Example of the Anderson-Darling Normality Test
from scipy.stats import anderson
data = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
result = anderson(data)
print('stat=%.3f' % (result.statistic))
for i in range(len(result.critical_values)):
	sl, cv = result.significance_level[i], result.critical_values[i]
	if result.statistic < cv:
		print('Probably Gaussian at the %.1f%% level' % (sl))
	else:
		print('Probably not Gaussian at the %.1f%% level' % (sl))
```

More Information

- [A Gentle Introduction to Normality Tests in Python](https://machinelearningmastery.com/a-gentle-introduction-to-normality-tests-in-python/)
- [scipy.stats.anderson](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.anderson.html)
- [Anderson-Darling test on Wikipedia](https://en.wikipedia.org/wiki/Anderson%E2%80%93Darling_test)

### Correlation Tests

This section lists statistical tests that you can use to check if two samples are related.

#### Pearson’s Correlation Coefficient

Tests whether two samples have a linear relationship.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample are normally distributed.
- Observations in each sample have the same variance.

Interpretation

- H0: the two samples are independent.
- H1: there is a dependency between the samples.

Python Code

```python
# Example of the Pearson's Correlation test
from scipy.stats import pearsonr
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [0.353, 3.517, 0.125, -7.545, -0.555, -1.536, 3.350, -1.578, -3.537, -1.579]
stat, p = pearsonr(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably independent')
else:
	print('Probably dependent')
```

More Information

- [How to Calculate Correlation Between Variables in Python](https://machinelearningmastery.com/how-to-use-correlation-to-understand-the-relationship-between-variables/)
- [scipy.stats.pearsonr](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.pearsonr.html)
- [Pearson’s correlation coefficient on Wikipedia](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)

#### Spearman’s Rank Correlation

Tests whether two samples have a monotonic relationship.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample can be ranked.

Interpretation

- H0: the two samples are independent.
- H1: there is a dependency between the samples.

Python Code

```python
# Example of the Spearman's Rank Correlation Test
from scipy.stats import spearmanr
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [0.353, 3.517, 0.125, -7.545, -0.555, -1.536, 3.350, -1.578, -3.537, -1.579]
stat, p = spearmanr(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably independent')
else:
	print('Probably dependent')
```

More Information

- [How to Calculate Nonparametric Rank Correlation in Python](https://machinelearningmastery.com/how-to-calculate-nonparametric-rank-correlation-in-python/)
- [scipy.stats.spearmanr](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html)
- [Spearman’s rank correlation coefficient on Wikipedia](https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient)

#### Kendall’s Rank Correlation

Tests whether two samples have a monotonic relationship.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample can be ranked.

Interpretation

- H0: the two samples are independent.
- H1: there is a dependency between the samples.

Python Code

```python
# Example of the Kendall's Rank Correlation Test
from scipy.stats import kendalltau
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [0.353, 3.517, 0.125, -7.545, -0.555, -1.536, 3.350, -1.578, -3.537, -1.579]
stat, p = kendalltau(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably independent')
else:
	print('Probably dependent')
```

More Information

- [How to Calculate Nonparametric Rank Correlation in Python](https://machinelearningmastery.com/how-to-calculate-nonparametric-rank-correlation-in-python/)
- [scipy.stats.kendalltau](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kendalltau.html)
- [Kendall rank correlation coefficient on Wikipedia](https://en.wikipedia.org/wiki/Kendall_rank_correlation_coefficient)

#### Chi-Squared Test

Tests whether two categorical variables are related or independent.

Assumptions

- Observations used in the calculation of the contingency table are independent.
- 25 or more examples in each cell of the contingency table.

Interpretation

- H0: the two samples are independent.
- H1: there is a dependency between the samples.

Python Code

```python
# Example of the Chi-Squared Test
from scipy.stats import chi2_contingency
table = [[10, 20, 30],[6,  9,  17]]
stat, p, dof, expected = chi2_contingency(table)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably independent')
else:
	print('Probably dependent')
```

More Information

- [A Gentle Introduction to the Chi-Squared Test for Machine Learning](https://machinelearningmastery.com/chi-squared-test-for-machine-learning/)
- [scipy.stats.chi2\_contingency](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.chi2_contingency.html)
- [Chi-Squared test on Wikipedia](https://en.wikipedia.org/wiki/Chi-squared_test)

### Stationary Tests

This section lists statistical tests that you can use to check if a time series is stationary or not.

#### Augmented Dickey-Fuller Unit Root Test

Tests whether a time series has a unit root, e.g. has a trend or more generally is autoregressive.

Assumptions

- Observations in are temporally ordered.

Interpretation

- H0: a unit root is present (series is non-stationary).
- H1: a unit root is not present (series is stationary).

Python Code

```python
# Example of the Augmented Dickey-Fuller unit root test
from statsmodels.tsa.stattools import adfuller
data = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
stat, p, lags, obs, crit, t = adfuller(data)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably not Stationary')
else:
	print('Probably Stationary')
```

More Information

- [How to Check if Time Series Data is Stationary with Python](https://machinelearningmastery.com/time-series-data-stationary-python/)
- [statsmodels.tsa.stattools.adfuller API](https://www.statsmodels.org/dev/generated/statsmodels.tsa.stattools.adfuller.html).
- [Augmented Dickey–Fuller test, Wikipedia](https://en.wikipedia.org/wiki/Augmented_Dickey%E2%80%93Fuller_test).

#### Kwiatkowski-Phillips-Schmidt-Shin

Tests whether a time series is trend stationary or not.

Assumptions

- Observations in are temporally ordered.

Interpretation

- H0: the time series is trend-stationary.
- H1: the time series is not trend-stationary.

Python Code

```python
# Example of the Kwiatkowski-Phillips-Schmidt-Shin test
from statsmodels.tsa.stattools import kpss
data = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
stat, p, lags, crit = kpss(data)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably Stationary')
else:
	print('Probably not Stationary')
```

More Information

- [statsmodels.tsa.stattools.kpss API](https://www.statsmodels.org/stable/generated/statsmodels.tsa.stattools.kpss.html#statsmodels.tsa.stattools.kpss).
- [KPSS test, Wikipedia](https://en.wikipedia.org/wiki/KPSS_test).

### Parametric Statistical Hypothesis Tests

This section lists statistical tests that you can use to compare data samples.

#### Student’s t-test

Tests whether the means of two independent samples are significantly different.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample are normally distributed.
- Observations in each sample have the same variance.

Interpretation

- H0: the means of the samples are equal.
- H1: the means of the samples are unequal.

Python Code

```python
# Example of the Student's t-test
from scipy.stats import ttest_ind
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [1.142, -0.432, -0.938, -0.729, -0.846, -0.157, 0.500, 1.183, -1.075, -0.169]
stat, p = ttest_ind(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably the same distribution')
else:
	print('Probably different distributions')
```

More Information

- [How to Calculate Parametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/parametric-statistical-significance-tests-in-python/)
- [scipy.stats.ttest\_ind](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html)
- [Student’s t-test on Wikipedia](https://en.wikipedia.org/wiki/Student%27s_t-test)

#### Paired Student’s t-test

Tests whether the means of two paired samples are significantly different.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample are normally distributed.
- Observations in each sample have the same variance.
- Observations across each sample are paired.

Interpretation

- H0: the means of the samples are equal.
- H1: the means of the samples are unequal.

Python Code

```python
# Example of the Paired Student's t-test
from scipy.stats import ttest_rel
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [1.142, -0.432, -0.938, -0.729, -0.846, -0.157, 0.500, 1.183, -1.075, -0.169]
stat, p = ttest_rel(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably the same distribution')
else:
	print('Probably different distributions')
```

More Information

- [How to Calculate Parametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/parametric-statistical-significance-tests-in-python/)
- [scipy.stats.ttest\_rel](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_rel.html)
- [Student’s t-test on Wikipedia](https://en.wikipedia.org/wiki/Student%27s_t-test)

#### Analysis of Variance Test (ANOVA)

Tests whether the means of two or more independent samples are significantly different.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample are normally distributed.
- Observations in each sample have the same variance.

Interpretation

- H0: the means of the samples are equal.
- H1: one or more of the means of the samples are unequal.

Python Code

```python
# Example of the Analysis of Variance Test
from scipy.stats import f_oneway
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [1.142, -0.432, -0.938, -0.729, -0.846, -0.157, 0.500, 1.183, -1.075, -0.169]
data3 = [-0.208, 0.696, 0.928, -1.148, -0.213, 0.229, 0.137, 0.269, -0.870, -1.204]
stat, p = f_oneway(data1, data2, data3)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably the same distribution')
else:
	print('Probably different distributions')
```

More Information

- [How to Calculate Parametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/parametric-statistical-significance-tests-in-python/)
- [scipy.stats.f\_oneway](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.f_oneway.html)
- [Analysis of variance on Wikipedia](https://en.wikipedia.org/wiki/Analysis_of_variance)

#### Repeated Measures ANOVA Test

Tests whether the means of two or more paired samples are significantly different.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample are normally distributed.
- Observations in each sample have the same variance.
- Observations across each sample are paired.

Interpretation

- H0: the means of the samples are equal.
- H1: one or more of the means of the samples are unequal.

Python Code

Currently not supported in Python.

More Information

- [How to Calculate Parametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/parametric-statistical-significance-tests-in-python/)
- [Analysis of variance on Wikipedia](https://en.wikipedia.org/wiki/Analysis_of_variance)

### Nonparametric Statistical Hypothesis Tests

#### Mann-Whitney U Test

Tests whether the distributions of two independent samples are equal or not.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample can be ranked.

Interpretation

- H0: the distributions of both samples are equal.
- H1: the distributions of both samples are not equal.

Python Code

```python
# Example of the Mann-Whitney U Test
from scipy.stats import mannwhitneyu
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [1.142, -0.432, -0.938, -0.729, -0.846, -0.157, 0.500, 1.183, -1.075, -0.169]
stat, p = mannwhitneyu(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably the same distribution')
else:
	print('Probably different distributions')
```

More Information

- [How to Calculate Nonparametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/nonparametric-statistical-significance-tests-in-python/)
- [scipy.stats.mannwhitneyu](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mannwhitneyu.html)
- [Mann-Whitney U test on Wikipedia](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test)

#### Wilcoxon Signed-Rank Test

Tests whether the distributions of two paired samples are equal or not.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample can be ranked.
- Observations across each sample are paired.

Interpretation

- H0: the distributions of both samples are equal.
- H1: the distributions of both samples are not equal.

Python Code

```python
# Example of the Wilcoxon Signed-Rank Test
from scipy.stats import wilcoxon
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [1.142, -0.432, -0.938, -0.729, -0.846, -0.157, 0.500, 1.183, -1.075, -0.169]
stat, p = wilcoxon(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably the same distribution')
else:
	print('Probably different distributions')
```

More Information

- [How to Calculate Nonparametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/nonparametric-statistical-significance-tests-in-python/)
- [scipy.stats.wilcoxon](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.wilcoxon.html)
- [Wilcoxon signed-rank test on Wikipedia](https://en.wikipedia.org/wiki/Wilcoxon_signed-rank_test)

#### Kruskal-Wallis H Test

Tests whether the distributions of two or more independent samples are equal or not.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample can be ranked.

Interpretation

- H0: the distributions of all samples are equal.
- H1: the distributions of one or more samples are not equal.

Python Code

```python
# Example of the Kruskal-Wallis H Test
from scipy.stats import kruskal
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [1.142, -0.432, -0.938, -0.729, -0.846, -0.157, 0.500, 1.183, -1.075, -0.169]
stat, p = kruskal(data1, data2)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably the same distribution')
else:
	print('Probably different distributions')
```

More Information

- [How to Calculate Nonparametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/nonparametric-statistical-significance-tests-in-python/)
- [scipy.stats.kruskal](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kruskal.html)
- [Kruskal-Wallis one-way analysis of variance on Wikipedia](https://en.wikipedia.org/wiki/Kruskal%E2%80%93Wallis_one-way_analysis_of_variance)

#### Friedman Test

Tests whether the distributions of two or more paired samples are equal or not.

Assumptions

- Observations in each sample are independent and identically distributed (iid).
- Observations in each sample can be ranked.
- Observations across each sample are paired.

Interpretation

- H0: the distributions of all samples are equal.
- H1: the distributions of one or more samples are not equal.

Python Code

```python
# Example of the Friedman Test
from scipy.stats import friedmanchisquare
data1 = [0.873, 2.817, 0.121, -0.945, -0.055, -1.436, 0.360, -1.478, -1.637, -1.869]
data2 = [1.142, -0.432, -0.938, -0.729, -0.846, -0.157, 0.500, 1.183, -1.075, -0.169]
data3 = [-0.208, 0.696, 0.928, -1.148, -0.213, 0.229, 0.137, 0.269, -0.870, -1.204]
stat, p = friedmanchisquare(data1, data2, data3)
print('stat=%.3f, p=%.3f' % (stat, p))
if p > 0.05:
	print('Probably the same distribution')
else:
	print('Probably different distributions')
```

More Information

- [How to Calculate Nonparametric Statistical Hypothesis Tests in Python](https://machinelearningmastery.com/nonparametric-statistical-significance-tests-in-python/)
- [scipy.stats.friedmanchisquare](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.kruskal.html)
- [Friedman test on Wikipedia](https://en.wikipedia.org/wiki/Friedman_test)

### Further Reading

This section provides more resources on the topic if you are looking to go deeper.

- [A Gentle Introduction to Normality Tests in Python](https://machinelearningmastery.com/a-gentle-introduction-to-normality-tests-in-python/)
- [How to Use Correlation to Understand the Relationship Between Variables](https://machinelearningmastery.com/how-to-use-correlation-to-understand-the-relationship-between-variables/)
- [How to Use Parametric Statistical Significance Tests in Python](https://machinelearningmastery.com/parametric-statistical-significance-tests-in-python/)
- [A Gentle Introduction to Statistical Hypothesis Tests](https://machinelearningmastery.com/statistical-hypothesis-tests/)


