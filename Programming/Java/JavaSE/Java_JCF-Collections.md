- [Java 集合：算法包装类 Collections](#java-%E9%9B%86%E5%90%88%EF%BC%9A%E7%AE%97%E6%B3%95%E5%8C%85%E8%A3%85%E7%B1%BB-collections)
  - [1. 排序](#1-%E6%8E%92%E5%BA%8F)
  - [2. 混排](#2-%E6%B7%B7%E6%8E%92)
  - [3. 搜索](#3-%E6%90%9C%E7%B4%A2)
  - [4. 其它](#4-%E5%85%B6%E5%AE%83)
  - [5. Refer Links](#5-refer-links)

# Java 集合：算法包装类 Collections

虽没有 C++ 标准库 STL 中实现的几十种算法丰富，但 Java 集合框架 JCF 也提供了基本的排序、二分查找等实用算法，这些算法实现主要封装于集合框架的工具类 [Collections](https://docs.oracle.com/javase/9/docs/api/java/util/Collections.html) 中。

## 1. 排序

Collections.sort 方法可以对实现了 List 接口的集合进行排序：

- `static <T extends Comparable<? super T>> void	sort​(List<T> list)`

  该方法假定集合的元素都实现了 Comparable 接口。

- `static <T> void	sort​(List<T> list, Comparator<? super T> c)`

  通过传入 Comparator 对象，可自定义排序的规则。

列表包含了数组结构和链表结构，其中对数组的排序算法基本上都是基于随机访问的，然而对链表排序时若采用基于随机访问的排序算法，效率会极低。因此，**Java 集合框架中的排序方法的实现是直接将所有元素转入一个数组，然后对数组进行排序，最后再将排序后的序列复制回列表**。

## 2. 混排

Collections.shuffle 方法的功能与 sort 正好相反，该方法会随机地混排列表中元素的顺序。

- `static void	shuffle​(List<?> list)`

- `static void	shuffle​(List<?> list, Random rnd)`

类似 sort 方法，若所给列表没有实现 RandomAccess 接口，该方法会将元素复制到数组中，然后打乱数组中元素的顺序，最后再将打乱顺序后的元素复制回列表。

## 3. 搜索

Collections.binarySearch​ 方法实现了二分查找算法。

- `static <T> int	binarySearch​(List<? extends Comparable<? super T>> list, T key)`

- `static <T> int	binarySearch​(List<? extends T> list, T key, Comparator<? super T> c)`

NOTE：
- 调用该方法时，集合必须是已排好序的，否则该方法会返回错误的答案。
- 返回值
  - 若调用返回数值非负数，表示匹配到的对象的索引；
  - 若返回负数，表示没有匹配到的元素，但可以利用返回值计算应该将 element 插入到集合的那个位置，以保持集合的有序性。若返回值为 i，则应插入的位置是 (-i - 1)。
- 只有采用随机访问时，二分查找才有意义。因此，若对链表结构调用 binarySearch​方法，它将自动变成线性查找。

## 4. 其它

Collections 中还包含了许多简单算法的封装实现，使用这些方法，**加快开发者开发效率的同时，更可提高代码可读性**。

- 返回集合中最大或最小的元素
  - `static <T extends Object & Comparable<? super T>> T	max​(Collection<? extends T> coll)`
  - `static <T> T	max​(Collection<? extends T> coll, Comparator<? super T> comp)`
  - `static <T extends Object & Comparable<? super T>> T	min​(Collection<? extends T> coll)`
  - `static <T> T	min​(Collection<? extends T> coll, Comparator<? super T> comp)`
- 将原列表的所有元素赋值到目标列表的相应位置上。新列表的长度至少应与原列表长度一样。
  - `static <T> void	copy​(List<? super T> dest, List<? extends T> src)`
- 将列表中所有位置设置所给值
  - `static <T> void	fill​(List<? super T> list, T obj)`
- 将所给所有值添加到集合中
  - `static <T> boolean	addAll​(Collection<? super T> c, T... elements)`
- 将集合中的所有匹配的元素的值替换为 newValue
  - `static <T> boolean	replaceAll​(List<T> list, T oldVal, T newVal)`
- 交互集合中指定的两个元素
  - `static void	swap​(List<?> list, int i, int j)`
- 逆置列表中元素的顺序，复杂度为 O(n)
  - `static void	reverse​(List<?> list)`
- 返回集合中与对象 o 相同的元素的个数
  - `static int	frequency​(Collection<?> c, Object o)`
- 若两个集合没有任何相同的元素，返回 true
  - `static boolean	disjoint​(Collection<?> c1, Collection<?> c2)`

## 5. Refer Links

[java 集合系列](http://blog.csdn.net/column/details/14681.html)