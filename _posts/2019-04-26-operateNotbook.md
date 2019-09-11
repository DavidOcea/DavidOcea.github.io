---
layout: post
title: 应用管理（日常命令）
date: 2019-04-26
tag: 闪马
---
# 应用管理（日常命令）：
### 1.mac brew使用：
* .安装:
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" `
* .brew:安装文件命令：
`brew install xxx`:安装 `brew uninstall xxx`:卸载

### 2.python便捷操作
* .根据配置文件快速更新pip包：
`pip install -r request.txt -i https://pypi.tuna.tsinghua.edu.cn/simple`
* .查看python安装路径：
   `（whereis)which python`
* .查看系统里过期的python库:
   `pip list`  #列出所有安装的库
  `pip list --outdated` #列出所有过期的库
* .过期库更新：
  `pip install --upgrade 库名` 
* .python复制文件
```python
from shutil import copyfile
copyfile(src, dst)
#src:源文件
#dst：目标文件
```
* .python进度条
```python
from tqdm import tqdm
for i in tqdm(range(1000)):   #tqdm(iterator),里面可以是任何可迭代对象
	pass  
>>>100%|███████████████████████████████████| 857K/857K [00:04<00:00, 246Kloc/s]
```

### 3.Linux常用命令：
* .tar 解包: (前面+z是达成zip包，不加是打包）
   `tar xvf filename.tar`
* .tar 打包: 
   `tar cvf filename.tar dirname`
* .安装一个新软件包：
   `apt-get install packagename` 
* .卸载一个已安装的软件包（保留配置文档）：
   `apt-get remove packagename`
* .卸载一个已安装的软件包（删除配置文档）：
   `apt-get remove --purge packagename` 
* .删除包及其依赖的软件包：
   `apt-get autoremove packagename` 
* .删除包及其依赖的软件包+配置文件，比上面的要删除的彻底一点：
   `apt-get autoremove --purge packagname`
* .有些软件很难卸载，而且还阻止了别的软件的应用，就能够用这个，但是有点冒险：    `dpkg --force-all --purge packagename `
* .linux 查看某个文件家中文件大小：
   `du -sh *`
* .linux 查看文件夹下文件个数：
   `ls -l|grep "^-"| wc -l`(其中双引号要改成英文的，不然会出错）
* .linux 查看某文件夹下头10个文件：
   `ls|head -n 10`
* .把一个文件复制到另一个文件夹：(注意后面的`.`，表示包括隐藏文件，`*`表示不包括隐藏文件的字文件，如果不加，表示连上级文件夹`dir1/`一起复制过去:
  `cp -r dir1/. dir2`
* .检查linux服务器的文件系统的磁盘空间占用情况:
  `df [operate][file]`  常用命令 `df -h`(宜读模式)
* 查找文件:根据名称查找文件
  `find / -name fileneme`
* 查看一个进程是否运行:查看所有关于tomcat的进程
  `ps -ef|grep tomcat`
* 查看文件，包括隐藏文件
  `ls -al`
* 修改文件权限
  `chmod 777 file.java`   //file.java的权限-rwxrwxrwx，r表示读、w表示写、x表示可执行
* 查看系统当前时间
  `date`  命令会输出 周几 几月 几日 时间 和 时间显示格式 和年份 `Sat Jan 20 04:39:49 CST 2018`;`date +"%Y-%m-%d"`显示如下：`[root@ming xxx]# date +"%Y-%m-%d" 2018-01-20`
* 查看线程个数（方便查看程序是否有误）
  `ps -Lf 端口号|wc -l`

### 4.git常用操作：（前5个是上传下载一般流程）
* .查看当前git仓库状态：
   `git status`
* .更新git状态：
   `git add *` (.)(-u修改过到)
   `git commit -m ‘fimename’`（fimename指本次上传到更新内容描述）
* .拉取当前分之最新代码：
   `git pull (git地址+master`
* .push到远程分之上：
   `git push origin master`(从指定（可以是别人的）分之下载：git pull zzj dev) 其中master是分支名
* .下载项目：
   `git clone +(git地址)`
* .产看本地git:
   `git remote -v`
* .查看git变化的详细情况：
   `git diff`
* .查看分之状况：
  `git branch -a`
* .相关知识：
  `https://blog.csdn.net/u013374164/article/details/78644576`[跳转](https://blog.csdn.net/u013374164/article/details/78644576)

### 5.vim常用操作：
* .使vim带格式粘贴：
   `:set paste`
* .vim复制粘贴：
    命令形式下：y（复制），p（粘贴）
* .vim撤销和取消撤销
    命令形式下：u（撤销），cotr+r（恢复） 

### 6.通用命令：
* .下载网页链接内容到本地只wget：
   `wget 'url'`
* .后台运行脚本：
   `nohup python -u test.py > out.log 2>&1 &`
* .查看文件后几行：（其中-n参数可以控制行，`tail -n 10 fime`(指后10行）head用法和tail差不多，是看前几行的）
   `tail -f (-f指实时追踪`）
* .查看gpu使用情况（安装naidia-smi):
   `nvidia-smi`
* .查看所有进程：(装有htop到话用htop命令）
   `ps ax`
* .jekyll sever 本地启动，（进入blog目录）
  `bundle exec jekyll serve`

### 7拓展-wget:
#### >目录
* [用Wget-O下载可以为下载的文件指定另外一个名字](#wget-O) 
* [用Wget -–limit-rate指定下载的速度](#wget-limit-rate)
* [续传下载用Wget -c](#wget-c)
* [后台下载用wget -b](#wget-b)
* [测试你要下载的地址用Wget –spider](#wget-spider)
* [增加重连次数用Wget -tries](#wget-tries)
* [下载多个文件/URLS用wget -i](#wget-i)
* [下载一个完整的网站用wget -mirror](#wget-mirror)
* [保存输出到日志文件而不是标准输出用wget -o(注意区别于下载重命名时用大些的O)
](#wget-o)
* [当超过指定大小时终止下载用wget -Q](#wget-Q)
* [下载特定文件类型的文件用wget -r -A](#wget-r-A)
* [指定不下载某一类型的文件用wget –reject](#wget-reject)
* [用wget实现FTP下载](#FTP)
* [wget下载有的资源时必须用选项 –no-check-certificate，否则会提示没有认证不允许下载](#wget-on-check)

#### >内容

* <a name='wget-O'></a>用Wget-O下载可以为下载的文件指定另外一个名字

默认情况下wget会用最后的斜线后面的所有字符来命名下载下来的文件，如上例所示保存的文件名为

```
Saving to: `epp331.exe@token=1329413178_4553efa847829f3ecef10c1bc256fcc0'
```

这不是我们所想要的，我们可以用-O选项来改变将文件保存为editplus.exe

```
D:\Hack stuff\wget>wget -O editplus.exe http://software-files-a.cnet.com/s/software/12/32/81/47/epp331.exe?token=1329413178_4553efa847829f3ecef10c1bc256fcc0&lop=link&ptype=3001&ontid=2352&siteId=4&edId=3&spi=537d5d5485f688682d82c481c4fb15a1&pid=12328147&psid=10018241&&fileName=epp331.exe
```

* <a name='wget-limit-rate'></a>用Wget--limit-rate指定下载的速度

如下面这个例子限制速度为300k

```
D:\Hack stuff\wget>wget --limit-rate=300k http://downloads.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf.zip?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fboost%2F&ts=1329379231&use_mirror=nchc
```

* 续<a name='wget-c'></a>传下载用Wget -c

当你在下载一个大文件时突然中断了那么这个选项就派上用场了

```
D:\Hack stuff\wget>wget -c http://downloads.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf.zip?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fboost%2F&ts=1329379231&use_mirror=nchc
```

* <a name='wget-b'></a>后台下载用wget -b

用此选项下载时只会初始化下载而不会显示相关信息

```
D:\Hack stuff\wget>wget -b http://downloads.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf.zip?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fboost%2F&ts=1329379231&use_mirror=nchc
Continuing in background, pid 6132.
Output will be written to `wget-log'.
```

下载以后会在wget目录下生产wget-log文件，用记事本打开可查看里面的内容如下所示

```
--2012-02-16 16:12:55--  http://downloads.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf.zip?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fboost%2FResolving downloads.sourceforge.net... 216.34.181.59
Connecting to downloads.sourceforge.net|216.34.181.59|:80... connected.
HTTP request sent, awaiting response... 302 Found
Location: http://nchc.dl.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf.zip [following]
--2012-02-16 16:12:56--  http://nchc.dl.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf.zip
Resolving nchc.dl.sourceforge.net... 211.79.60.17
Connecting to nchc.dl.sourceforge.net|211.79.60.17|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31421410 (30M) [application/zip]
Saving to: `boost_1_47_pdf.zip.4'

     0K .......... .......... .......... .......... ..........  0% 19.7K 25m51s
    50K .......... .......... .......... .......... ..........  0% 29.1K 21m40s
   100K .......... .......... .......... .......... ..........  0% 20.8K 22m35s
   150K .......... .......... .......... .......... ..........  0% 19.5K 23m26s
   200K .......... .......... .......... .......... ..........  0% 18.4K 24m13s
   250K .......... .......... .......... .......... ..........  0% 20.8K 24m13s
   300K .......... .......... .......... .......... ..........  1% 18.2K 24m41s
   350K .......... .......... .......... .......... ..........  1% 23.5K 24m16s
```

* <a name='wget-spider'></a>测试你要下载的地址用Wget --spider

```
wget --spider DOWNLOAD-URL
```

如果所给URL是正确的则会显示

```
Resolving downloads.sourceforge.net... 216.34.181.59
Connecting to downloads.sourceforge.net|216.34.181.59|:80... connected.
HTTP request sent, awaiting response... 302 Found
Location: http://ncu.dl.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf.zip [following]
Spider mode enabled. Check if remote file exists.
--2012-02-16 16:21:08--  http://ncu.dl.sourceforge.net/project/boost/boost-docs/
1.47.0/boost_1_47_pdf.zip
Resolving ncu.dl.sourceforge.net... 140.115.17.45
Connecting to ncu.dl.sourceforge.net|140.115.17.45|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 31421410 (30M) [application/zip]
Remote file exists.
```

否则显示

```
Spider mode enabled. Check if remote file exists.
--2012-02-16 16:23:06--  http://downloads.sourceforge.net/project/boost/boost-docs/1.47.0/boost_1_47_pdf222.zip?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fboost%2F
Resolving downloads.sourceforge.net... 216.34.181.59
Connecting to downloads.sourceforge.net|216.34.181.59|:80... connected.
HTTP request sent, awaiting response... 404 Not Found
Remote file does not exist -- broken link!!!
```

* <a name='wget-tries'></a>增加重连次数用Wget -tries

在网络有问题的情况次选项尤其有用，默认是wget会重连20次以成功完成下载，我们可以把他增加为我们期待的次数

```
wget --tries=100 DOWNLOAD-URL
```

* <a name='wget-i'></a>下载多个文件/URLS用wget -i

首先把所有要下载的文件或者URL存到一个记事本中，比如aa.txt，里面内容如下

```
URL1
URL2
URL3
URL4
```

接下来输入如下代码就可以批量下载了

```
wget -i aa.txt
```

* <a name='wget-mirror'></a>下载一个完整的网站用wget -mirror

以下实现是你想完整的下载一个网站用于本地浏览

```
wget --mirror  -p --convert-links -P LOCAL-DIR WEBSITE-URL
```

--mirror:打开镜像选项
-p:下载所有用于显示给定网址所必须的文件
--convert-links：下载以后，转换链接用于本地显示
-P LOCAL_DIR：保存所有的文件或目录到指定的目录下

* <a name='wget-o'></a>保存输出到日志文件而不是标准输出用wget -o(注意区别于下载重命名时用大些的O)

当你想要把信息保存到一个文件而不是在终端显示时用以下代码。

```
wget -o download.log DOWNLOAD-URL
```

* <a name='wget-Q'></a>当超过指定大小时终止下载用wget -Q

当文件已下载10M，此时你想停止下载可以使用下面的命令行

```
wget -Q10m -i FILE-WHICH-HAS-URLS
```

`注意：此选项只能在下载多个文件时有用，当你下载一个文件时没用。`

* <a name='wget-r-A'></a>下载特定文件类型的文件用wget -r -A

你可以用此方法下载一下文件：
~从一个网站下载所有图片
~从一个网站下载所有视频
~从一个网站下载所有PDF文件

```
wget -r -A.pdf http://url-to-webpage-with-pdfs/
```

* <a name='wget-reject'></a>指定不下载某一类型的文件用wget --reject

你发现一个网站很有用，但是你不想下载上面的图片，因为太占流量，此时你可以用如下命令。

```
wget --reject=gif WEBSITE-TO-BE-DOWNLOADED
```

* <a name='FTP'></a>用wget实现FTP下载

匿名FTP下载用

```
wget ftp-url
```

有用户名和密码的FTP下载

```
wget --ftp-user=USERNAME --ftp-password=PASSWORD DOWNLOAD-URL
```

* <a name='wget-on-check'></a>wget下载有的资源时必须用选项 --no-check-certificate，否则会提示没有认证不允许下载

```
wget --no-check-certificate URL
```

