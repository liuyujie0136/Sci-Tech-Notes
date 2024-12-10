# 简单网页版IGV
> 一个基于igv.js实现的python脚本  
> 来源：[简书 - xuzhougeng](https://www.jianshu.com/p/2aff024c7819)

Xu Zhougeng根据igv.js写了一个Python脚本，可以输出一个网页，然后通过利用Python的网页服务器打开一个端口，直接在网页上进行查看比对完的BAM、BigWig文件，并辅以BED或GTF注释。

代码详见：[igv_web.py](https://liuyujie0136.github.io/Sci-Tech-Notes/python/igv_web.py)

运行：
```bash
python3 igv_web.py -r ref/genome.fa -w bw/*.bw -g gtf/genes.gtf
python3 -m http.server 9876
```
