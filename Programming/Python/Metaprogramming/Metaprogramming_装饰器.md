- [元编程：装饰器 (Decorator)](#元编程装饰器-decorator)
  - [1. Function Decorator](#1-function-decorator)
    - [1.1. 函数装饰函数](#11-函数装饰函数)
    - [1.2. 函数装饰类](#12-函数装饰类)
    - [1.3. 函数装饰类方法](#13-函数装饰类方法)
  - [2. Class Decorator](#2-class-decorator)
    - [2.1. 类装饰函数](#21-类装饰函数)
    - [2.2. 类装饰类](#22-类装饰类)
    - [2.3. 类装饰类方法](#23-类装饰类方法)
  - [3. Refer Links](#3-refer-links)

# 元编程：装饰器 (Decorator)

> A function returning another function, usually applied as a function transformation using the `@wrapper` syntax. Common examples for decorators are `classmethod()` and `staticmethod()`.
>
> The decorator syntax is merely syntactic sugar, the following two function definitions are semantically equivalent:

装饰器本质上是一个返回 Python 函数的函数，它让其他函数在不需要做任何代码变动的前提下在代码运行期间动态增加功能。实际上，装饰器也是一个闭包，它把一个函数当做参数然后返回一个替代版函数。

类似于 Java 中的注解 (Annotation)，装饰器经常用于有切面需求的场景，比如：**插入日志、性能测试、事务处理、缓存、权限控制、异常拦截、自动重试**等场景。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量与函数功能本身无关的雷同代码并继续重用。概括的讲，装饰器的作用就是为已经存在的对象添加额外的功能。事实上， python 的许多框架中都大量的使用了装饰器，也说明了装饰器的强大与优雅。

装饰器在导入模块时立即执行，而被装饰的函数只在明确调用时运行。

## 1. Function Decorator

### 1.1. 函数装饰函数

- 无参装饰器
  ```python
  def use_logging(func):

      def wrapper(*args, **kwargs):
          logging.warn("%s is running" % func.__name__)
          return func(*args, **kwargs)
      return wrapper

  def bar():
      print('i am bar')

  bar = use_logging(bar)
  bar()
  ```
  函数 use_logging 就是装饰器，它把执行真正业务方法的 func 包裹在函数里面，看起来像 bar 被 use_logging 装饰了，这种编程方式被称为面向切面的编程 (Aspect-Oriented Programming)。

  为进一步简化操作，Python 提供了 `@` 符号作为装饰器的语法糖，在定义函数的时候使用，避免再一次赋值操作：
  ```python
  def use_logging(func):

      def wrapper(*args, **kwargs):
          logging.warn("%s is running" % func.__name__)
          return func(*args)
      return wrapper

  @use_logging
  def foo():
      print("i am foo")

  foo()
  ```

  e.g. 经典案例：用 decorator 实现斐波拉契数列
  ```python
  from functools import wraps

  def memo(fn):
      cache = {}
      miss = object()

      @wraps(fn)
      def wrapper(*args):
          result = cache.get(args, miss)
          if result is miss:
              result = fn(*args)
              cache[args] = result
          return result

      return wrapper

  @memo
  def fib(n):
      if n < 2:
          return n
      return fib(n - 1) + fib(n - 2)
  ```

- 带参装饰器

  装饰器的语法允许我们在调用时，提供其它参数，比如 `@decorator(a)`。这样，就为装饰器的编写和使用提供了更大的灵活性。
  ```python
  def use_logging(level):
      def decorator(func):
          def wrapper(*args, **kwargs):
              if level == "warn":
                  logging.warn("%s is running" % func.__name__)
              return func(*args, **kwargs)
          return wrapper

      return decorator

  @use_logging(level="warn")
  def foo(name='foo'):
      print("i am %s" % name)

  foo()
  ```
  当使用 `@use_logging(level="warn")` 调用的时候，Python 能够发现这一层的封装，并把参数传递到装饰器的环境中。

  e.g. 使用 decorator 封装异常处理
  ```python
  from functools import wraps

  def try_except(errors=(Exception,), default_value='default'):
      def decorator(func):
          @wraps(func)
          def wrapper(*args, **kwargs):
              try:
                  return func(*args, **kwargs)
              except errors as e:
                  print "Got error! ", repr(e)
                  return default_value

          return wrapper

      return decorator

  @try_except(errors=(ValueError, KeyError))
  def example1(a):
      return a['b']

  if __name__ == '__main__':
      print example1({})
  ```

NOTE:
- 使用 `functools.wraps` 保留元信息

  使用装饰器极大地复用了代码，但是他有一个缺点就是原函数的元信息不见了，比如函数的 `docstring`、`__name__`、参数列表等：
  ```python
  def logged(func):
      def with_logging(*args, **kwargs):
          print func.__name__ + " was called"
          return func(*args, **kwargs)
      return with_logging

  @logged
  def f(x):
    """does some math"""
    return x + x * x

  print f.__name__    # prints 'with_logging'
  print f.__doc__     # prints None
  ```
  因此，可以使用 `functools.wraps` 来把原函数的元信息拷贝到装饰器函数中，从而使得装饰器函数也有和原函数一样的元信息：
  ```python
  from functools import wraps

  def logged(func):
      @wraps(func)
      def with_logging(*args, **kwargs):
          print func.__name__ + " was called"
          return func(*args, **kwargs)
      return with_logging

  @logged
  def f(x):
      """does some math"""
      return x + x * x

  print f.__name__  # prints 'f'
  print f.__doc__   # prints 'does some math'
  ```

- 嵌套装饰器

  装饰器的顺序
  ```python
  @a
  @b
  @c
  def f():
  ```
  等效于
  ```python
  f = a(b(c(f)))
  ```

  e.g.
  ```python
  def sayName(func):
      print('name')

      def inner():
          print("I'm Yu")
          return func()

      return inner

  def sayAge(func):
      print('age')

      def inner():
          print("i'm 30")
          return func()

      return inner

  @sayName
  @sayAge
  def sayHi():
      print('Hello, World')

  if __name__ == '__main__':
      sayHi()
  ```
  运行结果：
  ```
  age
  name
  I'm Yu
  i'm 30
  Hello, World
  ```
  分析：
  1. Python 解释器执行到第一个装饰器 `@sayName`，在接下来发现装饰器下边不是一个函数而是另一个装饰器，解释器会执行第二个的装饰器 `@sayAge`，然后把 sayHi 函数传入装饰器 sayAge，所以首先输出了“age”。
  1. 当 `@sayAge` 装饰完成，此时的 sayHi 函数地址指向了 sayAge.inner 的地址，解释器会返回去执行 `@sayName` 装饰器来装饰新的 sayHi，从而输出了“name”。
  1. 接着函数当前指向 `sayName.inner` 会先输出“I'm Yu”，在这里返回的 `func()` 其实就是返回的 `sayAge.inner`，所以在下面输出 i'm 30，最后输出原本 sayHi 的“Hello, World”

### 1.2. 函数装饰类

装饰的对象是类时，**只在类进行实例化（执行 `__init__` 方法）的时候，会调用装饰器；此后实例化的类对象调用任何类成员都不会再调用装饰器**。

e.g.
```python
def wrapClass(cls):
    def inner(a):
        print('class name:', cls.__name__)
        return cls(a)
    return inner

@wrapClass
class Foo():
    def __init__(self, a):
        self.a = a

    def fun(self):
        print('self.a =', self.a)

m = Foo('xiemanR')
m.fun()
```

若希望可以对类进行装饰后，**自动对类的所有方法进行装饰**，可参考 [How to decorate all functions of a class without typing it over and over for each method? [duplicate]?](https://stackoverflow.com/questions/6307761/how-to-decorate-all-functions-of-a-class-without-typing-it-over-and-over-for-eac):
```python
def for_all_methods(decorator):
    def decorate(cls):
        for attr in cls.__dict__: # there's propably a better way to do this
            if callable(getattr(cls, attr)):
                setattr(cls, attr, decorator(getattr(cls, attr)))
        return cls
    return decorate

@for_all_methods(mydecorator)
class C(object):
    def __init__(self): pass
    def m1(self): pass
    def m2(self, x): pass
    ...
```

### 1.3. 函数装饰类方法

e.g.
```python
def tracer(func):
    count = 0
    def wrapper(*args,**kwargs):
        nonlocal count
        count += 1
        print("the count now is %s:%s"%(count,func.__name__))
        return func(*args,**kwargs)  # 不同于类修饰类方法，此处不用传入 self
    return wrapper

class Person:
    def __init__(self):
        pass
    @tracer
    def sayHi(self,name):
        print("Hi %s"%name)
    @tracer
    def sayHello(self):
        print("Hello guys")

p = Person()
p.sayHi("Corkine")
p.sayHello()
```

## 2. Class Decorator

被类装饰时，被装饰者相当于充当装饰器类的一个实例。

当实现 `__call__` 方法时，类可以被当做一个函数使用，因此利用这一点，在 `__call__` 魔术方法中传递参数并且进行函数的保存和使用，可以实现 Class Decorator。相比函数装饰器，类装饰器具有灵活度大、高内聚、封装性等优点。

调用顺序：
1. Python 解释器解释到 `@decorator(...)` 时，调用装饰器类的 `__init__(...)` 进行实例化。
1. Python 解释器解释到被装饰的 fucntion 或 class 时，调用装饰器类的 `__call__(...)` 方法。

### 2.1. 类装饰函数

- 无参数装饰器

  e.g.
  ```python
  class Foo(object):
      def __init__(self, func):
      self._func = func

      def __call__(self):
          print ('class decorator runing')
          self._func()
          print ('class decorator ending')

  @Foo
  def bar():
      print ('bar')

  bar()
  ```

- 带参数装饰器

  **当装饰器需要带参数时，最佳实践证明，最好在装饰器外面套上一层函数作为参数保存的位置**，在里面可以放置之前的嵌套函数装饰器或者类装饰器。

  e.g.
  ```python
  import time
  def timer(trace=True, label=""): #同理，这里也不需要写 func 传递进去。因为 func 这个参数的传递是 timer()(func) 在这个位置
      class Timer:
          def __init__(self, func):
              self.func = func
              self.alltime = 0
              self.trace = trace
              self.label = label
          def __call__(self,*args,**kwargs):
              now = time.clock()
              r = self.func(*args,**kwargs)
              end = time.clock()
              t = end - now
              if trace:
                  print("%s Time for %s is %.5f"%(label,self.func.__name__,t))
              return r
      return Timer # 注意这里默认不需要写 () 传递 func 进去

  #本质上是这样调用的：timer(trace,label)(func)(n=10000)
  @timer(trace=True,label="CCC==>")
  def test(n):
      return [x**20 for x in range(n)]
  test(100000)
  test(2000000)
  @timer(trace=False,label="CCC==>")
  def test2(n):
      return [x**20 for x in range(n)]
  test2(1000)
  print("This is END")
  ```
  运行结果：
  ```
  CCC==> Time for test is 0.06214
  CCC==> Time for test is 1.00973
  This is END
  ```

### 2.2. 类装饰类

- 无参数装饰器

  e.g.
  ```python
  class ShowClassName(object):
      def __init__(self, cls):
          self._cls = cls

      def __call__(self, a):
          print('class name:', self._cls.__name__)
          return self._cls(a)

  @ShowClassName
  class Foobar(object):
      def __init__(self, a):
          self.value = a

      def fun(self):
          print(self.value)

  a = Foobar('xiemanR')
  a.fun()
  ```

  e.g.
  ```python
  class singleton:
      def __init__(self, aClass): # On @ decoration
          self.aClass = aClass
          self.instance = None
      def __call__(self, *args): # On instance creation
          if self.instance == None:
              self.instance = self.aClass(*args) # One instance per class
          return self.instance

  @singleton
  class Person: # Rebinds Person to onCall
      def __init__(self, name, hours, rate): # onCall remembers Person
          self.name = name
          self.hours = hours
          self.rate = rate
      def pay(self):
          return self.hours * self.rate

  cm = Person("Corkine",23,12)
  print(cm.pay())
  mv = Person("Corkine2",230,32) #如果当前有实例的话，则直接调用此实例而不是新建一个实例
  print(mv.pay())
  ```

- 带参数装饰器

  e.g.
  ```python
  def wrapper(level):
      print level

      class Wrapper:
          def __init__(self, cls):
              self._cls = cls

          def __call__(self, *args, **kwargs):
              print args[0]
              return self._cls(args[0])

      return Wrapper

  @wrapper(level='test')
  class Foobar(object):
      def __init__(self, a):
          self.value = a

      def fun(self):
          print 'class inner: ', self.value

  if __name__ == '__main__':
      a = Foobar('hello')
      a.fun()
  ```
  运行结果：
  ```
  test
  hello
  class inner:  hello
  ```

### 2.3. 类装饰类方法

e.g.
```python
class tracer:
    def __init__(self, func):
        self.count = 0
        self._func = func

    def __call__(self, *args, **kwargs):
        self.count += 1
        print("The count now is %s：%s" % (self.count, self._func.__name__))
        # return self._func(*args, **kwargs)  # 此处会抛出错误：TypeError: sayHi() takes exactly 2 arguments (1 given)
        return self._func(self, *args, **kwargs)  # 正常运行

class Person:
    def __init__(self):
        pass

    @tracer
    def sayHi(self, name):
        print("Hello %s" % name)

    @tracer
    def sayHello(self):
        print("Hello guys")

if __name__ == '__main__':
    p = Person()
    p.sayHi("Corkine")
    p.sayHello()
```

## 3. Refer Links

[如何理解 Python 装饰器？](https://www.zhihu.com/question/26930016)

[深入讨论 Python 装饰器 （与 Java Aop 对比思考)](https://juejin.im/post/5d568a07e51d4561a705bae4)

[PYTHON 修饰器的函数式编程](https://coolshell.cn/articles/11265.html)

[Python 的函数是怎么传递参数的？](https://www.zhihu.com/question/20591688)

TODO:

[Python 学习笔记 - 装饰器和元类基础](https://blog.mazhangjing.com/2018/03/13/learn_python_1/)