- [日志模块 logging](#日志模块-logging)
  - [1. 基本概念](#1-基本概念)
  - [2. 日志级别](#2-日志级别)
  - [3. 使用方式](#3-使用方式)
    - [3.1. 模块函数](#31-模块函数)
    - [3.2. 四大组件类](#32-四大组件类)
  - [4. Refer Links](#4-refer-links)

# 日志模块 logging

## 1. 基本概念

logging 模块是 Python 的一个标准库模块，其定义的函数和类为应用程序和库的开发实现了一个灵活的事件日志系统。

由标准库模块提供日志记录 API 的关键好处是所有 Python 模块都可以使用这个日志记录功能。所以，你的应用日志可以将你自己的日志信息与来自第三方模块的信息整合起来。

## 2. 日志级别

logging 模块默认定义了以下几个日志等级：
- DEBUG: 最详细的日志信息，典型应用场景是问题诊断和埋点分析。
- INFO: 信息详细程度仅次于 DEBUG，通常只记录关键节点信息，用于确认一切都是按照我们预期的那样进行工作。
- WARNING: 当某些不期望的事情发生时记录的信息（如磁盘可用空间较低），但是此时应用程序还是正常运行的。**WARNING 是默认的日志记录级别**。
- ERROR: 由于一个更严重的问题导致某些功能不能正常运行时记录的信息。
- CRITICAL: 当发生严重错误，导致应用程序不能继续运行时记录的信息。

NOTE:
- logging 模块允许开发人员自定义其他日志级别，但是通常不推荐这么做，尤其是在开发供别人使用的库时，因为这会导致日志级别的混乱。

- 当为某个应用程序指定一个日志级别后，应用程序会记录所有日志级别大于或等于指定日志级别的日志信息，小于该等级的日志记录才会被丢弃，而不是仅仅记录指定级别的日志信息。

- 开发应用程序或部署开发环境时，可以使用 DEBUG 或 INFO 级别的日志获取尽可能详细的日志信息来进行开发或部署调试；应用上线或部署生产环境时，应该使用 WARNING、ERROR 甚至 CRITICAL 级别的日志来降低机器的 I/O 压力和提高获取错误日志信息的效率。

- 日志级别的指定通常都是在应用程序的配置文件中进行指定的。

src：
```python
CRITICAL = 50
FATAL = CRITICAL
ERROR = 40
WARNING = 30
WARN = WARNING
INFO = 20
DEBUG = 10
NOTSET = 0

_levelNames = {
    CRITICAL : 'CRITICAL',
    ERROR : 'ERROR',
    WARNING : 'WARNING',
    INFO : 'INFO',
    DEBUG : 'DEBUG',
    NOTSET : 'NOTSET',
    'CRITICAL' : CRITICAL,
    'ERROR' : ERROR,
    'WARN' : WARNING,
    'WARNING' : WARNING,
    'INFO' : INFO,
    'DEBUG' : DEBUG,
    'NOTSET' : NOTSET,
}
```

## 3. 使用方式

### 3.1. 模块函数

- `logging.debug(msg, *args, **kwargs)`: 创建一条严重级别为 DEBUG 的日志记录。
- `logging.info(msg, *args, **kwargs)`: 创建一条严重级别为 INFO 的日志记录。
- `logging.warning(msg, *args, **kwargs)`: 创建一条严重级别为 WARNING 的日志记录。
- `logging.error(msg, *args, **kwargs)`: 创建一条严重级别为 ERROR 的日志记录。
- `logging.critical(msg, *args, **kwargs)`: 创建一条严重级别为 CRITICAL 的日志记录。
- `logging.log(level, *args, **kwargs)`: 创建一条严重级别为 level 的日志记录。
- `logging.basicConfig(**kwargs)`: 对 root logger 进行一次性配置。

NOTE：

- 事实上，logging 所提供的模块级别的日志记录函数也是对 logging 日志系统四大基础组件类的封装而已。

- 在进行异常处理的时候，通常我们会直接将异常进行字符串格式化，但其实可以直接指定一个参数将 traceback 打印出来，示例如下：
  ```python
  import logging

  logging.basicConfig(level=logging.DEBUG, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')

  try:
      result = 5 / 0
  except Exception as e:
      # bad
      logging.error('Error: %s', e)
      # good
      logging.error('Error', exc_info=True)
      # good
      logging.exception('Error')
  ```
  如果我们直接使用字符串格式化的方法将错误输出的话，是不会包含 Traceback 信息的，但如果我们加上 exc_info 参数或者直接使用 exception() 方法打印的话，那就会输出 Traceback 信息了：
  ```
  2018-06-03 22:24:31,927 - root - ERROR - Error: division by zero
  2018-06-03 22:24:31,927 - root - ERROR - Error
  Traceback (most recent call last):
    File "/private/var/books/aicodes/loggingtest/demo9.py", line 6, in <module>
      result = 5 / 0
  ZeroDivisionError: division by zero
  2018-06-03 22:24:31,928 - root - ERROR - Error
  Traceback (most recent call last):
    File "/private/var/books/aicodes/loggingtest/demo9.py", line 6, in <module>
      result = 5 / 0
  ZeroDivisionError: division by zero
  ```

- 在日志输出的时候经常我们会用到字符串拼接的形式，很多情况下我们可能会使用字符串的 format() 来构造一个字符串，但事实上 logging 的模块函数都提供对字符串拼接的支持：
  ```python
  import logging

  logging.basicConfig(level=logging.DEBUG, format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')

  # bad
  logging.debug('Hello {0}, {1}!'.format('World', 'Congratulations'))
  # good
  logging.debug('Hello %s, %s!', 'World', 'Congratulations')
  ```
  只需要在第一个参数写上要打印输出的模板，占位符用 %s、%d 等表示即可，然后在后续参数添加对应的值就可以了。

### 3.2. 四大组件类

- `loggers`: 提供应用程序代码直接使用的接口。
- `handlers`: 用于将日志记录发送到指定的目的位置。
- `filters`: 提供更细粒度的日志过滤功能，用于决定哪些日志记录将会被输出（其它的日志记录将会被忽略）。
- `formatters`: 用于控制日志信息的最终输出格式。

## 4. Refer Links

[logging — Python 3.7.4 文档](https://docs.python.org/zh-cn/3/howto/logging.html)

[logging — Python 2.7.16 文档](https://docs.python.org/zh-cn/2/howto/logging.html)

TODO:

[Python 中 logging 模块的基本用法](https://cuiqingcai.com/6080.html)

[Python 日志库 logging 总结](https://juejin.im/post/5bc2bd3a5188255c94465d31)

[Python 之日志处理（logging 模块）](https://www.cnblogs.com/yyds/p/6901864.html)

[读懂掌握 Python logging 模块源码 （附带一些 example）](https://www.cnblogs.com/piperck/p/9634133.html)