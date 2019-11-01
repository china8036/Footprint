- [Python 基础：代码结构](#python-基础代码结构)
  - [1. 流程控制](#1-流程控制)
    - [1.1. if、 elif 和 else](#11-if-elif-和-else)
    - [1.2. while、else](#12-whileelse)
    - [1.3. for、else](#13-forelse)
    - [1.4. with & Context Managers](#14-with--context-managers)
    - [1.5. yield](#15-yield)
      - [1.5.1. yield && Generator](#151-yield--generator)
      - [1.5.2. yield && Coroutine](#152-yield--coroutine)
  - [2. 推导式 (comprehensions)](#2-推导式-comprehensions)
    - [2.1. 列表推导式 (list comprehensions)](#21-列表推导式-list-comprehensions)
    - [2.2. 字典推导式 (dict comprehensions)](#22-字典推导式-dict-comprehensions)
    - [2.3. 集合推导式 (set comprehensions)](#23-集合推导式-set-comprehensions)
    - [2.4. 生成器推导式 (generator comprehensions)](#24-生成器推导式-generator-comprehensions)
  - [3. 函数](#3-函数)
    - [3.1. 参数](#31-参数)
      - [3.1.1. 参数类型](#311-参数类型)
        - [3.1.1.1. 位置参数](#3111-位置参数)
        - [3.1.1.2. 关键字参数](#3112-关键字参数)
      - [3.1.2. 参数默认值](#312-参数默认值)
        - [3.1.2.1. 使用函数参数的默认值来实现函数静态变量的功能](#3121-使用函数参数的默认值来实现函数静态变量的功能)
      - [3.1.3. 使用 * 和 ** 收集参数](#313-使用--和--收集参数)
      - [3.1.4. 参数传递方式](#314-参数传递方式)
    - [3.2. 函数文档字符串](#32-函数文档字符串)
      - [3.2.1. 定义](#321-定义)
      - [3.2.2. 输出](#322-输出)
    - [3.3. 内部函数](#33-内部函数)
    - [3.4. 闭包](#34-闭包)
    - [3.5. 匿名函数：lambda()](#35-匿名函数lambda)
  - [4. 模块、包](#4-模块包)
    - [4.1. 命令行参数](#41-命令行参数)
    - [4.2. import 语句](#42-import-语句)
    - [4.3. 模块](#43-模块)
      - [4.3.1. 导入整个模块](#431-导入整个模块)
      - [4.3.2. 导入模块的某一部分](#432-导入模块的某一部分)
      - [4.3.3. 导入后使用别名](#433-导入后使用别名)
    - [4.4. 包](#44-包)
      - [4.4.1. 包结构](#441-包结构)
      - [4.4.2. `__init__.py`](#442-__init__py)
  - [5. Refer Links](#5-refer-links)

# Python 基础：代码结构

Python 通过代码缩进来区分代码块结构，避免输入太多的花括号和关键字，使用空白来区分代码结构；

## 1. 流程控制

### 1.1. if、 elif 和 else

```python
if color == "red":
  print("It's a tomato")
elif color == "green":
  print("It's a green pepper")
elif color == "bee purple":
  print("I don't know what it is, but only bees can see it")
else:
  print("I've never heard of the color", color)
```

### 1.2. while、else

```python
# 如果 while 循环正常结束（没有使用 break 跳出），程序将进入到可选的 else 段
>>> while position < len(numbers):
... 	number = numbers[position]
... 	if number % 2 == 0:
... 		print('Found even number', number)
... 		break
... 	position += 1
... else: #没有执行 break
... 	print('No even number found')
```

### 1.3. for、else

列表（例如 rabbits）、字符串、元组、字典、集合等都是 Python 中可迭代的对象：

- 元组或者列表在一次迭代过程产生一项；

- 字符串在每一次迭代中会产生一个字符；

- 对一个字典（或者字典的 keys() 函数）进行迭代将返回字典中的键，如果想对字典的值进行迭代，可以使用字典的 values() 函数，如果需要以元组的形式返回键值对进行迭代，可以使用字典的 `items()` 函数；

例：
```python
>>> for value in accusation.values():
... 	print(value)
>>> for item in accusation.items():
... 	print(item)
```
类似于 while， for 循环也可以使用可选的 else 代码段，**用来判断 for 循环是否正常结束（没有调用 break 跳出），否则会执行 else 段**。

例：
```python
>>> for cheese in cheeses:
... 	print('This shop has some lovely', cheese)
... 	break
... else: # 没有 break 表示没有找到奶酪
... 	print('This is not much of a cheese shop, is it?')
```

TODO:

**注意：当使用 for .. in .. 进行迭代时，python 会将可迭代对象的所有迭代值都读到内存中，再进行迭代，因此，若迭代器包含了海量数据，直接迭代会导致大量内存被占用，此时可采用推导式或者生成器的方式。**

### 1.4. with & Context Managers

从 Python2.5 开始 ([PEP 343](https://www.python.org/dev/peps/pep-0343/))，Python 中引入了 with 语句，结合上下文管理器 (Context Managers)，可以实现对资源进行有控制的访问，确保不管使用过程中是否发生异常都会执行必要的清理操作，释放资源：
```python
with context_expression [as target(s)]:
    with-body
```

e.g.
```python
def m3():
    with open("output.txt", "w") as f:
        f.write("Python")
```
open 方法的返回值赋值给变量 f，当离开 with 代码块的时候，系统会自动调用 f.close() 方法， with 的作用和使用 try/finally 语句是一样的。

**with 语句支持同时使用多个 Context Managers**，不需要嵌套多个 with：
```python
with A() as a, B() as b:
    suite
```

### 1.5. yield

#### 1.5.1. yield && Generator

通过 yield 语句可以实现生成器函数。

yield 对应的值在函数被调用时不会立刻返回，而是调用 next 方法时才返回。

下一次迭代时，从上一次迭代遇到的 yield 后面的代码继续开始执行。直到将生成器函数执行完毕，生成器会返回一个异常，告知迭代器迭代结束。

在 [PEP 380](https://www.python.org/dev/peps/pep-0380/) 中，针对 `yield` 语句引入了 `yield from` 语法：`yield from generator()` 相当于 `for x in generator(): yield x`。yield from 语法可以用来方便地组合不同的 generator。

e.g.
```python
def odds(n):
    for i in range(n):
        if i % 2 == 1:
            yield i

def evens(n):
    for i in range(n):
        if i % 2 == 0:
            yield i

def odd_even(n):
    for x in odds(n):
        yield x
    for x in evens(n):
        yield x

for x in odd_even(6):
    print(x)
#=> 1, 3, 5, 0, 2, 4
```
可以改写为：
```python
def odds(n):
    for i in range(n):
        if i % 2 == 1:
            yield i

def evens(n):
    for i in range(n):
        if i % 2 == 0:
            yield i
def odd_even(n):
    yield from odds(n)
    yield from evens(n)
```

#### 1.5.2. yield && Coroutine

TODO: https://lotabout.me/2017/Python-Generator/

```python
def averager():
    sum = 0
    num = 0
    while True:
        sum += (yield sum / num if num > 0 else 0)
        num += 1

x = averager()
x.send(None)
#=> 0
x.send(1)
#=> 1.0
x.send(2)
#=> 1.5
x.send(3)
#=> 2.0
```

## 2. 推导式 (comprehensions)

推导式是从一个或者多个迭代器快速简洁地创建数据结构的一种方法。它可以将循环和条件判断结合，从而避免语法冗长的代码。使用推导式更符合 Python 的代码风格。

### 2.1. 列表推导式 (list comprehensions)

[List Comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)

语法：`[expression for item in iterable if condition]`，返回一个 list 对象。

例 1：
```python
>>> for n in [number*2 for number in range(1,6) if number % 2 == 1]:
...     print(n)
...
2
6
10
```
例 2：
```python
>>> rows = range(1,4)
>>> cols = range(1,3)
>>> cells = [(row, col) for row in rows for col in cols]
>>> for row, col in cells:
... print(row, col)
...
1 1
1 2
2 1
2 2
3 1
3 2
```
例 3:
```python
>>> matrix = [
...     [1, 2, 3, 4],
...     [5, 6, 7, 8],
...     [9, 10, 11, 12],
... ]
>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

### 2.2. 字典推导式 (dict comprehensions)

语法：`{ key_expression : value_expression for expression in iterable if condition }`，返回一个 dict 对象。

例：对字符串 'letters' 中出现的字母进行循环，计算出每个字母出现的次数
```python
>>> word = 'letters'
>>> letter_counts = {letter: word.count(letter) for letter in set(word)}
>>> letter_counts
{'t': 2, 'l': 1, 'e': 2, 'r': 1, 's': 1}
# 字符串中 t 和 e 都出现了两次，第一次调用 word.count() 时已经计算得到相应的值，利用集合使每个字母只出现一次，避免两次调用 word.count(letter) 浪费时间
```

### 2.3. 集合推导式 (set comprehensions)

语法：`{ value_expression for expression in iterable if condition }`，返回一个 set 对象。

例：
```python
>>> a_set = {number for number in range(1,6) if number % 3 == 1}
>>> a_set
{1, 4}
```

### 2.4. 生成器推导式 (generator comprehensions)

元组没有推导式，**使用圆括号创建的是一个生成器推导式**，生成器推导式返回一个生成器对象：
```python
>>> number_thing = (number for number in range(1, 6))
>>> type(number_thing)
<class 'generotor'>
```
可以直接对生成器对象进行迭代：
```python
>>> for number in number_thing:
...   print(number)
...
1
2
3
4
5
```

## 3. 函数

代码复用的第一步是使用函数，它是命名的用于区分的代码段。 函数可以接受任何数字或者其他类型的输入作为参数，并且返回数字或者其他类型的结果；

函数命名规范和变量命名一样（必须使用字母或者下划线 _ 开头，仅能含有字母、数字和下划线）；

参数传递为值传递：当调用含参数的函数时， 这些参数的值会被复制给函数中的对应参数；

一个函数可以接受任何数量（包括 0）的任何类型的值作为输入变量，并且返回任何数量（包括 0）的任何类型的结果。如果函数不显式调用 return 函数，那么会默认返回 None；

### 3.1. 参数

#### 3.1.1. 参数类型

##### 3.1.1.1. 位置参数

位置参数即按参数的位置顺序依次传递参数值。尽管这种方式很常见，但是位置参数的一个弊端是必须熟记每个位置的参数的含义。

例：
```python
>>> def menu(wine, entree, dessert):
...   return {'wine': wine, 'entree': entree, 'dessert': dessert}
...
>>> menu('chardonnay', 'chicken', 'cake')
```

##### 3.1.1.2. 关键字参数

为了避免位置参数带来的混乱，调用参数时可以指定对应参数的名字，甚至可以采用与函数定义不同的顺序调用：
```python
>>> menu(entree='beef', dessert='bagel', wine='bordeaux')
{'dessert': 'bagel', 'wine': 'bordeaux', 'entree': 'beef'}
```
可以把位置参数和关键字参数混合使用，如果同时出现两种参数形式，首先应该考虑的是位置参数：
```python
>>> menu('frontenac', dessert='flan', entree='fish')
```

#### 3.1.2. 参数默认值

```python
>>> def menu(wine, entree, dessert='pudding'):
... return {'wine': wine, 'entree': entree, 'dessert': dessert}
```

NOTE: 默认参数值在函数被定义时已经计算出来，而不是在程序运行时。**常见的一个错误是把可变的数据类型（例如列表或者字典）当作默认参数值**。

##### 3.1.2.1. 使用函数参数的默认值来实现函数静态变量的功能

Python 中是不支持静态变量的，但是我们可以通过函数的默认值，基于 “**Python 中函数默认值的初始化只会被执行一次**“ 这一理论，来实现静态变量的功能。

当函数的默认值是内容是可变的类（如列表）时，类的内容可变，而类的名字没变。（相当于开辟的内存区域没有变，而其中内容可以变化）。

例：TODO:
```python
def f(a, L=[]):
    L.append(a)
    return L

print f(1)
print f(2)
print f(3)
print f(4,['x'])
print f(5)
输出是：
[1]
[1, 2]
[1, 2, 3]
['x', 4]
[1, 2, 3, 5]
```

为什么最后 “print f(5)”的输出是 “[1, 2, 3, 5]”呢？

这是因为 “print f(4,['x'])”时，默认变量并没有被改变，因为默认变量的初始化只是被执行了一次（第一次使用默认值调用)，初始化执行开辟的内存区（我们可以称之为默认变量）没有被改变，所以最后的输出结果是“[1, 2, 3, 5]”。

#### 3.1.3. 使用 * 和 ** 收集参数

- 使用 * 收集位置参数

  当参数被用在函数内部时，星号将一组可变数量的位置参数集合成参数值的**元组**。
  ```python
  >>> def print_args(*args):
  ...   print('Positional argument tuple:', args)
  >>> print_args(3, 2, 1, 'wait!', 'uh...')
  Positional argument tuple: (3, 2, 1, 'wait!', 'uh...')
  ```
  若函数同时有限定的位置参数，那么 `*args` 会收集剩下的参数。

- 使用 ** 收集关键字参数

  使用两个星号可以将参数收集到一个字典中，参数的名字是字典的键，对应参数的值是**字典**的值。
  ```python
  >>> def print_kwargs(**kwargs):
  ...   print('Keyword arguments:', kwargs)
  >>> print_kwargs(wine='merlot', entree='mutton', dessert='macaroon')
  Keyword arguments: {'dessert': 'macaroon', 'wine': 'merlot', 'entree': 'mutton'}
  ```

NOTE: 当调用含 `*` 或 `**` 参数的方法时，如果使用的是位置参数，则自动传给 `*` 参数；如果使用的是关键字参数，则自动传给 `**` 参数：
```python
In [1]: def f(*args, **kwargs):
   ...:     print args
   ...:     print '------'
   ...:     print kwargs
   ...:

In [2]: f(k='v')
()
------
{'k': 'v'}

In [3]: f('v')
('v',)
------
{}
```

#### 3.1.4. 参数传递方式

**在 Python 中，函数参数的传递方式都是引用传递**。事实上，Python 中函数参数的引用传递可以理解为 C 语言中的 const 指针 (your_type* const your_variable) 参数传递，**传递的是指针（地址），它所指向的对象可以被修改产生副作用，但变量本身不能修改指向其他对象**。

e.g.1
```python
def test_immutable_for_simpledata(args):
    print('============')
    print(2, id(args))
    args = args + 10
    print(3, args)
    print(4, id(args))
    print('============')

if __name__ == '__main__':
    cur = 5000
    print(1, id(cur))
    test_immutable_for_simpledata(cur)
    print(5, cur)
    print(6, id(cur))
```
运行结果：
```
1 42301856
============
2 42301856
3 5010
4 42301616
============
5 5000
6 42301856
```
函数内和函数外打印出来的地址是一样的，这*可以证明 Python 对不可变类型传参是使用的引用传参*，但是为什么函数中用了 args+10，函数外面的 args 值还是没变呢？我们可以看到第四步的 args 的地址其实已经变了。即说明加 10 和未加 10 两个时间点是两个不同的对象。为什么是这样呢？这里是**基本类型的不可变对象，对它进行运算操作其实就是创建新的对象，然后将原先的变量名绑定到新的对象上**。

e.g.2
```python
def test_immutable_for_tuple(args):
    print('============')
    print(2, id(args))
    args[0].append(5)
    print(3, args)
    print(4, id(args))
    print('============')
if __name__ == '__main__':
    cur = ([0], 1)
    print(1, id(cur))
    test_immutable_for_tuple(cur)
    print(5, cur)
    print(6, id(cur))
```
运行结果：
```
1 33951020
============
2 33951020
3 ([0, 5], 1)
4 33951020
============
5 ([0, 5], 1)
6 33951020
```
对于不可变对象的函数传参，依然是传的引用（地址)。对于简单类型，在函数内对其操作之所以不会影响函数范围外的值，是因为运算中的赋值操作产生了新的对象，而不是对原有对象的改变。而对于一些复杂类型，就可以比较清晰的看出，函数内参数的改变同样会影响函数外的变量。

e.g.3
```python
def test(list):
    list = [1]

if __name__ == '__main__':
    ls = [1, 2, 3]

    test(ls)
    print(ls)
```
运行结果：
```
[1, 2, 3]
```
在 test 方法中，list 变量存的是 `[1, 2, 3]` 的地址值，因此当执行 `list = [1]` 后，list 变量所存放的值变成了新 list 对象 `[1]` 的地址，但这并不影响 `ls` 仍然存放的是 `[1, 2, 3]` 的地址值。

e.g.4
```python
def test(list):
    list[0] = 2

if __name__ == '__main__':
    ls = [1, 2, 3]

    test(ls)
    print(ls)
```
运行结果：
```
[2, 2, 3]
```
在 test 方法中，list 变量存的是 `[1, 2, 3]` 的地址值，因此当执行 `list[0] = 2` 后，将 `[1, 2, 3]` 对象更改为了 `[2, 2, 3]`，因此也影响到了 `ls`。

### 3.2. 函数文档字符串

#### 3.2.1. 定义

```python
# 可以定义非常长的文档字符串，加上详细的规范说明
def print_if_true(thing, check):
  '''
  Prints the first argument if a second argument is true.
  The operation is:
  1. Check whether the *second* argument is true.
  2. If it is, print the *first* argument.
  '''
  if check:
  print(thing)
```
#### 3.2.2. 输出

1.	使用 help() 可以打印输出一个函数的参数列表和规范的文档，如 help(echo)；

2.	使用 print（functionName.__doc__) 可打印函数的文档字符串；

### 3.3. 内部函数

当需要在函数内部多次执行复杂的任务时，内部函数是非常有用的，从而避免了循环和代码的堆叠重复；

例：
```python
>>> def knights(saying):
... 	def inner(quote):
... 		return "We are the knights who say: '%s'" % quote
... 	return inner(saying)
>>> knights('Ni!')
"We are the knights who say: 'Ni!'"
```

### 3.4. 闭包

闭包是一个可以由另一个函数动态生成的实例函数，并且可以改变和存储函数外创建的变量的值。

例：
```python
>>> def knights2(saying):
... 	def inner2():
... 		return "We are the knights who say: '%s'" % saying
... 	return inner2
... 	# knights2() 返回的是 inner2 函数的复制（没有直接调用）。所以它就是一个闭包：一个被动态创建的可以记录外部变量的函数；
...
```
用不同的参数调用 knights2()，分别赋值给 a 和 b：
```python
>>> a = knights2('Duck')
>>> b = knights2('Hasenpfeffer')
>>> type(a)
<class 'function'>
>>> type(b)
<class 'function'>
```
它们是函数，同时也是闭包：
```python
>>> a
<function knights2.<locals>.inner2 at 0x10193e158>
>>> b
<function knights2.<locals>.inner2 at 0x10193e1e0>
```
如果调用它们，它们会记录被 knights2 函数创建时的外部变量参数 saying：
```python
>>> a()
"We are the knights who say: 'Duck'"
>>> b()
"We are the knights who say: 'Hasenpfeffer'"
```

NOTE：一般情况下，如果一个函数结束，函数的内部所有东西都会被释放，局部变量都会消失。闭包是一种特殊情况，如果外函数在结束时发现有自己的临时变量将会在内部函数中用到，就会把这个临时变量绑定给了内部函数，然后再结束。

### 3.5. 匿名函数：lambda()

Python 中， lambda 函数是用一个语句表达的匿名函数。可以用它来代替小的函数，常用于简化回调函数的定义；

例：
```python
>>> def edit_story(words, func):
...   for word in words:
...     print(func(word))
>>> stairs = ['thud', 'meow', 'thud', 'hiss']
```
调用 edit_story 函数的普通写法：
```python
>>> def enliven(word): # 让这些单词更有情感
...   return word.capitalize() + '!'
>>> edit_story(stairs, enliven)
Thud!
Meow!
Thud!
Hiss!
```
调用 edit_story 函数的 lambda 函数写法：
```python
# lambda 函数接收一个参数 word，在冒号和末尾圆括号之间的部分为函数的定义；
# 语法： lambda 参数名：返回值表达式
>>> edit_story(stairs, lambda word: word.capitalize() + '!')
Thud!
Meow!
Thud!
Hiss!
```

## 4. 模块、包

### 4.1. 命令行参数

```python
import sys
print(‘program arguments: ’, sys.argv)
```
命令行中：
```shell
$ python test2.py
Program arguments: ['test2.py']
$ python test2.py tra la la
Program arguments: ['test2.py', 'tra', 'la', 'la']
```

### 4.2. import 语句

- import 语句使用形式：

  1)	`import subpackage1.a # 将模块 subpackage.a 导入全局命名空间，例如访问 a 中属性时用 subpackage1.a.attr`

  2)	`from subpackage1 import a #　将模块 a 导入全局命名空间，例如访问 a 中属性时用 a.attr_a`

  3)	`from subpackage.a import attr_a # 将模块 a 的属性直接导入到命名空间中，例如访问 a 中属性时直接用 attr_a`

  注：使用 from 语句可以把模块直接导入当前命名空间，from 语句并不引用导入对象的命名空间，而是将被导入对象直接引入当前命名空间；

- 引用机制：可以被 import 语句导入的对象是以下类型：

  - 模块文件（.py 文件）

  - C 或 C++ 扩展（已编译为共享库或 DLL 文件）

  - 包（包含多个模块）

  - 内建模块（使用 C 编写并已链接到 Python 解释器中）

- 模块通常为单独的。py 文件，可以用 import 直接引用，可以作为模块的文件类型有。py、.pyo、.pyc、.pyd、.so、.dll；

- import 语句可以在程序的任何位置使用，你可以在程序中多次导入同一个模块，但模块中的代码仅仅在该模块被首次导入时执行。后面的 import 语句只是简单的创建一个到模块名字空间的引用而已；

- sys.modules 字典中保存着所有被导入模块的模块名到模块对象的映射；

- 当导入模块时，解释器按照 sys.path 列表中的目录顺序来查找导入文件（标准 sys 模块下的一系列目录名和 ZIP 压缩文件）：
  ```python
  import sys
  >>> print(sys.path)

  # Linux:
  ['', '/usr/local/lib/python3.4',
  '/usr/local/lib/python3.4/plat-sunos5',
  '/usr/local/lib/python3.4/lib-tk',
  '/usr/local/lib/python3.4/lib-dynload',
  '/usr/local/lib/python3.4/site-packages']

  # Windows:
  ['', 'C:\\WINDOWS\\system32\\python34.zip', 'C:\\Documents and Settings\\weizhong', 'C:\\Python34\\DLLs', 'C:\\Python34\\lib', 'C:\\Python34\\lib\\plat-win', 'C:\\Python34\\lib\\lib-tk', 'C:\\Python34\\Lib\\site-packages\\pythonwin', 'C:\\Python34', 'C:\\Python34\\lib\\site-packages', 'C:\\Python34\\lib\\site-packages\\win32', 'C:\\Python34\\lib\\site-packages\\win32\\lib', 'C:\\Python34\\lib\\site-packages\\wx-2.6-msw-unicode']

  # 其中 list 第一个元素空字符串代表当前目录
  ```

- 在导入模块后，解释器做以下工作：

  1)	已导入模块的名称创建新的命名空间，通过该命名空间就可以访问导入模块的属性和方法。

  2)	在新创建的命名空间中执行源代码文件。

  3)	创建一个名为源代码文件的对象，该对象引用模块的名字空间，这样就可以通过这个对象访问模块中的函数及变量

TODO: https://www.jianshu.com/p/4bb742d7d672

import glovar 和 from comon import glovar 的命名空间是一样的，key 都是 glovar

from common.glovar import x 则不一样，test.py 中这样 from import，就相当于在 test.py 文件中写了一行代码 x = 1,
此时 x 就是 test 自己命名空间中的变量。所以 x 只在 test.py 中有效，无聊自己如何对 x 修改，都无法影响 glovar 中的 x

### 4.3. 模块

模块可以在多个文件之间创建和使用 Python 代码；

一个模块就是一个 py 文件，模块名是不带 .py 扩展的另外一个 Python 文件的文件名；

如果你想要保证实例的唯一性， 使用模块是最好的选择。不管模块在程序中被引用多少次，始终只有一个实例被加载。（可以把 Python 模块理解为 Java/C++ 中的单例）；

#### 4.3.1. 导入整个模块

导入整个模块后，使用模块内对象只需加上模块名作为前缀即可；

例：
```python
def get_description(): #看到下面的文档字符串了吗？
"""Return random weather, just like the pros"""
from random import choice
possibilities = ['rain', 'snow', 'sleet', 'fog', 'sun', 'who knows']
return choice(possibilities)

import report
description = report.get_description()
print("Today's weather:", description)
```

#### 4.3.2. 导入模块的某一部分

直接导入模块的某一部分（如函数、类）后，可直接使用，不用前缀；
```python
from random import choice
possibilities = ['rain', 'snow', 'sleet', 'fog', 'sun', 'who knows']
return choice(possibilities)
```

#### 4.3.3. 导入后使用别名

```python
import report as wr
wr.get_description()
```

### 4.4. 包

为了使 Python 应用更具可扩展性，你可以把多个模块组织成文件层次，称之为包。

#### 4.4.1. 包结构

多个相关联的模块组成一个包，以便于维护和使用，同时能有限的避免命名空间的冲突。一般来说，包的结构可以是这样的：

```
package
  |- subpackage1
      |- __init__.py
      |- a.py
  |- subpackage2
      |- __init__.py
      |- b.py
```

#### 4.4.2. `__init__.py`

`__init__.py` 文件的作用是将文件夹变为一个 Python 模块，Python 中的每个模块的包中，都有`__init__.py` 文件。我们在导入一个包时，实际上是导入了它的`__init__.py`文件。

通常`__init__.py` 文件为空，但是我们还可以为它增加其他的功能：

- 批量导入模块

  可以在__init__.py 文件中批量导入我们所需要的模块，而不再需要一个一个的导入
  ```python
  # package
  # __init__.py
  import re
  import urllib
  import sys
  import os

  # a.py
  import package
  print(package.re, package.urllib, package.sys, package.os)
  ```

- 定义`__all__`变量
  ```python
  # __init__.py
  __all__ = ['os', 'sys', 're', 'urllib']

  # a.py
  # 会把注册在__init__.py 文件中__all__列表中的模块和包导入到当前文件中来
  from package import *
  ```

例：

1. 在 sources 目录下新建 weekly.py 和 daily.py：

    boxes/sources/daily.py：
    ```python
    def forecast():
      'fake daily forecast'
      return 'like yesterday
    ```
    boxes/sources/weekly.py：
    ```python
    def forecast():
      """Fake weekly forecast"""
      return ['snow', 'more snow', 'sleet', 'freezing rain', 'rain', 'fog', 'hail']
    ```

1. 在 sources 目录下添加一个文件： init.py。这个文件可以是空的，但是 Python 需要它，以便把该目录作为一个包；

1. 在主程序中使用包：

    boxes/weather.py：
    ```python
    from sources import daily, weekly

    print("Daily forecast:", daily.forecast())
    print("Weekly forecast:")
    for number, outlook in enumerate(weekly.forecast(), 1):
      print(number, outlook)
    ```

## 5. Refer Links
