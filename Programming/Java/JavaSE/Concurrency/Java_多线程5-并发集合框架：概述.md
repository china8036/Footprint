- [Java 多线程 并发集合框架：概述](#java-%E5%A4%9A%E7%BA%BF%E7%A8%8B-%E5%B9%B6%E5%8F%91%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%EF%BC%9A%E6%A6%82%E8%BF%B0)
  - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
  - [2. 类谱图](#2-%E7%B1%BB%E8%B0%B1%E5%9B%BE)
  - [3. 非阻塞集合 (Non-Blocking Collection)](#3-%E9%9D%9E%E9%98%BB%E5%A1%9E%E9%9B%86%E5%90%88-non-blocking-collection)
  - [4. 阻塞集合 (Blocking Collection)](#4-%E9%98%BB%E5%A1%9E%E9%9B%86%E5%90%88-blocking-collection)

# Java 多线程 并发集合框架：概述

## 1. 基本概念

JDK 在 java.util 包下为开发者提供了 Java 集合框架，极大提高了开发效率，但这些集合类都是非线程安全的，也就是说，在多线程环境下，使用这些集合可能会导致到数据不一致性等并发安全的问题。因此，**从 JDK1.5 开始，Java 在 java.util.concurrent 包下提供了支持多线程并发的集合类，即 Java 并发集合框架**。

## 2. 类谱图

![image](http://img.cdn.firejq.com/jpg/2018/4/2/ba7d19c24edd41b29a86d81f4ce81e2b.jpg)

java.util.concurrent 包中提供了映射、有序集、队列的线程安全的高效实现，它们使用复杂的算法，通过允许并发访问数据结构的不同部分来使竞争最小化。

## 3. 非阻塞集合 (Non-Blocking Collection)

**非阻塞集合包括添加和移除数据的方法。当集合已满或为空时，在调用添加或者移除方法时会返回 null 或者抛出异常，但线程不会被阻塞**。

在 JDK 的并发包中，常见的非阻塞集合有以下几个：
- `ConcurrentHashMap`
- `ConcurrentSkipListMap`
- `ConcurrentSkipListSet`
- `ConcurrentLinkedQueue`
- `ConcurrentLinkedDeque`
- `CopyOnWriteArrayList`
- `CopyOnWriteArraySet`

## 4. 阻塞集合 (Blocking Collection)

**阻塞集合包括添加和移除数据的方法。当集合已满或者为空时，被调用的添加或移除方法就不能立即执行，那么调用这个方法的线程将被阻塞，一直到该方法可以被成功执行为止**。

并发集合框架中的阻塞集合主要是 BlockingQueue 接口的一系列实现类，也就是阻塞队列：
- `LinkedBlockingQueue`：无限阻塞队列
- `LinkedBlockingDeque`：无限双端阻塞队列
- `ArrayBlockingQueue`: 有限阻塞队列
- `PriorityBlockingQueue`: 无限优先级阻塞队列
- `DelayQueue`：延迟队列
- `LinkedTransferQueue`：允许生产者线程等待，直到消费者线程准备就绪可以接收一个元素。
