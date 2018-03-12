- [Java 集合：Collection 族 - Queue](#java-%E9%9B%86%E5%90%88%EF%BC%9Acollection-%E6%97%8F---queue)
  - [1. Queue 接口](#1-queue-%E6%8E%A5%E5%8F%A3)
  - [2. PriorityQueue](#2-priorityqueue)
  - [3. Deque 接口](#3-deque-%E6%8E%A5%E5%8F%A3)
  - [4. ArrayDeque](#4-arraydeque)
  - [5. LinkedList](#5-linkedlist)
  - [6. Refer Links](#6-refer-links)

# Java 集合：Collection 族 - Queue

## 1. Queue 接口

队列 [Queue](https://docs.oracle.com/javase/9/docs/api/java/util/Queue.html) 接口是 Collection 接口的扩展，它支持有效地在集合的尾部添加元素、在头部删除元素，但不支持在集合的中间位置添加元素。

常用 API
- Insert：将给定元素添加到队列末尾。
  - `boolean	add​(E e)`：若队列已满，抛出一个异常。
  
  - `boolean	offer​(E e)`：若队列已满，返回 false。
  
- Remove：删除并返回队列头部的元素。
  - `E	remove​()`：若队列为空，抛出一个异常。
  
  - `E	poll​()`：若队列为空，返回 false。

- Examine：返回队列头部的元素但不删除。
  - `E	element​()`：若队列为空，抛出一个异常。

  - `E	peek​()`：若队列为空，返回 null。

## 2. PriorityQueue

优先级队列 [PriorityQueue](https://docs.oracle.com/javase/9/docs/api/java/util/PriorityQueue.html) 是 Queue 接口的实现类，队列中的元素可以按照任意的顺序插入，却总是按照排序的顺序进行检索。

优先级队列使用堆作为数据结构实现，因此不需要对所有的元素进行排序。每次调用 remove 方法，都会删除当前优先级队列中最小的元素；每次调用 add 方法，都会将最小的元素移动到根。

同 TreeSet 一样，优先级队列中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时必须提供一个 Comparator。

优先级队列的典型应用场景是任务的调度。任务以随机顺序添加到任务队列中，但每一次进行任务调度时，都会将一个优先级最高的元素从队列中删除并开始执行（可设置优先级为 1 时最高）。

- 构造器
  - `PriorityQueue​()`
  - `PriorityQueue​(int initialCapacity)`	
  - `PriorityQueue​(int initialCapacity, Comparator<? super E> comparator)`

## 3. Deque 接口

双端队列（double ended queue） [Deque](https://docs.oracle.com/javase/9/docs/api/java/util/Deque.html) 接口是 Queue 接口的扩展，支持有效地在头部和尾部同时添加或删除元素，但同样不支持在集合的中间位置添加元素。**它既可以当作栈使用，也可以当作队列使用。**

ArrayDeque 和 LinkedList 都是该接口的实现类，但官方更推荐使用 AarryDeque 用作栈和队列。

常用 API
- Insert：将给定元素添加到双端队列的头部或末尾。
  - `void	addFirst​(E e)`
  - `void	addLast​(E e)`
  - `boolean	offerFirst​(E e)`
  - `boolean	offerLast​(E e)`
- Remove：删除并返回队列头部或尾部的元素。
  - `E	removeFirst​()`
  - `E	removeLast​()`
  - `E	pollFirst​()`
  - `E	pollLast​()`
- Examine：返回双端队列头部或尾部的元素但不删除。
  - `E	getFirst​()`
  - `E	getLast​()`
  - `E	peekFirst​()`
  - `E	peekLast​()`

## 4. ArrayDeque

[ArrayDeque](https://docs.oracle.com/javase/9/docs/api/java/util/ArrayDeque.html) 依赖于可变数组实现，作为栈来使用时，效率要高于 Stack；作为队列使用时，效率相较于基于双向链表的 LinkedList 也要更好一些。

当需要使用栈时，Java 已不推荐使用 Stack，而是推荐使用更高效的 ArrayDeque。

ArrayDeque 不支持为 null 的元素。

- 构造器
  - `ArrayDeque​()`
  - `ArrayDeque​(int numElements)`：用给定的初始容量构造一个无限双端队列。

## 5. LinkedList
 
## 6. Refer Links
