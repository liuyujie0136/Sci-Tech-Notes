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

