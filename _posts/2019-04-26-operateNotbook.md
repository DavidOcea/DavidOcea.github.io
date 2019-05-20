---
layout: post
title: 应用管理（日常命令）
date: 2019-04-26
tag: 闪马
---
# 应用管理（日常命令）：
### 1.mac brew使用：
* .安装:
    /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
* .brew:安装文件命令：
    brew install xxx:安装；brew uninstall xxx

### 2.python便捷操作
* .根据配置文件快速更新pip包：
    pip install -r request.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
* .查看python安装路径：
  （whereis)which python
* .查看系统里过期的python库:
  pip list  #列出所有安装的库
  pip list --outdated #列出所有过期的库
* .过期库更新：
  pip install --upgrade 库名 

### 3.Linux常用命令：
* .tar 解包: (前面+z是达成zip包，不加是打包）
    tar xvf filename.tar
* .tar 打包: 
    tar cvf filename.tar dirname
* .安装一个新软件包：
    apt-get install packagename  
* .卸载一个已安装的软件包（保留配置文档）：
    apt-get remove packagename 
* .卸载一个已安装的软件包（删除配置文档）：
    apt-get remove --purge packagename 
* .删除包及其依赖的软件包：
    apt-get autoremove packagename 
* .删除包及其依赖的软件包+配置文件，比上面的要删除的彻底一点：
    apt-get autoremove --purge packagname 
* .有些软件很难卸载，而且还阻止了别的软件的应用，就能够用这个，但是有点冒险：    dpkg --force-all --purge packagename 

### 4.git常用操作：（前5个是上传下载一般流程）
* .查看当前git仓库状态：
    git status
* .更新git状态：
    git add * (.)(-u修改过到)
    git commit -m ‘fimename’ （fimename指本次上传到更新内容描述）
* .拉取当前分之最新代码：
    git pull (git地址+master）
* .push到远程分之上：
    git push origin master (从指定（可以是别人的）分之下载：git pull zzj dev) 其中master是分支名
* .下载项目：
    git clone +(git地址)
* .产看本地git:
    git remote -v
* .查看git变化的详细情况：
    git diff
    
* .查看分之状况：
  git branch -a
  
    
* .相关知识：
  https://blog.csdn.net/u013374164/article/details/78644576

### 5.vim常用操作：
* .使vim带格式粘贴：
    :set paste
* .vim复制粘贴：
    命令形式下：y（复制），p（粘贴）
* .vim撤销和取消撤销
    命令形式下：u（撤销），cotr+r（恢复） 

### 6.通用命令：
* .下载网页链接内容到本地只wget：
    wget 'url'
* .后台运行脚本：
    nohup python -u test.py > out.log 2>&1 &
* .查看文件后几行：（其中-n参数可以控制行，tail -n 10 fime(指后10行）head用法和tail差不多，是看前几行的）
    tail -f (-f指实时追踪）
* .查看gpu使用情况（安装naidia-smi):
    nvidia-smi
* .查看所有进程：(装有htop到话用htop命令）
    ps ax
* .jekyll sever 本地启动，（进入blog目录）
   bundle exec jekyll serve





