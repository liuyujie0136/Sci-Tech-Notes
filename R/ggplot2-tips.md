# ggplot2使用技巧

- [ggplot2使用技巧](#ggplot2使用技巧)
  - [R Graphics Cookbook, 2nd edition](#r-graphics-cookbook-2nd-edition)
  - [ggplot2高效实用指南 - 生信宝典](#ggplot2高效实用指南---生信宝典)
  - [ggplot2实用教程精选 - YuLabSMU](#ggplot2实用教程精选---yulabsmu)
  - [Use R package export to export Rplots to PPT](#use-r-package-export-to-export-rplots-to-ppt)
    - [Get the latest development version from GitHub](#get-the-latest-development-version-from-github)
    - [Getting Started](#getting-started)
    - [An example Rscript](#an-example-rscript)
  - [对数据框的不同列循环作图](#对数据框的不同列循环作图)
  - [Use `coord_cartesian` instead of `scale_y_continuous`](#use-coord_cartesian-instead-of-scale_y_continuous)
  - [在散点图上添加线性拟合方程和R值](#在散点图上添加线性拟合方程和r值)
  - [ggplot2多子图对齐坐标轴](#ggplot2多子图对齐坐标轴)


## R Graphics Cookbook, 2nd edition
> [https://r-graphics.org/](https://r-graphics.org/)

## ggplot2高效实用指南 - 生信宝典
> [https://mp.weixin.qq.com/s/v3qtAgIMpo6vxxHUjlFagQ](https://mp.weixin.qq.com/s/v3qtAgIMpo6vxxHUjlFagQ)

## ggplot2实用教程精选 - YuLabSMU
> [https://mp.weixin.qq.com/s/8dZA2HkrBytuvlSepQ1Ucw](https://mp.weixin.qq.com/s/8dZA2HkrBytuvlSepQ1Ucw)

## Use R package export to export Rplots to PPT
> export is an R package to easily export active R graphs and statistical output in publication quality to Microsoft Office, HTML, and Latex. More information on [GitHub](https://github.com/tomwenseleers/export).

### Get the latest development version from GitHub

```r
install.packages("officer")
install.packages("rvg")
install.packages("openxlsx")
install.packages("ggplot2")
install.packages("flextable")
install.packages("xtable")
install.packages("rgl")
install.packages("stargazer")
install.packages("tikzDevice")
install.packages("xml2")
install.packages("broom")
install.packages("devtools")

devtools::install_github("tomwenseleers/export")
```

### Getting Started

```r
library(export)
      
?graph2ppt
?graph2doc
?graph2svg
?graph2png
?table2ppt
?table2tex
?table2excel
?table2doc
?table2html

## export of ggplot2 plot
library(ggplot2)
qplot(Sepal.Length, Petal.Length, data = iris, color = Species, size = Petal.Width, alpha = I(0.7))
# export to Powerpoint      
graph2ppt()      
graph2ppt(file="ggplot2_plot.pptx", aspectr=1.7)
# add 2nd slide with same graph 9 inches wide and A4 aspect ratio
graph2ppt(file="ggplot2_plot.pptx", width=9, aspectr=sqrt(2), append=TRUE) 
# add 3rd slide with same graph with fixed width & height
graph2ppt(file="ggplot2_plot.pptx", width=6, height=5, append=TRUE) 
# export to Word
graph2doc()
# export to bitmap or vector formats
graph2svg()
graph2png()
graph2tif()
graph2jpg()

## export of aov Anova output
fit=aov(yield ~ block + N * P + K, npk)
x=summary(fit)
# export to Powerpoint
table2ppt(x=x)
table2ppt(x=x,file="table_aov.pptx")
table2ppt(x=x,file="table_aov.pptx",digits=4,append=TRUE)
table2ppt(x=x,file="table_aov.pptx",digits=4,digitspvals=1,font="Times New Roman",pointsize=16,append=TRUE)
# export to Word
table2doc(x=x)
# export to Excel
table2excel(x=x, file = "table_aov.xlsx",digits=4,digitspvals=1,sheetName = "Anova_table", add.rownames = TRUE)
# export to Latex
table2tex(x=x)
# export to HTML
table2html(x=x)
```

### An example Rscript
> by liuyujie0136

```r
## Export plots to PPT
# library package "export"
library(export)

# get system time for suitable file name
t=as.character(Sys.time())
t=gsub(" ","-",t)
t=gsub(":","-",t)
fname=paste0("Rplot-",t,".pptx")

# export plots
graph2ppt(file=fname)
```

## 对数据框的不同列循环作图

使用`aes_string`

```r
library(ggplot2)
data <- data.frame(x = c(1, 2, 3),
                   y1 = c(1, 3, 4),
                   y2 = c(2, 5, 7),
                   y3 = c(5, 2, 9))
for (y in c("y1", "y2", "y3")) {
  p <- ggplot(data = data, aes_string(x = "x", y = y)) +
    geom_point()
  print(p)
}
```


## Use `coord_cartesian` instead of `scale_y_continuous`

Example:

```r
ggplot(df, aes(x = Group, y = Count)) +
    geom_boxplot(outlier.colour = NA) + 
    coord_cartesian(ylim = c(0, 100))
```

From the `coord_cartesian` documentation:

> Setting limits on the coordinate system will zoom the plot (like you're looking at it with a magnifying glass), and will not change the underlying data like setting limits on a scale (e.g. scale_y_continuous) will.

You can also use `scale_y_continuous` alongside `coord_cartesian` to modify `breaks`, `minor_breaks` and `expand` etc. Just don't supply it with the `ylim` argument!


## 在散点图上添加线性拟合方程和R值

借用`ggpubr`包

```r
library(ggplot2)
library(ggpubr)

set.seed(1234)
df <- data.frame(x = c(1:100))
df$y <- 2 + 3 * df$x + rnorm(100, sd = 40)

p1 <- ggplot(data = df,
             mapping = aes(x = x,
                           y = y)) +
  geom_point() +
  geom_smooth(method = "lm") +
  theme_classic() +
  labs(x = "X",
       y = "Y") +
  theme(axis.title = element_text(size = 10)) +
  stat_cor(label.y = 300,
           aes(label = paste(..rr.label.., ..p.label.., sep = "~`,`~"))) +
  stat_regline_equation(label.x = 5, label.y = 280)

ggsave("p1.pdf",
       annotate_figure(p1, fig.lab = "(a)", fig.lab.size = 20),
       height = 4,
       width = 4)
```


## ggplot2多子图对齐坐标轴
> https://zhuanlan.zhihu.com/p/161401082

使用`cowplot::plot_grid(align = "vh")`，（`align = "v"`：垂直方向上对齐，`align = "h"`：水平方向上对齐）

例：`plot_grid(p1, p2, p3, p4, ncol = 2, align = "vh")`

另，可用于 ggplot2 子图排版的 package 有：

- gridExtra
- patchwork
- cowplot

