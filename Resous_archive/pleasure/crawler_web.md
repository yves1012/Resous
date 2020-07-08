# 如何扒取整站内容

刚看到一个很棒的资料站，想把里面的内容下载下来，嫌弃复制粘贴的方式太麻烦，于是写了个爬虫脚本去抓取，由于网站设置了反爬策略，不成功。

苦思冥想，wget 命令解决。

wget 可以以递归的方式下载整站，并可以将下载的页面中的链接转换为本地链接，加上相关参数后就可以变成功能强大的下载工具。

```
wget -r -p -np -k 网址

-r, –recursive（递归） specify recursive download.（指定递归下载）
-k, –convert-links（转换链接） make links in downloaded HTML point to local files.（将下载的HTML页面中的链接转换为相对链接即本地链接）
-p, –page-requisites（页面必需元素） get all images, etc. needed to display HTML page.（下载所有的图片等页面显示所需的内容）
-np, –no-parent（不追溯至父级） don’t ascend to the parent directory.
另外断点续传用-nc参数 日志 用-o参数
```

随便选一个网站进行试验：
```
[root@iZbp131uq20xx1y0zvhsskZ ~]# wget -r -p -np -k https://www.baidu.com 
--2020-01-13 17:47:12--  https://www.baidu.com/
Resolving www.baidu.com (www.baidu.com)... 180.101.49.11, 180.101.49.12
Connecting to www.baidu.com (www.baidu.com)|180.101.49.11|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2443 (2.4K) [text/html]
Saving to: ‘www.baidu.com/index.html’

100%[=======================================================================================>] 2,443       --.-K/s   in 0s      

2020-01-13 17:47:12 (787 MB/s) - ‘www.baidu.com/index.html’ saved [2443/2443]

Loading robots.txt; please ignore errors.
--2020-01-13 17:47:12--  https://www.baidu.com/robots.txt
Reusing existing connection to www.baidu.com:443.
HTTP request sent, awaiting response... 200 OK
Length: 2814 (2.7K) [text/plain]
Saving to: ‘www.baidu.com/robots.txt’

100%[=======================================================================================>] 2,814       --.-K/s   in 0s      

2020-01-13 17:47:12 (40.1 MB/s) - ‘www.baidu.com/robots.txt’ saved [2814/2814]

Loading robots.txt; please ignore errors.
--2020-01-13 17:47:12--  http://www.baidu.com/robots.txt
Connecting to www.baidu.com (www.baidu.com)|180.101.49.11|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2814 (2.7K) [text/plain]
Saving to: ‘www.baidu.com/robots.txt’

100%[=======================================================================================>] 2,814       --.-K/s   in 0s      

2020-01-13 17:47:12 (619 MB/s) - ‘www.baidu.com/robots.txt’ saved [2814/2814]

FINISHED --2020-01-13 17:47:12--
Total wall clock time: 0.1s
Downloaded: 3 files, 7.9K in 0s (104 MB/s)
Converting www.baidu.com/index.html... 0-4
Converted 1 files in 0 seconds.
```

执行完成后，目录下会出现一个新文件夹，里面包含下载内容：
```
[root@iZbp131uq20xx1y0zvhsskZ ~]# ll
total 4
drwxr-xr-x 2 root root 4096 Jan 13 17:47 www.baidu.com
```

将文件打包下载即可：
```
tar -cvf log.tar 1.log  # 仅打包不压缩
tar -zcvf log.tar.gz 1.log  # 打包后以 gzip 压缩
tar -jcvf log.tar.bz2 1.log  # 打包后，以 bzip2 压缩
```
