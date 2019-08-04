- [Python 基础：数据类型](#Python-基础数据类型)
  - [1. 基本数据类型](#1-基本数据类型)
    - [1.1. 布尔值](#11-布尔值)
    - [1.2. 数字](#12-数字)
      - [1.2.1. int](#121-int)
        - [1.2.1.1. 大小](#1211-大小)
        - [1.2.1.2. 基数](#1212-基数)
      - [1.2.2. float](#122-float)
    - [1.3. 字符串](#13-字符串)
      - [1.3.1. 创建](#131-创建)
      - [1.3.2. 输出](#132-输出)
      - [1.3.3. 使用`+`进行拼接](#133-使用进行拼接)
      - [1.3.4. 使用`*`进行复制](#134-使用进行复制)
      - [1.3.5. 使用`[]`提取字符](#135-使用提取字符)
      - [1.3.6. 使用`[start:end:step]`分片](#136-使用startendstep分片)
      - [1.3.7. 使用 len() 获取长度](#137-使用-len-获取长度)
      - [1.3.8. 使用字符串方法](#138-使用字符串方法)
        - [1.3.8.1. 使用 split() 分割](#1381-使用-split-分割)
        - [1.3.8.2. 使用 join() 合并](#1382-使用-join-合并)
        - [1.3.8.3. replace()](#1383-replace)
        - [1.3.8.4. strip()](#1384-strip)
        - [1.3.8.5. 大小写转换](#1385-大小写转换)
        - [1.3.8.6. 格式排版](#1386-格式排版)
  - [2. 容器类型](#2-容器类型)
    - [2.1. 列表](#21-列表)
      - [2.1.1. 特性](#211-特性)
      - [2.1.2. 创建](#212-创建)
      - [2.1.3. 使用 [offset] 获取、修改元素](#213-使用-offset-获取修改元素)
      - [2.1.4. 使用切片 [start:end:step] 提取元素](#214-使用切片-startendstep-提取元素)
      - [2.1.5. 使用 len() 获取长度](#215-使用-len-获取长度)
      - [2.1.6. 使用 in 判断值是否存在](#216-使用-in-判断值是否存在)
      - [2.1.7. 使用 del 删除指定位置的元素](#217-使用-del-删除指定位置的元素)
      - [2.1.8. 使用 list 方法](#218-使用-list-方法)
        - [2.1.8.1. 使用 append() 添加元素至尾部](#2181-使用-append-添加元素至尾部)
        - [2.1.8.2. 使用 extend() 或 += 合并列表](#2182-使用-extend-或--合并列表)
        - [2.1.8.3. 使用 insert() 在指定位置插入元素](#2183-使用-insert-在指定位置插入元素)
        - [2.1.8.4. 使用 remove() 删除具有指定值的元素](#2184-使用-remove-删除具有指定值的元素)
        - [2.1.8.5. 使用 pop() 获取并删除指定位置的元素](#2185-使用-pop-获取并删除指定位置的元素)
        - [2.1.8.6. 使用 index() 查询具有特定值的元素位](#2186-使用-index-查询具有特定值的元素位)
        - [2.1.8.7. 使用 count() 记录特定值出现的次数](#2187-使用-count-记录特定值出现的次数)
        - [2.1.8.8. 使用 sort() 重新排列元素](#2188-使用-sort-重新排列元素)
        - [2.1.8.9. 使用 = 赋值， 使用 copy() 复制](#2189-使用--赋值-使用-copy-复制)
    - [2.2. 元组](#22-元组)
      - [2.2.1. 特性](#221-特性)
      - [2.2.2. 创建](#222-创建)
      - [2.2.3. 元组解包](#223-元组解包)
      - [2.2.4. 元组与列表的比较](#224-元组与列表的比较)
    - [2.3. 字典](#23-字典)
      - [2.3.1. 特性](#231-特性)
      - [2.3.2. 创建](#232-创建)
      - [2.3.3. 使用 [key] 添加或修改元素](#233-使用-key-添加或修改元素)
      - [2.3.4. 使用 del 删除具有指定键的元素](#234-使用-del-删除具有指定键的元素)
      - [2.3.5. 使用 in 判断是否存在](#235-使用-in-判断是否存在)
      - [2.3.6. 使用 dict 的方法](#236-使用-dict-的方法)
        - [2.3.6.1. 使用 clear() 删除所有元素](#2361-使用-clear-删除所有元素)
        - [2.3.6.2. 使用 update() 合并字典](#2362-使用-update-合并字典)
        - [2.3.6.3. 使用 get() 获取键值](#2363-使用-get-获取键值)
        - [2.3.6.4. 使用 keys() 获取所有键](#2364-使用-keys-获取所有键)
        - [2.3.6.5. 使用 values() 获取所有值](#2365-使用-values-获取所有值)
        - [2.3.6.6. 使用 items() 获取所有键值对](#2366-使用-items-获取所有键值对)
        - [2.3.6.7. 使用 = 赋值， 使用 copy() 复制](#2367-使用--赋值-使用-copy-复制)
    - [2.4. 集合](#24-集合)
      - [2.4.1. 特性](#241-特性)
      - [2.4.2. 创建](#242-创建)
      - [2.4.3. 使用 in 测试值是否存在](#243-使用-in-测试值是否存在)
      - [2.4.4. 集合运算操作](#244-集合运算操作)
        - [2.4.4.1. 交集运算](#2441-交集运算)
        - [2.4.4.2. 并集运算](#2442-并集运算)
        - [2.4.4.3. 差集运算](#2443-差集运算)
        - [2.4.4.4. 异或集运算](#2444-异或集运算)
        - [2.4.4.5. 子集](#2445-子集)
        - [2.4.4.6. 超集](#2446-超集)
  - [3. 类型转换](#3-类型转换)
    - [3.1. 自动类型转换](#31-自动类型转换)
    - [3.2. 强制类型转换](#32-强制类型转换)
      - [3.2.1. int()](#321-int)
      - [3.2.2. float()](#322-float)
      - [3.2.3. str()](#323-str)
      - [3.2.4. list()](#324-list)
      - [3.2.5. tuple()](#325-tuple)
      - [3.2.6. dict()](#326-dict)
      - [3.2.7. set()](#327-set)
  - [4. 类型判断](#4-类型判断)
    - [4.1. 基本类型](#41-基本类型)
    - [4.2. 引用类型](#42-引用类型)
  - [5. Refer Links](#5-Refer-Links)

# Python 基础：数据类型

## 1. 基本数据类型

Python 是**强类型**的（strongly typed）（用户无法修改一个已有对象的类型，即使它包含的值是可变的）的**动态语言**（解释执行）；

### 1.1. 布尔值

Python 里最简单的数据类型是布尔型，它只有两个可选值： True 和 False。当转换为整数时，它们分别代表 1 和 0。

下面的情况都会被 python 解释器认为是 False：

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

### 1.2. 数字

#### 1.2.1. int

##### 1.2.1.1. 大小

在 Python 3 中， long 类型已不复存在，**int 类型变为可以存储任意大小的整数，甚至超过 64 位**；

##### 1.2.1.2. 基数

在 Python 中，整数默认使用十进制数（以 10 为底），除非你在数字前添加前缀，显式地指定使用其他基数（base）。

在 Python 中，除十进制外你还可以使用其他三种进制的数字：
- 0b 或 0B 代表二进制（以 2 为底）
- 0o 或 0O 代表八进制（以 8 为底）
- 0x 或 0X 代表十六进制（以 16 为底）

Python 解释器会打印出它们对应的十进制整数。

#### 1.2.2. float

整数全部由数字组成，而浮点数（在 Python 里称为 float）包含非数字的小数点。

### 1.3. 字符串

对 Unicode 的支持使得 Python 3 可以包含世界上任何书面语言以及许多特殊符号；字符串的本质是字符序列；

**Python 字符串是不可变的。你无法对原字符串进行修改，但可以将字符串的一部分复制到新字符串，来达到相同的修改效果；**

#### 1.3.1. 创建

将一系列字符包裹在一对单引号或一对双引号中即可创建字符串，**无论使用哪种引号， Python 对字符串的处理方式都是一样的，没有任何区别**；

三元引号在创建短字符串时没有什么特殊用处。它多用于创建多行字符串；

#### 1.3.2. 输出

`print()` 会**把包裹字符串的引号截去**，仅输出其实际内容，易于阅读。它还会**自动地在各个输出部分之间添加空格**，并**在所有输出的最后添加换行符**：
```python
>>> print(99, 'bottles', 'would be enough.')
99 bottles would be enough.
```

#### 1.3.3. 使用`+`进行拼接

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

#### 1.3.4. 使用`*`进行复制

```python
>>> start = 'Na ' * 4 + '\n'
>>> middle = 'Hey ' * 3 + '\n'
>>> end = 'Goodbye.'
>>> print(start + start + middle + end)
Na Na Na Na
Hey Hey Hey
Goodbye.
```
#### 1.3.5. 使用`[]`提取字符

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

#### 1.3.6. 使用`[start:end:step]`分片

分片操作（slice） 可以从一个字符串中抽取子字符串（字符串的一部分）。我们使用一对方括号、起始偏移量 start、终止偏移量 end 以及可选的步长 step 来定义一个分片。其中一些可以省略。分片得到的子串包含从 start 开始到 end 之前的全部字符。

- `[:]` 提取从开头到结尾的整个字符串

- `[start:]` 从 start 提取到结尾

- `[:end]` 从开头提取到 end - 1

- `[start:end]` 从 start 提取到 end - 1

- `[start:end:step]` 从 start 提取到 end - 1，每 step 个字符提取一个

#### 1.3.7. 使用 len() 获取长度

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

#### 1.3.8. 使用字符串方法

**字符串方法需要通过字符串调用。**

##### 1.3.8.1. 使用 split() 分割

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

##### 1.3.8.2. 使用 join() 合并

join() 函数与 split() 函数正好相反：它将包含若干子串的列表分
```python
>>> crypto_list = ['Yeti', 'Bigfoot', 'Loch Ness Monster']
>>> crypto_string = ', '.join(crypto_list) #join 函数是字符串方法，需要通过字符串调用
>>> print('Found and signing book deals:', crypto_string)
Found and signing book deals: Yeti, Bigfoot, Loch Ness Monster 解，并将这些子串合成一个完整的大的字符串。
```

##### 1.3.8.3. replace()

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

##### 1.3.8.4. strip()

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

##### 1.3.8.5. 大小写转换

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

##### 1.3.8.6. 格式排版

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

## 2. 容器类型

### 2.1. 列表

**字符串、列表、元组是 python 的三大序列结构**；

#### 2.1.1. 特性

1.	可变；

2.	有序；

3.	允许重复；

4.	元素类型可不同；

#### 2.1.2. 创建

列表可以由零个或多个元素组成，元素之间用逗号分开，整个列表被方括号所包裹：
```python
>>> empty_list = [ ]
>>> weekdays = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
>>> big_birds = ['emu', 'ostrich', 'cassowary']
>>> first_names = ['Graham', 'John', 'Terry', 'Terry', 'Michael']
```
也可以使用 list() 函数来创建一个空列表：
```python
>>> another_empty_list = list()
>>> another_empty_list
[]
```

#### 2.1.3. 使用 [offset] 获取、修改元素

和字符串一样，通过偏移量可以从列表中提取对应位置的元素：
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo']
>>> marxes[0]
'Groucho'
>>> marxes[2] = 'Wanda'
>>> marxes
['Groucho', 'Chico', 'Wanda']
```

#### 2.1.4. 使用切片 [start:end:step] 提取元素

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

#### 2.1.5. 使用 len() 获取长度

#### 2.1.6. 使用 in 判断值是否存在

判断一个值是否存在于给定的列表中有许多方式，其中最具有 Python 风格的是使用 in：
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> 'Groucho' in marxes
True
>>> 'Bob' in marxes
False
```

#### 2.1.7. 使用 del 删除指定位置的元素

当列表中一个元素被删除后，位于它后面的元素会自动往前移动填补空出的位置，且列表长度减 1。
```python
>>> del marxes[-1]
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
```
del 是 Python 语句，是赋值语句（=）的逆过程：它将一个 Python 对象与它的名字分离。如果这个对象无其他名称引用，则其占用空间也被会清除

#### 2.1.8. 使用 list 方法

##### 2.1.8.1. 使用 append() 添加元素至尾部
```python
>>> marxes.append('Zeppo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo']
```
##### 2.1.8.2. 使用 extend() 或 += 合并列表

使用 extend() 可以将一个列表合并到另一个列表中。
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> others = ['Gummo', 'Karl']
>>> marxes.extend(others)
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo', 'Gummo', 'Karl']
>>> marxes += others
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo', 'Gummo', 'Karl']
```
##### 2.1.8.3. 使用 insert() 在指定位置插入元素
```python
>>> marxes.insert(3, 'Gummo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
```
##### 2.1.8.4. 使用 remove() 删除具有指定值的元素
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
>>> marxes.remove('Gummo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo']
```
##### 2.1.8.5. 使用 pop() 获取并删除指定位置的元素
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
##### 2.1.8.6. 使用 index() 查询具有特定值的元素位
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> marxes.index('Chico')
1
```
##### 2.1.8.7. 使用 count() 记录特定值出现的次数
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo']
>>> marxes.count('Harpo')
1
>>> marxes.count('Bob')
0
```
##### 2.1.8.8. 使用 sort() 重新排列元素

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

##### 2.1.8.9. 使用 = 赋值， 使用 copy() 复制

### 2.2. 元组

#### 2.2.1. 特性

1.	不可变；

2.	有序；

3.	允许重复；

4.	元素类型可不同；

#### 2.2.2. 创建

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

#### 2.2.3. 元组解包

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

#### 2.2.4. 元组与列表的比较

- 相比列表，元组占用的空间较小

- 不会意外修改元组的值；

- 可以将元组用作字典的键；

- 命名元组可以作为对象的替代；

- 函数的参数是以元组形式传递的；

### 2.3. 字典

#### 2.3.1. 特性

1.	无序性；

2.	键不可变，键值可变；

3.	元素允许重复；

#### 2.3.2. 创建

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

#### 2.3.3. 使用 [key] 添加或修改元素

向字典中添加元素非常简单，只需指定该元素的键并赋予相应的值即可。
- 如果该元素的键已经存在于字典中， 那么该键对应的旧值会被新值取代。
- 如果该元素的键并未在字典中出现，则会被加入字典。
与列表不同，不需要担心赋值过程中 Python 会抛出越界异常。

#### 2.3.4. 使用 del 删除具有指定键的元素

```python
>>> del pythons['Marx']
```

#### 2.3.5. 使用 in 判断是否存在

用于判断某一个键（**不是键值**）是否存在于一个字典中；

#### 2.3.6. 使用 dict 的方法

##### 2.3.6.1. 使用 clear() 删除所有元素

##### 2.3.6.2. 使用 update() 合并字典

使用 update() 可以将一个字典的键值对复制到另一个字典中去。如果待添加的字典与待扩充的字典包含同样的键，新归入字典的值会取代原有的值。

##### 2.3.6.3. 使用 get() 获取键值

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

##### 2.3.6.4. 使用 keys() 获取所有键

在 Python 3 中，keys() 方法会返回 `dict_keys()`，它是键的迭代形式，需要调用 list() 将 dict_keys 转换为列表类型。
```python
>>> signals = {'green': 'go', 'yellow': 'go faster', 'red': 'smile for the camera'}
>>> signals.keys()
dict_keys(['green', 'red', 'yellow'])
>>> list( signals.keys() )
['green', 'red', 'yellow']
```

##### 2.3.6.5. 使用 values() 获取所有值

```python
>>> list( signals.values() )
['go', 'smile for the camera', 'go faster']
```

##### 2.3.6.6. 使用 items() 获取所有键值对

```python
>>> list( signals.items() )
[('green', 'go'), ('red', 'smile for the camera'), ('yellow', 'go faster')]
```

每一个键值对以元组的形式返回。

##### 2.3.6.7. 使用 = 赋值， 使用 copy() 复制

### 2.4. 集合

集合就像舍弃了值，仅剩下键的字典一样。**如果你仅仅想知道某一个元素是否存在而不关心其他的，使用集合是个非常好的选择**。如果需要为键附加其他信息的话，建议使用字典。

#### 2.4.1. 特性

1.	无序；

2.	元素可变；

3.	不允许重复；

#### 2.4.2. 创建

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

#### 2.4.3. 使用 in 测试值是否存在

这是集合里最常用的功能。

#### 2.4.4. 集合运算操作

##### 2.4.4.1. 交集运算

使用特殊标点符号 `&` 或者集合函数 intersection() 获取集合的交集（两集合共有元素）
```python
>>> a = {1, 2}
>>> b = {2, 3} #交集运算
>>> a & b
{2}
>>> a.intersection(b)
{2}
```

##### 2.4.4.2. 并集运算

使用 `|` 或者 union() 函数来获取集合的并集（至少出现在一个集合中的元素）：
```python
>>> a | b
{1, 2, 3}
>>> a.union(b)
{1, 2, 3}
```

##### 2.4.4.3. 差集运算

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
##### 2.4.4.4. 异或集运算

使用 `^` 或者 symmetric_difference() 可以获得两个集合的异或集（仅在两个集合中出现一次）：
```python
>> a ^ b
{1, 3}
>>> a.symmetric_difference(b)
{1, 3}
```

##### 2.4.4.5. 子集

使用 `<=` 或者 issubset() 可以判断一个集合是否是另一个集合的子集（第一个集合的所有元素都出现在第二个集合中）：
```python
>>> a <= b
False
>>> a.issubset(b)
False
```

使用 `<` 可以判断一个集合是否是另外一个集合的真子集；

##### 2.4.4.6. 超集

使用 `>=` 或者 issuperset() 可以进行判断一个集合是否是另外一个集合的超集（第二个集合的所有元素都出现在第一个集合中）；
使用 `>` 可以判断一个集合是否是另外一个集合的真超集；

## 3. 类型转换

### 3.1. 自动类型转换

如果混合使用多种不同的数字类型进行计算， Python 会自动地进行类型转换：
```python
>>> 4 + 7.0
11.0
```
与整数或浮点数混合使用时，布尔型的 False 会被当作 0 或 0.0， Ture 会被当作 1 或 1.0：
```python
>>> True + 2
3
>>> False + 5.0
5.0
```

### 3.2. 强制类型转换

#### 3.2.1. int()

使用 int() 可以将其他类型转换为整型：
当将布尔值转换为整数时，它们分别代表 1 和 0：
```python
>>> int(True)
1
>>> int(False)
0
```
当将浮点数转换为整数时，所有小数点后面的部分会被舍去：
```python
>>> int(98.6)
98
>>> int(1.0e4)
10000
```
将仅包含数字和正负号的字符串转换为整数：
```python
>>> int('99')
99
>>> int('-23')
-23
>>> int('+12')
12
```
但无法接受包含小数点或指数的字符串：（可以通过 flaot() 实现类型转换）
```python
>>> int('98.6')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '98.6'
>>> int('1.0e4')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '1.0e4
```
如果将一个与数字无关的类型转化为整数，会得到一个异常：
```python
>>> int('99 bottles of beer on the wall')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: '99 bottles of beer on the wall'
>>> int('')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
ValueError: invalid literal for int() with base 10: ''
```

#### 3.2.2. float()

使用 float() 函数可以将其他数字类型转换为浮点型：

布尔型在计算中等价于 1.0 和 0.0：
```python
>>> float(True)
1.0
>>> float(False)
0.0
```
将整数转换为浮点数仅仅需要添加一个小数点：
```python
>>> float(98)
98.0
>>> float('99')
99.0
```
此外，也可以将包含有效浮点数（数字、正负号、小数点、指数及指数的前缀 e）的字符串转换为真正的浮点型数字：
```python
>>> float('98.6')
98.6
>>> float('-1.5')
-1.5
>>> float('1.0e4')
10000.0
```

#### 3.2.3. str()

使用 str() 可以将其他 Python 数据类型转换为字符串：
```python
>>> str(98.6)
'98.6'
>>> str(1.0e4)
'10000.0'
>>> str(True)
'True
```
当你调用 print() 函数或者进行字符串差值（string interpolation）时， Python 内部会自动使用 str() 将非字符串对象转换为字符串。

#### 3.2.4. list()

list() 函数可以将其他数据类型转换成列表类型：
```python
>>> list('cat')
['c', 'a', 't']
>>> a_tuple = ('ready', 'fire', 'aim')
>>> list(a_tuple)
['ready', 'fire', 'aim']
```

#### 3.2.5. tuple()

tuple() 函数可以用其他类型的数据来创建元组：
```python
>>> marx_list = ['Groucho', 'Chico', 'Harpo']
>>> tuple(marx_list)
('Groucho', 'Chico', 'Harpo')
```

#### 3.2.6. dict()

使用 dict() 将任何包含双值子序列的序列转换成字典。

（双值子序列：每个子序列的第一个元素作为键，第二个元素作为值）
```python
# 包含双值列表的列表
>>> lol = [ ['a', 'b'], ['c', 'd'], ['e', 'f'] ]
>>> dict(lol)
{'c': 'd', 'a': 'b', 'e': 'f'}
# 包含双值元组的列表
>>> lot = [ ('a', 'b'), ('c', 'd'), ('e', 'f') ]
>>> dict(lot)
{'c': 'd', 'a': 'b', 'e': 'f'}
# 双字符的字符串组成的列表
>>> los = [ 'ab', 'cd', 'ef' ]
>>> dict(los)
{'c': 'd', 'a': 'b', 'e': 'f'}
```

#### 3.2.7. set()

使用 set() 将其他类型转换为集合；

可以利用已有列表、字符串、元组或字典的内容来创建集合，其中重复的值会被丢弃。

例：
```python
>>> set( 'letters' )
{'l', 'e', 't', 'r', ' 	s'}
```
当字典作为参数传入 set() 函数时，只有键会被使用：
```python
>>> set( {'apple': 'red', 'orange': 'orange', 'cherry': 'red'} )
{'apple', 'cherry', 'orange'}
```

## 4. 类型判断

### 4.1. 基本类型

基本类型可以用 type() 判断：
```python
>>> type(123)
<class 'int'>
>>> type('str')
<class 'str'>
>>> type(None)
<type(None) 'NoneType'>
```
**if 语句中，可直接使用 int、str 等关键字**：
```python
>>> type(123)==type(456)
True
>>> type(123)==int
True
>>> type('abc')==type('123')
True
>>> type('abc')==str
True
>>> type('abc')==type(123)
False
```

### 4.2. 引用类型

如果一个变量指向函数或者类，也可以用 type() 判断：
```python
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```
若要在 if 语句中判断一个对象是否是函数，需要使用 types 模块中定义的常量：
```python
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```

## 5. Refer Links
