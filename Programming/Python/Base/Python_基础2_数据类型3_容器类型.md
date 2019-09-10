- [Python 基础：数据类型 - 容器类型](#python-基础数据类型---容器类型)
  - [1. 列表](#1-列表)
    - [1.1. 特性](#11-特性)
    - [1.2. 创建](#12-创建)
    - [1.3. 使用 [offset] 获取、修改元素](#13-使用-offset-获取修改元素)
    - [1.4. 使用切片 [start:end:step] 提取元素](#14-使用切片-startendstep-提取元素)
    - [1.5. 使用 len() 获取长度](#15-使用-len-获取长度)
    - [1.6. 使用 in 判断值是否存在](#16-使用-in-判断值是否存在)
    - [1.7. 使用 del 删除指定位置的元素](#17-使用-del-删除指定位置的元素)
    - [1.8. 使用 list 方法](#18-使用-list-方法)
      - [1.8.1. 使用 append() 添加元素至尾部](#181-使用-append-添加元素至尾部)
      - [1.8.2. 使用 extend() 或 += 合并列表](#182-使用-extend-或--合并列表)
      - [1.8.3. 使用 insert() 在指定位置插入元素](#183-使用-insert-在指定位置插入元素)
      - [1.8.4. 使用 remove() 删除具有指定值的元素](#184-使用-remove-删除具有指定值的元素)
      - [1.8.5. 使用 pop() 获取并删除指定位置的元素](#185-使用-pop-获取并删除指定位置的元素)
      - [1.8.6. 使用 index() 查询具有特定值的元素位](#186-使用-index-查询具有特定值的元素位)
      - [1.8.7. 使用 count() 记录特定值出现的次数](#187-使用-count-记录特定值出现的次数)
      - [1.8.8. 使用 sort() 重新排列元素](#188-使用-sort-重新排列元素)
      - [1.8.9. 使用 = 赋值， 使用 copy() 复制](#189-使用--赋值-使用-copy-复制)
  - [2. 元组](#2-元组)
    - [2.1. 特性](#21-特性)
    - [2.2. 创建](#22-创建)
    - [2.3. 元组解包](#23-元组解包)
    - [2.4. 元组与列表的比较](#24-元组与列表的比较)
  - [3. 字典](#3-字典)
    - [3.1. 特性](#31-特性)
    - [3.2. 创建](#32-创建)
    - [3.3. 使用 [key] 添加或修改元素](#33-使用-key-添加或修改元素)
    - [3.4. 使用 del 删除具有指定键的元素](#34-使用-del-删除具有指定键的元素)
    - [3.5. 使用 in 判断是否存在](#35-使用-in-判断是否存在)
    - [3.6. 使用 dict 的方法](#36-使用-dict-的方法)
      - [3.6.1. 使用 clear() 删除所有元素](#361-使用-clear-删除所有元素)
      - [3.6.2. 使用 update() 合并字典](#362-使用-update-合并字典)
      - [3.6.3. 使用 get() 获取键值](#363-使用-get-获取键值)
      - [3.6.4. 使用 keys() 获取所有键](#364-使用-keys-获取所有键)
      - [3.6.5. 使用 values() 获取所有值](#365-使用-values-获取所有值)
      - [3.6.6. 使用 items() 获取所有键值对](#366-使用-items-获取所有键值对)
      - [3.6.7. 使用 = 赋值， 使用 copy() 复制](#367-使用--赋值-使用-copy-复制)
  - [4. 集合](#4-集合)
      - [4.1. 特性](#41-特性)
    - [2.4.2. 创建](#242-创建)
    - [2.4.3. 使用 in 测试值是否存在](#243-使用-in-测试值是否存在)
    - [2.4.4. 集合运算操作](#244-集合运算操作)
      - [2.4.4.1. 交集运算](#2441-交集运算)
      - [2.4.4.2. 并集运算](#2442-并集运算)
      - [2.4.4.3. 差集运算](#2443-差集运算)
      - [2.4.4.4. 异或集运算](#2444-异或集运算)
      - [2.4.4.5. 子集](#2445-子集)
      - [2.4.4.6. 超集](#2446-超集)
  - [5. Refer Links](#5-refer-links)

# Python 基础：数据类型 - 容器类型

## 1. 列表

**字符串、列表、元组是 python 的三大序列结构**；

### 1.1. 特性

1.	可变；

2.	有序；

3.	允许重复；

4.	元素类型可不同；

### 1.2. 创建

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

快速创建 10*10 的二维列表并全部初始化为 0:
```python
N = [[0]*10 for i in range(10)]
```

### 1.3. 使用 [offset] 获取、修改元素

和字符串一样，通过偏移量可以从列表中提取对应位置的元素：
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo']
>>> marxes[0]
'Groucho'
>>> marxes[2] = 'Wanda'
>>> marxes
['Groucho', 'Chico', 'Wanda']
```

### 1.4. 使用切片 [start:end:step] 提取元素

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

### 1.5. 使用 len() 获取长度

### 1.6. 使用 in 判断值是否存在

判断一个值是否存在于给定的列表中有许多方式，其中最具有 Python 风格的是使用 in：
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> 'Groucho' in marxes
True
>>> 'Bob' in marxes
False
```

### 1.7. 使用 del 删除指定位置的元素

当列表中一个元素被删除后，位于它后面的元素会自动往前移动填补空出的位置，且列表长度减 1。
```python
>>> del marxes[-1]
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
```
del 是 Python 语句，是赋值语句（=）的逆过程：它将一个 Python 对象与它的名字分离。如果这个对象无其他名称引用，则其占用空间也被会清除

### 1.8. 使用 list 方法

#### 1.8.1. 使用 append() 添加元素至尾部
```python
>>> marxes.append('Zeppo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo']
```
#### 1.8.2. 使用 extend() 或 += 合并列表

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
#### 1.8.3. 使用 insert() 在指定位置插入元素
```python
>>> marxes.insert(3, 'Gummo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
```
#### 1.8.4. 使用 remove() 删除具有指定值的元素
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Gummo', 'Zeppo']
>>> marxes.remove('Gummo')
>>> marxes
['Groucho', 'Chico', 'Harpo', 'Zeppo']
```
#### 1.8.5. 使用 pop() 获取并删除指定位置的元素
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
#### 1.8.6. 使用 index() 查询具有特定值的元素位
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo', 'Zeppo']
>>> marxes.index('Chico')
1
```
#### 1.8.7. 使用 count() 记录特定值出现的次数
```python
>>> marxes = ['Groucho', 'Chico', 'Harpo']
>>> marxes.count('Harpo')
1
>>> marxes.count('Bob')
0
```
#### 1.8.8. 使用 sort() 重新排列元素

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

#### 1.8.9. 使用 = 赋值， 使用 copy() 复制

## 2. 元组

### 2.1. 特性

1.	不可变；

2.	有序；

3.	允许重复；

4.	元素类型可不同；

### 2.2. 创建

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

### 2.3. 元组解包

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

### 2.4. 元组与列表的比较

- 相比列表，元组占用的空间较小

- 不会意外修改元组的值；

- 可以将元组用作字典的键；

- 命名元组可以作为对象的替代；

- 函数的参数是以元组形式传递的；

## 3. 字典

### 3.1. 特性

1.	无序性；

2.	键不可变，键值可变；

3.	元素允许重复；

### 3.2. 创建

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

### 3.3. 使用 [key] 添加或修改元素

向字典中添加元素非常简单，只需指定该元素的键并赋予相应的值即可。
- 如果该元素的键已经存在于字典中， 那么该键对应的旧值会被新值取代。
- 如果该元素的键并未在字典中出现，则会被加入字典。
与列表不同，不需要担心赋值过程中 Python 会抛出越界异常。

### 3.4. 使用 del 删除具有指定键的元素

```python
>>> del pythons['Marx']
```

### 3.5. 使用 in 判断是否存在

用于判断某一个键（**不是键值**）是否存在于一个字典中；

### 3.6. 使用 dict 的方法

#### 3.6.1. 使用 clear() 删除所有元素

#### 3.6.2. 使用 update() 合并字典

使用 update() 可以将一个字典的键值对复制到另一个字典中去。如果待添加的字典与待扩充的字典包含同样的键，新归入字典的值会取代原有的值。

#### 3.6.3. 使用 get() 获取键值

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

#### 3.6.4. 使用 keys() 获取所有键

在 Python 3 中，keys() 方法会返回 `dict_keys()`，它是键的迭代形式，需要调用 list() 将 dict_keys 转换为列表类型。
```python
>>> signals = {'green': 'go', 'yellow': 'go faster', 'red': 'smile for the camera'}
>>> signals.keys()
dict_keys(['green', 'red', 'yellow'])
>>> list( signals.keys() )
['green', 'red', 'yellow']
```

#### 3.6.5. 使用 values() 获取所有值

```python
>>> list( signals.values() )
['go', 'smile for the camera', 'go faster']
```

#### 3.6.6. 使用 items() 获取所有键值对

```python
>>> list( signals.items() )
[('green', 'go'), ('red', 'smile for the camera'), ('yellow', 'go faster')]
```

每一个键值对以元组的形式返回。

#### 3.6.7. 使用 = 赋值， 使用 copy() 复制

## 4. 集合

集合就像舍弃了值，仅剩下键的字典一样。**如果你仅仅想知道某一个元素是否存在而不关心其他的，使用集合是个非常好的选择**。如果需要为键附加其他信息的话，建议使用字典。

#### 4.1. 特性

1.	无序；

2.	元素可变；

3.	不允许重复；

### 2.4.2. 创建

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

### 2.4.3. 使用 in 测试值是否存在

这是集合里最常用的功能。

### 2.4.4. 集合运算操作

#### 2.4.4.1. 交集运算

使用特殊标点符号 `&` 或者集合函数 intersection() 获取集合的交集（两集合共有元素）
```python
>>> a = {1, 2}
>>> b = {2, 3} #交集运算
>>> a & b
{2}
>>> a.intersection(b)
{2}
```

#### 2.4.4.2. 并集运算

使用 `|` 或者 union() 函数来获取集合的并集（至少出现在一个集合中的元素）：
```python
>>> a | b
{1, 2, 3}
>>> a.union(b)
{1, 2, 3}
```

#### 2.4.4.3. 差集运算

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
#### 2.4.4.4. 异或集运算

使用 `^` 或者 symmetric_difference() 可以获得两个集合的异或集（仅在两个集合中出现一次）：
```python
>> a ^ b
{1, 3}
>>> a.symmetric_difference(b)
{1, 3}
```

#### 2.4.4.5. 子集

使用 `<=` 或者 issubset() 可以判断一个集合是否是另一个集合的子集（第一个集合的所有元素都出现在第二个集合中）：
```python
>>> a <= b
False
>>> a.issubset(b)
False
```

使用 `<` 可以判断一个集合是否是另外一个集合的真子集；

#### 2.4.4.6. 超集

使用 `>=` 或者 issuperset() 可以进行判断一个集合是否是另外一个集合的超集（第二个集合的所有元素都出现在第一个集合中）；
使用 `>` 可以判断一个集合是否是另外一个集合的真超集；

## 5. Refer Links
