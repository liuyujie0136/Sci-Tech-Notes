# Python使用技巧与注意事项
> Collected by liuyujie0136

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

### 编写函数计算组合数<img src="http://chart.googleapis.com/chart?cht=tx&chl=C^{i}_{n}" style="border:none;">
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

![python-IDLE-settings](https://liuyujie0136.github.io/Sci-Tech-Notes/python/python-IDLE.png)

## [Biopython教程与手册](https://biopython-cn.readthedocs.io/zh_CN/latest/)

## [Python教程系列-王的机器](https://mp.weixin.qq.com/mp/appmsgalbum?action=getalbum&__biz=MzIzMjY0MjE1MA==&scene=1&album_id=1352817590674194433&count=3&uin=NTM4NTg2OTA5&key=aa37eea3a2616ab374acae3a140a6236676d3bd8daacbafb519fe3e44010e8becd1c8204d6577ef30fa031062646595b3c09548f08216a5aa1fb2adc07e68fc5e5f42a004a77efa52721a9cf7bce99e1d392ef5cc5fad0c7d7df72b25453ca10c28b3ebeb4e4eb204a3e644db23bbf307fb4cc4f4f0840a0eaa7e63280e09289&devicetype=Windows+10+x64&version=6300002f&lang=zh_CN&ascene=1&pass_ticket=zHXxNRWO9vaAdMHWYrW7Si%2FjFsybBIzRU11g9UIV9Ps42Y4mLOvCrvw4DPWir0Pr)

## [Python基础教程-C语言中文网](http://c.biancheng.net/python/)

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

