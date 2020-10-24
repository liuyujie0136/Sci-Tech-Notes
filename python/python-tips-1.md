# Python使用技巧与注意事项-Part1
> Collected by liuyujie0136

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

## Python计算排列数与组合数

### 编写函数计算组合数\\(C^{i}_{n}\\)
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

![python-IDLE-settings](https://github.com/liuyujie0136/Sci-Tech-Notes/raw/main/python/python-IDLE.png)

## [Biopython教程与手册](https://biopython-cn.readthedocs.io/zh_CN/latest/)




