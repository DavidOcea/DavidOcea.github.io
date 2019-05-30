---
layout: post
title: python环境管理--virtualenv
date: 2019-05-25
tags: python/闪马
---

## 介绍
	实际项目中可能遇到不同项目需要不同版本的python或者不同的框架，怎样方便快捷的管理自己的python环境十分重要，然而这里管理的工具包又是其中的重点。目前有很多方便快捷的python环境管理工具，如conda/docker/virtualenv.
	今天就简单介绍一下virtualenv,因为自己正在用，感觉挺方便的，所以记录下来，方便回顾。

### 目录
* [安装](#install of virtualenv)
* [基本应用](#base usage)

### <a name="install of virtualenv"></a>安装
* 安装：`pip install virtualenv`

### <a name="base usage"></a>基本应用
* 1.为一个工程创建一个虚拟环境：
```
$ cd my_project_dir
$ virtualenv venv　　#venv为虚拟环境目录名，目录名自定义
```

virtualenv venv 将会在当前的目录中创建一个文件夹，包含了Python可执行文件，以及 pip 库的一份拷贝，这样就能安装其他包了。虚拟环境的名字（此例中是 venv ）可以是任意的；若省略名字将会把文件均放在当前目录。

　　在任何你运行命令的目录中，这会创建Python的拷贝，并将之放在叫做 venv 的文件中。

	* 你可以选择使用一个Python解释器：
```
$ virtualenv -p /usr/bin/python2.7 venv　　　　# -p参数指定Python解释器程序路径
```
(关于本地python安装目录在个人应用管理日常笔记python中有）
这将会使用 /usr/bin/python2.7 中的Python解释器。

* 2.要开始使用虚拟环境，其需要被激活：

```
$ source venv/bin/activate　　　
```

从现在起，任何你使用pip安装的包将会放在 venv 文件夹中，与全局安装的Python隔绝开。

像平常一样安装包，比如：
```
$ pip install requests
```

* 3.如果你在虚拟环境中暂时完成了工作，则可以停用它：

```
$ . venv/bin/deactivate
```
这将会回到系统默认的Python解释器，包括已安装的库也会回到默认的。

要删除一个虚拟环境，只需删除它的文件夹。（执行 rm -rf venv ）。

这里virtualenv 有些不便，因为virtual的启动、停止脚本都在特定文件夹，可能一段时间后，你可能会有很多个虚拟环境散落在系统各处，你可能忘记它们的名字或者位置。

