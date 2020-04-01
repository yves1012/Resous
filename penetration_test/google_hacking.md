# 通过 Google Hacking 语法进行信息采集

## 语法简介
* site:
> 搜索范围限制在某网站或顶级域名中，这个也很有用，例如: site:www.4ngel.net 将返回所有和 4ngel.net 这个站有关的URL。

* filetype:
> 搜索特定类型文档，例如: filetype:doc 将返回所有以 doc 结尾的文件 URL，当然如果你找.bak、.mdb或.inc也是可以的，获得的信息也许会更丰富。

* inurl:
> 搜索指定的字符是否存在于URL中，例如: inurl:admin 将返回N个类似于这样的连接:http://www.xxx.com/xxx/admin 用来找管理员登陆的URL，allinurl 也同 inurl 类似，可指定多个字符。


* intext:
> 正文中搜索某个字符做为搜索条件，如在google里输入 intext:动网 将返回所有在网页正文部分包含”动网”的网页，allintext:使用方法和intext类似。

* intitle:
> 标题中搜索是否有我们所要找的字符,例如搜索 intitle:安全天使 将返回所有网页标题中包含”安全天使”的网页,同理allintitle:也同intitle类似.

* cache:
> 搜索谷歌缓存页面（类似百度快照）搜索google里关于某些内容的缓存,有时候也许能找到一些好东西。

* define:
> 查询单词或者术语的定义，搜索某个词语的定义，搜索 define:hacker 将返回关于hacker的定义。

* info:
> 查找指定站点的一些基本信息。

* link:
> 搜索所有链接到某个URL地址的网页，例如搜索 link:www.4ngel.net 可以返回所有和 www.4ngel.net 做了链接的URL。

* inanchor:
> 锚链链接搜索在做网站中有时候用锚点来链接一个页面中的其它部分内容，这样方便浏览和定位。也就是说锚点链接的内容通常是网页内容中重要的章节或内容的开始部分，因而对它们的搜索也更能反映网页的主题内容，提高搜索结果的准确度。对于熟悉网页制作的人来说，可以从网页源代码中查看有锚点的HTML代码。

* related:
> 相关网址查找与某个页面结构内容相似的页面，“related”用来搜索结构内容方面相似的网页。related语法对于发现某一类信息非常有用，比如当你用related搜索一个图书馆网址的时候会出来大量图书馆的网站，如【related:lib.nit.net.cn】；当搜索某期刊网址的时候，能搜索出大量给学科领域的相关期刊，如【related:www.lis.ac.cn】。

## 初阶搜索
**1、默认模糊搜索、自动拆分短语**

搜索引擎基本一样的语法，直接在搜索框中输入搜索词时，谷歌默认进行模糊搜索，并能对长短语或语句进行自动拆分成小的词进行搜索。

**2、短语精确搜索。 — “”**

给关键词加上半角引号实现精确搜索，不进行分词。

**3、通配符。 — “※”**

谷歌的通配符是星号“*”，必须在精确搜索符双引号内部使用。用通配符代替关键词或短语中无法确定的字词。

**4、点号匹配任意字符。 — .**

与通配符星号“*”不一样的是，点号“.”匹配的是字符，不是字、短语等内容。保留的字符有[、(、-等。

**5、布尔逻辑：与或非：and，|,or,-”**

布尔逻辑是许多检索系统的基本检索技术，在搜索引擎中也一样适用，在谷歌网页搜索中需要注意的是：谷歌和许多搜索引擎一样，多个词间的逻辑关系默认的是逻辑与（空格）。当用逻辑算符的时候，词与逻辑算符之间用需要空格分隔，包括后面讲的各种语法，均要有空格。逻辑非是特例，即减号必须与对应的词连在一起。对于复杂的逻辑关系，可用括号分组。

**6、基本搜索符号约束**

加号“+”用于强制搜索，即必须包含加号后的内容。一般与精确搜索符一起应用。关键词前加“-”减号,要求搜索结果中包含关键词,但不包含减号后的关键词，用关于搜索结果的筛选。

**7、数字范围: ..**

用两个点号“..”表示一个数字范围。一般应用于日期、货币、尺寸、重量、高度等范围的搜索。用作范围时最好给一定的含义。

**8、括号分组: ()**

逻辑组配时分组，避免逻辑混乱。括号“()”是分组符号。

## 高阶用法
**1、查找可以未经授权就可以访问的phpMyAdmin的后台页面**

inurl:.php? intext:CHARACTER_SETS,COLLATIONS, ?intitle:phpmyadmin

**2、搜索可能存在openssl心脏出血漏洞的站点**

"OPENSSL" AND "1.0.1 SERVER AT" OR "1.0.1A SERVER AT" OR "1.0.1B SERVER AT" OR "1.0.1C SERVER AT" OR "1.0.1D SERVER AT" OR "1.0.1E SERVER AT" OR "1.0.1F SERVER AT"

## 查找注射点
site:xx.com filetype:asp

site:tw inurl:asp?id= 这个是找台湾的

site:jp inurl:asp?id= 这个是找日本的

site:ko inurl:asp?id= 这个是找韩国的

intitle:旁注- 网站xxxfiletype:asp

inurl:editor/db/

inurl:eWebEditor/db/

inurl:bbs/data/

inurl:databackup/

inurl:blog/data/

inurl:okedata

inurl:bbs/database/

inurl:conn.asp

inurl:inc/conn.asp

## 上传漏洞
/eWebEditor/upload.asp #eWebEditor上传页面

/editor/upload.asp #eWebEditor上传页面

/bbs/upfile.asp #动网论坛上传页面

/forum/upfile.asp #动网论坛上传页面

/dvbbs/upfile.asp #动网论坛上传页面

/upfile_soft.asp #动力管理系统上传页面

/upload.asp?action=upfile #乔客6.0上传页面

/upfile.asp #动网论坛上传页面

/bbs/down_addsoft.asp #动网论坛插件上传页面

/bbs/down_picupfile.asp #动网论坛插件上传页面

/down_picupload.asp #动网论坛插件上传页面

/admin/admin_upfile.asp #管理员后台上传页面

/admin/upfile.asp #管理员后台上传页面

/admin/upload.asp #管理员后台上传页面

/admin/uploadfaceok.asp #尘缘上传页面

/news/admin/upfile.asp #新闻管理上传页面

/admin_upfile.asp #飞龙文章管理系统 v2.0

/user_upfile.asp #飞龙文章管理系统 v2.0

/upload_flash.asp #秋叶购物商城上传页面

/Saveannounce_upload.asp #购物中心上传页面

/UploadFace.asp #沸腾展望新闻系统 v1.1

/bbs/diy.asp #Domian3.0默认木马

/UploadSoft/diy.asp #Domian3.0默认木马

/diy.asp #Domian3.0默认木马

/upload/upload.asp #某某文章管理系统

/mybbs/saveup.asp #MYBBS论坛上传页面

/dxxobbs/upload.asp #DxxoBBS论坛上传页面

/img_upfile.asp #任我飞扬驿站上传页面

/Upfile_SoftPic.asp #动力管理系统上传页面

/upfile_flash.asp #秋叶购物商城上传页面

## 数据库
database/PowerEasy4.mdb #动易网站管理系统4.03数据库

database/PowerEasy5.mdb

database/PowerEasy6.mdb

database/PowerEasy2005.mdb

database/PowerEasy2006.mdb

database/PE_Region.mdb

data/dvbbs7.mdb #动网论坛数据库

databackup/dvbbs7.mdb #动网论坛备份数据库

bbs/databackup/dvbbs7.mdb #动网论坛备份数据库

data/zm_marry.asp #动网sp2美化版数据库

databackup/dvbbs7.mdb

admin/data/qcdn_news.mdb #青创文章管理系统数据库

firend.mdb #交友中心数据库

database/newcloud6.mdb #新云管理系统6.0数据库

database/%23newasp.mdb #新云网站系统

blogdata/L-BLOG.mdb #L-BLOG v1.08数据库

blog/blogdata/L-BLOG.mdb #L-BLOG v1.08数据库

database/bbsxp.mdb #BBSXP论坛数据库

bbs/database/bbsxp.mdb #BBSXP论坛数据库

access/sf2.mdb #雪人论坛程序v2.0数据库

data/Leadbbs.mdb #LeadBBS论坛 v3.14数据库

bbs/Data/LeadBBS.mdb #LeadBBS论坛 v3.14数据库

bbs/access/sf2.mdb #雪人论坛程序v2.0数据库

fdnews.asp #六合专用BBS数据库

bbs/fdnews.asp #六合专用BBS数据库

admin/ydxzdate.asa #雨点下载系统 v2.0+sp1数据库

data/down.mdb #动感下载系统xp ver2.0数据库

data/db1.mdb #动感下载系统xp v1.3数据库

database/Database.mdb #轩溪下载系统 v3.1数据库

db/xzjddown.mdb #lhdownxp下载系统数据库

db/play.asp #娱乐先锋论坛 v3.0数据库

mdb.asp #惊云下载系统 v1.2数据库

admin/data/user.asp #惊云下载系统 v3.0数据库

data_jk/joekoe_data.asp #乔客6.0数据库

data/news3000.asp #沸腾展望新闻系统 v1.1数据库

data/appoen.mdb #惠信新闻系统4.0数据库

data/12912.asp #飞龙文章管理系统 v2.1数据库

database.asp #动感极品下载管理系统 v3.5

download.mdb #华仔软件下载管理系统 v2.3

dxxobbs/mdb/dxxobbs.mdb #dxxobbs论坛数据库

db/6k.asp #6kbbs 用户名:admin 密码:6kadmin

database/snowboy.mdb #雪孩论坛 默认后台admin/admin_index.asp

database/%23mmdata.mdb #依爽社区

editor/db/ewebeditor.mdbeWebEditor/db/ewebeditor.mdb

## 管理入口
admin

admin_index

admin_admin

index_admin

admin/index

admin/default

admin/manage

admin/login

manage_index

index_manage

superadmin

说明.txt

manager/login

manager/login.asp

manager/admin.asp

login/admin/admin.asp

houtai/admin.asp

guanli/admin.asp

denglu/admin.asp

admin_login/admin.asp

admin_login/login.asp

admin/manage/admin.asp

admin/manage/login.asp

admin/default/admin.asp

admin/default/login.asp

member/admin.asp

member/login.asp

administrator/admin.asp

administrator/login.asp

phpmyadmin

include/config.inc.php

include/config.php

lib/config.php
