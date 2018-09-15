- [Java 集合：Map 族 - Map 接口](#java-集合map-族---map-接口)
  - [1. 基本概念](#1-基本概念)
  - [2. 常用 API](#2-常用-api)
  - [3. HashMap 与 TreeMap、LinkedHashMap 的区别](#3-hashmap-与-treemaplinkedhashmap-的区别)
  - [4. Refer Links](#4-refer-links)

# Java 集合：Map 族 - Map 接口

![image](/resources/images/map-api-class-diagram.png)

## 1. 基本概念

[Map](https://docs.oracle.com/javase/9/docs/api/java/util/Map.html) 是 Java 集合类库中的两个基本接口之一。

```java
public interface Map<K,V> {/* ... */}
```

## 2. 常用 API

不同于 Collection 接口，Map 用于存储键值对的映射。因此，其 API 与 Collection 存在显著差别：
- `V put​(K key, V value)`

  不同于 Collection 中添加元素的 add 方法，Map 中使用 put 添加元素，包含了一对键值对。返回 Map 中原先 key 已有的 value，若原先没有该 key，则返回 null。

  NOTE：**键可以为 null，键值不可以为 null**。

- `default V putIfAbsent​(K key, V value)`

  只有当键存在时，才会放入一个值。返回 Map 中原先 key 已有的 value。

- `V get​(Object key)`

  通过 key 从 Map 获取键值。

- `default V getOrDefault​(Object key, V defaultValue)`

  通过 key 从 Map 获取键值。若在映射中未找到这个键，则返回 defaultValue。

- `default void	forEach​(BiConsumer<? super K,? super V> action)`

  该方法从 Java SE 8 开始加入，可通过为该方法提供一个 lambda 表达式，对迭代器的每一个元素调用该 lambda 表达式进行处理。
  ```java
  maps.forEach((k, v) -> System,out,println(k, v));
  ```

- `boolean containsKey​(Object key`

  若映射中已存在该键，返回 true。

- `boolean containsValue​(Object value)`

  若映射中已存在该键值，返回 true。

## 3. HashMap 与 TreeMap、LinkedHashMap 的区别

- HashMap
  
  HashMap 有相对最好的时间效率，但不保证有序（[当 Key 为 Integer 且在 0~2^32-1 范围内时是按 Key 值有序的](https://www.zhihu.com/question/28414001)）。

  相对于 HashMap 和 LinkedHashMap 这些 hash 表的时间复杂度 O(1)（不考虑冲突情况），TreeMap 的增删改查的时间复杂度为 O(logn) 就显得效率较低。而由于 LinkedHashMap 需要维护元素的插入顺序，因此性能略低于 HashMap。

  因此，在需要快速增删改查的存储功能且不需要考虑顺序一致的因素时，应使用 HashMap。 

- LinkedHashMap 

  LinkedHashMap 具有排序功能，且是根据元素的插入顺序进行排序的。
  
  HashMap 并不保证任何顺序性，而 LinkedHashMap 额外保证了 Map 的遍历顺序与 put 顺序一致的有序性。LinkedHashMap 可以避免对 HashMap/HashTable 中 Key-Value 进行排序，只要在插入时按顺序插入即可，同时又避免使用 TreeMap 所增加的成本。

  因此，在需要快速增删改查且需要保证遍历和插入顺序一致的存储功能时，应使用 LinkedHashMap。

- TreeMap 

  TreeMap 具有排序功能，且是根据元素的 Key 值的大小进行排序的。

  由于 TreeMap 是基于红黑树的实现的排序 Map，对于增删改查以及统计的时间复杂度都控制在 O(logn) 的级别上，相对于 HashMap 和 LinkedHashMap 的统计操作的（最大的 key，最小的 key，大于某一个 key 的所有 Entry 等等) 时间复杂度 O(n) 具有更高的时间效率 O(logn)。

  因此，在需要基于排序的统计功能时，应使用 TreeSet。

## 4. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)