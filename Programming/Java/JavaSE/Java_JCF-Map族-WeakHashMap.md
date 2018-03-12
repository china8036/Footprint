- [Java 集合：Map 族 - WeakHashMap 实现类](#java-%E9%9B%86%E5%90%88%EF%BC%9Amap-%E6%97%8F---weakhashmap-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
    - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [2. 常用 API](#2-%E5%B8%B8%E7%94%A8-api)
    - [3. 源码分析](#3-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
    - [4. Refer Links](#4-refer-links)
    
# Java 集合：Map 族 - WeakHashMap 实现类

## 1. 基本概念

对于 HashMap 存在这样的问题：由于 GC 跟踪活动的对象，只要映射对象是活动的，其中所有的桶也是活动的，因此即时一条映射的键值已不再使用了，GC 也不会回收，这导致了开发者需要负责从长期存活的映射表中删除无用的值。

为解决这个问题，Java 集合框架提供了 [WeakHashMap](https://docs.oracle.com/javase/9/docs/api/java/util/WeakHashMap.html)。当对键的唯一引用来自散列条目时，WeakHashMap 将与 GC 协同工作一起删除键值对。

WeakHashMap 使用弱引用保存键。WeakReference 对象将引用保存到另外一个对象中，即散列键。若某个对象只能由 WeakReference 引用，GC 仍然会回收它，但要将引用该对象的 WeakReference 放入队列中。WeakHashMap 会周期性检查队列，找出新添加的弱引用并删除其对应的条目。

## 2. 常用 API

## 3. 源码分析

## 4. Refer Links
