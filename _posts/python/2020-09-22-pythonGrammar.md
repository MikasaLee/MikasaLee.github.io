---
layout: post
title: python语法中需要注意的知识点
categories: python
description: 
keywords: python,grammar
---

2020-9-22：三年研究生生涯正式开始，今天重新开始学习 python 。这篇文章记录 python 语法中需要注意的地方（虽然有 pycharm 警示，但自己毕竟没有重视过 python，）。


# 1、 过一遍官方文档中遇到的问题
[官方文档中文版](https://docs.python.org/zh-cn/3/tutorial/index.html)

初步定三部分 ：
+ [Python教程]([https://docs.python.org/zh-cn/3/tutorial/index.html)
+ [Python 标准库](https://docs.python.org/zh-cn/3/library/index.html#library-index)
+ [Python语言参考](https://docs.python.org/zh-cn/3/reference/index.html#reference-index)

## 1.1、 Python教程

+ [编码风格](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#intermezzo-coding-style)
+ IndentationError: expected an indented block（缩进错误： 期望一个缩进的块）：Indentation（缩进）
+ 布尔值是 `True` 和 `False` 。而不是 `true/TRUE` 
+ 格式化输出时：`print("xxx")` 而不是 `printf`
+ 单行注释：`#`
+ 除法运算符 `/` 永远返回浮点数类型；如果想返回整数类型，使用 `//` 运算符
+ 乘方运算符 `**` ：因为 `**` 比 `-`有更高的优先级, 所以 `-3**2` 会被解释成 `-(3**2)` ，因此结果是 `-9`. 为了避免这个并且得到结果 `9`, 你可以用这个式子 `(-3)**2`
+ 在交互模式下，上一次打印出来的表达式被赋值给变量 `_` ：这个变量应该被使用者当作是只读类型。不要向它显式地赋值——你会创建一个和它名字相同独立的本地变量，它会使用魔法行为屏蔽内部变量。
+ 字符串切片 ：结果是左闭右开：[) ，所以 `s[:i] + s[i:]` 总是等于 `s`
	```python
	word = 'python'
	word[0:2] #out: py 
	```
+ Python 中的字符串不能被修改，它们是 immutable 的。因此，向字符串的某个索引位置赋值会产生一个错误.
	```python
	>>> word[0] = 'J'
	Traceback (most recent call last):
  	 File "<stdin>", line 1, in <module>
	TypeError: 'str' object does not support item assignment
	```
+ 内建函数 `len()` 返回一个字符串的长度
+ 列表（list） :Python 中可以通过组合一些值得到多种 *复合* 数据类型。其中最常用的 *列表* ，可以通过方括号括起、逗号分隔的一组值（元素）得到。一个 *列表* 可以包含不同类型的元素，但通常使用时各个元素类型相同:
	```python
	>>> squares = [1, 4, 9, 16, 25]
	>>> squares
	[1, 4, 9, 16, 25]
	```
	所有的切片操作都返回一个包含所请求元素的新列表。 这意味着以下切片操作会返回列表的一个 **浅拷贝**:
	```python
	>>> squares[:]
	[1, 4, 9, 16, 25]
	```
+ `if` `while` `for(当然py只有for-each)`的条件不需要加空格，
+ `if` 的 demo : **注意关键字分别为 `if` `elif` `else`**
	```python
	>>> x = int(input("Please enter an integer: "))
Please enter an integer: 42
	>>> if x < 0:
	...     x = 0
	...     print('Negative changed to zero')
	... elif x == 0:
	...     print('Zero')
	... elif x == 1:
	...     print('Single')
	... else:
	...     print('More')
	...
More
	```
	
+ `for` 的 demo : `python` 中只支持 `for-each`而没有传统的 `for`。
	```python
	>>> words = ['cat', 'window', 'defenestrate']
	>>> for w in words:
	...     print(w, len(w))
	...
  cat 3
  window 6
  defenestrate 12
	```
+ `range()` :**一般情况下**：理解成返回一个 列表(list) 就行
	
	```python
	>>> a = ['Mary', 'had', 'a', 'little', 'lamb']
	>>> for i in range(len(a)):
...     print(i, a[i])
...
0 Mary
1 had
2 a
3 little
4 lamb
	```
**但是**：
	
	```python
	>>> print(range(10))
	range(0, 10) #而不是期望的 0,1,2,3....
	#所以为了规范 以后用 range是外面一律套list：list(range(10))
	```
>range() 所返回的对象在许多方面表现得像一个列表，但实际上却并不是（ python2 的时候返回的是列表）。此对象会在你迭代它时基于所希望的序列返回连续的项，但它没有真正生成列表，这样就能节省空间。
>我们称这样对象为 iterable（迭代值），也就是说，适合作为这样的目标对象：函数和结构期望从中获取连续的项直到所提供的项全部耗尽。 我们已经看到 for 语句就是这样一种结构，而接受可迭代对象的函数的一个例子是 sum():

**具体参考知乎[这篇回答](https://www.zhihu.com/search?type=content&q=range%20python)**

**牢记：<font color='red'>以后使用range时一律套list</font>**



+ 循环中的 else 子句:
```python
>>> for n in range(2, 10):
...     for x in range(2, n):
...         if n % x == 0:
...             print(n, 'equals', x, '*', n//x)
...             break
...     else:
...         # loop fell through without finding a factor
...         print(n, 'is a prime number')
...
2 is a prime number
3 is a prime number
4 equals 2 * 2
5 is a prime number
6 equals 2 * 3
7 is a prime number
8 equals 2 * 4
9 equals 3 * 3
```

+ `for` 的 `else` 语句:循环语句可能带有 else 子句；它会在循环耗尽了可迭代对象 (使用 for) 或循环条件变为假值 (使用 while) 时被执行
```python
>>> for n in range(2,10):
...     print(n)
... else:
...     print("end")
... 
2
3
4
5
6
7
8
9
end
```

+ `pass` 语句: `pass` 语句什么也不做。当语法上需要一个语句，但程序需要什么动作也不做时，可以使用它。例如:
```python
>>> while True:
...     pass  # Busy-wait for keyboard interrupt (Ctrl+C)
...	  # 这应该是等同于 java 的 while(true){} ，空循环体的情况。如果没有pass，python报错
```


+ 定义函数: `def`

  不带返回值：
```python
>>> def fib(n):    # write Fibonacci series up to n
...     """Print a Fibonacci series up to n."""
...     a, b = 0, 1
...     while a < n:
...         print(a, end=' ')
...         a, b = b, a+b
...     print()
...
>>> # Now call the function we just defined:
... fib(2000)
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597
```
​       带返回值：
```python
>>> def fib2(n):  # return Fibonacci series up to n
...     """Return a list containing the Fibonacci series up to n."""
...     result = []
...     a, b = 0, 1
...     while a < n:
...         result.append(a)    # see below
...         a, b = b, a+b
...     return result
...
>>> f100 = fib2(100)    # call it
>>> f100                # write the result
[0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89]
```

+ `in` 关键字：以测试一个序列是否包含某个值。


```python
>>> i = 'y'
>>> if i in ['y','yes']:
...     print("yes")
... 
yes
>>> i = 'n'
>>> if i in ['y','yes']:
...     print("yes")
... 
>>> 
```

+ <font color='red'>重要警告： 默认值只会执行一次。这条规则在默认值为可变对象（列表、字典以及大多数类实例）时很重要。比如，下面的函数会存储在后续调用中传递给它的参数:</font>


```python
def f(a, L=[]):
    L.append(a)
    return L

print(f(1))
print(f(2))
print(f(3))

// out:
[1]
[1, 2]
[1, 2, 3]
```

如果你不想要在后续调用之间共享默认值，你可以这样写这个函数:

```python
def f(a, L=None):
    if L is None:
        L = []
    L.append(a)
    return L
```

+ 函数形参中的关键字参数： 当存在一个形式为 `**name` 的最后一个形参时，它会接收一个字典 (参见 映射类型 --- dict)，其中包含除了与已有形参相对应的关键字参数以外的所有关键字参数。 这可以与一个形式为 `*name`，接收一个包含除了已有形参列表以外的位置参数的 元组 的形参 (将在下一小节介绍) 组合使用 (`*name` 必须出现在 `**name` 之前。) 例如，如果我们这样定义一个函数:
```python
def cheeseshop(kind, *arguments, **keywords):
    print("-- Do you have any", kind, "?")
    print("-- I'm sorry, we're all out of", kind)
    for arg in arguments:
        print(arg)
    print("-" * 40)
    for kw in keywords:
        print(kw, ":", keywords[kw])
```

+ 列表推导式：

```python
squares = [x**2 for x in range(10)]
#squares = [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

+ `del` 语句：
```python
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> a
[]
```

+ 元组：和列表类似，只不过**元组是不可变的**
```python
>>> t = (123,34,45)
>>> t[0]
123
>>> t[0] = (123123)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
```

+ 集合( `set` )：

```python
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # show that duplicates have been removed
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # fast membership testing
True
>>> 'crabgrass' in basket
False

>>> # Demonstrate set operations on unique letters from two words
...
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # unique letters in a
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # letters in a but not in b
{'r', 'd', 'b'}
>>> a | b                              # letters in a or b or both
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # letters in both a and b
{'a', 'c'}
>>> a ^ b                              # letters in a or b but not both
{'r', 'd', 'b', 'm', 'z', 'l'}
```

+ 字典
```python
>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'jack': 4098, 'sape': 4139, 'guido': 4127}
>>> tel['jack']
4098
>>> del tel['sape']
>>> tel['irv'] = 4127
>>> tel
{'jack': 4098, 'guido': 4127, 'irv': 4127}
>>> list(tel)
['jack', 'guido', 'irv']
>>> sorted(tel)
['guido', 'irv', 'jack']
>>> 'guido' in tel
True
>>> 'jack' not in tel
False
```

创建字典的其他方法：
```python
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)]) 		#dict() 构造函数可以直接从键值对序列里创建字典:
{'sape': 4139, 'guido': 4127, 'jack': 4098}
>>> {x: x**2 for x in (2, 4, 6)} 		#此外，字典推导式可以从任意的键值表达式中创建字典
{2: 4, 4: 16, 6: 36}
>>> dict(sape=4139, guido=4127, jack=4098)		#当关键字是简单字符串时，有时直接通过关键字参数来指定键值对更方便
{'sape': 4139, 'guido': 4127, 'jack': 4098}
```

+ 字典循环的使用技巧：

```python
#item()方法
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.items():		# 自动将 key 给了 k，value给了 v
...     print(k, v)
...
gallahad the pure
robin the brave

# enumerate() 函数
>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print(i, v)
...
0 tic
1 tac
2 toe

#当同时在两个或更多序列中循环时，可以用 zip() 函数将其内元素一一匹配。
>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for q, a in zip(questions, answers):
...     print('What is your {0}?  It is {1}.'.format(q, a))
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.
```

+ 模块搜索路径
>当一个名为 spam 的模块被导入的时候，解释器首先寻找具有该名称的内置模块。如果没有找到，然后解释器从 sys.path 变量给出的目录列表里寻找名为 spam.py 的文件。sys.path 初始有这些目录地址:
    + 包含输入脚本的目录（或者未指定文件时的当前目录）。
    + PYTHONPATH （一个包含目录名称的列表，它和shell变量 PATH 有一样的语法）。
    + 取决于安装的默认设置
>注解：
在支持符号链接的文件系统上，包含输入脚本的目录是在追加符号链接后才计算出来的。换句话说，包含符号链接的目录并 没有 被添加到模块的搜索路径上。
在初始化后，Python程序可以更改 sys.path。包含正在运行脚本的文件目录被放在搜索路径的开头处， 在标准库路径之前。这意味着将加载此目录里的脚本，而不是标准库中的同名模块。 除非有意更换，否则这是错误。更多信息请参阅 标准模块。

+ [包](https://docs.python.org/zh-cn/3/tutorial/modules.html#packages)


+ 文件读取

**注意：read()和readline()都会将换行符读出来，所以还需要再手动处理掉**

```python
>>> f.read()
'This is the entire file.\n'
```


+ 异常：关键字为 `try` `except` 
```python
>>> while True:
...     try:
...         x = int(input("Please enter a number: "))
...         break
...     except ValueError:
...         print("Oops!  That was no valid number.  Try again...")
...
```

+ `raise` 抛出异常
```python
>>> raise NameError('HiThere')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: HiThere
```
+ `finally` 清理操作
```python
>>> try:
...     raise KeyboardInterrupt
... finally:
...     print('Goodbye, world!')
...
Goodbye, world!
KeyboardInterrupt
Traceback (most recent call last):
  File "<stdin>", line 2, in <module>
```
>如果存在 finally 子句，则 finally 子句将作为 try 语句结束前的最后一项任务被执行。 finally 子句不论 try 语句是否产生了异常都会被执行。 以下几点讨论了当异常发生时一些更复杂的情况：
    + 如果在执行 try 子句期间发生了异常，该异常可由一个 except 子句进行处理。 如果异常没有被某个 except 子句所处理，则该异常会在 finally 子句执行之后被重新引发。
    + 异常也可能在 except 或 else 子句执行期间发生。 同样地，该异常会在 finally 子句执行之后被重新引发。
    + 如果在执行 try 语句时遇到一个 break, continue 或 return 语句，则 finally 子句将在执行 break, continue 或 return 语句之前被执行。
    + 如果 finally 子句中包含一个 return 语句，则返回值将来自 finally 子句的某个 return 语句的返回值，而非来自 try 子句的 return 语句的返回值。




## 1.2、目前觉得用不到，跳过。等用到了再学的玩意

+ [特殊参数](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#special-parameters) 
+ [任意的参数列表](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#arbitrary-argument-lists)
+ [Lambda 表达式](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#lambda-expressions)
+ [文档字符串](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#documentation-strings)
+ [“编译过的”Python文件](https://docs.python.org/zh-cn/3/tutorial/modules.html#compiled-python-files)
+ [使用 json 保存结构化数据](https://docs.python.org/zh-cn/3/tutorial/inputoutput.html#saving-structured-data-with-json)
+ [用户自定义异常](https://docs.python.org/zh-cn/3/tutorial/errors.html#user-defined-exceptions)
+ [生成器](https://docs.python.org/zh-cn/3/tutorial/classes.html#generators)