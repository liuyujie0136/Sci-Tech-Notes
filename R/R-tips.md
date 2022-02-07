# R使用技巧

- [R使用技巧](#r使用技巧)
  - [Statistics for Biologists - Nature Collection](#statistics-for-biologists---nature-collection)
  - [Quick-R](#quick-r)
  - [R Graphics Cookbook, 2nd edition](#r-graphics-cookbook-2nd-edition)
  - [ggplot2高效实用指南 - 生信宝典](#ggplot2高效实用指南---生信宝典)
  - [ggplot2实用教程精选 - YuLabSMU](#ggplot2实用教程精选---yulabsmu)
  - [修改RStudio的help文档样式](#修改rstudio的help文档样式)
  - [R参数控制: `options()` - 示例](#r参数控制-options---示例)
  - [R语言修改临时文件目录](#r语言修改临时文件目录)
  - [利用R语言解压与压缩 `.tar.gz` `.zip` `.gz` `.bz2` 等文件](#利用r语言解压与压缩-targz-zip-gz-bz2-等文件)
    - [`.zip`](#zip)
    - [`.tar.gz`](#targz)
    - [`.gz` 与 `.bz2`](#gz-与-bz2)
      - [(1) 直接解压](#1-直接解压)
      - [(2) 直接读取](#2-直接读取)
  - [Excel中像`dplyr::left_join`那样连接两个工作表](#excel中像dplyrleft_join那样连接两个工作表)
    - [示例](#示例)
    - [处理方法](#处理方法)
    - [`VLOOKUP`参数](#vlookup参数)
  - [Chi-square test of independence in R](#chi-square-test-of-independence-in-r)
    - [Introduction](#introduction)
    - [Data](#data)
    - [Chi-square test of independence in R](#chi-square-test-of-independence-in-r-1)
    - [Conclusion and interpretation](#conclusion-and-interpretation)
    - [Combination of plot and statistical test](#combination-of-plot-and-statistical-test)
    - [Related articles](#related-articles)


## [Statistics for Biologists - Nature Collection](https://www.nature.com/collections/qghhqm/)

## [Quick-R](https://www.statmethods.net/)

## [R Graphics Cookbook, 2nd edition](https://r-graphics.org/)

## [ggplot2高效实用指南 - 生信宝典](https://mp.weixin.qq.com/s/v3qtAgIMpo6vxxHUjlFagQ)

## [ggplot2实用教程精选 - YuLabSMU](https://mp.weixin.qq.com/s/8dZA2HkrBytuvlSepQ1Ucw)

## 修改RStudio的help文档样式

找到RStudio安装目录，一般为`C:\Program Files\RStudio`，打开`resources`文件夹，先备份`R.css`文件为`R.css.bak`，再用管理员权限将其替换为如下内容（可能需要在别处新建文件并写入内容再拷贝至该目录覆盖原文件）：

```css
/*
 * R.css
 *
 * Copyright (C) 2009-16 by RStudio, Inc.
 *
 * Unless you have received this program directly from RStudio pursuant
 * to the terms of a commercial license agreement with RStudio, then
 * this program is licensed to you under the terms of version 3 of the
 * GNU Affero General Public License. This program is distributed WITHOUT
 * ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING THOSE OF NON-INFRINGEMENT,
 * MERCHANTABILITY OR FITNESS FOR A PARTICULAR PURPOSE. Please refer to the
 * AGPL (http://www.gnu.org/licenses/agpl-3.0.txt) for more details.
 *
 */

body, td {
   font-family: 'Source Sans Pro', 'Lucida Grande', Verdana, Arial, sans-serif !important;
   font-size: 15px !important;
}

body.macintosh, body.macintosh td {
   font-family: Consolas;
}

body.macintosh code,
body.macintosh pre {
   font-family: Consolas;
}

body code {
   font-family: Consolas;
}

::selection {
   background: rgb(181, 213, 255);
}

::-moz-selection{
   background: rgb(181, 213, 255);
}

a:visited {
   color: rgb(50%, 0%, 50%);
}

h1 {
   font-size: x-large;
}

h2 {
   font-size: x-large;
   font-weight: normal;
}

h3 {
   color: rgb(35%, 35%, 35%);
}

h4 {
   color: rgb(35%, 35%, 35%);
   font-style: italic;
}

h5 {
   color: rgb(35%, 35%, 35%);
}

h6 {
   color: rgb(35%, 35%, 35%);
   font-style: italic;
}

.rstudio-themes-flat.rstudio-themes-dark-grey h1,
.rstudio-themes-flat.rstudio-themes-dark-grey h2,
.rstudio-themes-flat.rstudio-themes-dark-grey h3,
.rstudio-themes-flat.rstudio-themes-dark-grey h4,
.rstudio-themes-flat.rstudio-themes-dark-grey h5,
.rstudio-themes-flat.rstudio-themes-dark-grey h6 {
   color: inherit;
}

.rstudio-themes-flat.rstudio-themes-dark-grey *::selection,
.rstudio-themes-flat.rstudio-themes-dark-grey *::selection {
   background: rgba(255, 255, 255, 0.15);
   color: #FFF;
}

img.toplogo {
   max-width: 4em;
   vertical-align: middle;
}

img.arrow {
   width: 30px;
   height: 30px;
   border: 0;
}

span.acronym {
   font-size: small;
}

span.env {
   font-family: Consolas;
}

span.file {
   font-family: Consolas;
}

span.option {
   font-family: Consolas;
}

span.pkg {
   font-weight: bold;
}

span.samp {
   font-family: Consolas;
}

div.vignettes a:hover {
   background: rgb(85%, 85%, 85%);
}

table p {
   margin-top: 0;
   margin-bottom: 6px;
}

table[summary="R argblock"] tr td:first-child {
   min-width: 24px;
   padding-right: 12px;
}

/* change code stype */

code {
   color: #316BFF;   /* comments: #008E00, string: #DF0002  key_words: #C800A4*/ 
   font-size: 110%;
   font-family: Consolas;
}
```

## R参数控制: `options()` - 示例

```r
options(stringsAsFactors = FALSE)
getOption("stringsAsFactors")
```

```r
options(scipen = 6, digits = 10)
getOption("scipen")
```


## R语言修改临时文件目录

在`~/.Renviron`中添加：`TMP = /home/lyj/Data/tmp/Rtmpdir`即可。

或，在R中运行：`write("TMP = '<your-desired-tempdir>'", file=file.path(Sys.getenv('R_USER'), '.Renviron'))`，与之类似。


## 利用R语言解压与压缩 `.tar.gz` `.zip` `.gz` `.bz2` 等文件

### `.zip`

- 压缩：`zip()`
- 解压：`unzip()`

若要压缩文件，就直接在 `zip()` 函数的第一个参数里面输入压缩后的文件名，第二个参数输入压缩前的文件名。解压文件直接在 `unzip()` 里面加上需要解压的文件名称即可。

### `.tar.gz`

- 压缩：`tar()`
- 解压：`untar()`

同 `.zip` 后缀的压缩文件。

### `.gz` 与 `.bz2`

这两个压缩文件与前面的相比，是最与众不同的，因为这两种后缀的文件，可以称之为压缩文件，也可以直接作为一个数据文件，当成 `data frame` 直接进行读取。因为其本身就是数据文件。

#### (1) 直接解压

R 中默认没有解压相关文件的函数，需要使用一个包：`R.utils`，然后如下述代码所示，利用 `gunzip()` 函数，即可解压。

```r
library(R.utils)
gunzip("file.gz", remove = TRUE)
bunzip2("file.bz2", remove = TRUE)
```

注意是这个函数里面多了一个 `remove` 参数，选择 `TRUE` 就会只保留解压后的文件，原压缩包会被删除，默认是 `TRUE`。

解压之后，我们可以直接用 `read.table()` 对其进行读取。

#### (2) 直接读取

当然，如果我们的目的只是读取其中的数据，而不是一定需要解压，则可以使用两个默认函数组合的形式，直接对数据进行读取：

```r
dat <- read.table(gzfile("file.gz"))  
```

而针对 2.10 版本之后的 R，还有另一种更方便的读取方式，就是直接使用 `read.table()` 对其进行读取。

```r
dat <- read.table("file.gz")
```


## Excel中像`dplyr::left_join`那样连接两个工作表

最近处理数据时遇到需要将Excel中两个表数据按指定列作为条件进行连接合并的需求，而Excel内置函数`VLOOKUP`可以方便地处理这种需求。

### 示例

现在有两个表：

Sheet1:

userid | level
:---:|:---:
1001 | 12
1002 | 15

Sheet2:

no | userid | username
:---:|:---:|:---:
1 | 1001 | test1
2 | 1002 | test2

希望合并后新得到的Sheet1:

· | A | B | C
:---:|:---:|:---:|:---:
1 | userid | level | username
2 | 1001 | 12 | test1
3 | 1002 | 15 | test2

### 处理方法

在`C2`位置插入函数

`=VLOOKUP(A2,Sheet2!$B:$C,2,FALSE)`

敲回车，然后自动填充就都有数据了

### `VLOOKUP`参数

- 第一个参数`A2`指以Sheet1的`A2`单元格中数据作为查找的字符，指定查找的值
- 第二个参数`Sheet2!$B:$C`指在工作表Sheet2中指定查找的范围
- 第三个参数是需要引用的数据在查找范围中的列号，因为需要引用username，在C列，因查找范围为B至C列，故为第2列
- 第四个参数为模糊查找开关，FALSE为精确匹配，TRUE为非精确
- 另外Sheet2中的数据不需要和Sheet1中完全相同，可以多也可以少，排序也不需要相同，查找不到的行会显示"#N/A"
- Sheet2中需要比对查找的列要放在第二个参数指定的比对区域的最前面，此处是B列


## Chi-square test of independence in R
> https://statsandr.com/blog/chi-square-test-of-independence-in-r/

### Introduction

This article explains how to perform the Chi-square test of independence in R and how to interpret its results. To learn more about how the test works and how to do it by hand, I invite you to read the article “[Chi-square test of independence by hand](https://statsandr.com/blog/chi-square-test-of-independence-by-hand/)”.

To briefly recap what have been said in that article, the Chi-square test of independence tests whether there is a relationship between two [categorical variables](https://statsandr.com/blog/variable-types-and-examples/#qualitative). The null and alternative hypotheses are:

- H<sub>0</sub> : the variables are independent, there is **no** relationship between the two categorical variables. Knowing the value of one variable does not help to predict the value of the other variable
- H<sub>1</sub> : the variables are dependent, there is a relationship between the two categorical variables. Knowing the value of one variable helps to predict the value of the other variable

The Chi-square test of independence works by comparing the observed frequencies (so the frequencies observed in your sample) to the expected frequencies if there was no relationship between the two categorical variables (so the expected frequencies if the null hypothesis was true).

### Data

For our example, let’s reuse the dataset introduced in the article “[Descriptive statistics in R](https://statsandr.com/blog/descriptive-statistics-in-r/)”. This dataset is the well-known `iris` dataset slightly enhanced. Since there is only one categorical variable and the Chi-square test of independence requires two categorical variables, we add the variable `size` which corresponds to `small` if the length of the petal is smaller than the median of all flowers, `big` otherwise:

```r
dat <- iris
dat$size <- ifelse(dat$Sepal.Length < median(dat$Sepal.Length),
  "small", "big"
)
```

We now create a [contingency table](https://statsandr.com/blog/descriptive-statistics-in-r/#contingency-table) of the two variables `Species` and `size` with the `table()` function:

```r
table(dat$Species, dat$size)

##             
##              big small
##   setosa       1    49
##   versicolor  29    21
##   virginica   47     3
```

The contingency table gives the observed number of cases in each subgroup. For instance, there is only one big setosa flower, while there are 49 small setosa flowers in the dataset.

It is also a good practice to draw a [barplot](https://statsandr.com/blog/descriptive-statistics-in-r/#barplot) to visually represent the data:

```r
library(ggplot2)
ggplot(dat) +
  aes(x = Species, fill = size) +
  geom_bar()
```

![](https://statsandr.com/blog/chi-square-test-of-independence-in-r_files/figure-html/unnamed-chunk-3-1.png)

If you prefer to visualize it in terms of proportions (so that bars all have a height of 1, or 100%):

```r
ggplot(dat) +
  aes(x = Species, fill = size) +
  geom_bar(position = "fill")
```

![](https://statsandr.com/blog/chi-square-test-of-independence-in-r_files/figure-html/unnamed-chunk-4-1.png)

This second barplot is particularly useful if there are a different number of observations in each level of the variable drawn on the xxx\-axis because it allows to compare the two variables on the same ground.

If you prefer to have the bars next to each other:

```r
ggplot(dat) +
  aes(x = Species, fill = size) +
  geom_bar(position = "dodge")
```

![](https://statsandr.com/blog/chi-square-test-of-independence-in-r_files/figure-html/unnamed-chunk-5-1.png)

See the article “[Graphics in R with ggplot2](https://statsandr.com/blog/graphics-in-r-with-ggplot2/)” to learn how to create this kind of barplot in `{ggplot2}`.

### Chi-square test of independence in R

For this example, we are going to test in R if there is a relationship between the variables `Species` and `size`. For this, the `chisq.test()` function is used:

```r
test <- chisq.test(table(dat$Species, dat$size))
test

## 
##  Pearson's Chi-squared test
## 
## data:  table(dat$Species, dat$size)
## X-squared = 86.035, df = 2, p-value < 2.2e-16
```

Everything you need appears in this output:

- the title of the test,
- which variables have been used,
- the test statistic,
- the degrees of freedom and
- the p-value of the test.

You can also retrieve the χ<sup>2</sup> test statistic and the p-value with:

```r
test$statistic

## X-squared 
##  86.03451

test$p.value

## [1] 2.078944e-19
```

If you need to find the expected frequencies, use `test$expected`.

If a warning such as “Chi-squared approximation may be incorrect” appears, it means that the smallest expected frequencies is lower than 5. To avoid this issue, you can either:

- gather some levels (especially those with a small number of observations) to increase the number of observations in the subgroups, or
- use the [Fisher’s exact test](https://statsandr.com/blog/fisher-s-exact-test-in-r-independence-test-for-a-small-sample/)

The Fisher’s exact test does not require the assumption of a minimum of 5 expected counts in the contingency table. It can be applied in R thanks to the function `fisher.test()`. This test is similar to the Chi-square test in terms of hypothesis and interpretation of the results. Learn more about this test in this [article](https://statsandr.com/blog/fisher-s-exact-test-in-r-independence-test-for-a-small-sample/) dedicated to this type of test.

Talking about assumptions, the Chi-square test of independence requires that the observations are independent. This is usually not tested formally, but rather verified based on the design of the experiment and on the good control of experimental conditions. If you are not sure, ask yourself if one observation is related to another (if one observation has an impact on another). If not, it is most likely that you have independent observations.

If you have dependent observations (paired samples), the McNemar’s or Cochran’s Q tests should be used instead. The McNemar’s test is used when we want to know if there is a significant change in two paired samples (typically in a study with a measure before and after on the same subject) when the variables have only two categories. The Cochran’s Q tests is an extension of the McNemar’s test when we have more than two related measures.

For your information, there are three other methods to perform the Chi-square test of independence in R:

1. with the `summary()` function
2. with the `assocstats()` function from the `{vcd}` package
3. with the `ctable()` function from the `{summarytools}` package

```r
# second method:
summary(table(dat$Species, dat$size))

## Number of cases in table: 150 
## Number of factors: 2 
## Test for independence of all factors:
##  Chisq = 86.03, df = 2, p-value = 2.079e-19

# third method:
vcd::assocstats(table(dat$Species, dat$size))

##                      X^2 df P(> X^2)
## Likelihood Ratio 107.308  2        0
## Pearson           86.035  2        0
## 
## Phi-Coefficient   : NA 
## Contingency Coeff.: 0.604 
## Cramer's V        : 0.757

# fourth method:
library(summarytools)
library(dplyr)
dat %>%
  ctable(Species, size,
    prop = "r", chisq = TRUE, headings = FALSE
  ) %>%
  print(
    method = "render",
    style = "rmarkdown",
    footnote = NA
  )
```

![](https://statsandr.com/blog/chi-square-test-of-independence-in-r_files/chi-square-test-of-independence-in-R-summarytools.png)

As you can see all four methods give the same results.

If you do not have the same p-values with your data across the different methods, make sure to add the `correct = FALSE` argument in the `chisq.test()` function to prevent from applying the Yate’s continuity correction, which is applied by default in this method.[<sup>1</sup>](https://statsandr.com/blog/chi-square-test-of-independence-in-r/#fn1)

### Conclusion and interpretation

From the output and from `test$p.value` we see that the p-value is less than the significance level of 5%. Like any other [statistical test](https://statsandr.com/blog/what-statistical-test-should-i-do/), if the p-value is less than the significance level, we can reject the null hypothesis. If you are not familiar with p-values, I invite you to read this [section](https://statsandr.com/blog/student-s-t-test-in-r-and-by-hand-how-to-compare-two-groups-under-different-scenarios/#a-note-on-p-value-and-significance-level-alpha).

In our context, rejecting the null hypothesis for the Chi-square test of independence means that there is a significant relationship between the species and the size. Therefore, knowing the value of one variable helps to predict the value of the other variable.

### Combination of plot and statistical test

I recently discovered the `mosaic()` function from the `{vcd}` package. This function has the advantage that it combines a [mosaic plot](https://statsandr.com/blog/descriptive-statistics-in-r/#mosaic-plot) (to visualize a contingency table) and the result of the Chi-square test of independence:

```r
library(vcd)

mosaic(~ Species + size,
  direction = c("v", "h"),
  data = dat,
  shade = TRUE
)
```

![](https://statsandr.com/blog/chi-square-test-of-independence-in-r_files/figure-html/unnamed-chunk-10-1.png)

As you can see, the mosaic plot is similar to the barplot presented above, but the p-value of the Chi-square test is also displayed at the bottom right.

Moreover, this mosaic plot with colored cases shows where the observed frequencies deviates from the expected frequencies if the variables were independent. The red cases means that the observed frequencies are _smaller_ than the expected frequencies, whereas the blue cases means that the observed frequencies are _larger_ than the expected frequencies.

An alternative is the `ggbarstats()` function from the `{ggstatsplot}` package:

```r
# load packages
library(ggstatsplot)
library(ggplot2)

# plot
ggbarstats(
  data = dat,
  x = size,
  y = Species
) +
  labs(caption = NULL) # remove caption
```

![](https://statsandr.com/blog/chi-square-test-of-independence-in-r_files/figure-html/unnamed-chunk-11-1.png)

From the plot, it seems that big flowers are more likely to belong to the `virginica` species, while small flowers tend to belong to the `setosa` species. Species and size are thus expected to be dependent.

This is confirmed thanks to the statistical results displayed in the subtitle of the plot. There are several results, but we can in this case focus on the p-value which is displayed after `p =` at the top (in the subtitle of the plot).

As with the previous tests, we reject the null hypothesis and we conclude that species and size are dependent (p-value < 0.001).

Thanks for reading. I hope the article helped you to perform the Chi-square test of independence in R and interpret its results. If you would like to learn how to do this test by hand and how it works, read the article “[Chi-square test of independence by hand](https://statsandr.com/blog/chi-square-test-of-independence-by-hand/)”.

As always, if you have a question or a suggestion related to the topic covered in this article, please add it as a comment so other readers can benefit from the discussion.

### Related articles

- [ANOVA in R](https://statsandr.com/blog/anova-in-r/)
- [Wilcoxon test in R: how to compare 2 groups under the non-normality assumption](https://statsandr.com/blog/wilcoxon-test-in-r-how-to-compare-2-groups-under-the-non-normality-assumption/)
- [Correlation coefficient and correlation test in R](https://statsandr.com/blog/correlation-coefficient-and-correlation-test-in-r/)
- [One-proportion and chi-square goodness of fit test](https://statsandr.com/blog/one-proportion-and-goodness-of-fit-test-in-r-and-by-hand/)
- [How to do a t-test or ANOVA for more than one variable at once in R](https://statsandr.com/blog/how-to-do-a-t-test-or-anova-for-many-variables-at-once-in-r-and-communicate-the-results-in-a-better-way/)

