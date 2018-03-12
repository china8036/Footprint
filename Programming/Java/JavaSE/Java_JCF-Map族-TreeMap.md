- [Java 集合：Map 族 - TreeMap 实现类](#java-%E9%9B%86%E5%90%88%EF%BC%9Amap-%E6%97%8F---treemap-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
    - [1. SortedMap 接口](#1-sortedmap-%E6%8E%A5%E5%8F%A3)
    - [2. TreeMap 实现类](#2-treemap-%E5%AE%9E%E7%8E%B0%E7%B1%BB)
        - [2.1. 基本概念](#21-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
        - [2.2. 常用 API](#22-%E5%B8%B8%E7%94%A8-api)
        - [2.3. 源码分析](#23-%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
    - [3. Refer Links](#3-refer-links)

# Java 集合：Map 族 - TreeMap 实现类

## 1. SortedMap 接口

## 2. TreeMap 实现类

### 2.1. 基本概念

### 2.2. 常用 API

树映射 [TreeMap](https://docs.oracle.com/javase/9/docs/api/java/util/TreeMap.html) 用键的整体顺序对元素进行排序，并将其组织成搜索树。比较函数只能作用于键。

若不需要排序，应选择 HashMap，避免用于排序的开销。

构造器
- `TreeMap​()	`
- `TreeMap​(Comparator<? super K> comparator)	`
- `TreeMap​(Map<? extends K,? extends V> m)	`
- `TreeMap​(SortedMap<K,? extends V> m)`

常用 API
- `Comparator<? super K>	comparator​()`

  返回对键的比较器。如果键使用 Comparable 接口的 compareTo 方法进行比较，返回 null。

- `K	firstKey​()`

  返回映射中的最小元素。

- `K	lastKey​()`

  返回映射中的最大元素。

- `Map.Entry<K,V>	firstEntry​()`

### 2.3. 源码分析

## 3. Refer Links
