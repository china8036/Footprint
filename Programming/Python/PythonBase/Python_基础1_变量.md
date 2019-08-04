- [Python 基础：变量](#python-基础变量)
  - [1. 变量命名规范](#1-变量命名规范)
  - [2. 全局、局部变量](#2-全局局部变量)
  - [3. Refer Links](#3-refer-links)

# Python 基础：变量

[Python 官方文档](https://docs.python.org/zh-cn/3/)

<!--TODO: [Python 有哪些黑魔法？](https://www.zhihu.com/question/29995881/answer/172961766)  -->

Python 里所有数据——布尔值、整数、浮点数、字符串，甚至大型数据结构、函数以及程序——都是**以对象（object） 的形式存在**的；

## 1. 变量命名规范

- 变量名只能包含大小写字母、数字、下划线；

- 变量名不允许以数字开头；

- 变量名中以两个下划线开头的变量有特殊意义，是 python 的保留用法，自定义变量不可使用这样的命名；

  例：

  函数名：function.__name__；

  函数文档字符串：function.__doc__；

  主程序名称：__main__

## 2. 全局、局部变量

为了读取全局变量而不是函数中的局部变量，需要在变量前面显式地加关键字 global；

- Python 提供了两个获取命名空间内容的函数：
  - locals() 返回一个局部命名空间内容的字典；
  - globals() 返回一个全局命名空间内容的字典。

## 3. Refer Links
