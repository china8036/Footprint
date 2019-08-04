- [C++ 标准库](#c-标准库)
  - [1. 概念辨析](#1-概念辨析)
  - [2. 内容](#2-内容)
  - [3. Refer Links](#3-refer-links)

# C++ 标准库

## 1. 概念辨析

TODO: [C++ 标准库和标准模板库](https://blog.csdn.net/sxhelijian/article/details/7552499)

- C++ 标准 (C++ standard)
- C++ 标准库 (C++ Standard Library)
- C++ 标准模板库 (C++ Standard Template Library, STL)

> The C++ standard consists of two parts: the core language and the standard library.
>
> A large part of the C++ library is based on the Standard Template Library (STL).

## 2. 内容

https://en.wikipedia.org/wiki/C%2B%2B#Standard_library

C++ Standard Library includes:
- aggregate types (vectors, lists, maps, sets, queues, stacks, arrays, tuples)
- algorithms (find, for_each, binary_search, random_shuffle, etc.)
- input/output facilities (iostream, for reading from and writing to the console and files)
- filesystem library
- localisation support
- smart pointers for automatic memory management
- regular expression support
- multi-threading library
- atomics support (allowing a variable to be read or written to by at most one thread at a time without any external synchronisation)
- time utilities (measurement, getting current time, etc.)
- a system for converting error reporting that doesn't use C++ exceptions into C++ exceptions
- a random number generator
- a slightly modified version of the C standard library (to make it comply with the C++ type system).

C++ 标准库包含了所有的 C 标准库，为了支持类型安全，做了一定的添加和修改。

C++标准程式库充分吸收了C标准程式库，并佐以少许的修改，使其与C++良好的运作。另一个大型的程式库部分，是以标准模板库（STL）为基础，STL于1994年2月正式成为ANSI/ISO C++。

C++ 标准库可以分为两部分：
- 标准函数库： 这个库是由通用的、独立的、不属于任何类的函数组成的。函数库承自 C 语言。标准函数库分为以下几类：
  - 输入 / 输出 I/O
  - 字符串和字符处理
  - 数学
  - 时间、日期和本地化
  - 动态分配
  - 其他
  - 宽字符函数
- 面向对象类库： 这个库是类及其相关函数的集合。标准的 C++ 面向对象类库定义了大量支持一些常见操作的类，比如输入 / 输出 I/O、字符串处理、数值处理。面向对象类库包含以下内容：
标准的 C++ I/O 类
  - String 类
  - 数值类
  - STL 容器类
  - STL 算法
  - STL 函数对象
  - STL 迭代器
  - STL 分配器
  - 本地化库
  - 异常处理类
  - 杂项支持库

## 3. Refer Links
