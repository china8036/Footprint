- [Java 集合：Map 族 - 概述](#java-%E9%9B%86%E5%90%88%EF%BC%9Amap-%E6%97%8F---%E6%A6%82%E8%BF%B0)
    - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
    - [2. 常用 API](#2-%E5%B8%B8%E7%94%A8-api)
    - [3. Refer Links](#3-refer-links)

# Java 集合：Map 族 - 概述

## 1. 基本概念

[Map](https://docs.oracle.com/javase/9/docs/api/java/util/Map.html) 是 Java 集合类库中的两个基本接口之一。

## 2. 常用 API

不同于 Collection 接口，Map 用于存储键值对的映射。因此，其 API 与 Collection 存在显著差别：
- `V	put​(K key, V value)`

  不同于 Collection 中添加元素的 add 方法，Map 中使用 put 添加元素，包含了一对键值对。

  NOTE：**键可以为 null，键值不可以为 null**。

- `default V	putIfAbsent​(K key, V value)`

  只有当键存在时，才会放入一个值。

- `V	get​(Object key)`

  通过 key 从 Map 获取键值。

- `default V	getOrDefault​(Object key, V defaultValue)`

  通过 key 从 Map 获取键值。若在映射中未找到这个键，则返回 defaultValue。

- `default void	forEach​(BiConsumer<? super K,? super V> action)`

  该方法从 Java SE 8 开始加入，可通过为该方法提供一个 lambda 表达式，对迭代器的每一个元素调用该 lambda 表达式进行处理。
  ```java
  maps.forEach(k, v) ->
    System,out,println(k, v);
  ```

- `boolean	containsKey​(Object key`

  若映射中已存在该键，返回 true。

- `boolean	containsValue​(Object value)`

  若映射中已存在该键值，返回 true。

## 3. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)