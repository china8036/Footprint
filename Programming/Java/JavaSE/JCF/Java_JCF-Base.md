- [Java 集合：概述](#java)
  - [1. 设计思想](#1)
  - [2. 类谱图](#2)
  - [3. 其它](#3)
    - [3.1. 元素都是对象](#31)
    - [3.2. API 对比](#32-api)
    - [3.3. 快速失败机制](#33)
  - [4. Refer Links](#4-refer-links)

# Java 集合：概述

Java 在 `java.util.*` 中为开发者提供了包含了一系列常用数据结构及其操作的工具包，即 Java 集合框架 (Java Collections Framework, JCF)。

为处理在多线程环境下的并发安全问题，从 Java 5 开始，在 java.util.concurrent 包下增加提供了一系列多线程支持的集合类。

## 1. 设计思想

- 接口与实现分离
  
  与现在数据结构类库常见的情况一样，Java 集合类库将接口与实现分离。同一个接口可以有多种实现方式，便于开发者选择最适合自己的实现类进行使用。一旦想要改变，由于实现自同一个接口，所包含的方法及方法调用结果完全相同，因此只需修改类对象的构造处，即可轻松选用另外一种实现方式。

  在集合类谱图中，还有一组以 `Abstract` 开头命名的类，这些类是为类库实现者设计的。其中默认实现了接口的大部分方法，开发者只需继承这些抽象类，便可只对个别方法进行重新实现，而不用实现接口的所有方法。
  

- 适配器设计模式

  Java 集合框架中大量使用了适配器模式进行实现。

## 2. 类谱图

![image](/resources/images/java-collections-framework.png)

Java 集合框架中有两个基本接口：Collection 和 Map。

## 3. 其它

### 3.1. 元素都是对象

集合类与数组不同，数组的元素既可以是基本类型的值，也可以是对象的引用；而集合中只能保存对象的引用，不能保存基本数据类型的值。

### 3.2. API 对比

- 统一 API

  - JCF 中的所有类都支持以下两个构造器：
    - `X()`: 空构造器，构造一个空的集合。
    - `X(Collection<? extends E> c)`: 使用指定集合构造一个新的集合。
  
  - `int size()`: 返回集合元素数量。
  
  - `boolean isEmpty()`: 返回集合是否为空。

  - `Object clone​()`: 返回集合对象的复制。

  - Collection 统一 API( 定义在 AbstractCollection 中 ):
    - `boolean contains(Object o)`:
    - `Object[] toArray()`:
    - `boolean add(E e)`:
    - `boolean addAll(Collection<? extends E> c)`:
    - `boolean remove(Object o)`: 
    - `boolean removeAll(Collection<?> c)`: 
  
  - Map 统一 API( 定义在 AbstractMap 中 ):
    - `V get(Object key)`: 
    - `V put(K key, V value)`: 
    - `void putAll(Map<? extends K, ? extends V> m)`:
    - `V remove(Object key)`:
    - `boolean containsValue(Object value)`: 
    - `boolean containsKey(Object key)`: 

- 特有 API
  
  - 栈结构
    - 基于双向链表实现：LinkedList
    - 基于循环数组实现：ArrayDeque
      - `void push(E e)`/`void addFirst(E e)`: 将指定元素压入栈。
      - `E pop()`/`E removeFirst()`: 返回栈顶元素，并将栈顶元素移除。栈为空时抛出 NoSuchElementException 异常。
      - `E element()`/`E getFirst()`: 返回栈顶元素但不移除。栈为空时抛出 NoSuchElementException 异常。

  - 队列结构
    - 基于双向链表实现：LinkedList
    - 基于循环数组实现：ArrayDeque
      - `boolean add(E e)`/`void addLast(E e)`: 将指定元素加入队列尾部。
      - `E remove()`/`E removeFirst()`: 返回队列头部元素，并将队列头部元素移除。队列为空时抛出 NoSuchElementException 异常。
      - `E element()`/`E getFirst()`: 返回队列头部元素但不移除。队列为空时抛出 NoSuchElementException 异常。

  - 优先队列结构
    - 基于最小堆实现：PriorityQueue
      - `boolean add(E e)`/`boolean offer(E e)`: 将指定元素加入队列。
      - `E poll​()`: 返回队列头部元素，并将队列头部元素移除。队列为空时返回 null。
      - `E peek()`: 返回队列头部元素但不移除。队列为空时返回 null。
    
  - map 结构
    - 基于哈希表实现：HashMap
    - 基于红黑树实现：TreeMap

  - set 结构
    - 基于哈希表实现：HashSet
    - 基于红黑树实现：TreeSet

  - 可变数组结构
    - ArrayList

  - 双向链表结构
    - LinkedList

### 3.3. 快速失败机制

TODO:

在 Java 集合框架中，很多类都实现了快速失败机制。该机制被触发时，会抛出并发修改异常 ConcurrentModificationException，以避免程序在将来不确定的时间里出现不确定的行为。

## 4. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)