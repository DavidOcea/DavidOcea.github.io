---
layout: post
title: Docker的应用笔记
date: 2019-06-26 
tags: 机器学习  
---

## 介绍
Docker是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的Linux机器上，也可以实现虚拟化，容器是完全使用沙箱机制，相互之间不会有任何接口。

Docker是世界领先的软件容器平台。开发人员利用Docker可以消除协作编码时“在我的机器上可正常工作”的问题。运维人员利用Docker可以在隔离容器中并行运行和管理应用，获得更好的计算密度。企业利用Docker可以构建敏捷的软件交付管道，以更快的速度、更高的安全性和可靠的信誉为Linux和Windows Server应用发布新功能。
<div align="center">
	<img src="/images/posts/Docker/fig0.png" height="400" width="650">  
</div> 

* Docker架构
<div align="center">
	<img src="/images/posts/Docker/fig0-1.png" height="400" width="650">  
</div> 

### 目录
* [Docker容器的创建、启动、和停止](#application of Docker)
* [Docker中容器的备份、恢复和迁移](#mv Docker)

### <a name="application of Docker"></a>Docker容器的创建、启动、和停止
* 容器是独立运行的一个或一组应用，及他们的运行环境。容器是Docker中的一个重要的概念。

#### `docker容器的启动有三种方式`
* `交互方式`，基于镜像新建容器并启动
例如我们可以启动一个容器，打印出当前的日历表
```
[root@rocketmq-nameserver4 ~]# docker run my/python:v1 cal ##my/python:v1fi为镜像名和标签
```
<div align="center">
	<img src="/images/posts/Docker/fig1-1.png" height="300" width="800">  
</div> 
我们还可以通过指定参数，启动一个bash交互终端。
```
[root@rocketmq-nameserver4 ~]# docker run -it my/python:v1 /bin/bash   #后面的/bin/bash不加也能进入
```
<div align="center">
	<img src="/images/posts/Docker/fig1-2.png" height="300" width="800">  
</div> 
参数-t让Docker分配一个伪终端并绑定在容器的标准输入上，-i让容器的标准输入保持打开。
使用docker run命令来启动容器，docker在后台运行的标准操作包括
1. 检查本地是否存在指定的镜像，不存在则从公有仓库下载
2. 使用镜像创建并启动容器
3. 分配一个文件系统，并在只读的镜像层外面挂载一层可读可写层
4. 从宿主主机配置的网桥接口中桥接一个虚拟接口道容器中去
5. 从地址池分配一个ip地址给容器
6. 执行用户指定的应用程序
7. 执行完毕之后容器被终止
<div align="center">
	<img src="/images/posts/Docker/fig1-3.png" height="300" width="800">  
</div> 
my/sinatra:v2基于training/sinatra镜像进行修改后的镜像，training/sinatra为公有仓库上的镜像。

* `短暂方式`，直接将一个已经终止的容器启动运行起来
可以使用docker start命令，直接将一个已经终止的容器启动运行起来。

```
[root@rocketmq-nameserver4 ~]# docker run my/python:v1 /bin/echo hello test 

>>>hello test
```
命令执行完，控制台会打印"hello test"，container就终止了，不过并没有消失，
可以用"docker ps -n 5 "看一下最新前5个的container，第一个就是刚刚执行过的container，可以再次执行一遍：docker start container_id

不过这次控制台看不到”hello test”了，只能看到ID，用logs命令才能看得到：docker logs container_id。
可以看到两个”hello test”了，因为这个container运行了两次。
<div align="center">
	<img src="/images/posts/Docker/fig1-4.png" height="300" width="800">  
</div> 

* `daemon方式`，守护态运行
即让软件作为长时间服务运行，这就是SAAS啊！

例如我们启动centos后台容器，每隔一秒打印当天的日历。
```
$ docker run -d centos /bin/sh -c "while true;do echo hello docker;sleep 1;done"
```

启动之后，我们使用docker ps -n 5查看容器的信息

要查看启动的centos容器中的输出，可以使用如下方式：
```
$ docker logs $CONTAINER_ID ##在container外面查看它的输出 
$ docker attach $CONTAINER_ID ##连接上容器实时查看：
```

#### `终止容器`
使用docker stop $CONTAINER_ID来终止一个运行中的容器。并且可以使用docker ps -a来看终止状态的容器。
<div align="center">
	<img src="/images/posts/Docker/fig1-5.png" height="300" width="800">  
</div> 

终止状态的容器，可以使用docker start来重新启动。
<div align="center">
	<img src="/images/posts/Docker/fig1-6.png" height="300" width="800">  
</div> 

使用docker restart命令来重启一个容器。
<div align="center">
	<img src="/images/posts/Docker/fig1-7.png" height="270" width="800">  
</div> 

### <a name="mv Docker"></a>Docker中容器的备份、恢复和迁移
#### `备份容器`
首先，为了备份Docker中的容器，我们会想看看我们想要备份的容器列表。要达成该目的，我们需要在我们运行着Docker引擎，并已创建了容器的Linux机器中运行 docker ps 命令。
```
# docker ps
```
<div align="center">
	<img src="/images/posts/Docker/fig2-1.png" height="100" width="800">  
</div> 
Docker Containers List
在此之后，我们要选择我们想要备份的容器，然后去创建该容器的快照。我们可以使用 docker commit 命令来创建快照。

```
# docker commit -p 30b8f18f20b4 container-backup
```
<div align="center">
	<img src="/images/posts/Docker/fig2-2.png" height="80" width="800">  
</div> 
Docker Commit
该命令会生成一个作为Docker镜像的容器快照，我们可以通过运行 docker images 命令来查看Docker镜像，如下。
```
# docker images
```
<div align="center">
	<img src="/images/posts/Docker/fig2-3.png" height="100" width="800">  
</div> 
Docker Images
正如我们所看见的，上面做的快照已经作为Docker镜像保存了。现在，为了备份该快照，我们有两个选择，一个是我们可以登录进Docker注册中心，并推送该镜像；另一个是我们可以将Docker镜像打包成tar包备份，以供今后使用。
如果我们想要在Docker注册中心上传或备份镜像，我们只需要运行 docker login 命令来登录进Docker注册中心，然后推送所需的镜像即可。
```
# docker login
```
<div align="center">
	<img src="/images/posts/Docker/fig2-4.png" height="130" width="800">  
</div> 
Docker Login
```
# docker tag a25ddfec4d2a arunpyasi/container-backup:test
# docker push arunpyasi/container-backup
```
<div align="center">
	<img src="/images/posts/Docker/fig2-5.png" height="300" width="800">  
</div> 
Docker Push
如果我们不想备份到docker注册中心，而是想要将此镜像保存在本地机器中，以供日后使用，那么我们可以将其作为tar包备份。要完成该操作，我们需要运行以下 docker save 命令。
```
# docker save -o ~/container-backup.tar container-backup
```
<div align="center">
	<img src="/images/posts/Docker/fig2-6.png" height="100" width="800">  
</div> 
taking tarball backup
要验证tar包是否已经生成，我们只需要在保存tar包的目录中运行 ls 命令即可。

#### `恢复容器`
接下来，在我们成功备份了我们的Docker容器后，我们现在来恢复这些制作了Docker镜像快照的容器。如果我们已经在注册中心推送了这些Docker镜像，那么我们仅仅需要把那个Docker镜像拖回并直接运行即可。
```
# docker pull arunpyasi/container-backup:test
```
<div align="center">
	<img src="/images/posts/Docker/fig2-7.png" height="300" width="800">  
</div> 
Docker Pull
但是，如果我们将这些Docker镜像作为tar包文件备份到了本地，那么我们只要使用 docker load 命令，后面加上tar包的备份路径，就可以加载该Docker镜像了。
```
# docker load -i ~/container-backup.tar
```
现在，为了确保这些Docker镜像已经加载成功，我们来运行 docker images 命令。

```
# docker images
```
在镜像被加载后，我们将用加载的镜像去运行Docker容器。

```
# docker run -d -p 80:80 container-backup
```
<div align="center">
	<img src="/images/posts/Docker/fig2-8.png" height="180" width="800">  
</div> 
Restoring Docker Tarball

#### `迁移Docker容器`
迁移容器同时涉及到了上面两个操作，备份和恢复。我们可以将任何一个Docker容器从一台机器迁移到另一台机器。在迁移过程中，首先我们将把容器 备份为Docker镜像快照。然后，该Docker镜像或者是被推送到了Docker注册中心，或者被作为tar包文件保存到了本地。如果我们将镜像推送 到了Docker注册中心，我们简单地从任何我们想要的机器上使用 docker run 命令来恢复并运行该容器。但是，如果我们将镜像打包成tar包备份到了本地，我们只需要拷贝或移动该镜像到我们想要的机器上，加载该镜像并运行需要的容器 即可。



