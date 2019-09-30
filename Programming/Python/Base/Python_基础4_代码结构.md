
- [Python 基础：代码结构](#python-基础代码结构)
  - [1. 流程控制](#1-流程控制)
    - [1.1. if、 elif 和 else](#11-if-elif-和-else)
    - [1.2. while、else](#12-whileelse)
    - [1.3. for、else](#13-forelse)
      - [1.3.1. 使用 zip() 并行迭代](#131-使用-zip-并行迭代)
      - [1.3.2. 使用 range() 生成自然数序列](#132-使用-range-生成自然数序列)
  - [2. 推导式](#2-推导式)
    - [2.1. 列表推导式](#21-列表推导式)
    - [2.2. 字典推导式](#22-字典推导式)
    - [2.3. 集合推导式](#23-集合推导式)
    - [2.4. 生成器推导式](#24-生成器推导式)
  - [3. 函数](#3-函数)
    - [3.1. 参数](#31-参数)
      - [3.1.1. 位置参数](#311-位置参数)
        - [3.1.1.1. 关键字参数](#3111-关键字参数)
      - [3.1.2. 参数默认值](#312-参数默认值)
        - [3.1.2.1. 使用函数参数的默认值来实现函数静态变量的功能](#3121-使用函数参数的默认值来实现函数静态变量的功能)
      - [3.1.3. 使用*和**收集参数](#313-使用和收集参数)
        - [3.1.3.1. 使用*收集位置参数](#3131-使用收集位置参数)
        - [3.1.3.2. 使用**收集关键字参数](#3132-使用收集关键字参数)
    - [3.2. 函数文档字符串](#32-函数文档字符串)
      - [3.2.1. 定义](#321-定义)
      - [3.2.2. 输出](#322-输出)
    - [3.3. 内部函数](#33-内部函数)
    - [3.4. 闭包](#34-闭包)
    - [3.5. 匿名函数：lambda()](#35-匿名函数lambda)
  - [4. 生成器](#4-生成器)
    - [4.1. 可迭代对象](#41-可迭代对象)
    - [4.2. 生成器函数](#42-生成器函数)
  - [5. 装饰器](#5-装饰器)
  - [6. 模块、包](#6-模块包)
    - [6.1. 命令行参数](#61-命令行参数)
    - [6.2. import 语句](#62-import-语句)
    - [6.3. 模块](#63-模块)
      - [6.3.1. 导入整个模块](#631-导入整个模块)
      - [6.3.2. 导入模块的某一部分](#632-导入模块的某一部分)
      - [6.3.3. 导入后使用别名](#633-导入后使用别名)
    - [6.4. 包](#64-包)
      - [6.4.1. 包结构](#641-包结构)
      - [6.4.2. `__init__.py`](#642-__init__py)
  - [7. Refer Links](#7-refer-links)

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

- 对一个字典（或者字典的 keys() 函数）进行迭代将返回字典中的键，如果想对字典的值进行迭代，可以使用字典的 values() 函数，如果需要以元组的形式返回键值对进行迭代，可以使用字典的 items() 函数；

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

**注意：当使用 for .. in .. 进行迭代时，python 会将可迭代对象的所有迭代值都读到内存中，再进行迭代，因此，若迭代器包含了海量数据，直接迭代会导致大量内存被占用，此时可采用推导式或者生成器的方式。** TODO:

#### 1.3.1. 使用 zip() 并行迭代

zip() 函数：

函数返回值既不是元组也不是列表，而是一个整合多个序列的可迭代变量，函数在最短的序列“用完”时就会停止。
```python
>>> english = 'Monday', 'Tuesday', 'Wednesday'
>>> french = 'Lundi', 'Mardi', 'Mercredi‘
>>> list( zip(english, french) )
[('Monday', 'Lundi'), ('Tuesday', 'Mardi'), ('Wednesday', 'Mercredi')]
>>> dict( zip(english, french) )
{'Monday': 'Lundi', 'Tuesday': 'Mardi', 'Wednesday': 'Mercredi'}
```

通过 zip() 函数可以实现对多个序列进行并行迭代：
```python
>>> days = ['Monday', 'Tuesday', 'Wednesday']
>>> fruits = ['banana', 'orange', 'peach']
>>> drinks = ['coffee', 'tea', 'beer']
>>> desserts = ['tiramisu', 'ice cream', 'pie', 'pudding']
>>> for day, fruit, drink, dessert in zip(days, fruits, drinks, desserts):
... print(day, ": drink", drink, "- eat", fruit, "- enjoy", dessert)
...
Monday : drink coffee - eat banana - enjoy tiramisu
Tuesday : drink tea - eat orange - enjoy ice cream
Wednesday : drink beer - eat peach - enjoy pie
```

#### 1.3.2. 使用 range() 生成自然数序列

range（start, stop, step）返回在特定区间的自然数可迭代序列，即 start, start+step, start+2*step, ……, stop-1；

参数说明：
- start 的默认值为 0；
- stop 没有默认值，为必须参数；
- step 的默认值是 1；

例：
```python
>>> for x in range(0,3):
... print(x)
...
0
1
2
>>> for x in range(2, -1, -1):
... print(x)
...
2
1
0
>>> list( range(0, 11, 2) )
[0, 2, 4, 6, 8, 10]
```

## 2. 推导式

推导式是从一个或者多个迭代器快速简洁地创建数据结构的一种方法。它可以将循环和条件判断结合，从而避免语法冗长的代码。使用推导式更符合 Python 的代码风格。

### 2.1. 列表推导式

语法：`[expression for item in iterable if condition]`

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

### 2.2. 字典推导式

语法：`{ key_expression : value_expression for expression in iterable if condition }`

例：对字符串 'letters' 中出现的字母进行循环，计算出每个字母出现的次数
```python
>>> word = 'letters'
>>> letter_counts = {letter: word.count(letter) for letter in set(word)}
>>> letter_counts
{'t': 2, 'l': 1, 'e': 2, 'r': 1, 's': 1}
# 字符串中 t 和 e 都出现了两次，第一次调用 word.count() 时已经计算得到相应的值，利用集合使每个字母只出现一次，避免两次调用 word.count(letter) 浪费时间
```

### 2.3. 集合推导式

语法：`{ value_expression for expression in iterable if condition }`

例：
```python
>>> a_set = {number for number in range(1,6) if number % 3 == 1}
>>> a_set
{1, 4}
```

### 2.4. 生成器推导式

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
一个生成器只能运行一次。**列表、集合、字符串和字典都存储在内存中，但是生成器仅在运行中产生值，不会被存下来，所以不能重新使用或者备份一个生成器**。**如果想再一次迭代此生成器，会发现它被擦除了**：
```python
>>> try_again = list(number_thing)
>>> try_again
[]
```

## 3. 函数

代码复用的第一步是使用函数，它是命名的用于区分的代码段。 函数可以接受任何数字或者其他类型的输入作为参数，并且返回数字或者其他类型的结果；

函数命名规范和变量命名一样（必须使用字母或者下划线 _ 开头，仅能含有字母、数字和下划线）；

参数传递为值传递：当调用含参数的函数时， 这些参数的值会被复制给函数中的对应参数；

一个函数可以接受任何数量（包括 0）的任何类型的值作为输入变量，并且返回任何数量（包括 0）的任何类型的结果。如果函数不显式调用 return 函数，那么会默认返回 None；

### 3.1. 参数

TODO: 函数参数传值、传引用：http://winterttr.me/2015/10/24/python-passing-arguments-as-value-or-reference/

#### 3.1.1. 位置参数

即按参数的位置顺序依次传递参数值；

尽管这种方式很常见，但是位置参数的一个弊端是必须熟记每个位置的参数的含义；

例：
```python
>>> def menu(wine, entree, dessert):
...   return {'wine': wine, 'entree': entree, 'dessert': dessert}
...
>>> menu('chardonnay', 'chicken', 'cake')
```

##### 3.1.1.1. 关键字参数

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
注：

默认参数值在函数被定义时已经计算出来，而不是在程序运行时。**常见的一个错误是把可变的数据类型（例如列表或者字典）当作默认参数值**。

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

#### 3.1.3. 使用*和**收集参数

##### 3.1.3.1. 使用*收集位置参数

当参数被用在函数内部时，星号将一组可变数量的位置参数集合成参数值的**元组**；
```python
>>> def print_args(*args):
...   print('Positional argument tuple:', args)
>>> print_args(3, 2, 1, 'wait!', 'uh...')
Positional argument tuple: (3, 2, 1, 'wait!', 'uh...')
```
若函数同时有限定的位置参数，那么 *args 会收集剩下的参数；

##### 3.1.3.2. 使用**收集关键字参数

使用两个星号可以将参数收集到一个字典中，参数的名字是字典的键，对应参数的值是**字典**的值。
```python
>>> def print_kwargs(**kwargs):
...   print('Keyword arguments:', kwargs)
>>> print_kwargs(wine='merlot', entree='mutton', dessert='macaroon')
Keyword arguments: {'dessert': 'macaroon', 'wine': 'merlot', 'entree': 'mutton'}
```

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

闭包是一个可以由另一个函数动态生成的函数，并且可以改变和存储函数外创建的变量的值。

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

## 4. 生成器

参考文章：

https://pyzh.readthedocs.io/en/latest/the-python-yield-keyword-explained.html

http://www.jianshu.com/p/d09778f4e055

### 4.1. 可迭代对象

当你建立了一个列表，你可以逐项地读取这个列表，这叫做一个可迭代对象：
例：mylist 就是一个可迭代的对象
```python
>>> mylist = [1, 2, 3]
>>> for i in mylist :
...    print(i)
1
2
3
```
可迭代对象也可以使用列表推导式生成：
```python
>>> mylist = [x*x for x in range(3)]
>>> for i in mylist :
...    print(i)
0
1
4
```

生成器是用来创建 Python 可迭代对象的一个对象。使用它可以迭代庞大的序列，且不需要在内存中创建和存储整个序列。通常，生成器是为迭代器产生数据的，但它只能被读取一次，因为它并不把所有的值放在内存中，而是实时地生成数据；

例：
```python
>>> mygenerator = (x*x for x in range(3))
>>> for i in mygenerator :
...    print(i)
0
1
4
```
上边的代码执行完毕之后，不可以再次使用 for i in mygenerator , 因为生成器只能被使用一次；

每次迭代生成器时，它会记录上一次调用的位置，并且返回下一个值。这一点和普通的函数是不一样的，一般函数都不记录前一次调用，而且都会在函数的第一行开始执行；

### 4.2. 生成器函数

当需要创建一个比较大的可迭代对象时，若使用推导式的方式会导致代码很繁琐，这时就可以将序列的生成写成一个生成器函数，生成器函数和普通函数类似，但是它的返回值使用 yield 语句声明而不是 return，使用 yield 关键字的函数不再是普通函数，它返回一个生成器；

当迭代器对生成器函数进行迭代时，每迭代一次遇到 yield 时就返回 yield 后面的值作为迭代返回值。重点是：下一次迭代时，从上一次迭代遇到的 yield 后面的代码继续开始执行。直到将生成器函数执行完毕，生成器会返回一个异常，告知迭代器迭代结束；

带有 yield 的函数不仅仅只用于 for 循环中，而且可用于某个函数的参数，只要这个函数的参数允许迭代参数。比如 array.extend 函数，它的原型是 array.extend(iterable)

例：重写一个 range() 生成器
```python
>>> def my_range(first=0, last=10, step=1):
... 	number = first
... 	while number < last:
... 		yield number
... 	number += step
```
函数返回的是一个生成器对象
```python
>>> ranger = my_range(1, 5)
>>> ranger
<generator object my_range at 0x101a0a168>
```
使用
```python
>>> for x in ranger:
... print(x)
...
1
2
4
```
例：理解的关键在于：下次迭代时，代码从 yield 的下一跳语句开始执行
```python
def yield_test(n):
    for i in range(n):
        yield call(i)
        print("i=",i)
    #做一些其它的事情
    print("do something.")
    print("end.")

def call(i):
    return i*2

#使用 for 循环
for i in yield_test(5):
    print(i,",")
```
执行结果：
```python
>>>
0 ,
i= 0
2 ,
i= 1
4 ,
i= 2
6 ,
i= 3
8 ,
i= 4
do something.
end.
>>>

```

## 5. 装饰器

TODO:

## 6. 模块、包

### 6.1. 命令行参数

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

### 6.2. import 语句

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

### 6.3. 模块

模块可以在多个文件之间创建和使用 Python 代码；

一个模块就是一个 py 文件，模块名是不带 .py 扩展的另外一个 Python 文件的文件名；

如果你想要保证实例的唯一性， 使用模块是最好的选择。不管模块在程序中被引用多少次，始终只有一个实例被加载。（可以把 Python 模块理解为 Java/C++ 中的单例）；

#### 6.3.1. 导入整个模块

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

#### 6.3.2. 导入模块的某一部分

直接导入模块的某一部分（如函数、类）后，可直接使用，不用前缀；
```python
from random import choice
possibilities = ['rain', 'snow', 'sleet', 'fog', 'sun', 'who knows']
return choice(possibilities)
```

#### 6.3.3. 导入后使用别名

```python
import report as wr
wr.get_description()
```

### 6.4. 包

为了使 Python 应用更具可扩展性，你可以把多个模块组织成文件层次，称之为包。

#### 6.4.1. 包结构

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

#### 6.4.2. `__init__.py`

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

1.	在 sources 目录下新建 weekly.py 和 daily.py：

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

2.	在 sources 目录下添加一个文件： init.py。这个文件可以是空的，但是 Python 需要它，以便把该目录作为一个包；

3.	在主程序中使用包：

      boxes/weather.py：
      ```python
      from sources import daily, weekly

      print("Daily forecast:", daily.forecast())
      print("Weekly forecast:")
      for number, outlook in enumerate(weekly.forecast(), 1):
        print(number, outlook)
      ```

## 7. Refer Links
