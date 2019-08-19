- [Python 基础：面向对象](#python-基础面向对象)
  - [1. 数据封装](#1-数据封装)
    - [1.1. 类定义](#11-类定义)
    - [1.2. 属性](#12-属性)
      - [1.2.1. 实例属性](#121-实例属性)
      - [1.2.2. 类属性](#122-类属性)
        - [1.2.2.1. 内置类属性](#1221-内置类属性)
      - [1.2.3. 实例属性和类属性的关系](#123-实例属性和类属性的关系)
      - [1.2.4. @property](#124-property)
    - [1.3. 方法](#13-方法)
      - [1.3.1. 实例方法](#131-实例方法)
      - [1.3.2. 类方法](#132-类方法)
        - [1.3.2.1. 魔法方法](#1321-魔法方法)
      - [1.3.3. 静态方法](#133-静态方法)
      - [1.3.4. 实例、静态、类方法辨析](#134-实例静态类方法辨析)
    - [1.4. 访问控制](#14-访问控制)
    - [1.5. 获取类的信息](#15-获取类的信息)
      - [1.5.1. type()](#151-type)
      - [1.5.2. isinstance()](#152-isinstance)
      - [1.5.3. dir()](#153-dir)
      - [1.5.4. getattr()、setattr()、hasattr()](#154-getattrsetattrhasattr)
    - [1.6. 枚举类](#16-枚举类)
      - [1.6.1. 元类](#161-元类)
    - [1.7. 继承](#17-继承)
    - [1.8. 方法重写](#18-方法重写)
      - [1.8.1. 基础重写方法](#181-基础重写方法)
      - [1.8.2. 运算符重载](#182-运算符重载)
    - [1.9. 多重继承](#19-多重继承)
  - [2. 多态](#2-多态)
    - [2.1. “开闭”原则](#21-开闭原则)
    - [2.2. 鸭子类型](#22-鸭子类型)
  - [3. Refer Links](#3-refer-links)

# Python 基础：面向对象

## 1. 数据封装

### 1.1. 类定义

类 (Class): 用来描述具有相同的属性和方法的对象的集合。它定义了该集合中每个对象所共有的属性和方法。对象是类的实例。

在 Python 中，可以通过 class 关键字定义自己的类，class 之后为类的名称并以冒号结尾，然后通过自定义的类对象类创建实例对象。
```python
class ClassName:
   ‘类的帮助信息‘   #类文档字符串
   class_suite  #类体
```

- 使用点 (.) 来访问对象的属性和方法。

- 创建实例是通过类名 +() 实现的；

### 1.2. 属性

例：
```python
class Student(object):
    count = 0
    books = []
    def __init__(self, name, age):
        self.__name = name
        self.__age = age
pass
```
通过内建函数 dir()，或者访问类的字典属性__dict__，这两种方式都可以查看类有哪些属性；

#### 1.2.1. 实例属性

在上述例子中，`self.__name`和`self.__age`属于实例属性；

实例数据属性只能通过实例访问，在类定义使用 self （相当于 Java、cpp 中的 this)，指向当前创建的类实例代表当前的实例；

创建类实例后，可以通过实例名动态地给一个实例变量绑定任何属性（即便这个属性在类定义中没有出现），但是这些添加的实例数据属性只属于该实例。对于两个实例变量，虽然它们都是同一个类的不同实例，但拥有的变量名称都可能不同；

通过定义 `__init__(self, xxx, xxx)` 方法，可以强制使得在创建类实例的时候就绑定一些属性，有了`__init__`方法，在创建实例的时候，就必须传入与__init__方法匹配的参数，但 self 不需要传，Python 解释器自己会把实例变量传进去；

#### 1.2.2. 类属性

在上述例子中，count、books 都属于类属性；

类数据属性属于类本身，可以通过类名、实例名进行访问 / 修改（虽然通过实例可以访问类属性，但是，不建议这么做，最好还是通过类名来访问类属性，从而避免属性隐藏带来的不必要麻烦，例如下边的例子）；

在类定义之后，可以通过类名动态添加类数据属性，新增的类属性也被类和所有实例共有；

需要注意：

- 实例属性的优先级高于类属性，即 python 解释器会优先查找实例属性，找不到所需属性时再查找类属性，因此，当有实例属性与类属性同名且用户通过实例名调用属性时，类属性会被实例属性 “覆盖”；

例：
```python
>>> class Student(object):
...     name = 'Student'
...
>>> s = Student() # 创建实例 s
>>> print(s.name) # 打印 name 属性，因为实例并没有 name 属性，所以会继续查找 class 的 name 属性
Student
>>> print(Student.name) # 打印类的 name 属性
Student
>>> s.name = 'Michael' # 给实例绑定 name 属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的 name 属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用 Student.name 仍然可以访问
Student
>>> del s.name # 如果删除实例的 name 属性
>>> print(s.name) # 再次调用 s.name，由于实例的 name 属性没有找到，类的 name 属性就显示出来了
Student
```

##### 1.2.2.1. 内置类属性

对于所有的类，都有一组特殊的属性：

| 类属性        | 含义                                       |
| ---------- | ---------------------------------------- |
| __name__   | 类的名字                                     |
| __doc__    | 类的文档字符串                                  |
| __bases__  | 类的所有父类组成的元组                              |
| __dict__   | 类的属性组成的字典                                |
| __module__ | 类所属的模块                                   |
| __class__  | 类对象的类型                                   |
| __slots__  | 限制类的动态绑定范围；  如：__slots__  = ('name', 'age')  用 tuple 定义允许绑定的属性名称 |

- 使用`__slots__`

  为了限制动态绑定，Python 允许在定义 class 的时候，定义一个特殊的__slots__变量，来限制该 class 实例能添加的属性；

  例：
  ```python
  class Student(object):
      __slots__ = ('name', 'age') # 用 tuple 定义允许绑定的属性名称
  >>> s = Student() # 创建新的实例
  >>> s.name = 'Michael' # 绑定属性'name'
  >>> s.age = 25 # 绑定属性'age'
  >>> s.score = 99 # 绑定属性'score'
  Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
  AttributeError: 'Student' object has no attribute 'score'
  ```

  注：

  `__slots__`定义的属性仅对当前类实例起作用，对继承的子类是不起作用的，除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`；

- 关于`if __name__ == '__main__'`: 的作用解析：

  例：
  Test.py
  ```python
  class Test:
  def __init(self):
  pass
  def f(self):
  print 'Hello, World!'

  if __name__ == '__main__':
      Test().f()
  ```
  直接运行 python 文件时，__name__ == '__main__'
  ```
  C:> python Test.py
  Hello, World!
  ```
  使用 import 导入 python 模块后，运行该模块时，__name__ == 模块名
  ```
  C:>python
  >>>import Test
  >>>Test.__name__                #Test 模块的__name__
  'Test'
  >>>__name__                       #当前程序的__name__
  '__main__'
  ```
  从而通过 if __name__ == '__main__'来判断是否是在直接运行该。py 文件，即若该文件作为脚本入口被 python 直接运行时，执行 if __name__ == '__main__' 中的代码；若该文件被视为模块导入到其它 py 文件时，不执行 if __name__ == '__main__' 中的代码；

#### 1.2.3. 实例属性和类属性的关系

http://blog.csdn.net/cc7756789w/article/details/46480829

**每个类的实例属性互不相同，因此每个类的每个实例属性都占有一块属于自己的内存空间；**

**而当一个类定义了一个类属性，该属性在内存中只占有一块空间，无论创建了多少个实例，每个实例的该属性都是这块内存空间的引用而已；直到某个类实例对该类的某个类属性进行了重新赋值的操作，则 python 就会为该实例创建一块新的内存空间，即原类属性的副本（初始值相同），然后再执行赋值操作，也就是说，执行完赋值操作后实例的类属性事实上已经与原来的类属性脱离了关系，相当于转换成了实例属性**。

验证：
```python
class AAA():
    aaa = 10

obj1 = AAA()
obj2 = AAA()
print obj1.aaa, obj2.aaa, AAA.aaa
print obj1.__dict__, obj2.__dict__, AAA.__dict__

obj1.aaa += 2
print obj1.aaa, obj2.aaa, AAA.aaa
print obj1.__dict__, obj2.__dict__, AAA.__dict__

AAA.aaa += 3
print obj1.aaa, obj2.aaa, AAA.aaa
print obj1.__dict__, obj2.__dict__, AAA.__dict__
```
执行结果：
```python
10 10 10
{} {} {'__module__': '__main__', 'aaa': 10, '__doc__': None}
12 10 10
{'aaa': 12} {} {'__module__': '__main__', 'aaa': 10, '__doc__': None}
12 13 13
{'aaa': 12} {} {'__module__': '__main__', 'aaa': 13, '__doc__': None}
```

因此，好的习惯是：

- 尽量把需要用户传入的属性作为实例属性，而把同类都一样的属性作为类属性。实例属性在每创造一个实例时都会初始化一遍。也就是说，要使得**不同的实例的实例属性可能不同，不同实例的类属性都相同**，从而节省内存的使用；

- 实例属性最好在 `__init__()` 中初始化（每次创建实例都会调用），内部调用时都需要加上 self，外部调用时用 `instancename.propertyname`；

  类属性最好在 `__init__()` 外初始化（不会每次创建实例都调用），在内部用 classname. 类属性名调用，外部用 classname. 类属性名调用（虽然既可以用 classname. 类属性名又可以用 instancename. 类属性名来调用）；

#### 1.2.4. @property

[廖雪峰的 python 教程：使用 @property](https://www.liaoxuefeng.com/wiki/897692888725344/923030547069856)

在绑定属性时：
- 如果直接把属性暴露出去，虽然写起来很简单，但却没办法检查参数，导致可以被随意更改：
  ```python
  s = Student()
  s.score = 9999
  ```
- 如果为了限制 score 的范围，可以通过一个 set_score() 方法来设置成绩，再通过一个 get_score() 来获取成绩，虽然可以对 setter 函数进行限制，但又过于繁琐：
  ```python
  class Student(object):
      def get_score(self):
          return self._score

      def set_score(self, value):
          if not isinstance(value, int):
              raise ValueError('score must be an integer!')
          if value < 0 or value > 100:
              raise ValueError('score must between 0 ~ 100!')
          self._score = value
  ```

因此，针对这一场景，python 提供了 @property 装饰器来便捷地为属性创建 setter 和 getter 方法；
```python
class Student(object):
    @property
    # 绑定一个实例属性 score，同时为其定义了 getter 方法
    def score(self):
        return self.__score

    @score.setter
    # 使用 @property 后，@property 本身又创建了装饰器 @score.setter，负责定义一个 setter 方法
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self.__score = value

    @score.deleter
    # 使用 @property 后，@property 本身又创建了装饰器 @score.deleter，负责定义一个 deleter 方法
    def score(self):
        del self.score
```
使用:
```python
>>> s = Student()
>>> s.score = 60 # 实际转化为 s.set_score(60)
>>> s.score # 实际转化为 s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

P.S.
- 若只定义 getter 方法，不定义 setter 方法就是设置一个只读属性：
  ```python
  class Student(object):
      # 以下的 birth 是可读写属性，而 age 就是一个只读属性，因为 age 可以根据 birth 和当前时间计算出来
      @property
      def birth(self):
          return self._birth

      @birth.setter
      def birth(self, value):
          self._birth = value

      @property
      def age(self):
          return 2014 - self._birth
  ```

- 装饰器 @property 的实现实际上是调用了 property() 函数：
  ```python
  class property([fget[, fset[, fdel[, doc]]]])
  ```
  - fget: 获取属性值的函数
  - fset: 设置属性值的函数
  - fdel: 删除属性值函数
  - doc: 属性描述信息

  e.g.
  ```python
  class C(object):
      def __init__(self):
          self._x = None

      def getx(self):
          return self._x

      def setx(self, value):
          self._x = value

      def delx(self):
          del self._x

      x = property(getx, setx, delx, "I'm the 'x' property.")
  ```


### 1.3. 方法

#### 1.3.1. 实例方法

实例方法是类中最常用的方法类型；

和普通的函数相比，实例方法只有一点不同：第一个参数必须是 self，self 类似于 C++ 中的"this"，代表的是类的实例，代表当前对象的地址，并且在调用时不用传递该参数；

**实例方法只能通过类实例进行调用**，这时候"self"就代表这个类实例本身。通过"self"可以直接访问实例的属性；

例：
```python
class Student(object):
    '''
    this is a Student class
    '''
    count = 0
    books = []
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def printInstanceInfo(self):
        print("%s is %d years old" % (self.name, self.age))
    pass

wilber = Student("Wilber", 28)
wilber.printInstanceInfo()
```

注：

- **self 不是 python 关键字，事实上可以将其换成任何字符串：**
  ```python
  class Test:
      def prt(xx):
          print(xx)
          print(xx.__class__)

  t = Test()
  t.prt()
  ```

- **类似实例属性可以被动态绑定，python 支持为类的实例动态绑定实例方法，且该方法只对该实例有效；**

  例：
  ```python
  >>> def set_age(self, age): # 定义一个函数作为实例方法
  ...     self.age = age
  ...
  >>> from types import MethodType
  >>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
  >>> s.set_age(25) # 调用实例方法
  >>> s.age # 测试结果
  25
  ```

#### 1.3.2. 类方法

类方法定义时使用装饰器 `@classmethod`；

类方法是只在类中运行而不在实例中运行的方法；

类方法以 cls 作为第一个参数，cls 表示类本身，通过 cls 来访问类的相关属性；

类方法的参数不能传入 self，因此不能访问实例属性；

类方法可以通过类名访问，也可以通过实例访问；

- 使用场景？意义何在？

  与类属性一样，类方法是类固有的方法与属性，不会因为实例不同而改变，不会因为创建多个实例而在内存中创建多个副本，其意义和目的是减少创建多个实例时所创造出来的内存空间，加快运行速度。

例：
```python
class Student(object):
    '''
    this is a Student class
    '''
    count = 0
    books = []
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def printClassInfo(cls):
        print(cls.__name__)
        print(dir(cls))
    pass

Student.printClassInfo()
wilber = Student("Wilber", 28)
wilber.printClassInfo()
```

类似类属性可以被动态绑定，python 支持动态绑定类方法，该方法对该类的所有实例有效；
```python
>>> def set_score(self, score):
...     self.score = score
...
>>> Student.set_score = set_score
```
给 class 绑定方法后，所有实例均可调用：
```python
>>> s.set_score(100)
>>> s.score
100
>>> s2.set_score(99)
>>> s2.score
99
```

##### 1.3.2.1. 魔法方法

命名为 __xxx__() 形式的类方法在 Python 中都是有特殊用途的，因此也被称为 “魔法方法”；

- `__len__()`

  `__len__`方法返回长度。在 Python 中，如果你调用 len() 函数试图获取一个对象的长度，实际上，在 len() 函数内部，它自动去调用该对象的`__len__()`方法，所以，下面的代码是等价的：
  ```python
  >>> len('ABC')
  3
  >>> 'ABC'.__len__()
  3
  ```
  我们自己写的类，如果也想用 len(myObj) 的话，就自己实现一个__len__() 方法：
  ```python
  >>> class MyDog(object):
  ...     def __len__(self):
  ...         return 100
  ...
  >>> dog = MyDog()
  >>> len(dog)
  100
  ```

- `__str__()`

  在使用 print 函数打印类时，python 解释器会自动调用类的 __str__() 方法；
  ```python
  >>> class Student(object):
  ...     def __init__(self, name):
  ...         self.name = name
  ...
  >>> print(Student('Michael')) # 调用缺省的 __str__() 方法
  <__main__.Student object at 0x109afb190>
  ```

  ```python
  >>> class Student(object):
  ...     def __init__(self, name):
  ...         self.name = name
  ...     def __str__(self):
  ...         return 'Student object (name: %s)' % self.name
  ...
  >>> print(Student('Michael')) # 调用自定义的__str__() 方法
  Student object (name: Michael)
  ```

- `__repr__()`

  当在交互式解释器中键入类的实例名时，会调用 __repr__() 方法；
  ```python
  >>> s = Student('Michael')
  >>> s
  <__main__.Student object at 0x109afb310>
  ```

  ```python
  class Student(object):
      def __init__(self, name):
          self.name = name
      def __str__(self):
          return 'Student object (name=%s)' % self.name
      __repr__ = __str__ # 通常__str__() 和__repr__() 代码都是一样的，所以采用偷懒的写法
  >>> s = Student('Michael')
  >>> s
  Student object (name: Michael)
  ```

- `__iter__()` 和 `__next__()`

  如果一个类想被用于 for ... in 循环，类似 list 或 tuple 那样，就必须实现一个__iter__() 方法，该方法返回一个迭代对象，然后，Python 的 for 循环就会不断调用该迭代对象的__next__() 方法拿到循环的下一个值，直到遇到 StopIteration 错误时退出循环

  以斐波那契数列为例，写一个 Fib 类，可以作用于 for 循环：
  <!-- TODO: 没搞懂？？？  -->
  ```python
  class Fib(object):
      def __init__(self):
          self.a, self.b = 0, 1 # 初始化两个计数器 a，b

      def __iter__(self):
          return self # 实例本身就是迭代对象，故返回自己

      def __next__(self):
          self.a, self.b = self.b, self.a + self.b # 计算下一个值 具体执行顺序是怎么样的？先谁给谁赋值？？
          if self.a > 100000: # 退出循环的条件
              raise StopIteration()
          return self.a # 返回下一个值

  >>> for n in Fib():
  # TODO: 具体迭代是怎么执行的？？？
  ...     print(n)
  ...
  1
  2
  3
  5
  ...
  46368
  75025
  ```

- [其它](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319098638265527beb24f7840aa97de564ccc7f20f6000 )

#### 1.3.3. 静态方法

与实例方法和类方法不同，静态方法没有参数限制（既不需要实例参数，也不需要类参数），定义的时候使用装饰器 @staticmethod；

静态方法与类相关但是不依赖类与实例；

- 使用场景？意义何在？

  经常有一些跟类有关系的功能但在运行时又不需要实例和类参与的情况下需要用到静态方法。比如更改环境变量或者修改其他类的属性等能用到静态方法。这种情况虽然可以直接用函数解决，但会扩散类内部的代码，造成维护困难，采用类的静态方法是最好的选择；

例：
```python
class Student(object):
    '''
    this is a Student class
    '''
    count = 0
    books = []
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @staticmethod
    def printClassAttr():
        print(Student.count)
        print(Student.books)
    pass

Student.printClassAttr()
wilber = Student("Wilber", 28)
wilber.printClassAttr()
```

静态方法与类方法的比较：

- **静态方法有点像函数工具库的作用，相当于一个相对独立的方法，而类方法则更接近类似 Java 面向对象概念中的静态方法；**

- 静态方法不能传入 self 参数，类成员方法需传入代表本类的 cls 参数；

- 静态方法与类方法都可以通过类或者实例来调用，都不能够调用实例属性；

#### 1.3.4. 实例、静态、类方法辨析

https://www.zhihu.com/question/20021164

```python
class Kls(object):
    def __init__(self, data):
        self.data = data
    def printd(self):
        print(self.data)
    @staticmethod
    def smethod(*arg):
        print('Static:', arg)
    @classmethod
    def cmethod(*arg):
        print('Class:', arg)

>>> ik = Kls(23)
>>> ik.printd()
23
>>> ik.smethod()
Static: ()
>>> ik.cmethod()
Class: (<class '__main__.Kls'>,)
>>> Kls.printd()
TypeError: unbound method printd() must be called with Kls instance as first argument (got nothing instead)
>>> Kls.smethod()
Static: ()
>>> Kls.cmethod()
Class: (<class '__main__.Kls'>,)
```

### 1.4. 访问控制

Python 中没有访问控制的关键字，例如 private、protected 等等。但是，在 Python 编码中，有一些约定来进行访问控制；

- 单下划线"`_`"

  在 Python 中，通过单下划线"`_`"来实现模块级别的私有化，一般约定以单下划线"`_`"开头的变量、函数为模块私有的，也就是说"from moduleName import *"将不会引入以单下划线"_"开头的变量、函数，如果访问将会抛出异常：`xx is not defined`；

  可以理解为以单下划线开头的表示的是 **protected** 类型的变量 / 方法，即保护类型只能允许其本身与子类进行访问，不能用于 from module import *；

- 双下划线"__"

  双下划线的表示的是私有类型 (private) 的变量 / 方法，不能在类地外部调用，只允许在这个类本身进行访问；

  通过内建函数 dir() 可以看到，以双下划线“`__`”开头命名的属性在运行时，属性名会被改为"`_类名__属性名`"（属性名前增加了单下划线和类名），如"`__address`"属性在运行时，属性名被改为了"`_Student__address`"，因此，若直接访问该属性，会抛出异常：object has no attribute；

  但即使是双下划线，也没有实现属性的私有化，通过下面的方式还是可以直接访问"__address"属性：
  ```python
  >>> wilber = Student("Wilber", 28)
  >>> print wilber._Student__address
  Shanghai
  ```
  需要注意：不同版本的 Python 解释器可能会把 __address 改成不同的变量名；

因此，可以看到，**python 中下划线"_"和" __"的使用，更多的是一种规范 / 约定，而不是像 cpp、Java 那样真正实现访问权限控制**。

### 1.5. 获取类的信息

#### 1.5.1. type()

使用 type() 返回对应的 Class 类型；

#### 1.5.2. isinstance()

使用 isinstance() 可以判断 class 的继承关系，例：

如果继承关系是：`object -> Animal -> Dog -> Husky`
```python
>>> a = Animal()
>>> d = Dog()
>>> h = Husky()
>>> isinstance(h, Husky)
True
>>> isinstance(h, Dog)
True
>>> isinstance(h, Animal)
True
```
能用 type() 判断的基本类型也可以用 isinstance() 判断：
```python
>>> isinstance('a', str)
True
>>> isinstance(123, int)
True
>>> isinstance(b'a', bytes)
True
```
并且还可以判断一个变量是否是某些类型中的一种，比如下面的代码就可以判断是否是 list 或者 tuple：
```python
>>> isinstance([1, 2, 3], (list, tuple))
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```

#### 1.5.3. dir()

如果要获得一个对象的所有属性和方法，可以使用`dir()`函数，它返回一个包含字符串的 list，比如，获得一个 str 对象的所有属性和方法：
```python
>>> dir('ABC')
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', '__gt__', '__hash__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mod__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__rmod__', '__rmul__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'capitalize', 'casefold', 'center', 'count', 'encode', 'endswith', 'expandtabs', 'find', 'format', 'format_map', 'index', 'isalnum', 'isalpha', 'isdecimal', 'isdigit', 'isidentifier', 'islower', 'isnumeric', 'isprintable', 'isspace', 'istitle', 'isupper', 'join', 'ljust', 'lower', 'lstrip', 'maketrans', 'partition', 'replace', 'rfind', 'rindex', 'rjust', 'rpartition', 'rsplit', 'rstrip', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

#### 1.5.4. getattr()、setattr()、hasattr()

如果试图获取不存在的属性，会抛出 AttributeError 的错误：
```python
>>> getattr(obj, 'z') # 获取属性'z'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'MyObject' object has no attribute 'z'
```
因此，所有类的父类 Oject 类提供了 getattr()、setattr()、hasattr() 方法：
```python
>>> hasattr(obj, 'x') # 有属性'x'吗？
True
>>> obj.x
9
>>> hasattr(obj, 'y') # 有属性'y'吗？
False
>>> setattr(obj, 'y', 19) # 设置一个属性'y'
>>> hasattr(obj, 'y') # 有属性'y'吗？
True
>>> getattr(obj, 'y') # 获取属性'y'
19
>>> obj.y # 获取属性'y'
19
```
要注意的是，只有在不知道对象信息的时候，我们才会去获取对象信息。如果可以直接写：
```python
sum = obj.x + obj.y
```
就不要写：
```python
sum = getattr(obj, 'x') + getattr(obj, 'y')
```

### 1.6. 枚举类

Python 提供了 Enum 类来实现枚举类；

Enum 可以把一组相关常量定义在一个 class 中，且 class 不可变，而且成员可以直接比较，每个常量都是 class 的一个唯一实例；

- 方式一：
  ```python
  from enum import Enum
  # 定义 Month 类型的枚举类
  Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
  # 可以直接使用 Month.Jan 来引用一个常量
  for name, member in Month.__members__.items():
  print(name, '=>', member, ',', member.value)
  # value 属性则是自动赋给成员的 int 常量，默认从 1 开始计数
  ```

- 方式二：从 Enum 派生出自定义类
  ```python
  from enum import Enum, unique

  @unique
  # @unique 装饰器可以帮助我们检查保证没有重复值
  class Weekday(Enum):
      Sun = 0 # Sun 的 value 被设定为 0
      Mon = 1
      Tue = 2
      Wed = 3
      Thu = 4
      Fri = 5
      Sat = 6
  ```
  使用：
  ```python
  >>> day1 = Weekday.Mon
  >>> print(day1)
  Weekday.Mon
  >>> print(Weekday.Tue)
  Weekday.Tue
  >>> print(Weekday['Tue'])
  Weekday.Tue
  >>> print(Weekday.Tue.value)
  2
  >>> print(day1 == Weekday.Mon)
  True
  >>> print(day1 == Weekday.Tue)
  False
  >>> print(Weekday(1))
  Weekday.Mon
  >>> print(day1 == Weekday(1))
  True
  >>> Weekday(7)
  Traceback (most recent call last):
    ...
  ValueError: 7 is not a valid Weekday
  >>> for name, member in Weekday.__members__.items():
  ...     print(name, '=>', member)
  ...
  Sun => Weekday.Sun
  Mon => Weekday.Mon
  Tue => Weekday.Tue
  Wed => Weekday.Wed
  Thu => Weekday.Thu
  Fri => Weekday.Fri
  Sat => Weekday.Sat

  ```

#### 1.6.1. 元类

[参考原文](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319106919344c4ef8b1e04c48778bb45796e0335839000 )

### 1.7. 继承

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

### 1.8. 方法重写

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

#### 1.8.1. 基础重写方法

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

#### 1.8.2. 运算符重载

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

### 1.9. 多重继承

通过多重继承，一个子类就可以同时获得多个父类的所有功能。

例：
```python
class Bat(Mammal, Flyable):
    pass
```

## 2. 多态

例：
```python
class Animal(object):
    def run(self):
        print('Animal is running...')
class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')

def run_twice(animal):
    animal.run()
```
当我们传入 Animal 的实例时：
```python
>>> run_twice(Animal())
Animal is running...
```
当我们传入 Dog 的实例时：
```python
>>> run_twice(Dog())
Dog is running...
```
当我们传入 Cat 的实例时：
```python
>>> run_twice(Cat())
Cat is running...
```

### 2.1. “开闭”原则

比如：
```python
class Animal(object):
    def run(self):
        print('Animal is running...')

class Dog(Animal):

    def run(self):
        print('Dog is running...')

class Cat(Animal):

    def run(self):
        print('Cat is running...')
```

多态的好处就是，当我们需要传入 Dog、Cat、Tortoise……时，我们只需要接收 Animal 类型就可以了，因为 Dog、Cat、Tortoise……都是 Animal 类型，然后，按照 Animal 类型进行操作即可。由于 Animal 类型有 run() 方法，因此，传入的任意类型，只要是 Animal 类或者子类，就会自动调用实际类型的 run() 方法，这就是多态的意思：

对于一个变量，我们只需要知道它是 Animal 类型，无需确切地知道它的子类型，就可以放心地调用 run() 方法，而具体调用的 run() 方法是作用在 Animal、Dog、Cat 还是 Tortoise 对象上，由运行时该对象的确切类型决定，这就是多态真正的威力：调用方只管调用，不管细节，而当我们新增一种 Animal 的子类时，只要确保 run() 方法编写正确，不用管原来的代码是如何调用的。

这就是著名的“开闭”原则：

1)	对扩展开放：允许新增 Animal 子类；

2)	对修改封闭：不需要修改依赖 Animal 类型的 run_twice() 等函数；

### 2.2. 鸭子类型

对于静态语言（例如 Java）来说，如果需要传入 Animal 类型，则传入的对象必须是 Animal 类型或者它的子类，否则，将无法调用 run() 方法。

对于 Python 这样的动态语言来说，则不一定需要传入 Animal 类型。我们只需要保证传入的对象有一个 run() 方法就可以了：
```python
class Timer(object):
    def run(self):
        print('Start...')
```
这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。

<!-- TODO: 理解：看有没有操作的方法，而不看类的类型是什么 -- 弱化了类型的概念和限制 -- 动态语言的特点 -- “看实力，不看学历” -->

Python 的 “file-like object“ 就是一种鸭子类型。对真正的文件对象，它有一个 read() 方法，返回其内容。但是，许多对象，只要有 read() 方法，都被视为 “file-like object“。许多函数接收的参数就是“file-like object“，你不一定要传入真正的文件对象，完全可以传入任何实现了 read() 方法的对象。

例如：

要实现一个读取图像的功能：
```python
def readImage(fp):
    if hasattr(fp, 'read'):
        return readData(fp)
return None
```
根据鸭子类型，有 read() 方法，不代表该 fp 对象就是一个文件流，它也可能是网络流，也可能是内存中的一个字节流，但只要 read() 方法返回的是有效的图像数据，就不影响读取图像的功能；

## 3. Refer Links
