- [Python 基础：数据类型 - 基本数据类型](#python-基础数据类型---基本数据类型)
  - [1. 布尔值](#1-布尔值)
  - [2. 数字](#2-数字)
    - [2.1. int](#21-int)
      - [2.1.1. 大小](#211-大小)
      - [2.1.2. 基数](#212-基数)
    - [2.2. float](#22-float)
  - [3. 字符串](#3-字符串)
    - [3.1. 字符串操作](#31-字符串操作)
    - [3.2. 字符串方法](#32-字符串方法)
    - [3.3. 字符串编码](#33-字符串编码)
      - [python2](#python2)
      - [python3](#python3)
  - [4. Refer Links](#4-refer-links)

# Python 基础：数据类型 - 基本数据类型

Python 是**强类型**的（strongly typed）的**动态**语言：
- 强类型：用户无法修改一个已有对象的类型，即使它包含的值是可变的。
- 动态：解释执行。

## 1. 布尔值

Python 里最简单的数据类型是布尔型，它只有两个可选值： `True` 和 `False`。当转换为整数时，它们分别代表 1 和 0。

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

## 2. 数字

### 2.1. int

#### 2.1.1. 大小

在 Python 3 中， long 类型已不复存在，**int 类型变为可以存储任意大小的整数，甚至超过 64 位**；

#### 2.1.2. 基数

在 Python 中，整数默认使用十进制数（以 10 为底），除非你在数字前添加前缀，显式地指定使用其他基数（base）。

在 Python 中，除十进制外你还可以使用其他三种进制的数字：
- 0b 或 0B 代表二进制（以 2 为底）
- 0o 或 0O 代表八进制（以 8 为底）
- 0x 或 0X 代表十六进制（以 16 为底）

Python 解释器会打印出它们对应的十进制整数。

### 2.2. float

整数全部由数字组成，而浮点数（在 Python 里称为 float）包含非数字的小数点。

## 3. 字符串

字符串的本质是字符序列；

**Python 字符串是不可变的。你无法对原字符串进行修改，但可以将字符串的一部分复制到新字符串，来达到相同的修改效果；**

### 3.1. 字符串操作

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

### 3.2. 字符串方法

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

### 3.3. 字符串编码




#### python2

在 Python 2 下进行中文输入输出是个危机四伏的事，特别是在你的代码里混合使用 str 与 unicode 时。

#### python3

在 Python 3 已经取消了 str，让所有的字符串都是 unicode。

## 4. Refer Links

TODO:

[也谈 Python 的中文编码处理](https://www.iteye.com/blog/in355hz-1860787)

https://sanyuesha.com/2016/11/06/python-string-unicode/

https://blog.csdn.net/y396397735/article/details/47807145

https://blog.csdn.net/qq_24342335/article/details/84561341
