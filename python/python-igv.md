# 简单网页版IGV
> 一个基于igv.js实现的python脚本  
> 来源：[简书 - xuzhougeng](https://www.jianshu.com/p/2aff024c7819)

当我想查看软件运行结束后得到的BAM, BigWig, BED和GTF文件时，我都需要先把他们下载到本地，然后用IGV打开。每每这个时候，我就会非常痛苦，因为我懒得下载。

为了解决这个问题，我根据igv.js写了一个Python脚本，输出一个网页，然后通过利用Python的网页服务器打开一个端口，直接在网页上进行查看。

代码详见：[igv_web.py](https://liuyujie0136.github.io/Sci-Tech-Notes/python/igv_web.py)

运行：
```
python3 igv_web.py -r ref/genome.fa -m bam/*.bam -b bed/feature.bed

python3 -m SimpleHTTPServer 9999
```

**另：本人编写的R包[tinyfuncr](https://github.com/liuyujie0136/tinyfuncr)也有对igv.js的包装，且支持更多的功能**
