- [Python 基础：概述](#python-基础概述)
  - [基础概念](#基础概念)
  - [语言特性](#语言特性)
  - [设计哲学](#设计哲学)
  - [PEP](#pep)
  - [安装配置](#安装配置)
    - [1. Windows](#1-windows)
    - [2. MacOS](#2-macos)
    - [3. Linux](#3-linux)
  - [4. Refer Links](#4-refer-links)

# Python 基础：概述

## 基础概念

> Python is a programming language that lets you work quickly and integrate systems more effectively.

> Python is an interpreted, high-level, general-purpose programming language. Created by Guido van Rossum and first released in 1991, Python's design philosophy emphasizes code readability with its notable use of significant whitespace. Its language constructs and object-oriented approach aim to help programmers write clear, logical code for small and large-scale projects.

## 语言特性

- Python是完全面向对象的语言，函数、模块、数字、字符串在Python中都是对象，并且完全支持继承、重载、派生、多重继承，有益于增强源代码的复用性。
- Python支持重载运算符，也支持泛型设计。
- 相对于Lisp这种传统的函数式编程语言，Python对函数式编程只提供了有限的支持。标准库 functools和itertools提供了与Haskell和Standard ML中类似的函数式程序设计工具。
- 与Scheme、Ruby、Perl、Tcl等动态类型编程语言一样，[Python](https://en.wikipedia.org/wiki/Python_(programming_language)) 拥有**动态类型系统**和**垃圾回收功能**，能够自动管理内存使用，并且支持多种编程范式，包括面向对象、命令式、函数式和过程式编程，且拥有一个巨大而广泛的标准库。
- Python 解释器几乎可以在所有的操作系统中运行，其中：CPython是一个用C语言编写的自由软件，当前由Python软件基金会管理。
- Python本身被设计为可扩展的。并非所有的特性和功能都集成到语言核心。Python提供了丰富的API和工具，以便程序员能够轻松地使用C、C++、Cython来编写扩展模块。Python编译器本身也可以被集成到其它需要脚本语言的程序内。因此，有很多人把Python作为一种“胶水语言”使用。使用Python将其他语言编写的程序进行集成和封装。在Google内部的很多项目，例如Google应用服务引擎使用C++或JIT技术编写性能要求极高的部分，然后用Python或Java/Go调用相应的模块。

## 设计哲学

Python的设计哲学是“优雅”、“明确”、“简单”。

Python开发者的哲学是“用一种方法，最好是只有一种方法来做一件事”，因此它和拥有明显个人风格的其他语言很不一样。在设计Python语言时，如果面临多种选择，Python开发者一般会拒绝花俏的语法，而选择明确没有或者很少有歧义的语法。这些准则被称为 [PEP20: Python Zen](https://www.python.org/dev/peps/pep-0020/)。

在Python解释器内运行import this可以获得完整的Python Zen:
```python
>>> import this
The Zen of Python
by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

## PEP 


## 安装配置

### 1. Windows

### 2. MacOS

由于在 MacOS 中系统自带了一个 Python 2.7.10 (MacOS 10.14.6)，若直接使用安装包进行安装，可能会造成版本冲突，影响系统自带的功能模块，因此，这里使用 HomeBrew 对多个 Python 版本进行管理。

```
~  brew install python2
~  brew install python
```

安装完成后，为避免和系统原有的 python 发生冲突，因此删除 /usr/local/bin 下的 python、pydoc 等软链接（否则直接运行 python 时由于 /usr/local/bin 的优先级比 /usr/bin 高，会与系统原生的 /usr/bin/python 发生冲突）。

- Pytthon 2.7.10，系统自带版本，位于 /usr/bin
- Python 2.7.16，HomeBrew 安装版本，位于 /usr/local/bin
- Python 3.7.4，HomeBrew 安装版本，位于 /usr/local/bin
```
~  python --version
Python 2.7.10
~  python2 --version
Python 2.7.16
~  python3 --version
Python 3.7.4
```

以后开发过程中使用 Python2 的时候应该使用命令 python2 而不是使用 python。因为，我的环境中命令 python 是 MacOS 自带的 2.7.10 版本。而命令 python2 则调用 HomeBrew 管理的 python2.7.14，它在 /usr/local/bin/ 目录中，是一个软链接，链接到 /usr/local/Cellar/python/2.7.14_2/bin/python2 中。命令 python3 同理。因此开发时需要区分这三者。

### 3. Linux

## 4. Refer Links

[Python3 官方文档](https://docs.python.org/zh-cn/3/)

[Index of Python Enhancement Proposals (PEPs)](https://www.python.org/dev/peps/)

[管理 Mac 的 Python 环境](https://www.jianshu.com/p/28e0c23bff84)

TODO:

[Python 有哪些黑魔法？](https://www.zhihu.com/question/29995881/answer/172961766)

