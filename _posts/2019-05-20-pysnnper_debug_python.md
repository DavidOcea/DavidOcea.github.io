---
layout: post
title: python调试器：PySnooper用法
date: 2019-05-20
tags: python/闪马
---

### 介绍
好记性不如烂笔头>一般情况下，在编写 Python 代码时，如果想弄清楚为什么 Python 代码没有按照预期执行的原因，比如你想知道哪些是正在运行，哪些没有运行，以及局部变量的值是什么...通常我们会使用包含断点和观察模式等功能成熟的调试器，或者直接使用 print 语句打印出来。
PySnooper允许你执行以上相同的操作，只需为要调试的函数添加一个装饰器即可，而不需要构建正确的 print 打印。你还将得到函数的详细日志，包括运行了哪些代码行、何时运行以及何时更改了局部变量。
### 安装及以应用
#### 安装
* 安装：安装及其简单
> pip install pysnooper

#### 应用
有两种方法应用：
* [直接装饰整个函数](#print function)
* [只输出部分内容](#print part)
### <a name="print function"></a>直接装饰整个函数
* 只需要通过添加`@pysnooper.snoop()`装饰器就可以了：

```python
import pysnooper
import random

@pysnooper.snoop()
def foo():
    lst = []
    for i in range(10):
        lst.append(random.randrange(1, 1000))
    txt = 'hello'
    lower = min(lst)
    upper = max(lst)
    mid = (lower + upper) / 2
    t = len(txt)
    print(lower, mid, upper)

foo()
```

输出如下：
```python
13:46:01.646519 call         6 def foo():
13:46:01.646726 line         7     lst = []
New var:....... lst = []
13:46:01.646787 line         8     for i in range(10):
New var:....... i = 0
13:46:01.646847 line         9         lst.append(random.randrange(1, 1000))
Modified var:.. lst = [550]
13:46:01.646964 line         8     for i in range(10):
Modified var:.. i = 1
13:46:01.647026 line         9         lst.append(random.randrange(1, 1000))
Modified var:.. lst = [550, 746]
13:46:01.647128 line         8     for i in range(10):
Modified var:.. i = 2
13:46:01.647228 line         9         lst.append(random.randrange(1, 1000))
Modified var:.. lst = [550, 746, 728]
13:46:01.647328 line         8     for i in range(10):
Modified var:.. i = 3
13:46:01.647422 line         9         lst.append(random.randrange(1, 1000))
Modified var:.. lst = [550, 746, 728, 535]
13:46:01.647536 line         8     for i in range(10):
Modified var:.. i = 4
13:46:01.647639 line         9         lst.append(random.randrange(1, 1000))
Modified var:.. lst = [550, 746, 728, 535, 687]
13:46:01.647747 line         8     for i in range(10):
Modified var:.. i = 5
13:46:01.647838 line         9         lst.append(random.randrange(1, 1000))
Modified var:.. lst = [550, 746, 728, 535, 687, 492]
13:46:01.647951 line         8     for i in range(10):
Modified var:.. i = 6
13:46:01.648063 line         9         lst.append(random.randrange(1, 1000))
Modified var:.. lst = [550, 746, 728, 535, 687, 492, ...]
13:46:01.648169 line         8     for i in range(10):
Modified var:.. i = 7
13:46:01.648260 line         9         lst.append(random.randrange(1, 1000))
13:46:01.648346 line         8     for i in range(10):
Modified var:.. i = 8
13:46:01.648446 line         9         lst.append(random.randrange(1, 1000))
13:46:01.648545 line         8     for i in range(10):
Modified var:.. i = 9
13:46:01.648645 line         9         lst.append(random.randrange(1, 1000))
13:46:01.648735 line         8     for i in range(10):
13:46:01.648810 line        10     txt = 'hello'
New var:....... txt = 'hello'
13:46:01.648906 line        11     with pysnooper.snoop('log.log'):
93 483.0 873
```
##### 特性：
* 1.可以通过自定义的方式将log输出到指定的文件：
```python
@pysnooper.snoop( /my/log/file.log )
```
* 2.查看一些非局部变量的变量值：
```python
@pysnooper.snoop(variables=( foo.bar ,  self.whatever ))
```
* 3.显示函数调用的函数的snoop行：
```python
@pysnooper.snoop(depth=2)
```
### <a name="print part"></a>只输出部分内容
当然，如果你不想输出整个函数，你可以选择只输出部分内容。
* 使用`with`模块：

```python
import pysnooper
import random

def foo():
    lst = []
    for i in range(10):
        lst.append(random.randrange(1, 1000))

    with pysnooper.snoop():
        lower = min(lst)
        upper = max(lst)
        mid = (lower + upper) / 2
        print(lower, mid, upper)

foo()
```

输出
```python
New var:....... i = 9
New var:....... lst = [681, 267, 74, 832, 284, 678, ...]
09:37:35.881721 line        10         lower = min(lst)
New var:....... lower = 74
09:37:35.882137 line        11         upper = max(lst)
New var:....... upper = 832
09:37:35.882304 line        12         mid = (lower + upper) / 2
74 453.0 832
New var:....... mid = 453.0
09:37:35.882486 line        13         print(lower, mid, upper)
```
* 注意：如果with模块报错，可能需要更新一下你的pysnooper到最新的版本。
> pip install -update pysnooper

### 官网地址
* [PySnooper 官方git地址](https://github.com/cool-RR/PySnooper)

