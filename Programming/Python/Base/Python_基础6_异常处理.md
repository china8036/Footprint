- [Python 基础：异常处理](#python-基础异常处理)
  - [1. 标准异常](#1-标准异常)
  - [2. 自定义异常](#2-自定义异常)
  - [3. 捕获异常：try/except/finally 语句块](#3-捕获异常tryexceptfinally-语句块)
  - [4. 抛出异常：raise 语句](#4-抛出异常raise-语句)
  - [5. 断言：assert 语句](#5-断言assert-语句)
  - [6. 最佳实践](#6-最佳实践)
  - [7. Refer Links](#7-refer-links)

# Python 基础：异常处理

在 Python 中，异常是一个对象，Python 为异常处理提供了 2 种方式：
- try/except/finally 语句块
- assert 断言

## 1. 标准异常

Python 在 exceptions 模块中内建了很多的标准异常类型，可以通过使用 dir() 函数来查看 exceptions 中的异常类型：
```python
import exceptions

print dir(exceptions)
```

[异常体系](https://docs.python.org/2.7/library/exceptions.html#exceptions.BaseException)：

- `BaseException`: 所有异常的基类
  - `SystemExit`: 解释器请求退出
  - `KeyboardInterrupt`: 用户中断执行（通常是输入 ^C)
  - `GeneratorExit`: 生成器 (generator) 发生异常来通知退出
  - `Exception`: 常规错误的基类
    - `StopIteration`: 迭代器没有更多的值
    - `StandardError`: 所有的内建标准异常的基类
      - `BufferError`
      - `ArithmeticError`: 所有数值计算错误的基类
        - `FloatingPointError`: 浮点计算错误
        - `OverflowError`: 数值运算超出最大限制
        - `ZeroDivisionError`: 除（或取模) 零 （所有数据类型)
      - `AssertionError`: 断言语句失败
      - `AttributeError`: 对象没有这个属性
      - `EOFError`: 没有内建输入，到达 EOF 标记
      - `EnvironmentError`: 操作系统错误的基类
        - `IOError`: 输入 / 输出操作失败
        - `OSError`: 操作系统错误
          - `WindowsError`: 系统调用失败 (Windows)
          - `VMSError`: VMS
      - `ImportError`: 导入模块 / 对象失败
      - `LookupError`: 无效数据查询的基类
        - `IndexError`: 序列中没有此索引 (index)
        - `KeyError`: 映射中没有这个键
      - `MemoryError`: 内存溢出错误（对于 Python 解释器不是致命的)
      - `NameError`: 未声明 / 初始化对象 （没有属性)
        - `UnboundLocalError`: 访问未初始化的本地变量
      - `ReferenceError`: 弱引用 (Weak reference) 试图访问已经垃圾回收了的对象
      - `RuntimeError`: 一般的运行时错误
        - `NotImplementedError`: 尚未实现的方法
      - `SyntaxError`: Python 语法错误
        - `IndentationError`: 缩进错误
          - `TabError`: Tab 和空格混用
      - `SystemError`: 一般的解释器系统错误
      - `TypeError`: 对类型无效的操作
      - `ValueError`: 传入无效的参数
        - `UnicodeError`: Unicode 相关的错误
          - `UnicodeDecodeError`: Unicode 解码时的错误
          - `UnicodeEncodeError`: Unicode 编码时错误
          - `UnicodeTranslateError`: Unicode 转换时错误
      - `Warning`: 警告的基类
        - `DeprecationWarning`: 关于被弃用的特征的警告
        - `FutureWarning`: 关于构造将来语义会有改变的警告
        - `OverflowWarning`: 旧的关于自动提升为长整型 (long) 的警告
        - `PendingDeprecationWarning`: 关于特性将会被废弃的警告
        - `RuntimeWarning`: 可疑的运行时行为 (runtime behavior) 的警告
        - `SyntaxWarning`: 可疑的语法的警告
        - `UserWarning`: 用户代码生成的警告

NOTE；
- 在捕获所有异常时更应该使用 Exception 而不是 BaseException，因为 BaseException 的其它三个子异常 SystemExit、KeyboardInterrupt、GeneratorExit 属于更高级别的异常，合理的做法应该是交给 Python 的解释器处理。

## 2. 自定义异常

> Programs may name their own exceptions by creating a new exception class (see Classes for more about Python classes). Exceptions should typically be derived from the Exception class, either directly or indirectly.

在 Python 中，所有的异常都属于类，且所有这些异常类都直接或间接地继承自 Exception 类，因此，开发者可以通过直接或间接地继承 Exception 类来实现自定义的异常类。

e.g.
```python
>>> class MyError(Exception):
...     def __init__(self, value):
...         self.value = value
...     def __str__(self):
...         return repr(self.value)
...
>>> try:
...     raise MyError(2*2)
... except MyError as e:
...     print 'My exception occurred, value:', e.value
...
My exception occurred, value: 4
>>> raise MyError('oops!')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
__main__.MyError: 'oops!'
```

e.g.

编写异常 UppercaseException，在一个字符串中碰到大写字母会被抛出。
```python
>>> class UppercaseException(Exception):
...   pass
...
# 即使没有定义 UppercaseException 的行为（注意到只使用 pass），也可以通过继承其父类 Exception 在抛出异常时输出错误提示
>>> words = ['eeenie', 'meenie', 'miny', 'MO']
>>> for word in words:
... 	if word.isupper():
... 		raise UppercaseException(word)
...
Traceback (most recent call last):
File "<stdin>", line 3, in <module>
__main__.UppercaseException: MO
```

NOTE:
- **如果内置异常已经包括了你需要的异常，建议考虑使用内置的异常类型**，如：你希望在函数参数错误时抛出一个异常，你可能并不需要定义一个 InvalidArgumentError，使用内置的 ValueError 即可。

- 自定义异常类 can be defined which do anything any other class can do，但通常 kept simple，只提供一些便于处理异常的关键信息。

- 自定义异常类通常模仿 the standard exceptions 的命名方式，end in “Error”。

## 3. 捕获异常：try/except/finally 语句块

在 Python 中，和大部分高级语言一样，使用了 try/except/finally 语句块来处理异常。

```python
try:
    <code>        #运行别的代码
except <Exception name>：
    <codee>       #该 try 子句捕获 'name' 异常
except <Exception name_1]>, <Exception name_2]>
    <code>        #except 语句按元组形式指定多个异常，该 try 子句捕获 'name_1' 和 `name_2` 异常
except <Exception name>，<Argument>:
    <code>        #该 try 子句捕获 'name' 异常，并且传递附加的参数，可作为输出的异常信息参数
except <Exception name> as alias:
    <code>        #该 try 子句捕获 'name' 异常，并设置别名，在 except 子句中可直接使用别名
else:
    <code>        #如果没有异常发生
finally:
    <code>        #退出 try 语句块时总会执行
```

NOTE:
- except 语句可以有多个，Python 会按 except 语句的顺序依次匹配你指定的异常，如果异常已经处理就不会再进入后面的 except 语句。
- 如果在 try 后的语句里发生了异常，却没有匹配的 except 子句，异常将被递交到上层的 try，直到程序的最上层（这样将结束程序，并打印默认的出错信息）。
- 如果 except 子句没有指定 Exception name，那么该子句可以捕获所有的异常。但这不是一个很好的方式，因为我们不能通过该程序有针对性地识别出具体的异常信息；**且通常不建议在不清楚逻辑的情况下捕获所有异常，有可能你隐藏了很严重的问题**。

- except 语句不是必须的，finally 语句也不是必须的，但是**二者必须要有一个**，否则就没有使用 try 语句块的意义了。
- 尽量使用内置的异常处理语句来替换 try/except 语句，比如 with 语句、getattr() 方法等。

e.g.
```python
def temp_convert(var):
    try:
        return int(var)
    except ValueError, Argument:
        print "参数没有包含数字、n", Argument

# 调用函数
temp_convert("xyz")
```

## 4. 抛出异常：raise 语句

通过 raise 语句可以在程序中主动抛出异常，等同于 C#和 Java 中的 `throw`：
```python
raise [Exception [, args [, traceback]]]
```
其中：
- Exception 是异常的类型（如 NameError），可选参数。
- args 是主动传递的异常参数，可选参数。
- trackback 是跟踪异常对象，可选参数。

NOTE:
- **如果捕获异常后要[重复抛出（直接传递异常）](http://www.ianbicking.org/blog/2007/09/re-raising-exceptions.html)，可以使用不带任何参数的 raise 语句**：
  ```python
  try:
      do_work()
  except Exception as e:
      # get detail from logging module
      logging.exception('Exception caught!')

      # get detail from sys.exc_info() method
      error_type, error_value, trace_back = sys.exc_info()
      print(error_value)

      # update the exception object
      e.args += ('more info',)
      raise
  ```
- 把字符串当成异常抛出看上去是一个非常简洁的办法，但其实是一个非常不好的习惯：
  ```python
  if is_work_done():
      pass
  else:
      raise "Work is not done!" # not cool
  ```
  在 Python2.4 以前是可以接受的做法，但是没有指定异常类型有可能会让下游没办法正确捕获并处理这个异常，从而导致你的程序难以维护。

## 5. 断言：assert 语句

断言被用作你接下来的程序执行，如果后面程序依赖于前面的程序，后面的程序又很重要，就是后面的程序执行肯定不能出错，所以在执行之前要做检查工作。

**若 assert 判断为 False，则抛出 AssertionError 异常**。

NOTE:
- 使用 if 语句也可以实现完全相同的效果，但相比之下 assert 更加优雅。
- 异常和断言
  - **assert 捕获的是程序员自己的错误，处理的是程序的开发期错误，assert 语句应满足在 release 代码中可以全部直接移除而不影响程序的执行。**
  - **exception 捕获的是用户或环境的错误，处理的是程序的运行期错误**。
  - 对于一个函数：
    - 在开始执行该函数之前，是需要满足一系列条件的，这些条件被称为“前条件”或者“先验条件”，如：参数不为空，某全局变量应该为 1 等。不满足前条件，是不能调用此函数的，如果出现了前条件不满足仍然调用了此函数，可以认为这是一个设计错误。检查前条件，应该使用 ASSERT 来处理。
    - 在开始执行该函数之后，也会满足一系列条件，这些条件被称为“后条件”或者“后验条件”，如：返回值满足什么关系，某全局变量设置为什么值等。这应该是函数执行的结果，在前条件满足的情况下，后条件如果没有满足是一种不正常的情况，那么应该使用异常来处理。

TODO:

[怎样解释 Design by Contract （契约式设计)?](https://www.zhihu.com/question/19864652)

《代码大全 2》第八章：防御式编程

## 6. 最佳实践

- 只处理你知道的异常，避免捕获所有异常然后吞掉它们。
- 抛出的异常应该说明原因，有时候你知道异常类型也猜不出所以然。
- 不要使用异常来控制流程，那样你的程序会无比难懂和难维护。
- 如果有需要，切记使用 finally 来释放资源。
- 如果有需要，请不要忘记在处理异常后做清理工作或者回滚操作。

- 如何在编程语言里处理错误，是一个至今仍然存在争议的主题。

  比如在 Python 中不推荐的多返回值方式，正是缺乏异常的 Go 语言中最核心的错误处理机制：
  ```python
  def create_item(name):
      if len(name) > MAX_LENGTH_OF_NAME:
          return None, 'name of item is too long'
      if len(CURRENT_ITEMS) > MAX_ITEMS_QUOTA:
          return None, 'items is full'
      return Item(name=name), ''

  def create_from_input():
      name = input()
      item, err_msg = create_item(name)
      if err_msg:
          print(f'create item failed: {err_msg}')
      else:
          print(f'item<{name}> created')
  ```
  另外，即使是异常机制本身，不同编程语言之间也存在着差别。异常，或是不异常，都是由语言设计者进行多方取舍后的结果，更多时候不存在绝对性的优劣之分。但是，单就 Python 语言而言，Python 具备完善的异常（Exception）机制，并且在某种程度上鼓励我们使用异常，使用异常来表达错误无疑是更符合 Python 哲学，更应该受到推崇的：
  ```python
  class CreateItemError(Exception):
      """创建 Item 失败时抛出的异常"""

  def create_item(name):
      """创建一个新的 Item

      :raises: 当无法创建时抛出 CreateItemError
      """
      if len(name) > MAX_LENGTH_OF_NAME:
          raise CreateItemError('name of item is too long')
      if len(CURRENT_ITEMS) > MAX_ITEMS_QUOTA:
          raise CreateItemError('items is full')
      return Item(name=name)

  def create_for_input():
      name = input()
      try:
          item = create_item(name)
      except CreateItemError as e:
          print(f'create item failed: {err_msg}')
      else:
          print(f'item<{name}> created')
  ```

## 7. Refer Links

[Python docs: Errors and Exceptions](https://docs.python.org/3.8/tutorial/errors.html#)

[Python 中的异常处理](https://segmentfault.com/a/1190000007736783)

[Python 工匠： 异常处理的三个好习惯](https://www.zlovezl.cn/articles/three-rituals-of-exceptions-handling/)

[Python 工匠：让函数返回结果的技巧](https://www.zlovezl.cn/articles/function-returning-tips/)

[什么时候用异常，什么时候用断言？](https://www.zhihu.com/question/24461924)