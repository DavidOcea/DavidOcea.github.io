---
layout: post
title: 踩坑宝典
date: 2019-07-22
tags: 闪马
---

### 20190712-python存储json中文乱码

```python
增加参数：zensure_ascii=False，eg:json.dumps(l_json, ensure_ascii=False)
```

### 20190712-python模型存储,net.load_state_dict(pre_model),碰到键值不完全匹配时无法导入	

```python
增加参数：False，net.load_state_dict(pre_model,False)，False表示允许键值不完全匹配，默认为true。
```

### 20190723-python 实现人工花屏效果（高级马赛克）

```python
import cv2
import random
 
#花屏效果
def flower_screen(img, rect):
    '''
    ran 打点的数量，i,j 范围
    '''
    outimg= img.copy()
    for i in range(0,rect[3],10):
        for j in range(0,rect[2],10):
            ran = random.random()
            if ran <=0.6 and i>15 and j>15:
                hash=outimg[i,j]#记录中心值
                width = random.randint(0,10)
                height = random.randint(0,15)
 
                for x in range(i-height,i+height):
                    for y in range(j-width,j+width):
                        outimg[x,y]= hash
 
    return outimg
 
img = cv2.imread('11.png')
height, width, _ =img.shape
rect = [0,0,width-10,height-20]
outimg = flower_screen(img,rect)
cv2.imwrite('12.png',outimg)

```

### 20190802- pytorch 模型加载要取module
* net.load_state_dict(pre_model,False)#,False：表示加载过程中允许忽略不同

```python
for k, v in pre_model['state_dict'].items():
        head = k[:7]
        if head == 'module.':
            name = k[7:] # remove `module.`
        else:
            name = k
        new_state_dict[name] = v
```

### 20190802-pytorch 中view()和expend_as()的区别

* view重新定义行列，要求目标定以后的col*raw和定义前的col*raw相等；`eg:2*1=1*2`
* expend_ad扩增，要求目标要有col或者raw=1，重复元素来达到指定的行列，且每次只能扩行或者列。`eg：2*1，2不变，1的内容重复来达到2*8`

```python
import torch
 
x = torch.randn(2, 1)
y = torch.randn(2,8)
 
print(x)
print(x.view(1,2))
print(x.expand_as(y))
 
》》》》》》
 
tensor([[-0.8176],
[ 0.4311]])
tensor([[-0.8176, 0.4311]])
tensor([[-0.8176, -0.8176, -0.8176, -0.8176, -0.8176, -0.8176, -0.8176, -0.8176],
[ 0.4311, 0.4311, 0.4311, 0.4311, 0.4311, 0.4311, 0.4311, 0.4311]])
[Finished in 0.3s]
```

### 20190827 解决报错 THCudaCheck FAIL file=/pytorch/aten/src/THC/THCGeneral.cpp line=405 error=11 : invalid argument

原因是显卡用的RTX 2080Ti，CUDA就要装10以上，这个时候pytorch不能直接用pip装，要这样：
```python
pip3 install https://download.pytorch.org/whl/cu100/torch-1.0.0-cp36-cp36m-linux_x86_64.whl
```

### 问题：ImportError: No module named google.protobuf.internal

```
# 解决办法1
sudo pip install protobuf
```

### 问题：ImportError: No module named cv2 报错处理

```
pip install opencv-python
```
