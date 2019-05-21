---
layout: post
title: python小技巧积累--题库（持续更新）
date: 2019-05-21
tags: python/闪马
---

## 介绍
	作为一名程序员，除了需要具备解决问题的思路以外，代码的质量和简洁性也很关键。python内置库中就有很多简洁而又优雅的操作，这里的知识都来源于网络积累，闲暇时整理下来方便温故。

### 目录
 
#### [选择正确的内置功能](#Select the correct built-in function)	
* [使用enumerate()而不是range()进行迭代](#use enumerate not range)
* [使用递推式构造列表而不是map()和filter()](#without map and filter)
* [使用断点breakpoint()调试而不是print()](#use breakpoint)
* [使用f-Strings格式化字符串](#use f-String)
* [使用sorted()对复杂列表进行排序](#use sorted)
#### [有效利用数据结构](#Use data structures effectively)
* [使用set存储唯一值](#use enumerate not range)
* [使用生成器节省内存](#use generator)
* [使用.get()和.setdefault()在字典中定义默认值](#use .get and setdefault)
#### [利用Python的标准库](#use Python standard library)
* [使用collections.defaultdict()处理缺少的字典键](#use collections.defdit)
* [使用collections.Counter计算Hashable对象 ](#use counter)
* [使用字符串常量访问公共字符串组](#use public string)
* [使用Itertools生成排列和组合](#use itertools)

## <a name='Select the correct built-in function'></a>选择正确的内置功能
Python有一个大型标准库，但只有一个内置函数的小型库，这些函数总是可用的，不需要导入。
### <a name='use enumerate not range'></a>1. 使用enumerate()而不是range()进行迭代
假如你需要遍历列表，同时访问索引和值。

有一个名为FizzBuzz的经典编码面试问题可以通过迭代索引和值来解决。在FizzBuzz中，你将获得一个整数列表，任务是执行以下操作：
* 用“fizz”替换所有可被3整除的整数
* 用“buzz”替换所有可被5整除的整数
* 将所有可被3和5整除的整数替换为“fizzbuzz”
通常，开发人员将使用range()解决此问题：

```python
>>> numbers = [45, 22, 14, 65, 97, 72]
>>> for i in range(len(numbers)):
...     if numbers[i] % 3 == 0 and numbers[i] % 5 == 0:
...         numbers[i] = 'fizzbuzz'
...     elif numbers[i] % 3 == 0:
...         numbers[i] = 'fizz'
...     elif numbers[i] % 5 == 0:
...         numbers[i] = 'buzz'
...
>>> numbers
['fizzbuzz', 22, 14, 'buzz', 97, 'fizz']
```

Range允许你通过索引访问数字元素，并且对于某些特殊情况也是一个很有用的工具。但在这种情况下，我们希望同时获取每个元素的索引和值，更优雅的解决方案使用enumerate()：

```python
>>> numbers = [45, 22, 14, 65, 97, 72]
>>> for i, num in enumerate(numbers):
...     if num % 3 == 0 and num % 5 == 0:
...         numbers[i] = 'fizzbuzz'
...     elif num % 3 == 0:
...         numbers[i] = 'fizz'
...     elif num % 5 == 0:
...         numbers[i] = 'buzz'
...
>>> numbers
['fizzbuzz', 22, 14, 'buzz', 97, 'fizz']
```

对于每个元素，enumerate()返回一个计数器和元素值。计数器默认为0，也是元素的索引。`不想在0开始你的计数？只需使用可选的start参数来设置偏移量`：

```python
>>> numbers = [45, 22, 14, 65, 97, 72]
>>> for i, num in enumerate(numbers, start=52):
...     print(i, num)
...
52 45
53 22
54 14
55 65
56 97
57 72
```

通过使用start参数，我们访问所有相同的元素，从第一个索引开始，但现在我们的计数从指定的整数值开始。

### <a name='without map and filter'></a>2. 使用递推式构造列表而不是map()和filter() 
一般使用者可能错误地认为它没有争议，但Guido有充分的理由想要从Python中删除map()和filter()。一个原因是Python支持递推式构造列表，它通常更容易阅读并支持与map()和filter()相同的功能。

让我们首先看看我们如何构造对map()的调用以及等效的递推构造列表：

```python
>>> numbers = [4, 2, 1, 6, 9, 7]
>>> def square(x):
...     return x*x
...
>>> list(map(square, numbers))
[16, 4, 1, 36, 81, 49]

>>> [square(x) for x in numbers]
[16, 4, 1, 36, 81, 49]
```

使用map()和列表推导的两种方法都返回相同的值，但列表推导更容易阅读和理解。下面我们可以对filter()及其等效的列表推导做同样的事情：

```python
>>> def is_odd(x):
...    return bool(x % 2)
...
>>> list(filter(is_odd, numbers))
[1, 9, 7]

>>> [x for x in numbers if is_odd(x)]
[1, 9, 7]
```

就像我们在map中看到的那样，filter和列表推导方法返回相同的值，但列表推导更容易理解。

来自其他语言的开发人员可能不同意构造列表比map和filter更容易阅读，但根据我的经验，初学者能够更直观地写出列表推导。但无论哪种方式，在编码面试中使用列表推导很少会出错，因为它会让你知道Python中最常见的是什么。
* 个人认为，无论是使用列表推导还是map和filter,只要熟练应用都可以。

### <a name='use breakpoint'></a>3. 使用断点breakpoint()调试而不是print()
你可能通过在代码中添加print并查看打印出的内容来调试一个小问题。这种方法起初效果很好，但很快变得很麻烦。另外，在编码面试设置中，你几乎不希望在整个代码中调用print()。

相反，你应该使用调试器。对于不是很琐碎的错误，它几乎总是比使用print()更快，并且鉴于调试是编写软件的重要部分，它表明你知道如何使用可以在工作中快速开发的工具。 

`如果你使用的是Python 3.7，则无需导入任何内容，只需在代码中要放入调试器的位置调用breakpoint()`：

```python
# Some complicated code with bugs
breakpoint()
```

调用breakpoint()会将你带入pdb，这是默认的Python调试器。在Python 3.6及更早版本中，你可以通过显式导入pdb来执行相同的操作：

```python
import pdb

pdb.set_trace()
```

像breakpoint()一样，pdb.set_trace()会将你带入pdb调试器。它不是那么简洁，而且需要记住的多一点。你可能想要尝试其他调试器，但pdb是标准库的一部分，因此它始终可用。无论你喜欢哪种调试器，在进行编码面试设置之前，都值得尝试使用它们来适应工作流程。

* 关于pdb的用法，如果不清楚，我有一篇blog专门整理过[点击跳转](https://davidocea.github.io/2019/05/pdb_debug_python/)

### <a name='use f-String'></a>4. 使用f-Strings格式化字符串  

Python有很多不同的方法来处理字符串格式化，有时候不知道使用哪个。在coding的面试中，如果使用Python 3.6+，建议的格式化方法是Python的f-strings。 

f-strings支持使用字符串格式化迷你语言，以及强大的字符串插值。这些功能允许你添加变量甚至有效的Python表达式，并在添加到字符串之前在运行时对它们进行评估：

```python
>>> def get_name_and_decades(name, age):
...     return f"My name is {name} and I'm {age / 10:.5f} decades old."
...
>>> get_name_and_decades("Maria", 31)
My name is Maria and I'm 3.10000 decades old.
```
f-string允许你将Maria放入字符串中，并在一个简洁的操作中添加具有所需格式的年龄。需要注意的一个风险是，如果你输出用户生成的值，那么可能会带来安全风险，在这种情况下，模板字符串可能是更安全的选择。

### <a name='use sorted'></a>5. 使用sorted()对复杂列表进行排序

大量的编码面试问题需要进行某种排序，并且有多种有效的方法可以进行排序。除非面试官希望你实现自己的排序算法，否则通常最好使用sorted()。你可能已经看到了排序的最简单用法，例如按升序或降序排序数字或字符串列表：

```python
>>> sorted([6,5,3,7,2,4,1])
[1, 2, 3, 4, 5, 6, 7]

>>> sorted(['cat', 'dog', 'cheetah', 'rhino', 'bear'], reverse=True)
['rhino', 'dog', 'cheetah', 'cat', 'bear]
```

默认情况下，sorted()已按升序对输入进行排序，而reverse关键字参数则按降序排序。

值得了解的是可选关键字key，它允许你在排序之前指定将在每个元素上调用的函数。添加函数允许自定义排序规则，如果要对更复杂的数据类型进行排序，这些规则特别有用：

```python
>>> animals = [
...     {'type': 'penguin', 'name': 'Stephanie', 'age': 8},
...     {'type': 'elephant', 'name': 'Devon', 'age': 3},
...     {'type': 'puma', 'name': 'Moe', 'age': 5},
... ]
>>> sorted(animals, key=lambda animal: animal['age'])
[
    {'type': 'elephant', 'name': 'Devon', 'age': 3},
    {'type': 'puma', 'name': 'Moe', 'age': 5},
    {'type': 'penguin', 'name': 'Stephanie, 'age': 8},
]
```

通过传入一个返回每个元素年龄的lambda函数，可以轻松地按每个字典的单个值对字典列表进行排序。在这种情况下，字典现在按年龄按升序排序。

## <a name='Use data structures effectively'></a>有效利用数据结构

算法在面试中得到了很多关注，但数据结构可能更为重要。在coding面试环境中，选择正确的数据结构会对性能产生重大影响。除了理论数据结构之外，Python还在其标准数据结构实现中内置了强大而方便的功能。这些数据结构在面试中非常有用，因为它们默认为你提供了许多功能，让你可以将时间集中在问题的其他部分。

### <a name='use enumerate not range'></a>1. 使用set存储唯一值 

我们通常需要从现有数据集中删除重复元素。新的开发人员有时会在列表应该使用集合时执行此操作，这会强制执行所有元素的唯一性。

假装你有一个名为get_random_word()的函数。它将始终从一小组单词中返回一个随机选择：

```python
>>> import random
>>> all_words = "all the words in the world".split()
>>> def get_random_word():
...    return random.choice(all_words)
```

你应该重复调用get_random_word()以获取1000个随机单词，然后返回包含每个唯一单词的数据结构。以下是两种常见的次优方法和一种好的方法。

`糟糕的方法`

get_unique_words()将值存储在列表中，然后将列表转换为集合：

```python
>>> def get_unique_words():
...     words = []
...     for _ in range(1000):
...         words.append(get_random_word())
...     return set(words)
>>> get_unique_words()
{'world', 'all', 'the', 'words'}
```

这种方法并不可怕，但它不必要地创建了一个列表，然后将其转换为集合。面试官几乎总是注意到（并询问）这种类型的设计选择。

`更糟糕的做法`

为避免从列表转换为集合，你现在可以在不使用任何其他数据结构的情况下将值存储在列表中。然后，通过将新值与列表中当前的所有元素进行比较来测试唯一性：

```python
>>> def get_unique_words():
...     words = []
...     for _ in range(1000):
...         word = get_random_word()
...         if word not in words:
...             words.append(word)
...     return words
>>> get_unique_words()
['world', 'all', 'the', 'words']
```

这比第一种方法更糟糕，因为你必须将每个新单词与列表中已有的每个单词进行比较。这意味着随着单词数量的增加，查找次数呈二次方式增长。换句话说，时间复杂度在O(N^2)的量级上增长。

`优秀的方法`

现在，你完全跳过使用列表，而是从头开始使用一组：

```python
>>> def get_unique_words():
...     words = set()
...     for _ in range(1000):
...         words.add(get_random_word())
...     return words
>>> get_unique_words()
{'world', 'all', 'the', 'words'}
``` 

除了从头开始使用集合之外，这可能与其他方法没有太大的不同。如果你考虑.add()中发生了什么，它甚至听起来像第二种方法：得到单词，检查它是否已经在集合中，如果没有，则将其添加到数据结构中。 

那么为什么使用与第二种方法不同的集合呢？ 

`它们是不同的，因为集合存储元素的方式允许接近恒定时间检查值是否在集合中，而不像需要线性时间查找的列表。`查找时间的差异意味着添加到集合的时间复杂度以O(N)的速率增长，这在大多数情况下比第二种方法的O(N^2)好得多。

### <a name='use generator'></a>2. 使用生成器节省内存 

前面提到，列表推导是方便的工具，但有时会导致不必要的内存使用。想象一下，你被要求找到前1000个完美正方形的总和，从1开始。你知道列表推导，所以你快速编写一个有效的解决方案：

```python
>>> sum([i * i for i in range(1, 1001)])
333833500
```

解决方案会列出1到1,000,000之间的每个完美平方，并对值进行求和。你的代码会返回正确的答案，但随后您的面试官会开始增加您需要总和的完美正方形的数量。 

起初，你的功能不断弹出正确的答案，但很快就开始放慢速度，直到最后这个过程似乎永远持续下去。这不是你想要在面试中发生的一件事。 

这里发生了什么？ 

它正在列出你要求的每个完美的方块，并将它们全部加起来。具有1000个完美正方形的列表在计算机术语中可能不会很大，但是1亿或10亿是相当多的信息，并且很容易占用计算机的可用内存资源。这就是这里发生的事情。

值得庆幸的是，有一种解决内存问题的快捷方法。你只需用括号替换方括号：

```python
>>> sum((i * i for i in range(1, 1001)))
333833500
```

换出括号会将列表推导更改为生成器表达式。当你知道要从序列中检索数据，但不需要同时访问所有数据的时候，生成器表达式非常适合。 

生成器表达式返回生成器对象，而不是创建列表。该对象知道它在当前状态中的位置（例如，i = 49）并且仅在被要求时计算下一个值。

因此，当sum通过重复调用.__ next __()来迭代生成器对象时，生成器检查i
等于多少，计算i * i，在内部递增i，并将正确的值返回到sum。该设计允许生成器用于大量数据序列，因为一次只有一个元素存在于内存中。

### <a name='use .get and setdefault'></a>3. 使用.get()和.setdefault()在字典中定义默认值  
最常见的编程任务之一涉及添加，修改或检索可能在字典中或可能不在字典中的项。Python字典具有优雅的功能，可以使这些任务简洁明了，但开发人员通常会在不需要时检查值。

想象一下，你有一个名为cowboy的字典，你想得到那个cowboy的名字。一种方法是使用条件显式检查key：

```python
>>> cowboy = {'age': 32, 'horse': 'mustang', 'hat_size': 'large'}
>>> if 'name' in cowboy:
...     name = cowboy['name']
... else:
...     name = 'The Man with No Name'
...
>>> name
'The Man with No Name'
```

此方法首先检查字典中是否存在name键，如果存在，则返回相应的值。否则，它返回默认值。

虽然清楚地检查key确实有效，但如果使用.get()，它可以很容易地用一行代替：

```python
>>> name = cowboy.get('name', 'The Man with No Name')
```

get()执行与第一种方法相同的操作，但现在它们会自动处理。如果key存在，则返回适当的值。否则，将返回默认值。

但是，如果你想在仍然访问name的key时使用默认值更新字典呢？ .get()在这里没有真正帮助你，所以你只需要再次显式检查这个值：

```python
>>> if 'name' not in cowboy:
...     cowboy['name'] = 'The Man with No Name'
...
>>> name = cowboy['name']
```

检查values并设置默认值是一种有效的方法，并且易于阅读，但Python再次使用.setdefault()提供了更优雅的方法：

```python
>>> name = cowboy.setdefault('name', 'The Man with No Name')
```

.setdefault()完成与上面代码片段完全相同的操作。它检查cowboy中是否存在名称，如果是，则返回该值。否则，它将cowboy ['name']设置为The Man with No Name并返回新值。

## <a name='use Python standard library'></a>利用Python的标准库
默认情况下，Python提供了许多功能，这些功能只是一个导入语句。它本身就很强大，但知道如何利用标准库可以增强你的编码面试技巧。

从所有可用模块中挑选最有用的部分很困难，因此本节将仅关注其实用功能的一小部分。希望这些对您在编码访谈中有用，并且您希望了解更多有关这些和其他模块的高级功能的信息。

### <a name='use collections.defdit'></a>1. 使用collections.defaultdict()处理缺少的字典键  
当你为单个键设置默认值时，.get()和.setdefault()可以正常工作，但通常需要为所有可能的未设置键设置默认值，尤其是在面试环境中进行编程时

假装你有一群学生，你需要记录他们在家庭作业上的成绩。输入值是具有格式（student_name，grade）的元组列表，但是你希望轻松查找单个学生的所有成绩而无需迭代列表。 

存储成绩数据的一种方法是使用将学生姓名映射到成绩列表的字典：

```python
>>> student_grades = {}
>>> grades = [
...     ('elliot', 91),
...     ('neelam', 98),
...     ('bianca', 81),
...     ('elliot', 88),
... ]
>>> for name, grade in grades:
...     if name not in student_grades:
...         student_grades[name] = []
...     student_grades[name].append(grade)
...
>>> student_grades
{'elliot': [91, 88], 'neelam': [98], 'bianca': [81]}
```

在这种方法中，你迭代学生并检查他们的名字是否已经是字典中的属性。如果没有，则将它们添加到字典中，并将空列表作为默认值。然后将实际成绩附加到该学生的成绩列表中。

但是有一个更简洁的方法，可以使用defaultdict，它扩展了标准的dict功能，允许你设置一个默认值，如果key不存在，它将按默认值操作：

```python
>>> from collections import defaultdict
>>> student_grades = defaultdict(list)
>>> for name, grade in grades:
...     student_grades[name].append(grade)
```

在这种情况下，你将创建一个defaultdict，它使用不带参数的list构造函数作为默认方法。没有参数的list返回一个空列表，因此如果名称不存在则defaultdict调用list()，然后再把学生成绩添加上。如果你想更炫一点，你也可以使用lambda函数作为值来返回任意常量。

利用defaultdict可以使代码更简洁，因为你不必担心key的默认值。相反，你可以在defaultdict里处理它们一次，然后key就终存在了。

### <a name='use counter'></a>2. 使用collections.Counter计算Hashable对象  
假如你有一长串没有标点符号或大写字母的单词，你想要计算每个单词出现的次数。 

你可以使用字典或defaultdict增加计数，但collections.Counter提供了一种更清晰，更方便的方法。 Counter是dict的子类，它使用0作为任何缺失元素的默认值，并且更容易计算对象的出现次数：

```python
>>> from collections import Counter
>>> words = "if there was there was but if 
... there was not there was not".split()
>>> counts = Counter(words)
>>> counts
Counter({'if': 2, 'there': 4, 'was': 4, 'not': 2, 'but': 1})
```

当你将单词列表传递给Counter时，它会存储每个单词以及该单词在列表中出现的次数。

如果你好奇两个最常见的词是什么？只需使用.most_common（）：

```python
>>> counts.most_common(2)
[('there', 4), ('was', 4)]
```

.most-common（）是一个方便的方法，只需按计数返回n个最频繁的输入。
### <a name='use public string'></a>3. 使用字符串常量访问公共字符串组
现在有一个琐事需要判断！‘A’>‘a’是真是假？
这是假的，因为A的ASCII代码是65，但a是97，65不大于97。为什么答案很重要？因为如果你想检查一个字符是否是英语字母表的一部分，一种流行的方法是看它是否在A和Z之间（在ASCII图表上是65和122）。

检查ascii代码是可行的，但是的却很笨拙，很容易弄乱，特别是当你记不清是小写还是大写的ascii字符排在第一位的时候。这时候，使用定义在字符串模块中的常量要容易得多。

你可以使用is_upper()，它返回字符串中的所有字符是否都是大写字母：

```python
>>> import string
>>> def is_upper(word):
...     for letter in word:
...         if letter not in string.ascii_uppercase:
...             return False
...     return True
...
>>> is_upper('Thanks Geir')
False
>>> is_upper('LOL')
True
```

is_upper()迭代word中的字母，并检查字母是否为string.ascii_大写字母的一部分。如果你打印出string.ascii_大写，你会发现它只是一个字符串，该值设置为文本“ABCDEFGHIJKLMNOPQRSTUVWXYZ”。

所有字符串常量都只是经常引用的字符串值的字符串。其中包括以下内容：
* string.ascii_letters
* string.ascii_uppercase
* string.ascii_lowercase
* string.digits
* string.hexdigits
* string.octdigits
* string.punctuation
* string.printable
* string.whitespace

这些更容易使用，更重要的是，更容易阅读。

### <a name='use itertools'></a>4. 使用Itertools生成排列和组合
例子：你去游乐园，决定找出每一对可能坐在过山车上的朋友。

除非生成这些配对是面试问题的主要目的，否则很可能生成所有可能的配对只是朝着工作算法前进的一个乏味的步骤。你可以自己用嵌套for循环计算它们，也可以使用强大的itertools库。

itertools有多个工具来生成可重复输入数据序列，但现在我们只关注两个常见函数：itertools.permutations()和itertools.combinations()。

itertools.permutations()构建所有排列的列表，这意味着它是输入值的每个可能分组的列表，其长度与count参数匹配。r关键字参数允许我们指定每个分组中有多少值：

```python
>>> import itertools
>>> friends = ['Monique', 'Ashish', 'Devon', 'Bernie']
>>> list(itertools.permutations(friends, r=2))
[('Monique', 'Ashish'), ('Monique', 'Devon'), ('Monique', 'Bernie'),
('Ashish', 'Monique'), ('Ashish', 'Devon'), ('Ashish', 'Bernie'),
('Devon', 'Monique'), ('Devon', 'Ashish'), ('Devon', 'Bernie'),
('Bernie', 'Monique'), ('Bernie', 'Ashish'), ('Bernie', 'Devon')]
```

 对于排列，元素的顺序很重要，因此(“sam”、“devon”)表示与(“devon”、“sam”)不同的配对，这意味着它们都将包含在列表中。

itertools.combinations()生成组合。这些也是输入值的可能分组，但现在值的顺序无关紧要。因为(‘sam’、‘devon’)和(‘devon’、‘sam’)代表同一对，所以输出列表中只会包含它们中的一个：

```python
>>> list(itertools.combinations(friends, r=2))
[('Monique', 'Ashish'), ('Monique', 'Devon'), ('Monique', 'Bernie'),
('Ashish', 'Devon'), ('Ashish', 'Bernie'), ('Devon', 'Bernie')]
```

由于值的顺序与组合有关，因此同一输入列表的组合比排列少。同样，因为我们将r设置为2，所以每个分组中都有两个名称。

.combinations和.permutations只是强大库的一个小例子，但是当你试图快速解决算法问题时，即使这两个函数也非常有用。

### 关于原文
链接：https://realpython.com/python-coding-interview-tips/[点击跳转](https://realpython.com/python-coding-interview-tips/)
感谢python中文社区
