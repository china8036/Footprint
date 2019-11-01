- [Python 基础：数据模型 (Data Model)](#python-基础数据模型-data-model)
  - [1. 数据封装](#1-数据封装)
  - [2. 数据变量](#2-数据变量)
    - [2.1. 变量命名](#21-变量命名)
    - [2.2. 变量作用域](#22-变量作用域)
  - [3. 赋值机制](#3-赋值机制)
  - [4. Refer Links](#4-refer-links)

# Python 基础：数据模型 (Data Model)

## 1. 数据封装

> Objects are Python’s abstraction for data. **All data in a Python program is represented by objects or by relations between objects**. (In a sense, and in conformance to Von Neumann’s model of a “stored program computer,” **code is also represented by objects**.)
>
> **Every object has an identity, a type and a value**.

**在 Python 中，所有类型的数据都是以对象 (object) 的形式存在的**。对象是一块系统分配的内存，其中存储着代表的 Value 以及一些与该对象有关的信息标记。

每个 object 具体由 indentity、type 和 value 这三个部分组成：
- **identity**: identity 可以类比为 object 在内存中的地址，一旦创建了 object 就不能再对 identity 进行修改。
- **type**: type 是 object 的类型参数，它决定了 object 所支持的操作 / 方法和取值范围，可以通过 type() 方法来获取。与 identity 相同，一旦创建了 object 就不能再对 type 进行修改。
- **value**: value 是数据对象 (object) 中存储的数据值。

  object 根据 value 是否可变，可分为 mutable object 和 immutable object：
  - **mutable**: Objects whose value can change are said to be mutable;
  - **immutable**: objects whose value is unchangeable once they are created are called immutable. (The value of an immutable container object that contains a reference to a mutable object can change when the latter’s value is changed; however the container is still considered immutable, because the collection of objects it contains cannot be changed. So, immutability is not strictly the same as having an unchangeable value, it is more subtle.)

  **An object’s mutability is determined by its type**; for instance, numbers, strings and tuples are immutable, while dictionaries and lists are mutable.

## 2. 数据变量

在 Python 中，**每个变量都是对数据对象 (object) 的引用，存储了其指向的数据对象的内存地址（相当于 C 语言中的指针）。因此，在 Python 中一个变量并没有类型，它的类型完全取决于它所指向的对象**。

### 2.1. 变量命名

- 变量名只能包含大小写字母、数字、下划线；

- 变量名不允许以数字开头；

- 变量名中以两个下划线开头的变量有特殊意义，是 python 的保留用法，自定义变量不可使用这样的命名；

  e.g.
  - 函数名：`function.__name__`
  - 函数文档字符串：`function.__doc__`
  - 主程序名称：`__main__`
  - 模块源代码文件：`math.__file__`

### 2.2. 变量作用域

为了读取全局变量而不是函数中的局部变量，需要在变量前面显式地加关键字 `global`。

Python 提供了两个获取命名空间内容的函数：
  - locals() 返回一个局部命名空间内容的字典；
  - globals() 返回一个全局命名空间内容的字典。

## 3. 赋值机制

Python 中的赋值过程可以分成三步，以 a = 3 为例：

![image](http://img.cdn.firejq.com/jpg/2019/10/25/d4ce8cf0dbeba2c3501ef32328359782.jpg)

1. 创建一个对象用来表示 3。
1. 起一个名字 a（如果之前没有用过这个名字）。
1. 将名字 a 与对象联系起来。

Python 的这种动态类型绑定与赋值机制十分灵活。但也可能带来一定风险。例如：
```python
>>> a = [1, 2, 3]
>>> b = a
>>> a[0] = 4
>>> b
[4, 2, 3]
```
如果希望 b 的对象不被改变，可以使用 `copy.deepcopy()`。系统将复制一个与 a 所指向的对象值相同但处于不同内存空间的新对象，并将名字 b 与其关联。

## 4. Refer Links

[Python DOCS. 3.1. Objects, values and types](https://docs.python.org/3.8/reference/datamodel.html#objects-values-and-types)

[Python 的动态类型绑定与赋值机制](https://blog.csdn.net/mwsong/article/details/1888654)