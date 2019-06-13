---
layout: post
title: (杂谈)优化python-加速
date: 2019-06-13 
tags: python/闪马  
---

## 介绍

作为一名优秀的程序员，不光要会写代码，好要保证优雅，高效。下面就是给python代码提速的九法。文章内容来源网络采集！

### 目录

* [分析代码运行时间-辅助工作](#analyze code time)
* [一、加速查找](#speedup finding)
* [二、加速循环](#speedup circulation)
* [三、加速函数](#speedup function)
* [四、使用标准库函数加速](#speedup by using standard lib)
* [五、使用高阶函数加速](#speedup by heigh order function)
* [六、使用numpy向量化加速](#speedup by numpy)
* [七、加速pandas](#speedup pandas)
* [八、使用Dask加速](#speedup by dask)
* [九、应用多线程多进程加速](#speedup by multithreading)
* [关于原文](#about origin)

### <a name="analyze code time"></a>分析代码运行时间-辅助工作

* 测算代码运行时间

普通方法

```python
import time
tic = time.time()
much_job = [x**2 for x in range(1,1000000,3)]
toc = time.time()
print('used {:.5}s'.format(toc-tic))
```

```
used 0.18965s
```

快捷方法（jupyter环境）

```python
%%time
much_job = [x**2 for x in range(1,1000000,3)]
```

```
CPU time:user 108 ms, sys:16 ms, total: 124ms
Wall time:159ms
```

* 测算代码多次运行平均时间

```python
from timeit import timeit
g = lambda x:x**2+1
def main():
	return(g(2)**120)

#timeit('main()',setup = 'from __main__ import main',number = 10)
timeit('main()',globals = {'main':main},number = 10)
```

```
1.482898369314957e-05
```

* 按调用函数分析代码运行时间

```python
def relu(x):
	return(x if x>0 else 0)
def main():
	result = [relu(x) for x in range(-100000,100000,1)]
	return result

import profile
profile.run('main()')
```

```
200006 function calls in 1.783 seconds
Orderde by: standard name
ncalls tottime percall cumtime percall filename:Lineno(function)
     1   0.000   0.000  1.783   1.783  :0(exec)
     1   0.000   0.000  0.000   0.000  :0(setprofile)
200000   0.868   0.000  0.868   1.781  <ipython-input-17-77436057cf53>:1(relu)
```

* 按行分析代码运行时间

```
!pip install line_profiler
%load_ext line_profiler
```

```python
def relu(x):
	return(x if x>0 else 0)
def main():
	result = [relu(x) for x in range(-100000,100000,1)]
	return result

from line_profiler import LineProfiler
lprofile = LineProfiler(main,relu)
lprofile.run('main()')
lprofile.print_stats()
```

```
Timer unit:1e-06 s
Total time:0.091906 s
File: <ipython-input-28-93de8f193d63>
Function:relu ad line 1

Line #    Hits    Time  Per Hit  % Time Line Contents
=====================================================
    1                                         def relu(x):
    2    200000  91906.0     0.5  100.0           return(x if x>0 else 0)
```

### <a name="speedup finding"></a>一、加速查找

* 用set而非list进行查找

```python
data = (i**2 + 1 for i in range(1000000))
list_data = list(data)  
set_data = set(data)    
```

```python
%%time
100 in list_data #低速
```

```
CPU times: user 16.2 ms, sys: 0 ns, total: 16.2 ms
Wall time: 16.1 ms
```

```python
%%time
100 in set_data  #高速
```

```
CPU times: user 4 µs, sys: 0 ns, total: 4 µs
Wall time: 7.15 µs
```

* 用dict而非两个list进行匹配查找

```python
list_a = [2*i-1 for i in range(1000000)]
list_b = [i**2 for i in list_a]
dict_ab = dict(zip(list_a,list_b))
```

```python
%%time
print(list_b[list_a.index(8765)]) #低速
```

```
75463969
CPU times: user 156 µs, sys: 0 ns, total: 156 µs
Wall time: 146 µs
```

```python
%%time
print(dict_ab.get(8687,None))  #高速
```

```
75463969
CPU times: user 0 ns, sys: 69 µs, total: 69 µs
Wall time: 61 µs
```

### <a name="speedup circulation"></a>二、加速循环

* 优先使用for循环而不是while循环

```python
#低速
%%time
s,i = 0,0
while i<10000:
	i = i + 1
	s = s + i
print(s)
```

```
50005000
CPU times: user 2.44 ms, sys: 0 ns, total: 2.44 ms
Wall time: 2.47 ms
```

```python
#高速
%%time
s = 0
for i in range(1,10001):
	s = s + i
print(s)
```

```
50005000
CPU times: user 1.46 ms, sys: 0 ns, total: 1.46 ms
Wall time: 1.45 ms
```

* 在循环体中避免重复计算

```python
#低速
%%time
a = [i**2+1 for i in range(2000)]
b = [i/sum(a) for i in a]
```

```
CPU times: user 39.8 ms, sys: 0 ns, total: 39.8 ms
Wall time: 39 ms
```

```python
#高速
%%time
sum_a = sum(a)
b = [i/sum_a for i in a]
```

```
CPU times: user 1.03 ms, sys: 0 ns, total: 1.03 ms
Wall time: 1.04 ms
```

### <a name="speedup function"></a>三、加速函数

* 用循环机制代替递归函数(⚠️这个起始本人并不是很推荐，有时候代码简介也很重要）

```python
#低速
%%time
def fib(n):
	return(1 if n in (1,2) else fib(n-1)+fib(n-2))
print(fib(30))
```

```
832040
CPU times: user 322 ms, sys: 24.2 ms, total: 346 ms
Wall time: 353 ms
```

```python
#高速
%%time
def fib(n):
	if n in (1,2):
		return(1)
	a,b = 1,1
	for i in range(2,n):
		a,b = b,a+b
	return(b)
print(fib(30))
```

```
832040
CPU times: user 61 µs, sys: 16 µs, total: 77 µs
Wall time: 68.9 µs
```

* 用缓存机制加速递归（推荐）

```python
#低速
%%time
def fib(n):
	return(1 if n in (1,2) else fib(n-1)+fib(n-2))
print(fib(30))
```

```
832040
CPU times: user 322 ms, sys: 24.2 ms, total: 346 ms
Wall time: 353 ms
```

```python
#高速
%%time
from functools import lru_cache

@lru_cache(100)
def fib(n):
	return(1 if n in (1,2) else fib(n-1)+fib(n-2))
print(fib(30))
```

```
832040
CPU times: user 645 µs, sys: 0 ns, total: 645 µs
Wall time: 497 µs
```

* 用numba加速python函数

```python
#低速
%%time
def my_power(x):
	return(x**2)

def my_power_sum(n):
	s = 0
	for i in range(1,n+1):
		s = s + my_power(i)
	return(s)

print(my_power_sum(100000))
```

```
333338333350000
CPU times: user 48.8 ms, sys: 0 ns, total: 48.8 ms
Wall time: 48 ms
```

```python
#高速
%%time
from numba import jit

@jit
def my_power(x):
	return(x**2)
@jit
def my_power_sum(n):
	s = 0
	for i in range(1,n+1):
		s = s + my_power(i)
	return(s)

print(my_power_sum(100000))
```

```
333338333350000
CPU times: user 48.8 ms, sys: 0 ns, total: 48.8 ms
Wall time: 4 ms
```

### <a name="speedup by using standard lib"></a>四、使用标准库函数加速

* 使用collection.Counter加速计数(这个经过我实测，发现可能低速比高速要快，不知道什么原因，但是使用高速方法代码更简洁。

```python
#低速
%%time
data = [x**2%1989 for x in range(200000)]

values_count = {}
for i in data:
	i_cnt = values_count.get(i,0)
	values_count[i] = i_cnt + 1
print(values_count.get(4,0))```

```
804
CPU times: user 171 ms, sys: 0 ns, total: 171 ms
Wall time: 170 ms
```

```python
#高速
%%time
data = [x**2%1989 for x in range(200000)]

from collections import Counter
values_count = Counter(data)
print(values_count.get(4,0))
```

```
804
CPU times: user 121 ms, sys: 0 ns, total: 121 ms
Wall time: 184 ms
```

* 使用collections.ChainMap加速字典合并

```python
#低速
%%time
dic_a ={i:i+1 for i in range(1,1000000,2)}
dic_b ={i:2*i+1 for i in range(1,1000000,3)}

result = dic_a.copy()
result.update(dic_b)
print(result.get(99,0))
```

```
100
CPU times: user 202 ms, sys: 14.9 ms, total: 217 ms
Wall time: 216 ms
```

```python
#高速
%%time
dic_a ={i:i+1 for i in range(1,1000000,2)}
dic_b ={i:2*i+1 for i in range(1,1000000,3)}

from collections import ChainMap
chain = ChainMap(dic_a,dic_b)
print(result.get(99,0))
```

```
100
CPU times: user 151 ms, sys: 45.2 ms, total: 196 ms
Wall time: 196 ms
```

### <a name="speedup by heigh order function"></a>五、使用高阶函数加速

* 使用map代替推导式进行加速

```python
#低速
%%time
result = [x**2 for x in range(1,1000000,3)]
```

```
CPU times: user 144 ms, sys: 6.82 ms, total: 151 ms
Wall time: 158 ms
```

```python
#高速
%%time
result = map(lambda x:x**2,range(1,1000000,3))
```

```
CPU times: user 2.93 ms, sys: 0 ns, total: 2.93 ms
Wall time: 2.93 ms
```

* 使用filter代替推倒时进行加速

```python
#低速
%%time
result = [x for x in range(1,1000000,3) if x%7==0]
```

```
CPU times: user 42.2 ms, sys: 0 ns, total: 42.2 ms
Wall time: 41.4 ms
```

```python
#高速
%%time
result = filter(lambda x:x%7==0,range(1,1000000,3))
```

```
CPU times: user 358 µs, sys: 61 µs, total: 419 µs
Wall time: 424 µs
```

### <a name="speedup by numpy"></a>六、使用numpy向量化加速

* 使用np.array代替list

```python
#低速
%%time
a = range(1,1000000,3)
b = range(1000000,1,-3)
c = [3*a[i]-2*b[i] for i in range(0,len(a))]
```

```
CPU times: user 159 ms, sys: 0 ns, total: 159 ms
Wall time: 166 ms
```

```python
#高速
%%time
import numpy as np
a = np.arange(1,1000000,3)
b = np.arange(1000000,1,-3)
c = 3*a - 2*b
```

```
CPU times: user 5.84 ms, sys: 302 µs, total: 6.14 ms
Wall time: 5.21 ms
```

* 使用np.ufunc代替math.func

```python
#低速
%%time
import math
a = range(1,1000000,3)
b = [math.log(x) for x in a]
```

```
CPU times: user 84.3 ms, sys: 3.12 ms, total: 87.4 ms
Wall time: 86.5 ms
```

```python
#高速
%%time
import numpy as np 
array_a = np.arange(1,1000000,3)
array_b = np.log(array_a)
```

```
CPU times: user 14.8 ms, sys: 0 ns, total: 14.8 ms
Wall time: 13.7 ms
```

* 使用np.where代替if

```python
#低速
%%time
import numpy as np 
array_a = np.arange(-100000,100000)
# np.vectorize可以将普通函数转换成支持量化的函数
relu = np.vectorize(lambda x: x if x>0 else 0)
array_b = relu(array_a)
```

```
CPU times: user 44 ms, sys: 0 ns, total: 44 ms
Wall time: 42.9 ms
```

```python
#高速
%%time
import numpy as np 
array_a = np.arange(-100000,100000)
# np.vectorize可以将普通函数转换成支持量化的函数
relu = lambda x:np.where(x>0,x,0)
array_b = relu(array_a)
```

```
CPU times: user 1.37 ms, sys: 268 µs, total: 1.64 ms
Wall time: 794 µs
```

### <a name="speedup pandas"></a>七、加速pandas

* 使用csv文件读写代替excel文件读写

```python
#低速
%%time
df.to_excel('data.xlsx')
```

```python
#高速
%%time
df.to_csv('data.csv')
```

* 使用pandas多进程工具pandarallel

```python
#低速
%%time
import pandas as pd 
import numpy as np 
df = pd.DataFrame(np.random.randint(-10,11,size=(10000,26)),columns = list('abcdefghijklmnopqrstuvwxyz'))
result = df.apply(np.sum,axis = 1)
```

```
CPU times: user 1.26 s, sys: 27.5 ms, total: 1.29 s
Wall time: 1.3 s
```

```python
#高速
%%time
import pandas as pd 
import numpy as np 
df = pd.DataFrame(np.random.randint(-10,11,size=(10000,26)),columns = list('abcdefghijklmnopqrstuvwxyz'))
from pandarallel import pandarallel
pandarallel.initialize(nb_workers=4)
result = df.parallel_apply(np.sum,axis=1)
```

```
CPU times: user 1.24 s, sys: 17.1 ms, total: 1.26 s
Wall time: 300 ms
```

### <a name="speedup by dask"></a>八、使用Dask加速

* 使用dask加速dataframe

```python
#低速
import pandas as pd 
import numpy as np 
df = pd.DataFrame(np.random.randint(0,6,size=(10000,5)),columns = list('abcde'))
df.groupby('a').mean()
```

```
CPU times: user 6.47 ms, sys: 0 ns, total: 6.47 ms
Wall time: 5.56 ms
```

```python
#高速
%%time
import pandas as pd 
import numpy as np 
df = pd.DataFrame(np.random.randint(0,6,size=(10000,5)),columns = list('abcde'))
import dask.datareame as dd
df_dask = dd.from_pandas(df,npartitions=40)
df_dask.groupby('a').mean().compute()
```

```
CPU times: user 4.81 ms, sys: 91 µs, total: 4.9 ms
Wall time: 1 ms
```

* 使用dask.delayed进行加速

```python
#低速
%%time
import time
def muchjob(x):
	time.sleep(5)
	return(x**2)

result = [muchjob(i) for i in range(5)]
```

```
CPU times: user 0 ns, sys: 1.87 ms, total: 1.87 ms
Wall time: 25 s
```

```python
#高速
%%time
import time
def muchjob(x):
	time.sleep(5)
	return(x**2)

from dask import delayed,compute
from dask import threaded,multiprocessing
values = [delayed(muchjob)(i) for i in range(5)]
result = compute(*values,scheduler='multiprocessing')
```

```
CPU times: user 22.1 ms, sys: 431 ms, total: 453 ms
Wall time: 5.61 s
```

### <a name="speedup by multithreading"></a>九、使用多线程多进程加速

* 应用多线程加速IO密集型任务

```python
#低速
%%time
def writefile(i):
	with open(str(i)+'.txt','w') as f:
		s = ('hello %d'%i)*100000
		f.write(s)

#串行任务
for i in range(10):
	writefile(i)
```

```python
#高速
%%time
def writefile(i):
	with open(str(i)+'.txt','w') as f:
		s = ('hello %d'%i)*100000
		f.write(s)

#多线程任务
thread_list = []
for i in range(10):
	t = threading.Thread(target=writefile,args=(i,))
	t.setDaemon(True) #设置为守护线程
	thread_list.append(t)

for t in thread_list:
	t.start() #启动线程

for t in thread_list:
	t.join() #等待子线程结束
```

* 应用多进程加速CPU密集型任务

```python
#低速
%%time
import time

def muchjob(x):
	time.sleep(5)
	return(x**2)

#串行任务
ans = [muchjob(i) for i in range(8)]
print(ans)
```

```
[0, 1, 4, 9, 16, 25, 36, 49]
CPU times: user 1.99 ms, sys: 381 µs, total: 2.37 ms
Wall time: 40 s
```

```python
#高速
%%time
import time
import multiprocessing

def muchjob(x):
	time.sleep(5)
	return(x**2)

#多进程任务
pool = multiprocessing.Pool(processes=4)
result = []
for i in range(8):
	result.append(pool.apply_async(muchjob,(i,)))
pool.close()
pool.join()
ans = [res.get() for res in result]
print(ans)
```

```
[0, 1, 4, 9, 16, 25, 36, 49]
CPU times: user 14.4 ms, sys: 33.4 ms, total: 47.8 ms
Wall time: 10.1 s
```

### <a name="about origin"></a>关于原文

原文来源于微信公众号[点击跳转](https://mp.weixin.qq.com/s?__biz=MzI2NjY5NzI0NA==&mid=2247486406&idx=1&sn=ad496f3f753fdafef3ab84d82bf99a44&chksm=ea8b64b5ddfceda31082a3916a9a0b0375b67d8a72a59ee959855ed515f53f50cde3e3d7ac36&mpshare=1&scene=1&srcid=0613X2Ub3OfFZcSliVgoc1iv&rd2werd=1#wechat_redirect)
