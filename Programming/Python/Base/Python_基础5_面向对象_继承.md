- [Python 基础：面向对象 - 继承](#python-基础面向对象---继承)
  - [1. 方法重写](#1-方法重写)
    - [1.1. 基础重写方法](#11-基础重写方法)
    - [1.2. 运算符重载](#12-运算符重载)
  - [2. 多重继承](#2-多重继承)
  - [3. Refer Links](#3-refer-links)

# Python 基础：面向对象 - 继承

<!-- TODO:

[Python 中既然可以直接通过父类名调用父类方法为什么还会存在 super 函数？](https://www.zhihu.com/question/20040039)

[全世界公认的对 super 讲解最透彻的一篇文章：Python’s super() considered super!](https://link.zhihu.com/?target=https%3A//rhettinger.wordpress.com/2011/05/26/super-considered-super/)

 -->

类的继承是面向对象的三大特性之一；

在 python 中实现类继承的语法：基类名写在括号（即继承元组）里，若多重继承（继承了多个类），则将多个基类写个括号里
```python
class SubClassName (ParentClass1[, ParentClass2, ...]):
   'Optional class documentation string'
   class_suit
```

在 python 中继承中特点：

- 所有类最终都会继承 Object 类；

- 在继承中基类的构造（`__init__()`方法）不会被自动调用，它需要在其派生类的构造中使用`super()`方法获取父类定义，`super().__init__(xxx)`；

- 在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别于在类中调用普通函数时并不需要带上 self 参数；

- Python 总是首先查找对应类型的方法，如果它不能在派生类中找到对应的方法，它才开始到基类中逐个查找。（先在本类中查找调用的方法，找不到才去基类中找）；

## 1. 方法重写

如果父类方法的功能不能满足子类的需求，可以在子类重写父类的方法；

例：
```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-

class Parent:        # 定义父类
   def myMethod(self):
      print('调用父类方法')

class Child(Parent): # 定义子类
   def myMethod(self):
      print('调用子类方法')

c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法
```

### 1.1. 基础重写方法

[参考原文](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319098638265527beb24f7840aa97de564ccc7f20f6000 )

- `__init__ ( self [,args...] )`

  构造函数；

  简单的调用方法：obj = className(args)；

- `__del__( self )`

  析构方法，删除一个对象；

  简单的调用方法 : dell obj；

- `__repr__( self )`

  转化为供解释器读取的形式；

  简单的调用方法 : repr(obj)；

  例：
  ```python
  class Student(object):
      def __init__(self, name):
          self.name = name
      def __repr__(self):
          return 'Student object (name=%s)' % self.name
  >>>Student('Michael')
  Student object (name: Michael)

  ```

- `__str__( self )`

  用于将值转化为适于阅读的形式；

  简单的调用方法 : str(obj)；

  eg:
  ```python
  >>> class Student(object):
  ...     def __init__(self, name):
  ...         self.name = name
  ...     def __str__(self):
  ...         return 'Student object (name: %s)' % self.name
  ...
  >>> print(Student('Michael'))
  Student object (name: Michael)

  ```

- `__cmp__ ( self, x )`

  对象比较；

  简单的调用方法 : cmp(obj, x)；

- `__iter__ (self)`

  如果一个类想被用于 for ... in 循环，类似 list 或 tuple 那样，就必须实现一个__iter__() 方法，该方法返回一个迭代对象，然后，Python 的 for 循环就会不断调用该迭代对象的__next__() 方法拿到循环的下一个值，直到遇到 StopIteration 错误时退出循环

  以斐波那契数列为例，写一个 Fib 类，可以作用于 for 循环：
  ```python
  class Fib(object):
      def __init__(self):
          self.a, self.b = 0, 1 # 初始化两个计数器 a，b

      def __iter__(self):
          return self # 实例本身就是迭代对象，故返回自己

      def __next__(self):
          self.a, self.b = self.b, self.a + self.b # 计算下一个值
          if self.a > 100000: # 退出循环的条件
              raise StopIteration()
          return self.a # 返回下一个值
  ```
  使用
  ```python
  >>> for n in Fib():
  ...     print(n)
  ```

- `__getitem__(self, n)`

  使用索引下表查找元素（像 list[0]）

  eg:
  ```python
  class Fib(object):
      def __getitem__(self, n):
          a, b = 1, 1
          for x in range(n):
              a, b = b, a + b
          return a
  ```
  使用
  ```python
  >>> f = Fib()
  >>> f[0]
  1
  >>> f[1]
  1
  >>> f[2]
  2
  >>> f[100]
  573147844013817084101
  ```

- `__getattr__`

- `__call__`

### 1.2. 运算符重载

可以使用 Python 的特殊方法（special method），有时也被称作魔术方法（magic method），来实现运算 / 比较操作符的功能。

- 比较运算符重载

  | 方法名                 | 使用            |
  | ------------------- | ------------- |
  | __eq__(self, other) | self == other |
  | __ne__(self, other) | self != other |
  | __lt__(self, other) | self < other  |
  | __gt__(self, other) | self > other  |
  | __le__(self, other) | self <= other |
  | __ge__(self, other) | self >= other |

- 数学运算符重载

  | 方法名                       | 使用            |
  | ------------------------- | ------------- |
  | __add__(self, other)      | self + other  |
  | __sub__(self, other)      | self - other  |
  | __mul__(self, other)      | self * other  |
  | __floordiv__(self, other) | self // other |
  | __truediv__(self, other)  | self / other  |
  | __mod__(self, other)      | self % other  |
  | __pow__(self, other)      | self ** other |

例：重载 + 运算符
```python
#!/usr/bin/python

class Vector:
   def __init__(self, a, b):
      self.a = a
      self.b = b

   def __str__(self):
      return 'Vector (%d, %d)' % (self.a, self.b)

   def __add__(self,other):
      return Vector(self.a + other.a, self.b + other.b)

v1 = Vector(2,10)
v2 = Vector(5,-2)
print v1 + v2
```

## 2. 多重继承

通过多重继承，一个子类就可以同时获得多个父类的所有功能。

例：
```python
class Bat(Mammal, Flyable):
    pass
```

## 3. Refer Links
