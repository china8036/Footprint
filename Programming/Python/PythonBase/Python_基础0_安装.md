- [Python 基础：安装](#Python-基础安装)
  - [1. Windows](#1-Windows)
  - [2. MacOS](#2-MacOS)
  - [3. Linux](#3-Linux)
  - [4. Refer Links](#4-Refer-Links)

# Python 基础：安装

## 1. Windows

## 2. MacOS

使用 HomeBrew 来分别安装 python2 和 python3:

https://www.jianshu.com/p/28e0c23bff84

HomeBrew 为我管理了两个版本的 Python，分别是 2.7.14 和 3.6.4，在 /usr/local/bin/ 目录下有相关命令。同时系统还有一个自带 2.7.10 版本的，放在 /usr/bin 目录中。

为避免和系统原有的 python 发生冲突，因此删除 /usr/local/bin 下的 python、pydoc 等软链接（否则直接运行 python 时由于 /usr/local/bin 的优先级比 /usr/bin 高，会与系统原生的 /usr/bin/python 发生冲突）。

以后开发过程中使用 Python2 的时候应该使用命令 python2 而不是使用 python。因为，我的环境中命令 python 是 MacOS 自带的 2.7.10 版本。而命令 python2 则调用 HomeBrew 管理的 python2.7.14，它在 /usr/local/bin/ 目录中，是一个软链接，链接到 /usr/local/Cellar/python/2.7.14_2/bin/python2 中。命令 python3 同理。因此开发时需要区分这三者。

操作：
```
~  brew install python2
~  brew install python
```
结果：
```
~  python --version
Python 2.7.10
~  python2 --version
Python 2.7.16
~  python3 --version
Python 3.7.4
```

## 3. Linux

## 4. Refer Links
