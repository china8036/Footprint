- [Java 集合：Collection 族 - Queue](#java-%E9%9B%86%E5%90%88%EF%BC%9Acollection-%E6%97%8F---queue)
  - [1. Queue 接口](#1-queue-%E6%8E%A5%E5%8F%A3)
    - [1.1. 基本概念](#11-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [1.2. 常用 API](#12-%E5%B8%B8%E7%94%A8-api)
  - [2. PriorityQueue 实现类](#2-priorityqueue-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
    - [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [2.2. 存储结构](#22-%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84)
    - [2.3. 常用 API](#23-%E5%B8%B8%E7%94%A8-api)
  - [3. Deque 扩展接口](#3-deque-%E6%89%A9%E5%B1%95%E6%8E%A5%E5%8F%A3)
    - [3.1. 基本概念](#31-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [3.2. 常用 API](#32-%E5%B8%B8%E7%94%A8-api)
  - [4. ArrayDeque 实现类](#4-arraydeque-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
    - [4.1. 基本概念](#41-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [4.2. 存储结构](#42-%E5%AD%98%E5%82%A8%E7%BB%93%E6%9E%84)
    - [4.3. 常用 API](#43-%E5%B8%B8%E7%94%A8-api)
  - [5. LinkedList 实现类](#5-linkedlist-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
  - [6. Refer Links](#6-refer-links)

# Java 集合：Collection 族 - Queue

![image](/resources/images/queue-api-class-diagram.png)

## 1. Queue 接口

### 1.1. 基本概念

[Queue](https://docs.oracle.com/javase/9/docs/api/java/util/Queue.html) 接口是 Collection 接口的扩展，用于实现队列这种 FIFO 的数据结构，它支持有效地在集合的尾部添加元素、在头部删除元素，但通常不允许在集合的中间位置添加元素，也不允许随机访问队列中的元素。

### 1.2. 常用 API

- Insert：将给定元素添加到队列末尾。
  - `boolean	add​(E e)`：若队列已满，抛出一个异常。
  - `boolean	offer​(E e)`：若队列已满，返回 false。
  
- Remove：删除并返回队列头部的元素。
  - `E	remove​()`：若队列为空，抛出一个异常。
  - `E	poll​()`：若队列为空，返回 false。

- Examine：返回队列头部的元素但不删除。
  - `E	element​()`：若队列为空，抛出一个异常。
  - `E	peek​()`：若队列为空，返回 null。

## 2. PriorityQueue 实现类

### 2.1. 基本概念

[PriorityQueue](https://docs.oracle.com/javase/9/docs/api/java/util/PriorityQueue.html) 是 AbstractQueue 抽象类的扩展，并没有直接实现 Queue 接口。它是一个**相对**标准的队列实现类，用于实现**优先级队列**这种数据结构。在 PriorityQueue 中，元素可以按照任意的顺序插入，但当调用 API 方法取出队列中的元素时，并不是取出最先进入队列的元素，而是取出队列中最小（或最大）的元素。从这个意义上看，PriorityQueue 违反了队列的基本规则：FIFO。

同 TreeSet 一样，优先级队列中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

PriorityQueue 是非线程安全的，若对线程安全有要求，应该使用线程安全的 PriorityBlockingQueue 类。

PriorityQueue 不允许元素为 null，当添加一个 null 元素时，将抛出 java.lang.NullPointerException 异常。

优先级队列的典型应用场景是任务的调度。任务以随机顺序添加到任务队列中，但每一次进行任务调度时，都会将一个优先级最高的元素从队列中删除并开始执行（可设置优先级为 1 时最高）。

### 2.2. 存储结构

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/20/2b732a7bbc959afc42d013cb40523a18.jpg)

PriorityQueue 内部实际上是一个**平衡二叉最小堆**，底层基于一个**数组**实现，因此：
- 不需要在每次添加 / 删除元素时对所有的元素进行排序。每次调用 remove 方法，都会删除当前优先级队列中最小的元素；每次调用 add 方法，都会将最小的元素移动到根。
- 当使用迭代器 iterator() 遍历 PriorityQueue 时，会按照内部数组的索引顺序进行返回，也就是二叉堆层序遍历的次序、元素添加的顺序。如果需要有序的遍历，可以考虑使用 `Arrays.sort(priorityQueue.toArray())` 或者通过使用 for 循环结合 `element​()` 方法 / `peek​()` 方法进行遍历。

### 2.3. 常用 API

- 构造器
  - `PriorityQueue​()`
  - `PriorityQueue​(int initialCapacity)`	
  - `PriorityQueue​(int initialCapacity, Comparator<? super E> comparator)`

- PriorityQueue​的 API 基本上与 Queue 接口的 API 相同。

## 3. Deque 扩展接口

### 3.1. 基本概念

[Deque](https://docs.oracle.com/javase/9/docs/api/java/util/Deque.html) 接口是 Queue 接口的扩展接口，用于实现一个双端队列（double ended queue），它支持同时在头部和尾部同时添加或删除元素，但同样不支持在集合的中间位置添加元素。**由于双端队列可以在头部和尾部添加元素，因此既可以当作栈使用，也可以当作队列使用。**

Java 为 Deque 接口提供了两个实现类：基于数组实现的 ArrayDeque 和基于链表实现的 LinkedList。由于实现了 Deque 接口，因此 **ArrayDeque 和 LinkedList 都可以用于栈和队列的实现，但官方更推荐使用 ArrayDeque 用作栈和队列**。

### 3.2. 常用 API

- Insert：将给定元素添加到双端队列的头部或末尾。
  - `void	addFirst​(E e)`: Inserts the specified element at the front of this deque if it is possible to do so immediately without violating capacity restrictions, throwing an IllegalStateException if no space is currently available.
  - `void	addLast​(E e)`: Inserts the specified element at the end of this deque if it is possible to do so immediately without violating capacity restrictions, throwing an IllegalStateException if no space is currently available.
  - `boolean	offerFirst​(E e)`: Inserts the specified element at the front of this deque unless it would violate capacity restrictions.
  - `boolean	offerLast​(E e)`: Inserts the specified element at the end of this deque unless it would violate capacity restrictions.
- Remove：删除并返回队列头部或尾部的元素。
  - `E	removeFirst​()`: Retrieves and removes the first element of this deque.
  - `E	removeLast​()`: Retrieves and removes the last element of this deque.
  - `E	pollFirst​()`: Retrieves and removes the first element of this deque, or returns null if this deque is empty.
  - `E	pollLast​()`: Retrieves and removes the last element of this deque, or returns null if this deque is empty.
- Examine：返回双端队列头部或尾部的元素但不删除。
  - `E	getFirst​()`: Retrieves, but does not remove, the first element of this deque.
  - `E	getLast​()`: Retrieves, but does not remove, the last element of this deque.
  - `E	peekFirst​()`: Retrieves, but does not remove, the first element of this deque, or returns null if this deque is empty.
  - `E	peekLast​()`: Retrieves, but does not remove, the last element of this deque, or returns null if this deque is empty.

## 4. ArrayDeque 实现类

### 4.1. 基本概念

[ArrayDeque](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayDeque.html) 是 Deque 接口的实现类。作为栈来使用时，效率要高于 Stack；作为队列使用时，效率相较于基于双向链表的 LinkedList 也要更好一些。因此，**官方推荐使用 ArrayDeque 作为栈与队列的实现类**。

ArrayDeque 是非线程安全的。

ArrayDeque 不允许元素为 null，当添加一个 null 元素时，将抛出 java.lang.NullPointerException 异常。

### 4.2. 存储结构

ArrayDeque 底层基于**循环数组**（circular array）实现，也就是说数组的任何一点都可能被看作起点或者终点，从而实现可以同时在数组两端插入或删除元素的需求。

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/20/886d803d1df785ba660c0f5c39feb131.jpg)

上图中我们看到，head 指向首端第一个有效元素，tail 指向尾端第一个可以插入元素的空位。因为是循环数组，所以 head 不一定总等于 0，tail 也不一定总是比 head 大。

### 4.3. 常用 API

- 构造器
  - `ArrayDeque​()`
  - `ArrayDeque​(int numElements)`：用给定的初始容量构造一个无限双端队列。
- ArrayDeque 的其它 API 与 Deque 接口的 API 基本相同。

## 5. LinkedList 实现类

## 6. Refer Links

[PriorityQueue 源码分析](https://www.jianshu.com/p/f79e4e2bd071)

[深入理解 Java PriorityQueue](https://www.cnblogs.com/CarpenterLee/p/5488070.html)

[Java ArrayDeque 源码剖析](https://www.cnblogs.com/CarpenterLee/p/5468803.html)