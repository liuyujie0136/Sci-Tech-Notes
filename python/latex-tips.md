# LaTeX使用技巧

- [LaTeX使用技巧](#latex使用技巧)
  - [Latex如何将长表格Table横置(旋转90度)](#latex如何将长表格table横置旋转90度)


## Latex如何将长表格Table横置(旋转90度)
> https://blog.csdn.net/haifeng_gu/article/details/108563975

在写论文时，长表格会超出页面的右边界，因此需要将长表格旋转90度横放在页面中。

**解决办法**

首先在导言区添加rotating宏包：`\usepackage[figuresright]{rotating}` ，然后，将

```latex
\begin{table}[thp]
	\caption{This is the caption}
	\centering
	\begin{tabular}{|c|c|c|c|}
		\hline
        .......
    \end{tabular}
\end{table}
```

中的`table`环境改为`sidewaystable`环境：

```latex
\begin{sidewaystable}[thp]
	\caption{This is the caption}
	\centering
	\begin{tabular}{|c|c|c|c|}
		\hline
		.......
	\end{tabular}
\end{sidewaystable}
```

> `\usepackage[figuresright]{rotating}`：图注在右，表头在左；使用`left`则相反

**参考**

1. [Rotate table with caption: “Not in outer par mode. \\begin{table}\[h\]”](https://tex.stackexchange.com/questions/50673/rotate-table-with-caption-not-in-outer-par-mode-begintableh)
2. [LaTeX技巧476：LaTeX如何将表格如何旋转（2）](http://blog.sina.com.cn/s/blog_5e16f1770100oavx.html)