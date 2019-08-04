- [STL Container](#stl-container)
  - [1. 概述](#1-概述)
  - [2. 成员函数表](#2-成员函数表)
  - [3. 内容](#3-内容)
    - [3.1. Sequence containers](#31-sequence-containers)
    - [3.2. Container adaptors](#32-container-adaptors)
    - [3.3. Associative containers](#33-associative-containers)
    - [3.4. Unordered associative container](#34-unordered-associative-container)
    - [3.5. Other](#35-other)
  - [4. Refer Links](#4-refer-links)

# STL Container

## 1. 概述

The Containers library is a generic collection of class templates and algorithms that allow programmers to easily implement common data structures like queues, lists and stacks. There are three classes of containers -- sequence containers, associative containers, and unordered associative containers -- each of which is designed to support a different set of operations.

Containers replicate structures very commonly used in programming: dynamic arrays (vector), queues (queue), stacks (stack), heaps (priority_queue), linked lists (list), trees (set), associative arrays (map)...

## 2. 成员函数表

This is a comparison chart with the different member functions present on each of the different containers:

![image](http://img.cdn.firejq.com/jpg/2018/7/3/8266c0aff1e7d9d42821d2287ae7ac08.jpg)

## 3. 内容

### 3.1. Sequence containers

Sequence containers implement data structures which can be accessed sequentially.
- array (C++11): static contiguous array

  只能在初始化时指定大小的数组，可视为内置数组的封装。

- vector: dynamic contiguous array

  动态数组，兼容 C 语言数组。vector 可以如同阵列一样的存取方式，例如使用下标（operator[]）运算子，并记得自己的长度资讯（size），您也可以使用物件的方式来存取 vector（push_back、pop_back）。

  使用 vector 可以轻易地定义多维可调整型阵列（std::vector<std::vector<...> >）。要使用 vector，必须含入 vector 表头档。vector 可在 O(1) 内完成在末尾插入 / 移除元素，但在 vector 中间或开头插入 / 移除元素，则需要消耗 O(n) 时间。

- list: doubly-linked list

  list 是一个双向链表，每个元素中存储着上一个元素和下一个元素的地址（指针），因此是一个双向链接的链表。与 vector 相比，其元素的访问速度较慢，而在已知元素位置的情况下，插入和删除速度较快。STL 容器中唯一支持事务语义。

- forward_list (C++11): singly-linked list

  list 的单链表版，去掉了一些操作。

- deque: double-ended queue

  可看做为能在常量时间内完成向开头或结尾插入或删除元素的 vector，但是修改之后，其迭代器的有效性就无法得到保障。

### 3.2. Container adaptors

stack, queue and priority_queue are implemented as container adaptors. Container adaptors are not full container classes, but classes that provide a specific interface relying on an object of one of the container classes (such as deque or list) to handle the elements. The underlying container is encapsulated in such a way that its elements are accessed by the members of the container adaptor independently of the underlying container class used.

- stack: adapts a container to provide stack (LIFO data structure).

- queue: adapts a container to provide queue (FIFO data structure).

- priority_queue: adapts a container to provide priority queue.

### 3.3. Associative containers

Associative containers implement sorted data structures that can be quickly searched (O(log n) complexity).
- set: collection of unique keys, sorted by keys

  不重复元素的集合。

- multiset: collection of keys, sorted by keys

  跟 set 具有相同功能，但允许重复的元素。

- map: collection of key-value pairs, sorted by keys, keys are unique

  关联数组，每个元素含有两个数据项，map 将一个数据项映射到另一个数据项中。

- multimap: collection of key-value pairs, sorted by keys

  跟 map 具有相同功能，但允许重复的键值。

### 3.4. Unordered associative container

containers Unordered associative containers implement unsorted (hashed) data structures that can be quickly searched (O(1) amortized, O(n) worst-case complexity).

- unordered_set (C++11): collection of unique keys, hashed by keys.

- unordered_multiset (C++11): collection of key-value pairs, hashed by keys, keys are unique.

- unordered_map (C++11): collection of keys, hashed by keys.

- unordered_multimap (C++11): collection of key-value pairs, hashed by keys.

### 3.5. Other

Two class templates share certain properties with containers, and are sometimes classified with them: bitset and valarray.

- bitset

  存储系列位类似的固定大小的布尔向量。实现按位运算，没有迭代器，不是序列。可视为 std::array<bool, N>。若需要改变序列长度，可用 std::vector<bool>。

- valarray

  数值类型的 std::vector。牺牲泛型能力而专为数值计算做了优化，例如在数组上的 sin 操作可对数组内所有数值取正弦。有些实现会对 std::valarray 应用向量指令等优化手段。

  一个观点是里面全是数值类型的 valarray 才是数学意义上的向量，而可以泛型的 vector 更该叫 array——编程语言中的数组。

## 4. Refer Links

[Cppreference Containers library](https://en.cppreference.com/w/cpp/container)

[Cplusplus Containers](http://www.cplusplus.com/reference/stl/)