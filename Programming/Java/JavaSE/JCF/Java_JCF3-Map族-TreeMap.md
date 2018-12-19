- [Java 集合：Map 族 - TreeMap 实现类](#java- 集合 map- 族 ---treemap- 实现类)
    - [1. SortedMap 接口](#1-sortedmap- 接口)
    - [2. NavigableMap 接口](#2-navigablemap- 接口)
    - [3. TreeMap 实现类](#3-treemap- 实现类)
        - [3.1. 基本概念](#31- 基本概念)
        - [3.2. 自然排序 & 定制排序](#32- 自然排序 -- 定制排序)
        - [3.3. 常用 API](#33- 常用 -api)
        - [3.4. 源码分析](#34- 源码分析)
            - [3.4.1. 类定义](#341- 类定义)
            - [3.4.2. 查找](#342- 查找)
            - [3.4.3. 遍历](#343- 遍历)
            - [3.4.4. 插入元素](#344- 插入元素)
            - [3.4.5. 删除元素](#345- 删除元素)
    - [5. Refer Links](#5-refer-links)

# Java 集合：Map 族 - TreeMap 实现类

## 1. SortedMap 接口

SortedMap 接口继承了 Map 接口，它提供了一些基于有序键的操作：
```java
/**
 * 返回包含 Key 值在 [fromKey, toKey) 范围内的 Map
 */
SortedMap<K,V> subMap(K fromKey, K toKey);

/**
 * 返回包含 Key 值在 (-∞, toKey) 范围内的 Map
 */
SortedMap<K,V> headMap(K toKey);();

/**
 * 返回包含 Key 值在 [fromKey, +∞) 范围内的 Map
 */
SortedMap<K,V> tailMap(K fromKey);

/**
 * 返回 Key 值最小的 K-V 的 Key 值
 */
K firstKey();

/**
 * 返回 Key 值最大的 K-V 的 Key 值
 */
K lastKey();

// ......
```

NOTE: **所有的有序的操作的顺序，都是基于 Key 值的排序，而不是 Value 值的排序**。若希望按 Value 值排序，可通过以下方法：
```java
int [] nums = new int[]{4, 1, 4, 4, 5, 5, 5, 5, 8, 2, 2, 3, 4, 1, 2};
Map<Integer, Integer> map = new TreeMap<>();
for (int i = 0; i < nums.length; i++)
	map.put(nums[i], map.getOrDefault(nums[i], 0) + 1);

System.out.println(map.toString()); // 默认按照 Key 值排序

List<Map.Entry<Integer, Integer>> entries = new ArrayList<>(map.entrySet());
Collections.sort(entries, (e1, e2) -> {
	return e1.getValue().compareTo(e2.getValue());	// 升序
	// return -e1.getValue().compareTo(e2.getValue()); 	// 降序
});
System.out.println(entries);				// 按照 Value 值排序
```

## 2. NavigableMap 接口

NavigableMap 接口继承了 SortedMap 接口，它在 SortedMap 接口的基础上声明了一些列具有导航功能的方法，通过这些导航方法，我们可以快速定位到目标的 key 或 Entry：
```java
/**
 * 返回红黑树中最小 Key 值所对应的 Entry
 */
Map.Entry<K,V> firstEntry();

/**
 * 返回红黑树中最大 Key 值所对应的 Entry
 */
Map.Entry<K,V> lastEntry();

/**
 * 返回最大的键 maxKey 所对应的键值对，且 maxKey 仅小于参数 key
 */
Map.Entry<K,V> lowerEntry(K key);

/**
 * 返回最小的键 minKey 所对应的键值对，且 minKey 仅大于参数 key
 */
Map.Entry<K,V> higherEntry(K key);

/**
 * 返回最大的键 maxKey，且 maxKey 仅小于参数 key
 */
K lowerKey(K key);

/**
 * 返回最小的键 minKey，且 minKey 仅大于参数 key
 */
K higherKey(K key);

// ......
```

## 3. TreeMap 实现类

### 3.1. 基本概念

[TreeMap](https://docs.oracle.com/javase/9/docs/api/java/util/TreeMap.html) 最早出现在 JDK 1.2 中，是 Java 集合框架中比较重要一个的实现。TreeMap 底层基于红黑树实现，可保证在 log(n) 时间复杂度内完成 containsKey、get、put 和 remove 操作，效率很高。另一方面，由于 TreeMap 基于红黑树实现，这为 TreeMap 保持键的有序性打下了基础，它用键的整体顺序对元素进行排序，并将其组织成搜索树。

若不需要排序，应选择 HashMap，避免用于排序的开销。

TreeMap 中的元素必须是可比较的，即必须实现了 Comparable 接口，或者在构造集时提供一个 Comparator，否则会抛出一个异常。

### 3.2. 自然排序 & 定制排序

TreeMap 支持两种排序方法：自然排序和定制排序。默认情况下，TreeMap 采用自然排序。

- 自然排序

	TreeMap 的自然排序会调用集合元素的 compareTo() 方法来比较元素之间的大小关系，然后将集合元素按升序排列。

- 定制排序

	TreeMap 的定制排序需要在创建 TreeMap 对象时，提供一个 Comparator 对象与该 TreeMap 对象相关联，由该 Comparator 对象负责集合元素的排序逻辑。由于 Comparator 是一个函数式接口，因此可以使用 lambda 表达式来代替 Comparator 对象。

### 3.3. 常用 API

构造器
- `TreeMap()`
- `TreeMap(Comparator<? super K> comparator)`
- `TreeMap(Map<? extends K,? extends V> m)	`
- `TreeMap(SortedMap<K,? extends V> m)`

常用 API

与 HashMap 相比，TreeMap 还提供了以下方法：
- `Comparator<? super K> comparator​()`: 如果 TreeMap 采用定制排序，则此方法返回对键的比较器。如果键使用自然排序，即通过 Comparable 接口的 compareTo 方法进行比较，则此方法返回 null。

- `K firstKey​()`: 返回映射中的最小元素。

- `K lastKey​()`: 返回映射中的最大元素。

- `Map.Entry<K,V> firstEntry​()`: 返回映射中

- `Map.Entry<K,V> lastEntry​()`: 返回映射中

### 3.4. 源码分析

JDK 1.8 中的 TreeMap 源码有三千多行。

#### 3.4.1. 类定义

TreeMap 继承自 AbstractMap，并实现了 NavigableMap 接口。

```java
public class TreeMap<K,V>
    extends AbstractMap<K,V>
    implements NavigableMap<K,V>, Cloneable, java.io.Serializable
```

#### 3.4.2. 查找

TreeMap 基于红黑树实现，而红黑树是一种自平衡二叉查找树，所以 TreeMap 的查找操作流程和二叉查找树一致。只不过在 TreeMap 中，节点（Entry）存储的是键值对<k,v>。在查找过程中，比较的是键的大小，返回的是值，如果没找到，则返回 null。TreeMap 中的查找方法是 get，具体实现在 getEntry 方法中，相关源码如下：

```java
public V get(Object key) {
    Entry<K,V> p = getEntry(key);
    return (p==null ? null : p.value);
}

final Entry<K,V> getEntry(Object key) {
    // Offload comparator-based version for sake of performance
    if (comparator != null)
        return getEntryUsingComparator(key);
    if (key == null)
        throw new NullPointerException();
    @SuppressWarnings("unchecked")
        Comparable<? super K> k = (Comparable<? super K>) key;
    Entry<K,V> p = root;

    // 查找操作的核心逻辑就在这个 while 循环里
    while (p != null) {
        int cmp = k.compareTo(p.key);
        if (cmp < 0)
            p = p.left;
        else if (cmp > 0)
            p = p.right;
        else
            return p;
    }
    return null;
}
```

#### 3.4.3. 遍历

TreeMap 有一个特性，即可以保证键的有序性，默认是正序。所以在遍历过程中，大家会发现 TreeMap 会从小到大输出键的值。那么，接下来就来分析一下 keySet 方法，以及在遍历 keySet 方法产生的集合时，TreeMap 是如何保证键的有序性的。相关代码如下：
```java
public Set<K> keySet() {
    return navigableKeySet();
}

public NavigableSet<K> navigableKeySet() {
    KeySet<K> nks = navigableKeySet;
    return (nks != null) ? nks : (navigableKeySet = new KeySet<>(this));
}

static final class KeySet<E> extends AbstractSet<E> implements NavigableSet<E> {
    private final NavigableMap<E, ?> m;
    KeySet(NavigableMap<E,?> map) { m = map; }

    public Iterator<E> iterator() {
        if (m instanceof TreeMap)
            return ((TreeMap<E,?>)m).keyIterator();
        else
            return ((TreeMap.NavigableSubMap<E,?>)m).keyIterator();
    }

    // 省略非关键代码
}

Iterator<K> keyIterator() {
    return new KeyIterator(getFirstEntry());
}

final class KeyIterator extends PrivateEntryIterator<K> {
    KeyIterator(Entry<K,V> first) {
        super(first);
    }
    public K next() {
        return nextEntry().key;
    }
}

abstract class PrivateEntryIterator<T> implements Iterator<T> {
    Entry<K,V> next;
    Entry<K,V> lastReturned;
    int expectedModCount;

    PrivateEntryIterator(Entry<K,V> first) {
        expectedModCount = modCount;
        lastReturned = null;
        next = first;
    }

    public final boolean hasNext() {
        return next != null;
    }

    final Entry<K,V> nextEntry() {
        Entry<K,V> e = next;
        if (e == null)
            throw new NoSuchElementException();
        if (modCount != expectedModCount)
            throw new ConcurrentModificationException();
        // 寻找节点 e 的后继节点
        next = successor(e);
        lastReturned = e;
        return e;
    }

    // 其他方法省略
}
```
从上面源码可以看出 keySet 方法返回的是 KeySet 类的对象。这个类实现了 Iterable 接口，可以返回一个迭代器。该迭代器的具体实现是 KeyIterator，而 KeyIterator 类的核心逻辑是在 PrivateEntryIterator 中实现的。上面的代码虽多，但核心代码还是 KeySet 类和 PrivateEntryIterator 类的 nextEntry 方法。KeySet 类就是一个集合，这里不分析了。而 nextEntry 方法比较重要，下面简单分析一下。

在初始化 KeyIterator 时，会将 TreeMap 中包含最小键的 Entry 传给 PrivateEntryIterator。当调用 nextEntry 方法时，通过调用 successor 方法找到当前 entry 的后继，并让 next 指向后继，最后返回当前的 entry。通过这种方式即可实现按正序返回键值的的逻辑。

#### 3.4.4. 插入元素

```java
public V put(K key, V value) {
    Entry<K,V> t = root;
    // 1. 如果根节点为 null，将新节点设为根节点
    if (t == null) {
        compare(key, key);
        root = new Entry<>(key, value, null);
        size = 1;
        modCount++;
        return null;
    }
    int cmp;
    Entry<K,V> parent;
    // split comparator and comparable paths
    Comparator<? super K> cpr = comparator;
    if (cpr != null) {
        // 2. 为 key 在红黑树找到合适的位置
        do {
            parent = t;
            cmp = cpr.compare(key, t.key);
            if (cmp < 0)
                t = t.left;
            else if (cmp > 0)
                t = t.right;
            else
                return t.setValue(value);
        } while (t != null);
    } else {
        // 与上面代码逻辑类似，省略
    }
    Entry<K,V> e = new Entry<>(key, value, parent);
    // 3. 将新节点链入红黑树中
    if (cmp < 0)
        parent.left = e;
    else
        parent.right = e;
    // 4. 插入新节点可能会破坏红黑树性质，这里修正一下
    fixAfterInsertion(e);
    size++;
    modCount++;
    return null;
}
```

插入后的平衡调整 fixAfterInsertion() 方法：

![image](http://img.cdn.firejq.com/jpg/2018/3/18/0998db93ad58a364f46d49edaee22db0.jpg)

#### 3.4.5. 删除元素

删除操作是红黑树最复杂的部分，原因是该操作可能会破坏红黑树性质 5（从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点），修复性质 5 要比修复其他性质（性质 2 和 4 需修复，性质 1 和 3 不用修复）复杂的多。

```java
public V remove(Object key) {
    Entry<K,V> p = getEntry(key);
    if (p == null)
        return null;

    V oldValue = p.value;
    deleteEntry(p);
    return oldValue;
}

private void deleteEntry(Entry<K,V> p) {
    modCount++;
    size--;

    /*
     * 1. 如果 p 有两个孩子节点，则找到后继节点，
     * 并把后继节点的值复制到节点 P 中，并让 p 指向其后继节点
     */
    if (p.left != null && p.right != null) {
        Entry<K,V> s = successor(p);
        p.key = s.key;
        p.value = s.value;
        p = s;
    } // p has 2 children

    // Start fixup at replacement node, if it exists.
    Entry<K,V> replacement = (p.left != null ? p.left : p.right);

    if (replacement != null) {
        /*
         * 2. 将 replacement parent 引用指向新的父节点，
         * 同时让新的父节点指向 replacement。
         */
        replacement.parent = p.parent;
        if (p.parent == null)
            root = replacement;
        else if (p == p.parent.left)
            p.parent.left  = replacement;
        else
            p.parent.right = replacement;

        // Null out links so they are OK to use by fixAfterDeletion.
        p.left = p.right = p.parent = null;

        // 3. 如果删除的节点 p 是黑色节点，则需要进行调整
        if (p.color == BLACK)
            fixAfterDeletion(replacement);
    } else if (p.parent == null) { // 删除的是根节点，且树中当前只有一个节点
        root = null;
    } else { // 删除的节点没有孩子节点
        // p 是黑色，则需要进行调整
        if (p.color == BLACK)
            fixAfterDeletion(p);

        // 将 P 从树中移除
        if (p.parent != null) {
            if (p == p.parent.left)
                p.parent.left = null;
            else if (p == p.parent.right)
                p.parent.right = null;
            p.parent = null;
        }
    }
}
```

删除后的平衡调整 fixAfterDeletion () 方法：

![image](http://img.cdn.firejq.com/jpg/2018/3/18/dea8f2e55cb857ed5b589931aff029ec.jpg)

## 5. Refer Links

[TreeMap 源码分析](http://www.coolblog.xyz/2018/01/11/TreeMap%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90/)
