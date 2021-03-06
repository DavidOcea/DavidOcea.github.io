---
layout: post
title: (奇技淫巧)python调试器：pdb用法
date: 2019-05-20
description: "基础操作手册"
tag: python/闪马
---

### 说明
虽然都是基础知识，但是好记性不如烂笔头，记录下来方便自己经常温故

### 目录
python pdb调试有两种方式：一种通过命令`python -m pdb xxx.py` 启动脚本，进入单步执行模式；另一种通过`import pdb` 之后，直接在代码里需要调试的地方放一个`pdb.set_trace()`，就可以设置一个断点。
* [通过命令python -m pdb xxx.py 启动脚本调试](#debug by python)
* [通过import pdb之后调试](#debug by import)

### <a name="debug by python"></a>通过python直接调试
#### 基本命令：

> 1）进入命令行Debug模式，python -m pdb xxx.py

> 2）h：（help）帮助

> 3）w：（where）打印当前执行堆栈

> 4）d：（down）执行跳转到在当前堆栈的深一层（个人没觉得有什么用处）

> 5）u：（up）执行跳转到当前堆栈的上一层

> 6）b：（break）添加断点
> * b 列出当前所有断点，和断点执行到统计次数
> * b line_no：当前脚本的line_no行添加断点
> * b filename:line_no：脚本filename的line_no行添加断点
> * b function：在函数function的第一条可执行语句处添加断点

> 7）tbreak：（temporary break）临时断点
> * 在第一次执行到这个断点之后，就自动删除这个断点，用法和b一样

> 8）cl：（clear）清除断点
> * cl 清除所有断点
> * cl bpnumber1 bpnumber2... 清除断点号为bpnumber1,bpnumber2...的断点
> * cl line_no 清除当前脚本lineno行的断点
> * cl filename:line_no 清除脚本filename的line_no行的断点

> 9）disable：停用断点，参数为bpnumber，和cl的区别是，断点依然存在，只是不启用

> 10）enable：激活断点，参数为bpnumber

> 11）s：（step）执行下一条命令
> * 如果本句是函数调用，则s会执行到函数的第一句(可以理解为进入函数体)

> 12）n：（next）执行下一条语句
> * 如果本句是函数调用，则执行函数，接着执行当前执行语句的下一条

> 13）r：（return）执行当前运行函数到结束

> 14）c：（continue）继续执行，直到遇到下一条断点

> 15）l：（list）列出源码
> * l 列出当前执行语句周围11条代码
> * l first 列出first行周围11条代码
> * l first second 列出first--second范围的代码，如果second'<'first，second将被解析为行数

> 16）a：（args）列出当前执行函数的函数

> 17）p expression：（print）输出expression的值

> 18）pp expression：好看一点的p expression

> 19）run：重新启动debug，相当于restart

> 20）q：（quit）退出debug

> 21）j lineno：（jump）设置下条执行的语句函数
> * 只能在堆栈的最底层跳转，向后重新执行，向前可直接执行到行号

> 22）unt：（until）执行到下一行（跳出循环），或者当前堆栈结束

> 23）condition bpnumber conditon，给断点设置条件，当参数condition返回True的时候bpnumber断点有效，否则bpnumber断点无效
>
#### 注意：
* 直接输入Enter，会执行上一条命令；
* 输入PDB不认识的命令，PDB会把他当做Python语句在当前环境下执行；

### <a name="debug by import"></a>通过import pdb后调试
#### 基本操作
* 直接修改xxx.py文件，添加`import pdb`之后，直接在代码里需要调试的地方放一个`pdb.set_trace()`，就可以设置一个断点， 程序会在`pdb.set_trace()`暂停并进入pdb调试环境，可以用pdb 变量名查看变量，或者c继续运行。
* 基本命令和注意事项同上⬆️

