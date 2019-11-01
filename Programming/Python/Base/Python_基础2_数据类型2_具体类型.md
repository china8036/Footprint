- [Python 基础：数据类型 - 基本类型](#python-基础数据类型---基本类型)
  - [1. Numeric Types](#1-numeric-types)
    - [1.1. int](#11-int)
      - [1.1.1. Boolean](#111-boolean)
    - [1.2. float](#12-float)
    - [1.3. complex](#13-complex)
  - [2. Sequence Types](#2-sequence-types)
    - [2.1. 列表 (list)](#21-列表-list)
      - [2.1.1. 特性](#211-特性)
      - [2.1.2. 常用操作](#212-常用操作)
      - [2.1.3. list 方法](#213-list-方法)
    - [2.2. 元组 (tuple)](#22-元组-tuple)
      - [2.2.1. 特性](#221-特性)
      - [2.2.2. 常用操作](#222-常用操作)
      - [2.2.3. 元组与列表的比较](#223-元组与列表的比较)
    - [2.3. 范围 (range)](#23-范围-range)
    - [2.4. Text Sequence Type: 字符串 (str)](#24-text-sequence-type-字符串-str)
      - [2.4.1. 字符串操作](#241-字符串操作)
      - [2.4.2. 字符串方法](#242-字符串方法)
      - [2.4.3. 字符串编码](#243-字符串编码)
      - [2.4.4. 字符串格式化](#244-字符串格式化)
    - [2.5. Binary Sequence Types: 字节串](#25-binary-sequence-types-字节串)
      - [2.5.1. 字节 (bytes)](#251-字节-bytes)
      - [2.5.2. 字节数组 (bytearray)](#252-字节数组-bytearray)
      - [2.5.3. memoryview](#253-memoryview)
  - [3. Mapping Types：字典 (dictionary)](#3-mapping-types字典-dictionary)
    - [3.1. 特性](#31-特性)
    - [3.2. 常用操作](#32-常用操作)
    - [3.3. dict 方法](#33-dict-方法)
  - [4. Set Types](#4-set-types)
    - [4.1. 集合 (set)](#41-集合-set)
      - [4.1.1. 特性](#411-特性)
      - [4.1.2. 常用操作](#412-常用操作)
      - [4.1.3. set 方法](#413-set-方法)
    - [4.2. frozenset](#42-frozenset)
  - [5. 迭代器 (Iterator Types)](#5-迭代器-iterator-types)
    - [5.1. 生成器 (Generator Types)](#51-生成器-generator-types)
    - [5.2. itertools](#52-itertools)
  - [6. 上下文管理器 (Context Manager Types)](#6-上下文管理器-context-manager-types)
    - [6.1. 实现方式](#61-实现方式)
    - [6.2. contextlib.contextmanager](#62-contextlibcontextmanager)
  - [7. 其它 Built-in 类型](#7-其它-built-in-类型)
    - [7.1. Modules](#71-modules)
    - [7.2. Classes and Class Instances](#72-classes-and-class-instances)
    - [7.3. Functions](#73-functions)
    - [7.4. Methods](#74-methods)
    - [7.5. Code Objects](#75-code-objects)
    - [7.6. Type Objects](#76-type-objects)
    - [7.7. The Null Object](#77-the-null-object)
    - [7.8. The Ellipsis Object](#78-the-ellipsis-object)
    - [7.9. The NotImplemented Object](#79-the-notimplemented-object)
  - [8. Refer Links](#8-refer-links)

# Python 基础：数据类型 - 基本类型

Python 是**强类型**的（strongly typed）的**动态**语言：
- 强类型：用户无法修改一个已有对象的类型，即使它包含的值是可变的。
- 动态：解释执行。

## 1. Numeric Types

There are three distinct numeric types: integers, floating point numbers, and complex numbers. In addition, Booleans are a subtype of integers.

- Integers have unlimited precision.
- Floating point numbers are usually implemented using double in C; (sys.float_info)
- Complex numbers have a real and imaginary part, which are each a floating point number.

The constructors int(), float(), and complex() can be used to produce numbers of a specific type.

### 1.1. int

- 大小

  在 Python 3 中， long 类型已不复存在，**int 类型变为可以存储任意大小的整数，甚至超过 64 位**；

- 基数

  在 Python 中，整数默认使用十进制数（以 10 为底），除非你在数字前添加前缀，显式地指定使用其他基数（base）。

  在 Python 中，除十进制外你还可以使用其他三种进制的数字：
  - 0b 或 0B 代表二进制（以 2 为底）
  - 0o 或 0O 代表八进制（以 8 为底）
  - 0x 或 0X 代表十六进制（以 16 为底）

  Python 解释器会打印出它们对应的十进制整数。

#### 1.1.1. Boolean

> Booleans are a subtype of integers.

Boolean 是一种特殊的 int 类型，也是 Python 里最简单的数据类型，它只有两个可选值： `True` 和 `False`。当转换为整数时，它们分别代表 1 和 0。

**下面的情况都会被 python 解释器认为是 False**：

| 类型       | 取值     |
| -------- | ------------- |
| 布尔       | 布尔  False     |
| null 类型  | null 类型  None |
| 整型       | 整型  0         |
| 浮点型      | 浮点型  0.0      |
| 空字符串     | 空字符串 ''       |
| 空列表      | 空列表  []       |
| 空元组      | 空元组  ()       |
| 空字典      | 空字典  {}       |
| 空集合  set | 空集合  set()    |

### 1.2. float

整数全部由数字组成，而浮点数（在 Python 里称为 float）包含非数字的小数点。

### 1.3. complex

## 2. Sequence Types

### 2.1. 列表 (list)

#### 2.1.1. 特性

1. 可变
1. 有序
1. 允许重复
1. 元素类型可不同

#### 2.1.2. 常用操作

- 创建

  - 列表可以由零个或多个元素组成，元素之间用逗号分开，整个列表被方括号所包裹：
    ```python
    >>> empty_list = [ ]
    >>> weekdays = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
    >>> big_birds = ['emu', 'ostrich', 'cassowary']
    >>> first_names = ['Graham', 'John', 'Terry', 'Terry', 'Michael']
    ```
  - 可以使用 list() 函数来创建一个空列表：
    ```python
    >>> another_empty_list = list()
    >>> another_empty_list
    []
    ```

  - 使用 list comprehensions 创建 list：
    ```python
    # 快速创建 10*10 的二维列表并全部初始化为 0
    N = [[0]*10 for i in range(10)]
    ```

- py2: 使用 [`map(function, iterable, ...)`](https://docs.python.org/2/library/functions.html#map) 遍历操作 list 的每一项并返回新的 list（py3 中 map() 返回一个 generator）：
  ```python
  # Apply function to every item of iterable and return a list of the results.
  >>>def square(x) :            # 计算平方数
  ...     return x ** 2
  ...
  >>> map(square, [1,2,3,4,5])   # 计算列表各个元素的平方
  [1, 4, 9, 16, 25]

  >>> map(lambda x: x ** 2, [1, 2, 3, 4, 5])  # 使用 lambda 匿名函数
  [1, 4, 9, 16, 25]

  >>> map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])  # 提供了两个列表，对相同位置的列表数据进行相加
  [3, 7, 11, 15, 19]
  ```

- py2: 使用 [`filter(function, iterable)`](https://docs.python.org/2/library/functions.html#filter) 过滤 list 中不符合 function（返回 False）的项并返回新的 list（py3 中 filter() 返回一个 generator）：
  ```python
  def is_odd(n):
      return n % 2 == 1

  print filter(is_odd, range(1, 101))
  ```

- 使用 [offset] 获取、修改元素

  和字符串一样，通过偏移量可以从列表中提取对应位置的元素：
  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo']
  >>> marxes[0]
  'Groucho'
  >>> marxes[2] = 'Wanda'
  >>> marxes
  ['Groucho', 'Chico', 'Wanda']
  ```

- 使用切片 [start:end:step] 提取元素

  列表的切片仍然是一个列表。
  ```python
  >>> marxes = ['Groucho', 'Chico,' 'Harpo']
  >>> marxes[0:2]
  ['Groucho', 'Chico']
  ```
  从列表的开头开始每 2 个提取一个元素：
  ```python
  >>> marxes[::2]
  ['Groucho', 'Harpo']
  ```
  从尾部开始提取，步长仍为 2：
  ```python
  >>> marxes[::-2]
  ['Harpo', 'Groucho']
  ```
  实现列表逆序：
  ```python
  >>> marxes[::-1]
  ['Harpo', 'Chico', 'Groucho']
  ```

- 使用 len() 获取长度

- 使用 in 判断值是否存在

  判断一个值是否存在于给定的列表中有许多方式，其中最具有 Python 风格的是使用 in：
  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
  >>> 'Groucho' in marxes
  True
  >>> 'Bob' in marxes
  False
  ```

- 使用 del 删除指定位置的元素

  当列表中一个元素被删除后，位于它后面的元素会自动往前移动填补空出的位置，且列表长度减 1。
  ```python
  >>> del marxes[-1]
  >>> marxes
  ['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
  ```
  del 是 Python 语句，是赋值语句（=）的逆过程：它将一个 Python 对象与它的名字分离。如果这个对象无其他名称引用，则其占用空间也被会清除

- 使用 enumerate() 遍历索引和元素值

  使用 [`enumerate(iterable, start=0)`](https://docs.python.org/3/library/functions.html#enumerate) 可获取列表中以 `[(0, seq[0]), (1, seq[1]), (2, seq[2]), ...]` 形式返回的 enumerate object，当指定 start 参数时，返回的 index 在原来的基础上加上 start。

  e.g.
  ```python
  >>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
  >>> list(enumerate(seasons))
  [(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
  >>> list(enumerate(seasons, start=1))
  [(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
  ```

  e.g.
  ```python
  >>> for i, v in enumerate(['tic', 'tac', 'toe']):
  ...     print(i, v)
  ...
  0 tic
  1 tac
  2 toe
  ```

  `enumerate()` 是一个 build-in function，通过 C++ 实现，其 Python 版的模拟实现如下：
  ```python
  # enumerate() 的模拟实现
  def enumerate(sequence, start=0):
      n = start
      for elem in sequence:
          yield n, elem
          n += 1
  ```

- 使用 zip() 同时遍历多个列表

  使用 [`zip(*iterables)`](https://docs.python.org/3/library/functions.html#zip) 可获取多个列表以 `[(v1, v2, v3...), ...]` 形式返回的 list。若多个列表的长度彼此不同，则返回 list 的长度以最短长度为准。

  e.g.
  ```python
  In [1]: l1=[1,1,1,1,1,1,1]
  In [2]: l2=[2,2,2,2,2,2]
  In [3]: l3=[3,3,3,3,3,3,3,3,3]

  In [4]: zip(l1, l2, l3)
  Out[4]: [(1, 2, 3), (1, 2, 3), (1, 2, 3), (1, 2, 3), (1, 2, 3), (1, 2, 3)]
  ```

  e.g.
  ```python
  >>> questions = ['name', 'quest', 'favorite color']
  >>> answers = ['lancelot', 'the holy grail', 'blue']
  >>> for q, a in zip(questions, answers):
  ...     print('What is your {0}?  It is {1}.'.format(q, a))
  ...
  What is your name?  It is lancelot.
  What is your quest?  It is the holy grail.
  What is your favorite color?  It is blue.
  ```

  通过 `zip(*zip(x, y))` 可对 zipped 的 x 和 y 进行 unzip：
  ```python
  >>> x = [1, 2, 3]
  >>> y = [4, 5, 6]
  >>> zipped = zip(x, y)
  >>> list(zipped)
  [(1, 4), (2, 5), (3, 6)]
  >>> x2, y2 = zip(*zip(x, y))
  >>> x == list(x2) and y == list(y2)
  True
  ```

  `zip()` 是一个 build-in function，通过 C++ 实现，其 Python 版的模拟实现如下：
  ```python
  # zip() 的模拟实现
  def zip(*iterables):
      # zip('ABCD', 'xy') --> Ax By
      sentinel = object()
      iterators = [iter(it) for it in iterables]
      while iterators:
          result = []
          for it in iterators:
              elem = next(it, sentinel)
              if elem is sentinel:
                  return
              result.append(elem)
          yield tuple(result)
  ```

  > Unlike in Python 2, the zip function in Python 3 returns an iterator.

  TODO: 为什么在 CPython2 中能找到 builtin_zip 的源码实现，而在 CPython3 中却找不到了 `grep -irF 'builtin_zip' ./*`？

#### 2.1.3. list 方法

- 使用 append() 添加元素至尾部

  ```python
  >>> marxes.append('Zeppo')
  >>> marxes
  ['Groucho', 'Chico', 'Harpo', 'Zeppo']
  ```

- 使用 extend() 或 +、+= 合并列表

  使用 extend() 可以将一个列表合并到另一个列表中。
  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
  >>> others = ['Gummo', 'Karl']
  >>> marxes.extend(others)
  >>> marxes
  ['Groucho', 'Chico', 'Harpo', 'Zeppo', 'Gummo', 'Karl']
  >>> marxes + others
  ['Groucho', 'Chico', 'Harpo', 'Zeppo', 'Gummo', 'Karl']
  >>> marxes += others
  >>> marxes
  ['Groucho', 'Chico', 'Harpo', 'Zeppo', 'Gummo', 'Karl']
  ```

- 使用 insert() 在指定位置插入元素
  ```python
  >>> marxes.insert(3, 'Gummo')
  >>> marxes
  ['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
  ```

- 使用 remove() 删除具有指定值的元素
  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
  >>> marxes.remove('Gummo')
  >>> marxes
  ['Groucho', 'Chico', 'Harpo', 'Zeppo']
  ```

- 使用 pop() 获取并删除指定位置的元素
  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
  >>> marxes.pop()
  'Zeppo'
  >>> marxes
  ['Groucho', 'Chico', 'Harpo']
  >>> marxes.pop(1)
  'Chico'
  >>> marxes
  ['Groucho', 'Harpo']
  ```

- 使用 index() 查询具有特定值的元素位
  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
  >>> marxes.index('Chico')
  1
  ```

- 使用 count() 记录特定值出现的次数
  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo']
  >>> marxes.count('Harpo')
  1
  >>> marxes.count('Bob')
  0
  ```

- 使用 sort() 重新排列元素

  列表方法 sort() 会对原列表进行排序，改变原列表内容；

  此外，python 还提供了通用函数 sorted()，会返回排好序的列表副本，原列表内容不变

  如果列表中的元素都是数字，它们会默认地被排列成从小到大的升序。如果元素都是字符串，则会按照字母表顺序排列：

  ```python
  >>> marxes = ['Groucho', 'Chico', 'Harpo']
  >>> sorted_marxes = sorted(marxes)
  >>> sorted_marxes
  ['Chico', 'Groucho', 'Harpo']
  ```
  有些时候甚至多种类型也可——例如整型和浮点型——只要它们之间能够自动地互相转换：
  ```python
  >>> numbers = [2, 1, 4.0, 3]
  >>> numbers.sort()
  >>> numbers
  [1, 2, 3, 4.0]
  ```
  默认的排序是升序的，通过添加参数 reverse=True 可以改变为降序排列

- 使用 = 赋值， 使用 copy() 复制

### 2.2. 元组 (tuple)

#### 2.2.1. 特性

1. 不可变。
1. 有序。
1. 允许重复。
1. 元素类型可不同。

#### 2.2.2. 常用操作

- 创建

  - 使用 () 创建一个空元组：
    ```python
    >>> empty_tuple = ()
    >>> empty_tuple
    ()
    ```

  - 使用后缀逗号创建非空元组：

    创建包含一个或多个元素的元组时，每一个元素后面都需要跟着一个逗号，即使只包含一个元素也不能省略：
    ```python
    >>> one_marx = 'Groucho',
    >>> one_marx
    ('Groucho',)
    ```
    如果创建的元组所包含的元素数量超过 1，最后一个元素后面的逗号可以省略：
    ```python
    >>> marx_tuple = 'Groucho', 'Chico', 'Harpo'
    >>> marx_tuple
    ('Groucho', 'Chico', 'Harpo')
    ```
    Python 的交互式解释器输出元组时会自动添加一对圆括号，**定义元组真正靠的是每个元素的后缀逗号而不是括号——但使用括号可以使程序更加清晰**；

- 元组解包

  ```python
  >>> marx_tuple = ('Groucho', 'Chico', 'Harpo')
  >>> a, b, c = marx_tuple
  >>> a
  'Groucho'
  >>> b
  'Chico'
  >>> c
  'Harpo
  ```
  **利用元组在一条语句中对多个变量的值进行交换，而不需要借助临时变量**：
  ```python
  >>> password = 'swordfish'
  >>> icecream = 'tuttifrutti'
  >>> password, icecream = icecream, password
  >>> password
  'tuttifrutti'
  >>> icecream
  'swordfish'
  ```

#### 2.2.3. 元组与列表的比较

- 相比列表，元组占用的空间较小

- 不会意外修改元组的值；

- 可以将元组用作字典的键；

- 命名元组可以作为对象的替代；

- 函数的参数是以元组形式传递的；

### 2.3. 范围 (range)

> The range type represents an immutable sequence of numbers and is commonly used for looping a specific number of times in for loops.

[range 对象](https://docs.python.org/3/library/stdtypes.html#ranges) 由 `range(start, stop[, step])` 方法创建：
Options:
- start 的默认值为 0。
- stop 没有默认值，为必须参数。
- step 的默认值是 1。

e.g.
```python
>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> list(range(1, 11))
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
>>> list(range(0, 30, 5))
[0, 5, 10, 15, 20, 25]
>>> list(range(0, 10, 3))
[0, 3, 6, 9]
>>> list(range(0, -10, -1))
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
>>> list(range(0))
[]
>>> list(range(1, 0))
[]
```

e.g.
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

### 2.4. Text Sequence Type: 字符串 (str)

字符串的本质是字符序列；

**Python 字符串是不可变的。你无法对原字符串进行修改，但可以将字符串的一部分复制到新字符串，来达到相同的修改效果；**

#### 2.4.1. 字符串操作

- 创建

  将一系列字符包裹在一对单引号或一对双引号中即可创建字符串，**无论使用哪种引号， Python 对字符串的处理方式都是一样的，没有任何区别**；

  三元引号在创建短字符串时没有什么特殊用处。它多用于创建多行字符串；

- 输出

  `print()` 会**把包裹字符串的引号截去**，仅输出其实际内容，易于阅读。它还会**自动地在各个输出部分之间添加空格**，并**在所有输出的最后添加换行符**：
  ```python
  >>> print(99, 'bottles', 'would be enough.')
  99 bottles would be enough.
  ```

- 使用`+`进行拼接

  在 Python 中，你可以使用 `+` 将多个字符串或字符串变量拼接起来
  ```python
  >>> 'Release the kraken! ' + 'At once!'
  'Release the kraken! At once!‘
  ```
  也可以直接将一个字面字符串（非字符串变量）放到另一个的后面直接实现拼接：
  ```python
  >>> "My word! " "A gentleman caller!"
  'My word! A gentleman caller!'
  ```
  进行字符串拼接时， Python 并不会自动为你添加空格，需要显示定义（调用 print() 进行打印时， Python 会在各个参数之间自动添加空格并在结尾添加换行符）。

- 使用`*`进行复制

  ```python
  >>> start = 'Na ' * 4 + '\n'
  >>> middle = 'Hey ' * 3 + '\n'
  >>> end = 'Goodbye.'
  >>> print(start + start + middle + end)
  Na Na Na Na
  Hey Hey Hey
  Goodbye.
  ```

- 使用`[]`提取字符

  在字符串名后面添加 []，并在括号里指定偏移量可以提取该位置的单个字符。

  - 第一个字符（最左侧）的偏移量为 0，下一个是 1，以此类推。

  - 最后一个字符（最右侧）的偏移量也可以用 -1 表示，这样就不必从头数到尾。偏移量从右到左紧接着为 -2、 -3，以此类推。

  ```python
  >>> letters = 'abcdefghijklmnopqrstuvwxyz'
  >>> letters[0]
  'a'
  >>> letters[1]
  'b
  >>> letters[-1]
  'z'
  >>> letters[-2]
  'y'
  ```
  **由于字符串是不可变的，因此你无法直接插入字符或改变指定位置的字符，也就是说所有提取的字符都是只读的**：
  ```python
  >>> name = 'Henny'
  >>> naume[0] = 'P'
  Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  TypeError: 'str' object does not support item assignment
  ```

  **为了达到改变字符串的效果，需要组合使用一些字符串函数**，例如 replace()，以及分片操作
  ```python
  >>> name = 'Henny'
  >>> name.replace('H', 'P')
  'Penny'
  >>> 'P' + name[1:]
  'Penny'
  ```

- 使用`[start:end:step]`分片

  分片操作（slice） 可以从一个字符串中抽取子字符串（字符串的一部分）。我们使用一对方括号、起始偏移量 start、终止偏移量 end 以及可选的步长 step 来定义一个分片。其中一些可以省略。分片得到的子串包含从 start 开始到 end 之前的全部字符。

  - `[:]` 提取从开头到结尾的整个字符串

  - `[start:]` 从 start 提取到结尾

  - `[:end]` 从开头提取到 end - 1

  - `[start:end]` 从 start 提取到 end - 1

  - `[start:end:step]` 从 start 提取到 end - 1，每 step 个字符提取一个

- 使用 len() 获取长度

  len() 函数可用于计算字符串（以及其它序列类型）包含的字符数：
  ```python
  >>> len(letters)
  26
  >>> empty = ""
  >>> len(empty)
  0
  ```

  如果你调用 len() 函数试图获取一个对象的长度，实际上，**在 len() 函数内部，它自动去调用该对象的__len__() 方法**，因此，对于自己写的类，如果也想用 len(myObj) 的话，就实现一个__len__() 方法即可，例：
  ```python
  >>> class MyDog(object):
  ...     def __len__(self):
  ...         return 100
  ...
  >>> dog = MyDog()
  >>> len(dog)
  100
  ```

#### 2.4.2. 字符串方法

**字符串方法需要通过字符串调用。**

- 使用 split() 分割

  ```python
  str.split(str="", num=string.count(str))
  ```
  - str -- 分隔符，默认为所有的空字符，包括空格、换行 (\n)、制表符 (\t) 等。
  - num -- 分割次数。默认为 -1, 即分隔所有。设置为 1 可将字符串分割为 2 部分。

  与广义函数 len() 不同，split 函数只适用于字符串类型。

  使用 split() 可以基于分隔符将字符串分割成由若干子串组成的列表。
  ```python
  >>> todos = 'get gloves,get mask,give cat vitamins,call ambulance'
  >>> todos.split(',')
  ['get gloves', 'get mask', 'give cat vitamins', 'call ambulance']
  ```
  如果不指定分隔符，那么 split() 将默认使用空白字符——换行符、空格、制表符。

- 用 join() 合并

  join() 函数与 split() 函数正好相反：它将包含若干子串的列表分
  ```python
  >>> crypto_list = ['Yeti', 'Bigfoot', 'Loch Ness Monster']
  >>> crypto_string = ', '.join(crypto_list) #join 函数是字符串方法，需要通过字符串调用
  >>> print('Found and signing book deals:', crypto_string)
  Found and signing book deals: Yeti, Bigfoot, Loch Ness Monster 解，并将这些子串合成一个完整的大的字符串。
  ```

- replace()

  使用 replace() 函数可以进行简单的子串替换。你需要传入的参数包括：需要被替换的子串，用于替换的新子串，以及需要替换多少处。**最后一个参数如果省略则默认只替换第一次出现的位置**：
  ```python
  >>> setup.replace('duck', 'marmoset')
  'a marmoset goes into a bar...'
  ```
  修改最多 100 处：
  ```python
  >>> setup.replace('a ', 'a famous ', 100)
  'a famous duck goes into a famous bar...'
  ```
  **也可使用正则表达式匹配**。

- strip()

  strip() 方法用于移除字符串头尾指定的字符（默认为空格或换行符）或字符序列。此外还有 lstrip() 和 rstrip()。

  ```python
  >>> setup = 'a duck goes into a bar...'
  >>> setup.strip('.')
  'a duck goes into a bar'
  ```

  若要去除字符串中间的空格，可使用 replace() 函数：
  ```python
  >>> setup = 'a duck goes into a bar...'
  >>> setup.replace(' ', '')
  'aduckgoesintoabar...'
  ```

  NOTE：由于字符串是不可变的，上述方法实际上没有一个对 setup 真正做了修改。它们都仅仅是获取了 setup 的值，进行某些操作后将操作结果赋值给了另一个新的字符串而已。

- 大小写转换

  让字符串首字母变成大写：
  ```python
  >>> setup.capitalize()
  'A duck goes into a bar...'
  ```

  让所有单词的开头字母变成大写：
  ```python
  >>> setup.title()
  'A Duck Goes Into A Bar...'
  ```

  让所有字母都变成大写：
  ```python
  >>> setup.upper()
  'A DUCK GOES INTO A BAR...'
  ```

  将所有字母转换成小写：
  ```python
  >>> setup.lower()
  'a duck goes into a bar...'
  ```

  将所有字母的大小写转换：
  ```python
  >>> setup.swapcase()
  'a DUCK GOES INTO A BAR...'
  ```

- 格式排版

  在 30 个字符位居中：
  ```python
  >>> setup.center(30)
  ' a duck goes into a bar... '
  ```
  左对齐：
  ```python
  >>> setup.ljust(30)
  'a duck goes into a bar... '
  ```
  右对齐：
  ```python
  >>> setup.rjust(30)
  ' a duck goes into a bar...'
  ```

#### 2.4.3. 字符串编码

- python2

  在 Python 2 下进行中文输入输出是个危机四伏的事，特别是在你的代码里混合使用 str 与 unicode 时。

- python3

  在 Python 3 已经取消了 str，让所有的字符串都是 unicode。

#### 2.4.4. 字符串格式化

https://www.cnblogs.com/zhaopanpan/p/8875765.html

### 2.5. Binary Sequence Types: 字节串

> The core built-in types for manipulating binary data are bytes and bytearray.
>
> They are supported by memoryview which uses the buffer protocol to access the memory of other binary objects without needing to make a copy.

字节 / 字节数组是 **Python3 中新增**的数据类型，它是一个由字节数据组成的不可变序列，序列中的每个元素都由 8 bit （2 个十六进制位）组成。由于和字符串一样是序列类型，bytes 和 str 除操作的数据单元不同之外，它们支持的所有方法都基本相同。

由于 bytes 保存的就是原始的字节（二进制格式）数据，因此 bytes 对象可用于在网络上传输数据，也可用于存储各种二进制格式的文件，比如图片、音乐等文件。

#### 2.5.1. 字节 (bytes)

- 创建

  - 直接在字符串前添加 b，可通过字符串来构造 bytes 对象，但此时要求字符串都是 ASCII 字符，否则会报错。
    ```python
    # 通过 b 前缀指定 hello 是 bytes 类型的值
    b = b'hello'
    print(b)  # b'hello'
    print(b[0])  # 104  # 如果单独取出一个元素，以字节形式展示
    print(b[2:4])  # b'll'
    a=b'测试'  # SyntaxError: bytes can only contain ASCII literal characters.
    ```

  - 通过字符串本身的 encode() 方法将字符串按指定字符集转换成字节串，如果不指定字符集，默认使用 UTF-8 字符集。
    ```python
    b = "学习 Python 很有趣".encode('utf-8')
    print(b)  # b'\xe5\xad\xa6\xe4\xb9\xa0Python\xe5\xbe\x88\xe6\x9c\x89\xe8\xb6\xa3'
    ```

  - 使用 bytes() 方法来构造 bytes 对象

    ```python
    class bytes([source[, encoding[, errors]]])
    ```
    Options:
    - source
      - 如果 source 为整数，则返回一个长度为 source 的初始化数组。
      - 如果 source 为字符串，则按照指定的 encoding 将字符串转换为字节序列。如果不指定字符集，默认使用 UTF-8 字符集。
      - 如果 source 为可迭代类型，则元素必须为 [0 ,255] 中的整数。
      - 如果 source 为与 buffer 接口一致的对象，则此对象也可以被用于初始化 bytearray。
    - 如果没有输入任何参数，默认初始化数组为 0 个元素。

    e.g.
    ```python
    >>>a = bytes([1,2,3,4])
    >>> a
    b'\x01\x02\x03\x04'
    >>> type(a)
    <class 'bytes'>
    >>>
    >>> a = bytes('hello, python 编程', encoding='utf-8')
    >>> a
    b'hello, python \xe7\xbc\x96\xe7\xa8\x8b'
    >>> type(a)
    <class 'bytes'>
    >>>
    ```

- 将 bytes 对象转换为 str：
  - 通过字符串本身的 decode() 方法将字节串按指定字符集转换成字符串：
    ```python
    a = bytes('hello, python 编程', encoding='utf-8')
    st = a.decode('utf-8')  # 将 bytes 对象解码成字符串，默认使用 UTF-8 进行解码
    print(st) # hello, python 编程
    ```
  - 使用 str() 方法：
    ```python
    b = bytes('hello, python 编程', encoding='utf-8')
    s = str(b, encoding = "utf-8")
    print(s)  # hello, python 编程
    ```

#### 2.5.2. 字节数组 (bytearray)

bytearray 是 bytes 的可变版本。

e.g.
```python
>>> by = b'abcd\x65'
>>> barr = bytearray(by)
>>> barr
bytearray(b'abcde\xff')
>>> barr[0] = 120
>>> barr
bytearray(b'xbcde\xff')
```

#### 2.5.3. memoryview

## 3. Mapping Types：字典 (dictionary)

### 3.1. 特性

1. 无序性
1. 键不可变，键值可变
1. 元素允许重复

### 3.2. 常用操作

- 创建

  用大括号（{}）将一系列以逗号隔开的键值对（key:value）包裹起来即可进行字典的创建。

  - 创建空字典，它不包含任何键值对：
    ```python
    >>> empty_dict = {}
    >>> empty_dict
    {}
    ```

  - 创建非空字典：
    ```python
    >>> bierce = {
    ... "day": "A period of twenty-four hours, mostly misspent",
    ... "positive": "Mistaken at the top of one's voice",
    ... "misfortune": "The kind of fortune that never misses",
    ... }
    # Python 允许在列表、元组或字典的最后一个元素后面添加逗号，这不会产生任何问题。
    ```
    在交互式解释器中输入字典名会打印出它所包含的所有键值对：
    ```python
    >>> bierce
    {'misfortune': 'The kind of fortune that never misses',
    'positive': "Mistaken at the top of one's voice",
    'day': 'A period of twenty-four hours, mostly misspent'}
    ```
    注：字典中元素的顺序是无关紧要的，实际存储顺序可能取决于你添加元素的顺序；

- 使用 [key] 添加或修改元素

  向字典中添加元素非常简单，只需指定该元素的键并赋予相应的值即可。
  - 如果该元素的键已经存在于字典中， 那么该键对应的旧值会被新值取代。
  - 如果该元素的键并未在字典中出现，则会被加入字典。
  与列表不同，不需要担心赋值过程中 Python 会抛出越界异常。

- 使用 del 删除具有指定键的元素

  ```python
  >>> del pythons['Marx']
  ```

- 使用 in 判断是否存在

  用于判断某一个键（**不是键值**）是否存在于一个字典中；

### 3.3. dict 方法

- 使用 clear() 删除所有元素

- 使用 update() 合并字典

  使用 update() 可以将一个字典的键值对复制到另一个字典中去。如果待添加的字典与待扩充的字典包含同样的键，新归入字典的值会取代原有的值。

- 使用 get() 获取键值

  get() 方法需要指定字典名，键以及一个可选值。如果键存在，会得到与之对应的值：
  ```python
  >>> pythons.get('Cleese')
  'John'
  ```
  若键不存在，且指定了可选值，那么 get() 函数将返回这个可选值：
  ```python
  >>> pythons.get('Marx', 'Not a Python')
  'Not a Python'
  ```
  否则，会得到 None（在交互式解释器中什么也不会显示）：
  ```python
  >>> pythons.get('Marx')
  >>>
  ```

  P.S. 如果直接使用 [key] 获取键值，如果该键不存在将会抛出 KeyError 异常。

- 使用 keys() 获取所有键

  在 Python 3 中，keys() 方法会返回 `dict_keys()`，它是键的迭代形式，需要调用 list() 将 dict_keys 转换为列表类型。
  ```python
  >>> signals = {'green': 'go', 'yellow': 'go faster', 'red': 'smile for the camera'}
  >>> signals.keys()
  dict_keys(['green', 'red', 'yellow'])
  >>> list( signals.keys() )
  ['green', 'red', 'yellow']
  ```

- 使用 values() 获取所有值

  ```python
  >>> list( signals.values() )
  ['go', 'smile for the camera', 'go faster']
  ```

- 使用 items() 获取所有键值对

  ```python
  >>> list( signals.items() )
  [('green', 'go'), ('red', 'smile for the camera'), ('yellow', 'go faster')]
  ```

  每一个键值对以元组的形式返回。

  NOTE：
  - 对于 Python < 3.6，遍历字典时，键值对的返回顺序与存储顺序是不同的。Python 不关心键值对的顺序，而只关心键和值之间的映射关系。

    e.g.
    ```python
    user_0 = {
        'username':'efermi',
        'first':'enrico',
        'last':'fermi'
    }

    for key,value in user_0.items():
        print("\nKey:" + key)
        print("Value:" + value)
    ```
    运行结果：
    ```
    Key：last
    Value：fermi

    Key：first
    Value：enrico

    Key：username
    Value：efermi
    ```
    若需要根据字典的插入顺序进行遍历，可通过 collections.OrderedDict 来实现。e.g.
    ```python
    d = collections.OrderedDict()
        d['f'] = 0
        d['w'] = 1
        d['a'] = 2
    for key, value in d.items():
        print key, value
    ```

  - 根据 [PEP 468](https://legacy.python.org/dev/peps/pep-0468/)，Python 3.6 改写了 dict 的内部算法，因此 3.6 的 dict 是有序的。

- 使用 = 赋值， 使用 copy() 复制

## 4. Set Types

### 4.1. 集合 (set)

集合就像舍弃了值，仅剩下键的字典一样。**如果你仅仅想知道某一个元素是否存在而不关心其他的，使用集合是个非常好的选择**。如果需要为键附加其他信息的话，建议使用字典。

#### 4.1.1. 特性

1. 无序。
1. 元素可变。
1. 不允许重复。

#### 4.1.2. 常用操作

- 创建
  - 使用 set() 函数创建一个空集合

    ```python
    >>> empty_set = set()
    >>> empty_set
    set()
    # 由于 [] 能创建一个空列表，你可能期望 {} 也能创建空集。但事实上， {} 会创建一个空字典，这也是为什么交互式解释器把空集输出为 set() 而不是{}
    ```

  - 使用大括号创建非空集合
    ```python
    >>> even_numbers = {0, 2, 4, 6, 8}
    >>> even_numbers
    {0, 8, 2, 4, 6}
    ```

- 使用 in 测试值是否存在

  这是集合里最常用的功能。

#### 4.1.3. set 方法

- 交集运算

  使用特殊标点符号 `&` 或者集合函数 intersection() 获取集合的交集（两集合共有元素）
  ```python
  >>> a = {1, 2}
  >>> b = {2, 3} #交集运算
  >>> a & b
  {2}
  >>> a.intersection(b)
  {2}
  ```

- 并集运算

  使用 `|` 或者 union() 函数来获取集合的并集（至少出现在一个集合中的元素）：
  ```python
  >>> a | b
  {1, 2, 3}
  >>> a.union(b)
  {1, 2, 3}
  ```

- 差集运算

  使用字符 `-` 或者 difference() 可以获得两个集合的差集（**结果为出现在第一个集合但不出现在第二个集合的元素**）：
  ```python
  >>> a - b
  {1}
  >>> a.difference(b)
  {1}
  >>> bruss - wruss
  set()
  >>> wruss - bruss
  {'cream'}
  ```
- 异或集运算

  使用 `^` 或者 symmetric_difference() 可以获得两个集合的异或集（仅在两个集合中出现一次）：
  ```python
  >> a ^ b
  {1, 3}
  >>> a.symmetric_difference(b)
  {1, 3}
  ```

- 子集

  使用 `<=` 或者 issubset() 可以判断一个集合是否是另一个集合的子集（第一个集合的所有元素都出现在第二个集合中）：
  ```python
  >>> a <= b
  False
  >>> a.issubset(b)
  False
  ```

  使用 `<` 可以判断一个集合是否是另外一个集合的真子集。

- 超集

  - 使用 `>=` 或者 issuperset() 可以进行判断一个集合是否是另外一个集合的超集（第二个集合的所有元素都出现在第一个集合中）。
  - 使用 `>` 可以判断一个集合是否是另外一个集合的真超集。

### 4.2. frozenset

## 5. 迭代器 (Iterator Types)

> Python supports a concept of iteration over containers. This is implemented using two distinct methods; these are used to allow user-defined classes to support iteration. Sequences, described below in more detail, always support the iteration methods.

**迭代器 (Iterator) 都是可用于迭代操作（for 循环）的对象**，它像 list 一样可以迭代获取其中的每一个元素，但它与 list 的区别在于，构建迭代器的时候，不像列表把所有元素一次性加载到内存，而是**以懒加载（lazy evaluation）方式返回元素**。比如 list 含有中一千万个整数，需要占超过 400M 的内存，而迭代器只需要几十个字节的空间。因为它并没有把所有元素装载到内存中，而是等到调用 next 方法时候才返回该元素（本质上 for 循环就是不断地调用迭代器的 next 方法）。

- 创建方式
  - 任何实现了 `__iter__()` 和 `__next__()` 方法的对象都是迭代器，其中 `__iter__()` 返回迭代器自身，`__next__()` 返回容器中的下一个值。

    e.g. 以斐波那契数列为例来实现一个迭代器：
    ```python
    class Fib:
        def __init__(self, n):
            self.prev = 0
            self.cur = 1
            self.n = n

        def __iter__(self):
            return self

        def __next__(self):
            if self.n > 0:
                value = self.cur
                self.cur = self.cur + self.prev
                self.prev = value
                self.n -= 1
                return value
            else:
                raise StopIteration()
        # 兼容 python2
        def __next__(self):
            return self.next()

    f = Fib(10)
    print([i for i in f])
    #[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
    ```

  - 使用 built-in function [`iter()`](https://docs.python.org/3/library/functions.html#iter) 方法来创建一个可迭代对象（全量加载）的迭代器（懒加载）:
    ```python
    iter(object[, sentinel])
    ```
    该方法返回一个 iterator object。
    - 若不指定 sentinel 参数，则 object 必须实现了 `__iter__()` 或 `__getitem__()` 方法（否则 TypeError is raised），`iter()` 将会通过这两个方法来创建一个 iterator object。
    - 若指定了 sentinel 参数，则返回的 iterator object 将不断调用其 `__next__()` 方法，直到迭代值与 sentinel 相等，就抛出 StopIteration 异常。

    e.g.
    ```python
    >>> x = [1, 2, 3]
    >>> y = iter(x)
    >>> z = iter(x)
    >>> next(y)
    1
    >>> next(y)
    2
    >>> next(z)
    1
    >>> type(x)
    <class 'list'>
    >>> type(y)
    <class 'list_iterator'>
    ```

    e.g. reading fixed-width blocks from a binary database file until the end of file is reached:
    ```python
    from functools import partial
    with open('mydata.db', 'rb') as f:
        for block in iter(partial(f.read, 64), b''):
            process_block(block)
    ```

- [概念辨析](https://nvie.com/posts/iterators-vs-generators/)

  ![image](http://img.cdn.firejq.com/jpg/2019/10/27/5d237e41229ada6769c63ac845344993.jpg)

  - 可迭代对象 (iterable): 凡是可以返回一个迭代器的对象都可称为可迭代对象，**可迭代对象和容器一样是一种通俗的叫法，并不是指某种具体的数据类型**，list、dict、set 都是可迭代对象。
  - 迭代器 (iterator): 迭代器是一个带状态的懒加载的对象，任何实现了 `__iter__()` 和 `__next__()` 方法的对象都是迭代器，其中 `__iter__()` 返回迭代器自身，`__next__()` 返回容器中的下一个值。通过对 iterable 调用 iter() 方法可生成其对应的 iterator。
  - 生成器 (generator): 生成器是一种特殊的迭代器（懒加载），不过这种迭代器更加优雅。它不需要再像 iterator 一样实现 `__iter__()` 和 `__next__()` 方法了，只需要一个 `yield` 关键字。

### 5.1. 生成器 (Generator Types)

[Python Wiki: Generator](https://wiki.python.org/moin/Generators)

生成器 (Generator) 本质上也是一个迭代器，因此它有和迭代器一样的特性。唯一的区别在于实现方式上不一样，Generator 更加简洁优雅，它不需要再像 iterator 一样实现 `__iter__()` 和 `__next__()` 方法了，只需要一个 `yield` 关键字。

相比列表 (list) 而言，迭代器最大的优势就是延迟计算、按需使用，从而提高开发体验和运行效率，因此在 Python 3 中，map、filter 等操作返回的不再是列表而是迭代器。

- 创建
  - **通过生成器函数（包含了 yield 语句的函数）创建 Generator**

    yield 对应的值在函数被调用时不会立刻返回，而是调用 next 方法时才返回。下一次迭代时，从上一次迭代遇到的 yield 后面的代码继续开始执行。直到将生成器函数执行完毕，生成器会返回一个异常，告知迭代器迭代结束。

    e.g. Generator 的实现方式比 Iterator 更加优雅：
    ```python
    def fib(n):
        prev, curr = 0, 1
        while n > 0:
            n -= 1
            yield curr
            prev, curr = curr, curr + prev

    print([i for i in fib(10)])
    #[1, 1, 2, 3, 5, 8, 13, 21, 34, 55]
    ```

    e.g.
    ```python
    >>> def func(n):
    ...     yield n*2
    ...
    >>> func
    <function func at 0x00000000029F6EB8>
    >>> g = func(5)
    >>> g
    <generator object func at 0x0000000002908630>
    >>>
    ```

  - **通过生成器推导式创建 Generator**

    ```python
    >>> g = (x*2 for x in range(10))
    >>> type(g)
    <type 'generator'>
    >>> l = [x*2 for x in range(10)]
    >>> type(l)
    <type 'list'>
    ```

- **py3：使用 [`map(function, iterable, ...)`](https://docs.python.org/3/library/functions.html#map) 遍历操作并返回一个 generator**（py2 中返回一个 list）：
  ```python
  # Return an iterator that applies function to every item of iterable, yielding the results.
  >>> def square(x) :            # 计算平方数
  ...     return x ** 2
  ...
  >>> map(square, [1,2,3,4,5])   # 计算列表各个元素的平方
  <map at 0x1091e7650>

  >>> map(lambda x: x ** 2, [1, 2, 3, 4, 5])  # 使用 lambda 匿名函数
  <map at 0x1091cee50>

  >>> map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])  # 提供了两个列表，对相同位置的列表数据进行相加
  <map at 0x10915b950>
  ```

- **py3: 使用 [`filter(function, iterable)`](https://docs.python.org/3/library/functions.html#filter) 过滤 list 中不符合 function（返回 False）的项并返回新的 generator**（py2 中 filter() 返回一个 list）：
  ```python
  def is_odd(n):
      return n % 2 == 1

  print(filter(is_odd, range(1, 101)))
  ```

NOTE:
- **一个生成器只能运行一次。列表、集合、字符串和字典都存储在内存中，但是生成器仅在运行中产生值，不会被存下来，所以不能重新使用或者备份一个生成器**。

  **如果想再一次迭代此生成器，会发现它被擦除了**：
  ```python
  >>> number_thing = (number for number in range(1, 6))
  >>> try_again = list(number_thing)
  >>> try_again
  [1, 2, 3, 4, 5]
  >>> try_again = list(number_thing)
  >>> try_again
  []
  ```

- **当使用 for .. in .. 进行迭代时，python 会将可迭代对象的所有迭代值都读到内存中，再进行迭代，因此，若迭代器包含了海量数据，直接迭代会导致大量内存被占用，此时可采用生成器的方式。**

### 5.2. itertools

为方便开发者使用迭代器，Python 标准库 [itertools](https://docs.python.org/3/library/itertools.html) 封装了常用的迭代器实现，可供开发者直接使用。

e.g.

- `itertools.accumulate`: 累加迭代器。
  ```python
  >>> x = itertools.accumulate(range(10))
  >>> print(list(x))
  [0, 1, 3, 6, 10, 15, 21, 28, 36, 45]
  ```

- `itertools.chain`: 连接多个列表或者迭代器并返回连接后的迭代器。
  ```python
  >>> x = itertools.chain(range(3), range(4), [3,2,1])
  >>> print(list(x))
  [0, 1, 2, 0, 1, 2, 3, 3, 2, 1]
  ```

- `itertools.permutations`: 创建指定数目的元素的所有排列（顺序有关）的迭代器。
  ```python
  >>> x = itertools.permutations(range(4), 3)
  >>> print(list(x))
  [(0, 1, 2), (0, 1, 3), (0, 2, 1), (0, 2, 3), (0, 3, 1), (0, 3, 2), (1, 0, 2), (1, 0, 3), (1, 2, 0), (1, 2, 3), (1, 3, 0), (1, 3, 2), (2, 0, 1), (2, 0, 3), (2, 1, 0), (2, 1, 3), (2, 3, 0), (2, 3, 1), (3, 0, 1), (3, 0, 2), (3, 1, 0), (3, 1, 2), (3, 2, 0), (3, 2, 1)]
  ```

- `itertools.product`: 创建多个列表和迭代器的积的迭代器。
  ```python
  >>> x = itertools.product('ABC', range(3))
  >>>
  >>> print(list(x))
  [('A', 0), ('A', 1), ('A', 2), ('B', 0), ('B', 1), ('B', 2), ('C', 0), ('C', 1), ('C', 2)]
  ```

- `itertools.repeat`: 创建一个拥有指定数目元素的迭代器。
  ```python
  >>> x = itertools.repeat(0, 5)
  >>> print(list(x))
  [0, 0, 0, 0, 0]
  ```

- `itertools.count`: 创建一个按指定 step 增长的迭代器。
  ```python
  >>> x = itertools.count(start=20, step=-1)
  >>> print(list(itertools.islice(x, 0, 10, 1)))
  [20, 19, 18, 17, 16, 15, 14, 13, 12, 11]
  ```

## 6. 上下文管理器 (Context Manager Types)

> A context manager is an object that **defines the runtime context to be established when executing a with statement**. The context manager handles the entry into, and the exit from, the desired runtime context for the execution of the block of code.
>
>  Context managers are **normally invoked using the with statement** (described in section The with statement), but can **also be used by directly invoking their methods**.
>
> Typical uses of context managers include **saving and restoring** various kinds of global state, **locking and unlocking** resources, **closing opened files**, etc.

从 Python2.5 开始 ([PEP 343](https://www.python.org/dev/peps/pep-0343/))，Python 中引入了 with 语句，结合上下文管理器 (Context Managers)，可以实现对资源进行有控制的访问，确保不管使用过程中是否发生异常都会执行必要的清理操作，释放资源。

### 6.1. 实现方式

实现一个 Context Managers 至少需要为其定义以下 2 个 method：

- `object.__enter__(self)`

  > Enter the runtime context related to this object. The with statement will bind this method’s return value to the target(s) specified in the as clause of the statement, if any.

  with 语句在进入 body 语句块之前，会调用 Context Managers 的 `__enter__()` 方法，并将返回值赋值给 `as` 子句中的 target 对象。

- `object.__exit__(self, exc_type, exc_value, traceback)`

  > Exit the runtime context related to this object. The parameters describe the exception that caused the context to be exited. If the context was exited without an exception, all three arguments will be None.

  with 语句在退出 body 语句块之后，会调用 Context Managers 的 `__exit__()` 方法，主要做异常处理操作。**在 `__exit__()` 方法中处理完异常后，如果返回 True，此时该异常就不会再被抛出**。

e.g.
```python
class File():
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode

    def __enter__(self):
        print("entering")
        self.f = open(self.filename, self.mode)
        return self.f

    def __exit__(self, *args):
        print("will exit")
        self.f.close()

if __name__ == '__main__':
    with File('out.txt', 'w') as f:
        print("writing")
        f.write('hello, python')
```

### 6.2. contextlib.contextmanager

Python 还提供了一个 [contextmanager](https://docs.python.org/3/library/contextlib.html#contextlib.contextmanager) 的装饰器，结合 generators 可以更进一步简化上下文管理器的实现方式：

> This function is a decorator that can be used to define a factory function for with statement context managers, without needing to create a class or separate __enter__() and __exit__() methods.

- 通过 yield 将函数分割成两部分，yield 之前的语句在 `__enter__()` 方法中执行，yield 之后的语句在 `__exit__()` 方法中执行。
- yield 语句将 value 赋值给 as 子句的 target 对象后，开始执行 with 语句的 body 语句块。执行完毕后，又会返回到 yield 语句。
- 若在 with 语句中抛出了异常，则将会在 yield 语句处被 reraised。因此，通常会将 yield 语句用 `try…except…finally` 包装起来。

e.g.
```python
from contextlib import contextmanager

@contextmanager
def managed_resource(*args, **kwds):
    # Code to acquire resource, e.g.:
    resource = acquire_resource(*args, **kwds)
    try:
        yield resource
    finally:
        # Code to release resource, e.g.:
        release_resource(resource)

>>> with managed_resource(timeout=3600) as resource:
...     # Resource is released at the end of this block,
...     # even if code in the block raises an exception
```

Python 在 contextlib 中也提供了一些 [Examples and Recipes](https://docs.python.org/3/library/contextlib.html#examples-and-recipes)。

## 7. 其它 Built-in 类型

https://docs.python.org/3/library/stdtypes.html#other-built-in-types

### 7.1. Modules

### 7.2. Classes and Class Instances

### 7.3. Functions

### 7.4. Methods

### 7.5. Code Objects

### 7.6. Type Objects

### 7.7. The Null Object

### 7.8. The Ellipsis Object

### 7.9. The NotImplemented Object

## 8. Refer Links

[Python Docs: Built-in Types](https://docs.python.org/3/library/stdtypes.html)

[The Python Tutorial 5. Data Structures](https://docs.python.org/3/tutorial/datastructures.html)

[Python Docs: Binary Sequence Types](https://docs.python.org/3/library/stdtypes.html#binary-sequence-types-bytes-bytearray-memoryview)

[Python Docs: with Statement Context Managers](https://docs.python.org/3.8/reference/datamodel.html#with-statement-context-managers)

[Python Docs: Context Managers](https://docs.python.org/3/library/stdtypes.html#context-manager-types)

[Python 之 itertools 详解](https://juejin.im/post/5af56230f265da0b93485cca)

TODO:

[也谈 Python 的中文编码处理](https://www.iteye.com/blog/in355hz-1860787)

https://sanyuesha.com/2016/11/06/python-string-unicode/

https://blog.csdn.net/y396397735/article/details/47807145

https://blog.csdn.net/qq_24342335/article/details/84561341
