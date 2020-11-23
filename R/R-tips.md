# R使用技巧与注意事项
> Collected by liuyujie0136

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
   font-size: 16pt;
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











