- [STL (Standard Template Library)](#stl-standard-template-library)
  - [1. 概述](#1-概述)
  - [2. 内容](#2-内容)
  - [3. Refer Links](#3-refer-links)

# STL (Standard Template Library)

## 1. 概述

TODO:

http://read.pudn.com/downloads47/ebook/159201/STLGuide.pdf

https://blog.csdn.net/sxhelijian/article/details/7552499


STL（Standard Template Library，标准模板库)是惠普实验室开发的一系列软件的统称。它是由Alexander Stepanov、Meng Lee 和 David R Musser在惠普实验室工作时所开发出来的。现在虽说它主要出现在C++中，但在被引入C++之前该技术就已经存在了很长的一段时间。


1994.7.14，ANSI/ISO C++ 标准化委员会开始将STL采纳为草案标准，从此STL进入了C++ 标准库，成为C++ 标准的一部分。现今，Microsoft Visual C++ 5.0 以上及 Borland C++ 4.0 以上都支持STL。

STL的代码从广义上讲分为三类：algorithm（算法）、container（容器）和iterator（迭代器），几乎所有的代码都采用了模板类和模版函数的方式，这相比于传统的由函数和类组成的库来说提供了更好的代码重用机会。在C++标准中，STL被组织为下面的13个头文件：`<algorithm>`、`<deque>`、`<functional>`、`<iterator>`、`<vector>`、`<list>`、`<map>`、`<memory>`、`<numeric>`、`<queue>`、`<set>`、`<stack>` 和 `<utility>`。

<!-- ----------------- -->

> Most C++ compilers, and all major ones, provide a standards-conforming implementation of the C++ standard library.

STL 的一个重要特点是数据结构和算法的分离。尽管这是个简单的概念，但这种分离确实使得 STL 变得非常通用。例如，由于 STL 的 sort() 函数是完全通用的，你可以用它来操作几乎任何数据集合，包括链表，容器和数组。

STL 另一个重要特性是它不是面向对象的。为了具有足够通用性，STL 主要依赖于模板而不是封装，继承和虚函数（多态性）——OOP 的三个要素。你在 STL 中找不到任何明显的类继承关系。这好像是一种倒退，但这正好是使得 STL 的组件具有广泛通用性的底层特征。另外，由于 STL 是基于模板，内联函数的使用使得生成的代码短小高效。

## 2. 内容

traits, iterator, container, algorithm 是 STL 的四架马车。

C++ 标准模板库的核心包括以下三个组件：
- 容器（Containers）

  容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。

- 算法（Algorithms）

  算法作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。

- 迭代器（iterators）

  迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。

## 3. Refer Links

[三十分钟掌握 STL](http://net.pku.edu.cn/~yhf/UsingSTL.htm)

[标准模板库](https://zh.wikipedia.org/wiki/%E6%A0%87%E5%87%86%E6%A8%A1%E6%9D%BF%E5%BA%93)