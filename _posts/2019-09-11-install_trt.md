---
layout: post
title: Ubuntu16.04 下 tensorRT安装
date: 2019-09-11 
tags: 深度学习  
---

### 环境准备
主要是根据工程环境需要，参考trt文档安装trt
* 1.查看trt适配情况 ：[链接跳转](https://docs.nvidia.com/deeplearning/sdk/tensorrt-support-matrix/index.html)，注意，不同版本的trt有不同版本的文档，请以最新文档为准。

<div align="center">
	<img src="/images/posts/TRT/1-1.png" height="180" width="900">  
</div> 

* 2.根据自己系统情况下载相关包
因为我自己拉的docker镜像是ubuntu16.04，CUDA10的驱动，所以我还需要下载cuDNN和TensorRT。

### 目录
* [cuDNN安装](#install cudnn)
* [tensorRT安装](#install tensorrt)
* [可能会遇到的问题](#mabe problem)

### <a name='install cudnn'></a>cuDNN安装

* 去官网下载合适版本的cuDNN ==> 解压 ==> 复制相关文件到系统的CUDA目录 

下载完安装包后如下：
```
root@iZbp120zfnu5353tdx5hq7Z:/data/workspace/docker/trtfile/test# ls
cudnn-10.0-linux-x64-v7.6.3.30.tgz
```

解压后如下：
```
root@iZbp120zfnu5353tdx5hq7Z:/data/workspace/docker/trtfile/test# tar xvf cudnn-10.0-linux-x64-v7.6.3.30.tgz
cudnn-10.0-linux-x64-v7.6.3.30.tgz       cudnn
```

复制cudnn中相关文件到系统CUDA相关目录中
```
sudo cp cuda/include/* /usr/local/cuda/include/
sudo cp cuda/lib64/* /usr/local/cuda/lib64/
```

上面完成后可能需要添加权限：（可以不做，一般原来就是可执行的）
```
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

查看cudnn版本是否安装好(显示如下则成功）:
```
root@3792e2c3dbce:/dacker/data/workspace/docker/torch2trt# cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
#define CUDNN_MAJOR 7
#define CUDNN_MINOR 6
#define CUDNN_PATCHLEVEL 3
--
#define CUDNN_VERSION (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

#include "driver_types.h"
```

### <a name='install tensorrt'>tensorRT安装</a>
* 去官网下载安装包 ==> 解压 ==> 进入到解压目录中的python目录，pip安装tensorrt ==> 配置环境变量（linux一般修改.bashrc文件即可）

下载完安装包并解压后如下：
```
root@iZbp120zfnu5353tdx5hq7Z:/data/workspace/docker/trtfile# ls
cuda  cudnn-10.0-linux-x64-v7.6.3.30.tgz  TensorRT-5.1.5.0  TensorRT-5.1.5.0.Ubuntu-16.04.5.x86_64-gnu.cuda-10.0.cudnn7.5.tar.gz
```

进入到解压目录中安装（根据你系统的版本安装相应的版本）
```
root@iZbp120zfnu5353tdx5hq7Z:/data/workspace/docker/trtfile/TensorRT-5.1.5.0/python# ls
tensorrt-5.1.5.0-cp27-none-linux_x86_64.whl  tensorrt-5.1.5.0-cp35-none-linux_x86_64.whl  tensorrt-5.1.5.0-cp37-none-linux_x86_64.whl
tensorrt-5.1.5.0-cp34-none-linux_x86_64.whl  tensorrt-5.1.5.0-cp36-none-linux_x86_64.whl

root@iZbp120zfnu5353tdx5hq7Z:/data/workspace/docker/trtfile/TensorRT-5.1.5.0/python# pip install tensorrt-5.1.5.0-cp36-none-linux_x86_64.whl

```

配置环境变量：

```
$ vim ~/.bashrc # 打开环境变量文件
# 将下面环境变量写入环境变量文件并保存
export LD_LIBRARY_PATH=TensorRT解压路径/lib:$LD_LIBRARY_PATH
# 使刚刚修改的环境变量文件生效
$ source ~/.bashrc
```

```
#当cuda环境没有指定时，也需要指定
export CUDA_INSTALL_DIR=/usr/local/cuda-9.0
export CUDNN_INSTALL_DIR=/usr/local/cuda-9.0
```

测试TensorRT 是否安装成功，进入python编辑器加载tensorrt:
```
>>>import tensorrt
```

### <a name='mabe problem'></a>可能会遇到的问题

```
Traceback (most recent call last):
  File "test.py", line 3, in <module>
    import torch2trt
  File "/dacker/data/workspace/docker/torch2trt/torch2trt/__init__.py", line 1, in <module>
    from . import core, handlers
  File "/dacker/data/workspace/docker/torch2trt/torch2trt/core.py", line 12, in <module>
    import tensorrt as trt
  File "/dacker/data/workspace/docker/evenv_docker/lib/python3.6/site-packages/tensorrt/__init__.py", line 1, in <module>
    from .tensorrt import *
ImportError: libcudnn.so.7: cannot open shared object file: No such file or directory
```

这个问题根据报错情况和cudnn有关，首先排查cudnn是否安装正确（排查方法安装中有）如果有问题，重装一遍，如果没有问题，检查环境变量配置，将安装过程中的环境变量配置完整，即可。（以为cuda安装环境的问题，有时也会报此类错，如果以上两点没问题，需要添加库及打布丁）

```
#打开bashrc (打不开请用sudo,有些环境需要sudo才能打开）
vim ~/.bashrc

#在里面添加路径(也可指定cuda版本）
export PATH="/usr/local/cuda/bin:$PATH"
export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"

#使改变生效
source .bashrc #此时终端在用户名目录下 ~

#检查是否添加成功（下面两条语句会打印出刚才添加的内容）
echo $PATH
echo $LD_LIBRARY_PATH

```

### 补充
英伟达官网：https://developer.nvidia.com/[跳转](https://developer.nvidia.com/)


