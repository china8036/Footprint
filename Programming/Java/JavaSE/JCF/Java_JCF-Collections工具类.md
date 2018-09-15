- [Java 集合：算法包装类 Collections](#java-集合算法包装类-collections)
	- [1. 针对 List 集合提供的方法](#1-针对-list-集合提供的方法)
		- [1.1. 排序 sort()](#11-排序-sort)
			- [1.1.1. API](#111-api)
			- [1.1.2. 实现原理](#112-实现原理)
		- [1.2. 混排 shuffle()](#12-混排-shuffle)
			- [1.2.1. API](#121-api)
			- [1.2.2. 实现原理](#122-实现原理)
		- [1.3. 交换元素 swap()](#13-交换元素-swap)
			- [1.3.1. API](#131-api)
			- [1.3.2. 实现原理](#132-实现原理)
		- [1.4. 反转 reverse()](#14-反转-reverse)
			- [1.4.1. API](#141-api)
			- [1.4.2. 实现原理](#142-实现原理)
		- [1.5. 二分查找 binarySearch​()](#15-二分查找-binarysearch)
			- [1.5.1. API](#151-api)
			- [1.5.2. 实现原理](#152-实现原理)
		- [1.6. 旋转 rotate()](#16-旋转-rotate)
			- [1.6.1. API](#161-api)
			- [1.6.2. 实现原理](#162-实现原理)
		- [1.7. 填充 fill()](#17-填充-fill)
			- [1.7.1. API](#171-api)
			- [1.7.2. 实现原理](#172-实现原理)
		- [1.8. 替换 replaceAll()](#18-替换-replaceall)
			- [1.8.1. API](#181-api)
			- [1.8.2. 实现原理](#182-实现原理)
		- [1.9. 索引查询 indexOfSubList() 和 lastIndexOfSubList()](#19-索引查询-indexofsublist-和-lastindexofsublist)
			- [1.9.1. API](#191-api)
			- [1.9.2. 实现原理](#192-实现原理)
		- [1.10. 复制 copy​()](#110-复制-copy)
	- [2. 为所有 Collection 提供的方法](#2-为所有-collection-提供的方法)
		- [2.1. 最值 max​() 和 min​()](#21-最值-max-和-min)
			- [2.1.1. API](#211-api)
			- [2.1.2. 实现原理](#212-实现原理)
		- [2.2. 其它方法](#22-其它方法)
	- [3. 集合转换](#3-集合转换)
		- [3.1. 不可变集合](#31-不可变集合)
			- [3.1.1. API](#311-api)
			- [3.1.2. 实现原理](#312-实现原理)
		- [3.2. 线程同步集合](#32-线程同步集合)
			- [3.2.1. API](#321-api)
			- [3.2.2. 实现原理](#322-实现原理)
		- [3.3. 类型检查集合](#33-类型检查集合)
			- [3.3.1. API](#331-api)
			- [3.3.2. 实现原理](#332-实现原理)
		- [3.4. 单一元素集合](#34-单一元素集合)
			- [3.4.1. API](#341-api)
			- [3.4.2. 实现原理](#342-实现原理)
	- [4. Refer Links](#4-refer-links)

# Java 集合：算法包装类 Collections

虽没有 C++ 标准库 STL 中实现的几十种算法丰富，但 Java 在集合框架 JCF 中也提供了基本的排序、二分查找等实用算法的实现，以及对集合进行排序、查询、修改、同步化等实用操作，这些实现主要封装于 JCF 的工具类 [Collections](https://docs.oracle.com/javase/9/docs/api/java/util/Collections.html) 中，使用这些方法，**加快开发者开发效率的同时，更可提高代码可读性**。

## 1. 针对 List 集合提供的方法

Collections 工具类中，包含了一部分专门用于 List 集合的方法：

### 1.1. 排序 sort()

#### 1.1.1. API

Collections 中提供了对 List 集合进行排序的方法：
- `static <T extends Comparable<? super T>> void sort​(List<T> list)`: 该方法假定集合的元素都实现了 Comparable 接口。

- `static <T> void sort​(List<T> list, Comparator<? super T> c)`: 该方法可通过传入 Comparator 对象，从而自定义排序的规则。

#### 1.1.2. 实现原理

Collections.sort() 源码：
```java
public static <T extends Comparable<? super T>> void sort(List<T> list) {
    // 若传入 List 集合是 ArrayList 对象，则直接调用 Arrays.sort() 进行排序，因为 ArrayList 的底层实现就是数组
    if (list.getClass() == ArrayList.class) {
        Arrays.sort(((ArrayList) list).elementData, 0, list.size());
        return;
    }

    // 若传入 List 集合不是 ArrayList 对象，通过 toArray() 转换为数组，再调用 Arrays.sort() 对该数组进行排序
    Object[] a = list.toArray();
    Arrays.sort(a);
    // 遍历原集合，将有序数组赋值到原集合的相应位置上
    ListIterator<T> i = list.listIterator();
    for (int j=0; j<a.length; j++) {
        i.next();
        i.set((T)a[j]);
    }
}

// 可定制排序逻辑，实现逻辑与不传入比较器的接口的实现逻辑完全相同
public static <T> void sort(List<T> listlist, Comparator<? super T> c) {
    if (list.getClass() == ArrayList.class) {
        Arrays.sort(((ArrayList) list).elementData, 0, list.size(), (Comparator) c);
        return;
    }

    Object[] a = list.toArray();
    Arrays.sort(a, (Comparator)c);
    ListIterator<T> i = list.listIterator();
    for (int j=0; j<a.length; j++) {
        i.next();
        i.set((T)a[j]);
    }
}
```

从源码中可知：

List 集合包含了数组结构和链表结构，其中对数组的排序算法基本上都是基于随机访问的，然而对链表排序时若采用基于随机访问的排序算法，效率会极低。

因此，**排序的实现是先检查 List 集合的实现：若基于数组，则直接进行排序；若基于链表，则先将所有元素转入一个数组，然后对数组进行排序，最后再将排序后的序列复制回链表**。

### 1.2. 混排 shuffle()

#### 1.2.1. API

Collections.shuffle() 方法的功能与 sort() 正好相反，该方法会随机地混排列表中元素的顺序，可用于模拟“洗牌”操作。
- `static void shuffle​(List<?> list)`
- `static void shuffle​(List<?> list, Random rnd)`

#### 1.2.2. 实现原理

```java
public static void shuffle(List<?> list) {
    Random rnd = r;
    if (rnd == null)
        r = rnd = new Random(); // harmless race.
    shuffle(list, rnd);
}

@SuppressWarnings({"rawtypes", "unchecked"})
public static void shuffle(List<?> list, Random rnd) {
    int size = list.size();
    // 若 List 集合元素数量小于 5 或支持随机访问
    if (size < SHUFFLE_THRESHOLD || list instanceof RandomAccess) {
        // 遍历集合，随机交换元素的位置
        for (int i=size; i>1; i--)
            swap(list, i-1, rnd.nextInt(i));
    } else {
        // 若 List 集合元素数量大于 5 且不支持随机访问
        // 则先将元素复制到数组中
        Object arr[] = list.toArray();

        // 然后打乱数组中元素的顺序
        for (int i=size; i>1; i--)
            swap(arr, i-1, rnd.nextInt(i));

        // 最后再将打乱顺序后的元素复制回列表
        // instead of using a raw type here, it's possible to capture
        // the wildcard but it will require a call to a supplementary
        // private method
        ListIterator it = list.listIterator();
        for (int i=0; i<arr.length; i++) {
            it.next();
            it.set(arr[i]);
        }
    }
}
```
从源码中可知：类似 sort 方法，若所给列表没有实现 RandomAccess 接口，该方法会将元素复制到数组中，然后打乱数组中元素的顺序，最后再将打乱顺序后的元素复制回列表。

### 1.3. 交换元素 swap()

#### 1.3.1. API

Collections.swap() 方法用于交换 List 集合中指定位置的两个元素。
- `static void swap​(List<?> list, int i, int j)`

#### 1.3.2. 实现原理

```java
@SuppressWarnings({"rawtypes", "unchecked"})
public static void swap(List<?> list, int i, int j) {
    // instead of using a raw type here, it's possible to capture
    // the wildcard but it will require a call to a supplementary
    // private method
    final List l = list;
    // 通过 List 接口的 get 和 set 方法进行元素值的交换
    l.set(i, l.set(j, l.get(i)));
}
```

List 接口的 set 和 get 方法定义：
```java
// 赋值后返回旧值
E set(int index, E element);
// 返回指定位置的值
E get(int index);
```

### 1.4. 反转 reverse()

#### 1.4.1. API

Collections.reverse() 方法可用于反转指定 List 集合中元素的顺序，复杂度为 O(n)。
- `static void reverse​(List<?> list)`

#### 1.4.2. 实现原理

```java
@SuppressWarnings({"rawtypes", "unchecked"})
public static void reverse(List<?> list) {
    int size = list.size();
    // 若 List 集合元素较少（小于 18）或支持随机访问
    if (size < REVERSE_THRESHOLD || list instanceof RandomAccess) {
        // 则直接从头开始，第一个元素和最后一个元素交换位置，一直交换到中间位置
        for (int i=0, mid=size>>1, j=size-1; i<mid; i++, j--)
            swap(list, i, j);
    } else {
        // 若 List 集合元素较多且不支持随机访问
        // 则使用两个迭代器，一个从头开始一个从尾开始，遍历、赋值
        ListIterator fwd = list.listIterator();
        ListIterator rev = list.listIterator(size);
        for (int i=0, mid=list.size()>>1; i<mid; i++) {
            Object tmp = fwd.next();
            fwd.set(rev.previous());
            rev.set(tmp);
        }
    }
}
```

### 1.5. 二分查找 binarySearch​()

#### 1.5.1. API

Collections.binarySearch​() 方法实现了二分查找算法，在 List 集合有序的情况下时间复杂度平均为 O(logn)。
- `static <T> int	binarySearch​(List<? extends Comparable<? super T>> list, T key)`
- `static <T> int	binarySearch​(List<? extends T> list, T key, Comparator<? super T> c)`

NOTE：
- 调用该方法时，集合必须是已排好序的，否则该方法会返回错误的答案。
- 返回值
  - 若调用返回数值非负数，表示匹配到的对象的索引；
  - 若返回负数，表示没有匹配到的元素，但可以利用返回值计算应该将 element 插入到集合的那个位置，以保持集合的有序性。若返回值为 i，则应插入的位置是 (-i - 1)。
- 只有采用随机访问时，二分查找才有意义。因此，若对链表结构调用 binarySearch​() 方法，它将自动变成线性查找。

#### 1.5.2. 实现原理

Collections.binarySearch() 源码：
```java
public static <T>
int binarySearch(List<? extends Comparable<? super T>> list, T key) {
    // 如果传入 List 支持随机访问或数据量小于二分查找的阈值 5000
    if (list instanceof RandomAccess || list.size()<BINARYSEARCH_THRESHOLD)
        return Collections.indexedBinarySearch(list, key);
    // 否则
    else
        return Collections.iteratorBinarySearch(list, key);
}

// 支持随机访问的 List 的二分查找实现，时间效率为 O(logn)
private static <T>
int indexedBinarySearch(List<? extends Comparable<? super T>> list, T key) {
    int low = 0;
    int high = list.size()-1;

    while (low <= high) {
        // 无符号右移 1 位，等于除以 2
        int mid = (low + high) >>> 1;
        // 由于此处的 List 支持随机访问，因此 get() 方法时间效率为 O(1)
        Comparable<? super T> midVal = list.get(mid);
        int cmp = midVal.compareTo(key);

        if (cmp < 0)
            low = mid + 1;
        else if (cmp > 0)
            high = mid - 1;
        else
            return mid; // key found
    }
    return -(low + 1);  // key not found
}

// 不支持随机访问的 List 的二分查找实现，实际上退化成了线性查找，时间效率为 O(n^2)
private static <T>
int iteratorBinarySearch(List<? extends Comparable<? super T>> list, T key)
{
    int low = 0;
    int high = list.size()-1;
    ListIterator<? extends Comparable<? super T>> i = list.listIterator();

    while (low <= high) {
        int mid = (low + high) >>> 1;
        // 由于此处的 List 不支持随机访问，因此 get() 方法时间效率达到了 O(n^2)
        Comparable<? super T> midVal = get(i, mid);
        int cmp = midVal.compareTo(key);

        if (cmp < 0)
            low = mid + 1;
        else if (cmp > 0)
            high = mid - 1;
        else
            return mid; // key found
    }
    return -(low + 1);  // key not found
}
// 对 List 进行线性查找，但做了一些优化
private static <T> T get(ListIterator<? extends T> i, int index) {
    T obj = null;
    int pos = i.nextIndex();
    if (pos <= index) {
        do {
            obj = i.next();
        } while (pos++ < index);
    } else {
        do {
            obj = i.previous();
        } while (--pos > index);
    }
    return obj;
}
```

### 1.6. 旋转 rotate()

#### 1.6.1. API

Collections.rotate() 方法用于 List 集合的旋转操作。
- `public static void rotate(List<?> list, int distance)`
  - 当 distance 为正数时，执行顺时针旋转：将 List 集合的后 distance 个元素“整体”移动到前面；
  - 当 distance 为负数时，执行逆时针旋转：将 List 集合的前 distance 个元素“整体”移动到后面。
  
  该方法不会改变原 List 集合的长度。

#### 1.6.2. 实现原理

```java
public static void rotate(List<?> list, int distance) {
    if (list instanceof RandomAccess || list.size() < ROTATE_THRESHOLD)
        rotate1(list, distance);
    else
        rotate2(list, distance);
}

private static <T> void rotate1(List<T> list, int distance) {
    int size = list.size();
    if (size == 0)
        return;
    distance = distance % size;
    if (distance < 0)
        distance += size;
    if (distance == 0)
        return;

    for (int cycleStart = 0, nMoved = 0; nMoved != size; cycleStart++) {
        T displaced = list.get(cycleStart);
        int i = cycleStart;
        do {
            i += distance;
            if (i >= size)
                i -= size;
            displaced = list.set(i, displaced);
            nMoved ++;
        } while (i != cycleStart);
    }
}

private static void rotate2(List<?> list, int distance) {
    int size = list.size();
    if (size == 0)
        return;
    int mid =  -distance % size;
    if (mid < 0)
        mid += size;
    if (mid == 0)
        return;

    reverse(list.subList(0, mid));
    reverse(list.subList(mid, size));
    reverse(list);
}
```

### 1.7. 填充 fill()

#### 1.7.1. API

Collections.fill() 方法使用指定元素 obj 替换指定 List 集合中的所有元素。
- `public static <T> void fill(List<? super T> list, T obj)`

#### 1.7.2. 实现原理

```java
public static <T> void fill(List<? super T> list, T obj) {
    int size = list.size();

    if (size < FILL_THRESHOLD || list instanceof RandomAccess) {
        for (int i=0; i<size; i++)
            list.set(i, obj);
    } else {
        ListIterator<? super T> itr = list.listIterator();
        for (int i=0; i<size; i++) {
            itr.next();
            itr.set(obj);
        }
    }
}
```

### 1.8. 替换 replaceAll()

#### 1.8.1. API

Collections.replaceAll() 方法会将集合中的所有匹配的元素的值替换为 newValue。
- `static <T> boolean	replaceAll​(List<T> list, T oldVal, T newVal)`

#### 1.8.2. 实现原理

```java
public static <T> boolean replaceAll(List<T> list, T oldVal, T newVal) {
    boolean result = false;
    int size = list.size();
    if (size < REPLACEALL_THRESHOLD || list instanceof RandomAccess) {
        if (oldVal==null) {
            for (int i=0; i<size; i++) {
                if (list.get(i)==null) {
                    list.set(i, newVal);
                    result = true;
                }
            }
        } else {
            for (int i=0; i<size; i++) {
                if (oldVal.equals(list.get(i))) {
                    list.set(i, newVal);
                    result = true;
                }
            }
        }
    } else {
        ListIterator<T> itr=list.listIterator();
        if (oldVal==null) {
            for (int i=0; i<size; i++) {
                if (itr.next()==null) {
                    itr.set(newVal);
                    result = true;
                }
            }
        } else {
            for (int i=0; i<size; i++) {
                if (oldVal.equals(itr.next())) {
                    itr.set(newVal);
                    result = true;
                }
            }
        }
    }
    return result;
}
```

### 1.9. 索引查询 indexOfSubList() 和 lastIndexOfSubList()

#### 1.9.1. API

Collections 类中为 List 集合提供了多种索引查询的方法：
- `public static int indexOfSubList(List<?> source, List<?> target)`: 返回子 List 对象 target 在父 List 对象 source 中第一次出现的位置索引；若父 List 中没有出现子 List，则返回 -1。
- `public static int lastIndexOfSubList(List<?> source, List<?> target)`: 返回子 List 对象 target 在父 List 对象 source 中最后一次出现的位置索引；若父 List 中没有出现子 List，则返回 -1。

#### 1.9.2. 实现原理

查找子列表最早 / 最晚出现的索引是面试时的典型问题。

```java
public static int indexOfSubList(List<?> source, List<?> target) {
    int sourceSize = source.size();
    int targetSize = target.size();
    // 记录父 List 和子 List 个数差距
    int maxCandidate = sourceSize - targetSize;

    // 若父 List 的元素个数小于阈值 35 或父 List 和子 List 都支持随机访问
    if (sourceSize < INDEXOFSUBLIST_THRESHOLD ||
        (source instanceof RandomAccess&&target instanceof RandomAccess)) {
    nextCand:// 定义跳转入口
        // 用两个游标，一个表示父列表的第一个元素，另一个表示可能是子列表元素范围的索引，在遍历过程中如果发现的确和子列表所有元素都相等，那就找到了子列表
        for (int candidate = 0; candidate <= maxCandidate; candidate++) {
            for (int i=0, j=candidate; i<targetSize; i++, j++)
                if (!eq(target.get(i), source.get(j)))
                    continue nextCand;  // Element mismatch, try next cand
            return candidate;  // All elements of candidate matched target
        }
    } else {  // Iterator version of above algorithm
        ListIterator<?> si = source.listIterator();
    nextCand:
        for (int candidate = 0; candidate <= maxCandidate; candidate++) {
            ListIterator<?> ti = target.listIterator();
            for (int i=0; i<targetSize; i++) {
                if (!eq(ti.next(), si.next())) {
                    // Back up source iterator to next candidate
                    for (int j=0; j<i; j++)
                        si.previous();
                    continue nextCand;
                }
            }
            return candidate;
        }
    }
    return -1;  // No candidate matched the target
}

public static int lastIndexOfSubList(List<?> source, List<?> target) {
    int sourceSize = source.size();
    int targetSize = target.size();
    int maxCandidate = sourceSize - targetSize;

    if (sourceSize < INDEXOFSUBLIST_THRESHOLD ||
        source instanceof RandomAccess) {   // Index access version
    nextCand:
        for (int candidate = maxCandidate; candidate >= 0; candidate--) {
            for (int i=0, j=candidate; i<targetSize; i++, j++)
                if (!eq(target.get(i), source.get(j)))
                    continue nextCand;  // Element mismatch, try next cand
            return candidate;  // All elements of candidate matched target
        }
    } else {  // Iterator version of above algorithm
        if (maxCandidate < 0)
            return -1;
        ListIterator<?> si = source.listIterator(maxCandidate);
    nextCand:
        for (int candidate = maxCandidate; candidate >= 0; candidate--) {
            ListIterator<?> ti = target.listIterator();
            for (int i=0; i<targetSize; i++) {
                if (!eq(ti.next(), si.next())) {
                    if (candidate != 0) {
                        // Back up source iterator to next candidate
                        for (int j=0; j<=i+1; j++)
                            si.previous();
                    }
                    continue nextCand;
                }
            }
            return candidate;
        }
    }
    return -1;  // No candidate matched the target
}
```

### 1.10. 复制 copy​()

将原列表的所有元素赋值到目标列表的相应位置上，新列表的长度至少应与原列表长度一样。
- `static <T> void copy​(List<? super T> dest, List<? extends T> src)`

## 2. 为所有 Collection 提供的方法

### 2.1. 最值 max​() 和 min​()

#### 2.1.1. API

返回集合中最大或最小的元素
- `static <T extends Object & Comparable<? super T>> T max​(Collection<? extends T> coll)`
- `static <T> T max​(Collection<? extends T> coll, Comparator<? super T> comp)`
- `static <T extends Object & Comparable<? super T>> T min​(Collection<? extends T> coll)`
- `static <T> T min​(Collection<? extends T> coll, Comparator<? super T> comp)`

#### 2.1.2. 实现原理

```java
public static <T extends Object & Comparable<? super T>> T min(Collection<? extends T> coll) {
    Iterator<? extends T> i = coll.iterator();
    T candidate = i.next();

    while (i.hasNext()) {
        T next = i.next();
        if (next.compareTo(candidate) < 0)
            candidate = next;
    }
    return candidate;
}

public static <T> T max(Collection<? extends T> coll, Comparator<? super T> comp) {
    if (comp==null)
        return (T)max((Collection) coll);

    Iterator<? extends T> i = coll.iterator();
    T candidate = i.next();

    while (i.hasNext()) {
        T next = i.next();
        if (comp.compare(next, candidate) > 0)
            candidate = next;
    }
    return candidate;
}
```

### 2.2. 其它方法

- 将所给所有值添加到集合中
  - `static <T> boolean	addAll​(Collection<? super T> c, T... elements)`
- 返回集合中与对象 o 相同的元素的个数
  - `static int	frequency​(Collection<?> c, Object o)`
- 若两个集合没有任何相同的元素，返回 true
  - `static boolean	disjoint​(Collection<?> c1, Collection<?> c2)`

## 3. 集合转换

Collections 工具类中除了对各种常用算法和操作进行了封装，还提供了一系列内部类集合，通过其提供的 API 可将原始集合转变为内部类类型。

### 3.1. 不可变集合

#### 3.1.1. API

有时候我们拿到一部分数据，这个数据不允许修改、删除，只允许读取，我们可以使用 `Collections.unmodifiableXxx()` 方法来实现，该方法会返回指定集合对象的不可变视图，集合参数可以是 List、Set、SortedSet、Map、SortedMap。
- `public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c)`
- `public static <T> Set<T> unmodifiableSet(Set<? extends T> s)`
- `public static <T> SortedSet<T> unmodifiableSortedSet(SortedSet<T> s)`
- `public static <T> NavigableSet<T> unmodifiableNavigableSet(NavigableSet<T> s)`
- `public static <T> List<T> unmodifiableList(List<? extends T> list)`
- `public static <K,V> Map<K,V> unmodifiableMap(Map<? extends K, ? extends V> m)`
- `public static <K,V> SortedMap<K,V> unmodifiableSortedMap(SortedMap<K, ? extends V> m)`

#### 3.1.2. 实现原理

首先看下最大范围的 `Collections.unmodifiableCollection(c)` 方法，它可以返回一个容器的包装类，这个包装类的添加、替换、删除操作都会抛出异常 UnsupportedOperationException。
```java
public static <T> Collection<T> unmodifiableCollection(Collection<? extends T> c) {
    return new UnmodifiableCollection<>(c);
}

// 定于一个内部类：不可变的集合
static class UnmodifiableCollection<E> implements Collection<E>, Serializable {
    private static final long serialVersionUID = 1820017752578914078L;

    final Collection<? extends E> c;

    UnmodifiableCollection(Collection<? extends E> c) {
        if (c==null)
            throw new NullPointerException();
        this.c = c;
    }

    // 普通查询操作都是支持的
    public int size()                   {return c.size();}
    public boolean isEmpty()            {return c.isEmpty();}
    public boolean contains(Object o)   {return c.contains(o);}
    public Object[] toArray()           {return c.toArray();}
    public <T> T[] toArray(T[] a)       {return c.toArray(a);}
    public String toString()            {return c.toString();}

    public Iterator<E> iterator() {
        return new Iterator<E>() {
            private final Iterator<? extends E> i = c.iterator();

            public boolean hasNext() {return i.hasNext();}
            public E next()          {return i.next();}
            public void remove() {    // 迭代器的 remove 也不支持
                throw new UnsupportedOperationException();
            }
            @Override
            public void forEachRemaining(Consumer<? super E> action) {
                // Use backing collection version
                i.forEachRemaining(action);
            }
        };
    }

    // 这里开始一系列修改操作就不支持了，调用会直接抛出异常
    public boolean add(E e) {
        throw new UnsupportedOperationException();
    }
    public boolean remove(Object o) {
        throw new UnsupportedOperationException();
    }

    public boolean containsAll(Collection<?> coll) {
        return c.containsAll(coll);
    }
    public boolean addAll(Collection<? extends E> coll) {
        throw new UnsupportedOperationException();
    }
    public boolean removeAll(Collection<?> coll) {
        throw new UnsupportedOperationException();
    }
    public boolean retainAll(Collection<?> coll) {
        throw new UnsupportedOperationException();
    }
    public void clear() {
        throw new UnsupportedOperationException();
    }

    // Override default methods in Collection
    @Override
    public void forEach(Consumer<? super E> action) {
        c.forEach(action);
    }

    @Override
    public boolean removeIf(Predicate<? super E> filter) {
        throw new UnsupportedOperationException();
    }
}
```
其他类型的不可变集合也基本是这样的，**用一个包装器类，持有实际集合类的引用，只对外提供查询的方法，增删改的通通抛异常**。

### 3.2. 线程同步集合

如果你需要将一个集合的所有操作都设置为线程安全的，可以使用 `Collections.synchronizedXxx()` 方法，该方法返回指定集合的线程同步的“包装版本”。

#### 3.2.1. API

- `public static <T> Collection<T> synchronizedCollection(Collection<T> c)`
- `public static <T> Set<T> synchronizedSet(Set<T> s)`
- `public static <T> SortedSet<T> synchronizedSortedSet(SortedSet<T> s)`
- `public static <T> NavigableSet<T> synchronizedNavigableSet(NavigableSet<T> s)`
- `public static <T> List<T> synchronizedList(List<T> list)`
- `public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m)`
- `public static <K,V> SortedMap<K,V> synchronizedSortedMap(SortedMap<K,V> m)`

#### 3.2.2. 实现原理

```java
public static <T> Collection<T> synchronizedCollection(Collection<T> c) {
    return new SynchronizedCollection<>(c);
}

// 第二个参数是用于同步的对象
static <T> Collection<T> synchronizedCollection(Collection<T> c, Object mutex) {
    return new SynchronizedCollection<>(c, mutex);
}

static class SynchronizedCollection<E> implements Collection<E>, Serializable {
    private static final long serialVersionUID = 3053995032091335093L;

    final Collection<E> c;  // 实际的集合
    final Object mutex;     // 同步的对象

    SynchronizedCollection(Collection<E> c) {
        this.c = Objects.requireNonNull(c);
        mutex = this;
    }

    SynchronizedCollection(Collection<E> c, Object mutex) {
        this.c = Objects.requireNonNull(c);
        this.mutex = Objects.requireNonNull(mutex);
    }

    public int size() {
        synchronized (mutex) {return c.size();}
    }
    public boolean isEmpty() {
        synchronized (mutex) {return c.isEmpty();}
    }
    public boolean contains(Object o) {
        synchronized (mutex) {return c.contains(o);}
    }
    public Object[] toArray() {
        synchronized (mutex) {return c.toArray();}
    }
    public <T> T[] toArray(T[] a) {
        synchronized (mutex) {return c.toArray(a);}
    }

    public Iterator<E> iterator() {
        return c.iterator(); // 迭代器没有进行同步操作，需要使用者自己同步
    }

    public boolean add(E e) {
        synchronized (mutex) {return c.add(e);}
    }
    public boolean remove(Object o) {
        synchronized (mutex) {return c.remove(o);}
    }

    public boolean containsAll(Collection<?> coll) {
        synchronized (mutex) {return c.containsAll(coll);}
    }
    public boolean addAll(Collection<? extends E> coll) {
        synchronized (mutex) {return c.addAll(coll);}
    }
    public boolean removeAll(Collection<?> coll) {
        synchronized (mutex) {return c.removeAll(coll);}
    }
    public boolean retainAll(Collection<?> coll) {
        synchronized (mutex) {return c.retainAll(coll);}
    }
    public void clear() {
        synchronized (mutex) {c.clear();}
    }
    public String toString() {
        synchronized (mutex) {return c.toString();}
    }
    // Override default methods in Collection
    @Override
    public void forEach(Consumer<? super E> consumer) {
        synchronized (mutex) {c.forEach(consumer);}
    }
    @Override
    public boolean removeIf(Predicate<? super E> filter) {
        synchronized (mutex) {return c.removeIf(filter);}
    }
    @Override
    public Spliterator<E> spliterator() {
        return c.spliterator(); // Must be manually synched by user!
    }
    @Override
    public Stream<E> stream() {
        return c.stream(); // Must be manually synched by user!
    }
    @Override
    public Stream<E> parallelStream() {
        return c.parallelStream(); // Must be manually synched by user!
    }
    private void writeObject(ObjectOutputStream s) throws IOException {
        synchronized (mutex) {s.defaultWriteObject();}
    }
}
```
可以看到，该内部类的实现很简单，几乎在所有方法上都添加了 synchronized，这样得到的集合在并发环境中效率会大打折扣，只适合对准确率要求比性能要求高的场景。

需要注意的是，我们在使用 Collections.synchronizedCollection(c) 返回的迭代器时，需要手动对迭代器进行同步，否则在迭代时有并发操作，可能会导致不确定性问题。

例：
```java
Collection c = Collections.synchronizedCollection(myCollection);
// ...
synchronized (c) {
		Iterator i = c.iterator(); // Must be in the synchronized block
		while (i.hasNext())
			foo(i.next());
}
```

### 3.3. 类型检查集合

#### 3.3.1. API

日常开发中我们经常需要使用 Object 作为返回值，然后在具体的使用处强转成指定的类型，在这个强转的过程中，编译器无法检测出强转是否成功。

这时我们就可以使用 `Collections.checkedXxx(c, type)` 方法创建一个只保存某个类型数据的集合，在添加时会进行类型检查操作。

虽然使用泛型也可以实现在编译时检查，但在运行时，泛型擦除成 Object，最终还是需要强转。

#### 3.3.2. 实现原理

```java
public static <E> Collection<E> checkedCollection(Collection<E> c,
                                                  Class<E> type) {
    return new CheckedCollection<>(c, type);
}

@SuppressWarnings("unchecked")
static <T> T[] zeroLengthArray(Class<T> type) {
    return (T[]) Array.newInstance(type, 0);
}

/**
 * @serial include
 */
static class CheckedCollection<E> implements Collection<E>, Serializable {
    private static final long serialVersionUID = 1578914078182001775L;

    final Collection<E> c;
    final Class<E> type;    // 目标类型

    void typeCheck(Object o) {
        if (o != null && !type.isInstance(o))    // 检查是否和目标类型一致
            throw new ClassCastException(badElementMsg(o));
    }

    private String badElementMsg(Object o) {
        return "Attempt to insert " + o.getClass() +
            " element into collection with element type " + type;
    }

    CheckedCollection(Collection<E> c, Class<E> type) {
        if (c==null || type == null)
            throw new NullPointerException();
        this.c = c;
        this.type = type;
    }

    public int size()                 { return c.size(); }
    public boolean isEmpty()          { return c.isEmpty(); }
    public boolean contains(Object o) { return c.contains(o); }
    public Object[] toArray()         { return c.toArray(); }
    public <T> T[] toArray(T[] a)     { return c.toArray(a); }
    public String toString()          { return c.toString(); }
    public boolean remove(Object o)   { return c.remove(o); }
    public void clear()               {        c.clear(); }

    public boolean containsAll(Collection<?> coll) {
        return c.containsAll(coll);
    }
    public boolean removeAll(Collection<?> coll) {
        return c.removeAll(coll);
    }
    public boolean retainAll(Collection<?> coll) {
        return c.retainAll(coll);
    }

    public Iterator<E> iterator() {
        // JDK-6363904 - unwrapped iterator could be typecast to
        // ListIterator with unsafe set()
        final Iterator<E> it = c.iterator();
        return new Iterator<E>() {
            public boolean hasNext() { return it.hasNext(); }
            public E next()          { return it.next(); }
            public void remove()     {        it.remove(); }};
        // Android-note: Should we delegate to it for forEachRemaining ?
    }

    public boolean add(E e) {    // 添加时调用了类型检查
        typeCheck(e);
        return c.add(e);
    }

    private E[] zeroLengthElementArray = null; // Lazily initialized

    private E[] zeroLengthElementArray() {
        return zeroLengthElementArray != null ? zeroLengthElementArray :
            (zeroLengthElementArray = zeroLengthArray(type));
    }

    public boolean addAll(Collection<? extends E> coll) {
        // 这个方法可以让我们避免并发时内容改变导致的问题
        return c.addAll(checkedCopyOf(coll));
    }

    @SuppressWarnings("unchecked")
    // 检查集合中的每个元素的类型
    Collection<E> checkedCopyOf(Collection<? extends E> coll) {
        Object[] a = null;
        try {
            E[] z = zeroLengthElementArray();
            a = coll.toArray(z);
            // Defend against coll violating the toArray contract
            if (a.getClass() != z.getClass())
                a = Arrays.copyOf(a, a.length, z.getClass());
        } catch (ArrayStoreException ignore) {
            // To get better and consistent diagnostics,
            // we call typeCheck explicitly on each element.
            // We call clone() to defend against coll retaining a
            // reference to the returned array and storing a bad
            // element into it after it has been type checked.
            //
            a = coll.toArray().clone();
            for (Object o : a)
                typeCheck(o);
        }
        // A slight abuse of the type system, but safe here.
        return (Collection<E>) Arrays.asList(a);
    }

    // Override default methods in Collection
    @Override
    public void forEach(Consumer<? super E> action) {c.forEach(action);}
    @Override
    public boolean removeIf(Predicate<? super E> filter) {
        return c.removeIf(filter);
    }
    @Override
    public Spliterator<E> spliterator() {return c.spliterator();}
    @Override
    public Stream<E> stream()           {return c.stream();}
    @Override
    public Stream<E> parallelStream()   {return c.parallelStream();
}
```
可以看到，**只是在 add(e) 和 addAll() 时进行了类型检查而已，不符合目标类型就会抛出 ClassCastException 异常**。

### 3.4. 单一元素集合

#### 3.4.1. API

有时候第三方方法需要的参数是一个 Map，而我们只有一组 key-value 数据怎么办，可以使用 `Collections.singletonMap(key, value)` 创建一个 Map。

`Collections.singletonXxx()`方法返回一个只包含指定的一个集合元素的、不可变的集合对象，此集合支持 List、Map 对象。

#### 3.4.2. 实现原理

```java
public static <K,V> Map<K,V> singletonMap(K key, V value) {
    return new SingletonMap<>(key, value);
}

private static class SingletonMap<K,V>
      extends AbstractMap<K,V>
      implements Serializable {
    private static final long serialVersionUID = -6979724477215052911L;

    private final K k;
    private final V v;

    // 构造函数中将参数的 k-v 保存到唯一的 k 和 v 中
    SingletonMap(K key, V value) {
        k = key;
        v = value;
    }

    public int size()                          {return 1;} // 集合长度始终为 1

    public boolean isEmpty()                   {return false;}

    public boolean containsKey(Object key)     {return eq(key, k);}

    public boolean containsValue(Object value) {return eq(value, v);}

    public V get(Object key)                   {return (eq(key, k) ? v : null);}

    private transient Set<K> keySet = null;
    private transient Set<Map.Entry<K,V>> entrySet = null;
    private transient Collection<V> values = null;

    public Set<K> keySet() {
        if (keySet==null)
            keySet = singleton(k);
        return keySet;
    }

    public Set<Map.Entry<K,V>> entrySet() {
        if (entrySet==null)
            entrySet = Collections.<Map.Entry<K,V>>singleton(
                new SimpleImmutableEntry<>(k, v));
        return entrySet;
    }

    public Collection<V> values() {
        if (values==null)
            values = singleton(v);
        return values;
    }

    // Override default methods in Map
    @Override
    public V getOrDefault(Object key, V defaultValue) {
        return eq(key, k) ? v : defaultValue;
    }

    @Override
    public void forEach(BiConsumer<? super K, ? super V> action) {
        action.accept(k, v);
    }

    @Override
    public void replaceAll(BiFunction<? super K, ? super V, ? extends V> function) {
        throw new UnsupportedOperationException();
    }

    @Override
    public V putIfAbsent(K key, V value) {
        throw new UnsupportedOperationException();
    }

    @Override
    public boolean remove(Object key, Object value) {
        throw new UnsupportedOperationException();
    }

    @Override
    public boolean replace(K key, V oldValue, V newValue) {
        throw new UnsupportedOperationException();
    }

    @Override
    public V replace(K key, V value) {
        throw new UnsupportedOperationException();
    }

    @Override
    public V computeIfAbsent(K key,
                             Function<? super K, ? extends V> mappingFunction) {
        throw new UnsupportedOperationException();
    }

    @Override
    public V computeIfPresent(K key,
                              BiFunction<? super K, ? super V, ? extends V> remappingFunction) {
        throw new UnsupportedOperationException();
    }

    @Override
    public V compute(K key,
                     BiFunction<? super K, ? super V, ? extends V> remappingFunction) {
        throw new UnsupportedOperationException();
    }

    @Override
    public V merge(K key, V value,
                   BiFunction<? super V, ? super V, ? extends V> remappingFunction) {
        throw new UnsupportedOperationException();
    }
}
```

## 4. Refer Links

[Java 常用工具类 Collections 源码分析](http://blog.csdn.net/u011240877/article/details/78348578)