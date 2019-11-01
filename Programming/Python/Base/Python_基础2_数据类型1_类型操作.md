- [Python 基础：数据类型 - 类型操作](#python-基础数据类型---类型操作)
  - [1. 类型转换](#1-类型转换)
    - [1.1. 自动类型转换](#11-自动类型转换)
    - [1.2. 强制类型转换](#12-强制类型转换)
      - [1.2.1. int()](#121-int)
      - [1.2.2. float()](#122-float)
      - [1.2.3. str()](#123-str)
      - [1.2.4. list()](#124-list)
      - [1.2.5. tuple()](#125-tuple)
      - [1.2.6. dict()](#126-dict)
      - [1.2.7. set()](#127-set)
  - [2. 类型判断](#2-类型判断)
    - [2.1. 基本类型](#21-基本类型)
    - [2.2. 引用类型](#22-引用类型)
  - [3. 类型提示](#3-类型提示)
  - [4. Refer Links](#4-refer-links)

# Python 基础：数据类型 - 类型操作

The principal built-in types are numerics, sequences, mappings, classes, instances and exceptions.

## 1. 类型转换

### 1.1. 自动类型转换

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

### 1.2. 强制类型转换

#### 1.2.1. int()

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

#### 1.2.2. float()

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

#### 1.2.3. str()

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

#### 1.2.4. list()

list() 函数可以将其他数据类型转换成列表类型：
```python
>>> list('cat')
['c', 'a', 't']
>>> a_tuple = ('ready', 'fire', 'aim')
>>> list(a_tuple)
['ready', 'fire', 'aim']
```

#### 1.2.5. tuple()

tuple() 函数可以用其他类型的数据来创建元组：
```python
>>> marx_list = ['Groucho', 'Chico', 'Harpo']
>>> tuple(marx_list)
('Groucho', 'Chico', 'Harpo')
```

#### 1.2.6. dict()

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

#### 1.2.7. set()

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

## 2. 类型判断

### 2.1. 基本类型

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

### 2.2. 引用类型

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

## 3. 类型提示

TODO:

[全面理解 Python 中的类型提示（Type Hints）](https://sikasjc.github.io/2018/07/14/type-hint-in-python/)

## 4. Refer Links
