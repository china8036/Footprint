- [TODO](#todo)
  - [1. 解释器实现](#1-解释器实现)
  - [2. 源码查看](#2-源码查看)
    - [2.1. help()](#21-help)
    - [2.2. inspect 模块](#22-inspect-模块)
    - [2.3. ipython 环境中使用 `??`](#23-ipython-环境中使用-)
    - [2.4. __file__ 属性](#24-__file__-属性)
    - [2.5. build-in function / type 源码](#25-build-in-function--type-源码)
    - [2.6. Object 源码](#26-object-源码)
  - [3. Refer Links](#3-refer-links)

# TODO

## 1. 解释器实现

Python 是一门语言，有语法等规范，但是落实到 Python 解释器的具体实现上，有多种不同的具体实现方式。事实上，由于整个 Python 语言从规范到解释器都是开源的，所以理论上，只要水平够高，任何人都可以编写 Python 解释器来执行 Python 代码（当然难度很大）。

- CPython: 用 C 实现的 Python 解释器。

  CPython 是最流行的 Python 实现，也是目前官方提供的默认解释器（Linux，OS X 等自带的也是这个版本）。最新的语言特性都是在这个上面先实现，基本包含了所有第三方库支持。

  绝大部分 Python 代码都可以在 PyPy 下运行，但是 PyPy 和 CPython 有一些是不同的，这就导致相同的 Python 代码在两种解释器下执行可能会有不同的结果，具体差异参考：[Differences between PyPy and CPython](https://pypy.readthedocs.io/en/latest/cpython_differences.html)。

  CPython 存在几个比较明显的缺陷：
  - 全局锁使 Python 在多线程效能上表现不佳。
  - CPython 无法支持 JIT（即时编译），导致其执行速度不及 Java 和 Javascipt 等语言。

  <!-- TODO:
    [Your Guide to the CPython Source Code](https://realpython.com/cpython-source-code-guide/)

    [为什么 CPython 需要 GIL?](https://www.zhihu.com/question/56170408)
  ->

- IPython

  IPython 是基于 CPython 之上的一个交互式解释器，它只是在交互方式上有所增强，但是执行 Python 代码的功能和 CPython 是完全一样的。

  CPython 用 `>>>` 作为提示符，而 IPython 用 `In[n]:` 作为提示符。

- PyPy: 用 Python 实现的 Python 解释器。

  PyPy 针对 CPython 的缺点进行了各方面的改良，性能得到了很大的提升，最重要的一点就是 Pypy 集成了 JIT，通过动态编译在性能上得到了显著的提升。JIT 混合了动态编译和静态编译的特性，仍然是一句一句编译源代码，但是会将翻译过的代码缓存起来以降低性能损耗。相对于静态编译代码，即时编译的代码可以处理延迟绑定并增强安全性。

  <!-- TODO: 为什么 JIT 比 CPython 快？快了多少？ -->

  但是，Pypy 无法支持官方的 C/Python API，导致无法使用例如 Numpy，Scipy 等重要的第三方库，这也是 Pypy 没有被广泛使用的原因之一。

- Pyston: 用 C++ 实现的 Python 解释器。

- IronPython: 用 C# 实现 Python 解释器，会将 Python 代码编译成 .Net 字节码执行。

- Jython: 用 Java 实现的 Python 解释器，会将 Python 代码编译成 Java 字节码执行。

NOTE：尽管 Python 解释器的具体实现很多，但事实上，如果要和 Java 或 .Net 平台交互，最好的办法不是用 Jython 或 IronPython，而是通过网络调用来交互，确保各程序之间的独立性。

## 2. 源码查看

P.S.
- [有的 python 内置函数怎么就一个 pass?](https://www.zhihu.com/question/61353466)

  由于作为解释器标准实现的一部分，Python 的很多内置函数也是用 c 语言实现的，因此，在正常情况下这些内置函数只有文档 pydoc 可用，而找不到对应的实现，源码都是看不到的。

  基于此，PyCharm 根据 pydoc 自动地生成这些函数的签名，但具体函数的实现内容只是用来生成文档和给静态分析工具看的假代码（如 pass），这些函数的真正实现在解释器源代码中，在一般的发行版中都看不到（需要特地去下载源码来看）。

### 2.1. help()

e.g.

stat 位于 os module，那么可以 `help(os)`，得到的结果中 FILE 就是其源文件位置。当然，如果这个 module 是 c module，那就看不到相应的源码了。

### 2.2. inspect 模块

inspect 模块用于收集 python 对象的信息，可以获取类或函数的参数的信息，源码，解析堆栈，对对象进行类型检查等。

e.g.

`inspect.getdoc()`

`inspect.getsourcefile()`

`inspect.getsourcelines()`

### 2.3. ipython 环境中使用 `??`

e.g.
```
In [1]: import os

In [2]: os??
```

### 2.4. __file__ 属性

e.g.
```
>>> import string
>>> string.__file__
'/usr/lib/python2.7/string.pyc'
```

### 2.5. build-in function / type 源码

https://stackoverflow.com/questions/8608587/finding-the-source-code-for-built-in-python-functions

https://github.com/python/cpython/blob/master/Python/bltinmodule.c

### 2.6. Object 源码

cpython/Objects:

e.g.
- list: https://github.com/python/cpython/blob/master/Objects/listobject.c
- dict: https://github.com/python/cpython/blob/master/Objects/dictobject.c
- enum: https://github.com/python/cpython/blob/master/Objects/enumobject.c

## 3. Refer Links

[CPython 是什么？PyPy 是什么？Python 和这两个东西有什么关系呢？Python 的底层使用什么语言实现？学习 Python 需要学习底层实现吗？](https://www.zhihu.com/question/20005950)

[怎样在 Python 中查询相关函数的源代码？](https://www.zhihu.com/question/31579772)

[廖雪峰：Python 解释器](https://www.liaoxuefeng.com/wiki/897692888725344/966138843228672)

TODO:

[Your Guide to the CPython Source Code](https://realpython.com/cpython-source-code-guide/)

[CPython internals: A ten-hour codewalk through the Python interpreter source code](http://pgbovine.net/cpython-internals.htm)

Built-in Source: https://github.com/python/cpython/blob/master/Python/bltinmodule.c

For Built-in Types: https://github.com/python/cpython/tree/master/Objects