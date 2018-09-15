- [Java JCF Arrays 工具类](#java-jcf-arrays- 工具类)
    - [1. 基本概念](#1- 基本概念)
    - [2. 数组拷贝 asList()](#2- 数组拷贝 -aslist)
        - [2.1. 基本概念](#21- 基本概念)
        - [2.2. 源码分析](#22- 源码分析)
    - [3. 排序 sort()](#3- 排序 -sort)
        - [3.1. 基本概念](#31- 基本概念)
        - [3.2. 源码分析](#32- 源码分析)
    - [4. 查找 binarySearch()](#4- 查找 -binarysearch)
        - [4.1. 基本概念](#41- 基本概念)
        - [4.2. 源码分析](#42- 源码分析)
    - [5. 元素填充 fill()](#5- 元素填充 -fill)
    - [6. 复制 copyOf()](#6- 复制 -copyof)
        - [6.1. 基本概念](#61- 基本概念)
        - [6.2. 源码分析](#62- 源码分析)
    - [7. 元素交换 swap()](#7- 元素交换 -swap)
    - [8. Refer Links](#8-refer-links)

# Java JCF Arrays 工具类

## 1. 基本概念

[Arrays](https://docs.oracle.com/javase/9/docs/api/java/util/Arrays.html) 是 Java 在 java.util 包中提供的一个操作数组的工具类，其内部定义了一些常见的用于操作数组的静态方法。

## 2. 数组拷贝 asList()

### 2.1. 基本概念

Arrays.asList() 方法通过拷贝的方式，返回一个固定长度的内部类 ArrayList。

方法接口：

`static <T> List<T>	asList​(T... a)`: Returns a fixed-size list backed by the specified array.

使用示例：
```java
// 用法 1
String[] a = new String[]{"5", "6", "7", "8"};
List<String> datas1 = Arrays.asList(a);

// 用法 2
List<String> datas2 = Arrays.asList("5", "6", "7", "8");

// 用法 3
int a = 1;
int b = 2;
int c = 3;
List<Integer> datas3 = Arrays.asList(a, b, c);

// 错误用法，会报错：incompatible types: inference variable T has incompatible bounds
// 改为：Integer [] a = {1, 2, 3, 4}; 即可
int [] a = {1, 2, 3, 4};
List<Integer> datas4 = Arrays.asList(a);
```

常用操作：
```java
// 将数组转化为 ArrayList
Integer [] nums =  {2,3,5,6,3,5,67,87,43};
List<Integer> a = new ArrayList<>(Arrays.asList(nums));
```

### 2.2. 源码分析

```java
@SafeVarargs
@SuppressWarnings("varargs")
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}

// Arrays.ArrayList 内部类，不同于 java.util.ArrayList，Arrays.ArrayList 并没有实现 List 接口，而是直接继承了 AbstractList，它是一个固定长度的 List 集合
private static class ArrayList<E> extends AbstractList<E>
        implements RandomAccess, java.io.Serializable
{
    private static final long serialVersionUID = -2764017481108945198L;
    private final E[] a;

    // 数组拷贝，把传递进来的数组赋给数组 a
    ArrayList(E[] array) {
        a = Objects.requireNonNull(array);
    }

    @Override
    public int size() {
        return a.length;
    }

    @Override
    public Object[] toArray() {
        return a.clone();
    }

    @Override
    @SuppressWarnings("unchecked")
    public <T> T[] toArray(T[] a) {
        int size = size();
        if (a.length < size)
            return Arrays.copyOf(this.a, size,
                                  (Class<? extends T[]>) a.getClass());
        System.arraycopy(this.a, 0, a, 0, size);
        if (a.length > size)
            a[size] = null;
        return a;
    }

    @Override
    public E get(int index) {
        return a[index];
    }

    @Override
    public E set(int index, E element) {
        E oldValue = a[index];
        a[index] = element;
        return oldValue;
    }

    @Override
    public int indexOf(Object o) {
        E[] a = this.a;
        if (o == null) {
            for (int i = 0; i < a.length; i++)
                if (a[i] == null)
                    return i;
        } else {
            for (int i = 0; i < a.length; i++)
                if (o.equals(a[i]))
                    return i;
        }
        return -1;
    }

    @Override
    public boolean contains(Object o) {
        return indexOf(o) != -1;
    }

    @Override
    public Spliterator<E> spliterator() {
        return Spliterators.spliterator(a, Spliterator.ORDERED);
    }

    @Override
    public void forEach(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        for (E e : a) {
            action.accept(e);
        }
    }

    @Override
    public void replaceAll(UnaryOperator<E> operator) {
        Objects.requireNonNull(operator);
        E[] a = this.a;
        for (int i = 0; i < a.length; i++) {
            a[i] = operator.apply(a[i]);
        }
    }

    @Override
    public void sort(Comparator<? super E> c) {
        Arrays.sort(a, c);
    }
}

```

从源码中可知：
- asList 方法的构造器是接受一个泛型的变长参数的，而基本数据类型是无法被泛型化的，因此该方法不接受基本数据类型的数组名，但可将基本数据类型的变量作为参数直接传递。

- 内部类 ArrayList 并没有 add 方法的具体实现，也就是说 AbstractList 内部的 add 方法并没有被覆盖，而 AbstractList 内部的 add 方法直接抛出了异常：
  ```java
  public void add(int index, E element) {
      throw new UnsupportedOperationException();
  }
  ```
  因此，不能对这个返回的 List 执行 add 方法，但可以调用 set 方法为元素赋值。

  同理，不能调用 remove 方法，但可以调用 get 方法获取元素。

## 3. 排序 sort()

### 3.1. 基本概念

Arrays.sort() 方法实现数组的排序，默认按升序进行排列。根据其可接受的参数类型，大致可分为两类：一类是基本数据类型的排序，一类是 Object 类型的排序。其中，每一类的排序都支持数组整体的排序和在数组内指定一段位置范围进行排序。

JDK8 为 Arrays 工具类增加了多线程并行排序的 parallelSort() 系列方法，与 sort 方法不同的是，他采用多线程并行的方式进行排序，当数据规模较大时和 sort 相比有明显优势。parallelSort() 系列方法可接受的参数类型与 sort() 方法基本相同。

方法接口：
- `static void sort​(xxx[] a)`: Sorts the specified array into ascending numerical order.
- `static void sort​(xxx[] a, int fromIndex, int toIndex)`: Sorts the specified range of the array into ascending order.

  其中，xxx 为基本数据类型 byte/char/double/float/int/long/short。

- `static void sort​(Object[] a)`: Sorts the specified array of objects into ascending order, according to the natural ordering of its elements.
- `static void sort​(Object[] a, int fromIndex, int toIndex)`: Sorts the specified range of the specified array of objects into ascending order, according to the natural ordering of its elements.

- `static <T> void sort​(T[] a, int fromIndex, int toIndex, Comparator<? super T> c)`: Sorts the specified range of the specified array of objects according to the order induced by the specified comparator.
- `static <T> void sort​(T[] a, Comparator<? super T> c)`: Sorts the specified array of objects according to the order induced by the specified comparator.

- `static void parallelSort​(char[] a)`: Sorts the specified array into ascending numerical order.
- `static void parallelSort​(char[] a, int fromIndex, int toIndex)`: Sorts the specified range of the array into ascending numerical order.
- `static <T extends Comparable<? super T>> void parallelSort​(T[] a)`:Sorts the specified array of objects into ascending order, according to the natural ordering of its elements.
- `static <T extends Comparable<? super T>> void parallelSort​(T[] a, int fromIndex, int toIndex)`: Sorts the specified range of the specified array of objects into ascending order, according to the natural ordering of its elements.
- `static <T> void parallelSort​(T[] a, int fromIndex, int toIndex, Comparator<? super T> cmp)`: Sorts the specified range of the specified array of objects according to the order induced by the specified comparator.
- `static <T> void parallelSort​(T[] a, Comparator<? super T> cmp)`: Sorts the specified array of objects according to the order induced by the specified comparator.

### 3.2. 源码分析

Arrays.sort() 主要使用了 2 种排序方法：快速排序、优化的归并排序（TimSort 排序）。其中，**快速排序主要用于对基本类型数据（int,short,long 等）数组进行排序，而归并排序和 TimSort 排序用于对对象类型数组进行排序**。两者时间复杂度都是 O(nlogn)，但是归并排序的需要额外的 n 个引用的空间。

> TimSort 排序算法是一种起源于归并排序和插入排序的混合排序算法，由 Tim Peters 于 2002 年在 Python 语言中提出。TimSort 的思想是先对待排序列进行分区，然后再对分区进行合并。TimSort 排序看起来和归并排序很相似，但是其中有一些针对反向和大规模数据的优化处理，是归并排序的优化版本。

**使用不同类型的排序算法主要是由于快速排序是不稳定的，而归并排序是稳定的**。
- 对于基本数据类型，稳定性没有意义。
- 对于对象类型，稳定性是比较重要的。因为对象相等的判断可能只是判断关键属性，最好保持相等对象的非关键属性的顺序与排序前一致。

**另外一个原因是由于归并排序相对而言比较次数比快速排序少，移动（对象引用的移动）次数比快速排序多，而对于对象来说，比较一般比移动更为耗时**。

- 基本数据类型的排序

  以下以 int 类型为例，long，short，char，byte，float，double 类型数组的排序同理：
  ```java
  // 对数组排序
  public static void sort(int[] a) {
      // 直接调用双基准快排
      DualPivotQuicksort.sort(a, 0, a.length - 1, null, 0, 0);
  }

  // 对数组从指定位置排序 
  public static void sort(int[] a, int fromIndex, int toIndex) {
      // 检测参数是否越界
      rangeCheck(a.length, fromIndex, toIndex);
      // 调用双基准快排
      DualPivotQuicksort.sort(a, fromIndex, toIndex - 1, null, 0, 0);
  }
  ```

- Object 类型数组排序

  ```java
  public static void sort(Object[] a) {
      if (LegacyMergeSort.userRequested)
          // 调用传统的归并排序
          legacyMergeSort(a);
      else
          // 调用优化的归并排序，即 TimSort 排序
          ComparableTimSort.sort(a, 0, a.length, null, 0, 0);
  }

  /**
    * Old merge sort implementation can be selected (for
    * compatibility with broken comparators) using a system property.
    * Cannot be a static boolean in the enclosing class due to
    * circular dependencies. To be removed in a future release.
    */
  static final class LegacyMergeSort {
      private static final boolean userRequested =
          java.security.AccessController.doPrivileged(
              new sun.security.action.GetBooleanAction(
                  "java.util.Arrays.useLegacyMergeSort")).booleanValue();
  }

  /** To be removed in a future release. */
  private static void legacyMergeSort(Object[] a) {
      Object[] aux = a.clone();
      mergeSort(aux, a, 0, a.length, 0);
  }
  ```
  ```java
  public static void sort(Object[] a, int fromIndex, int toIndex) {
      rangeCheck(a.length, fromIndex, toIndex);
      if (LegacyMergeSort.userRequested)
          legacyMergeSort(a, fromIndex, toIndex);
      else
          ComparableTimSort.sort(a, fromIndex, toIndex, null, 0, 0);
  }

  /** To be removed in a future release. */
  private static void legacyMergeSort(Object[] a,
                                      int fromIndex, int toIndex) {
      Object[] aux = copyOfRange(a, fromIndex, toIndex);
      mergeSort(aux, a, fromIndex, toIndex, -fromIndex);
  }
  ```
  ```java
  public static <T> void sort(T[] a, Comparator<? super T> c) {
      if (c == null) {
          sort(a);
      } else {
          if (LegacyMergeSort.userRequested)
              legacyMergeSort(a, c);
          else
              TimSort.sort(a, 0, a.length, c, null, 0, 0);
      }
  }

  /** To be removed in a future release. */
  private static <T> void legacyMergeSort(T[] a, Comparator<? super T> c) {
      T[] aux = a.clone();
      if (c==null)
          mergeSort(aux, a, 0, a.length, 0);
      else
          mergeSort(aux, a, 0, a.length, 0, c);
  }
  ```
  ```java
  public static <T> void sort(T[] a, int fromIndex, int toIndex,
                              Comparator<? super T> c) {
      if (c == null) {
          sort(a, fromIndex, toIndex);
      } else {
          rangeCheck(a.length, fromIndex, toIndex);
          if (LegacyMergeSort.userRequested)
              legacyMergeSort(a, fromIndex, toIndex, c);
          else
              TimSort.sort(a, fromIndex, toIndex, c, null, 0, 0);
      }
  }

  /** To be removed in a future release. */
  private static <T> void legacyMergeSort(T[] a, int fromIndex, int toIndex,
                                          Comparator<? super T> c) {
      T[] aux = copyOfRange(a, fromIndex, toIndex);
      if (c==null)
          mergeSort(aux, a, fromIndex, toIndex, -fromIndex);
      else
          mergeSort(aux, a, fromIndex, toIndex, -fromIndex, c);
  }
  ```

- 多线程排序
  ```java
  // 进行多线程排序的数组最小长度 2^13
  public static final int MIN_ARRAY_SORT_GRAN = 1 << 13;

	// 使用 fork-Join 框架，充分利用多核，对大的集合进行切分然后再归并排序
  public static void parallelSort(int[] a) {
      int n = a.length, p, g;
      if (n <= MIN_ARRAY_SORT_GRAN ||
          (p = ForkJoinPool.getCommonPoolParallelism()) == 1)
          DualPivotQuicksort.sort(a, 0, n - 1, null, 0, 0);
      else
          new ArraysParallelSortHelpers.FJInt.Sorter
              (null, a, new int[n], 0, n, 0,
                ((g = n / (p << 2)) <= MIN_ARRAY_SORT_GRAN) ?
                MIN_ARRAY_SORT_GRAN : g).invoke();
  }
  ```

## 4. 查找 binarySearch()

### 4.1. 基本概念

Arrays 工具类对查找的实现，主要支持二分查找 binarySearch()，其方法 API 与排序类似，分为基本数据类型数组的查找和对象数据类型的查找。

- `static int	binarySearch​(xxx[] a, xxx key)`: Searches the specified array of bytes for the specified value using the binary search algorithm.
- `static int	binarySearch​(xxx[] a, int fromIndex, int toIndex, xxx key)`: Searches a range of the specified array of bytes for the specified value using the binary search algorithm.
- `static int	binarySearch​(Object[] a, int fromIndex, int toIndex, Object key)`: Searches a range of the specified array for the specified object using the binary search algorithm.
- `static int	binarySearch​(Object[] a, Object key)`: Searches the specified array for the specified object using the binary search algorithm.
- `static <T> int	binarySearch​(T[] a, int fromIndex, int toIndex, T key, Comparator<? super - T> c)`: Searches a range of the specified array for the specified object using the binary search algorithm.
- `static <T> int	binarySearch​(T[] a, T key, Comparator<? super T> c)`: Searches the specified array for the specified object using the binary search algorithm.

### 4.2. 源码分析

```java
public static int binarySearch(int[] a, int key) {
    return binarySearch0(a, 0, a.length, key);
}
// 可以在指定范围排序
public static int binarySearch(int[] a, int fromIndex, int toIndex,
                                int key) {
    rangeCheck(a.length, fromIndex, toIndex);
    return binarySearch0(a, fromIndex, toIndex, key);
}
//Object[] 数组中的 Object 必须实现了 Comparable 接口
public static int binarySearch(Object[] a, int fromIndex, int toIndex,
                                Object key) {
    rangeCheck(a.length, fromIndex, toIndex);
    return binarySearch0(a, fromIndex, toIndex, key);
}
// 提供 Comparator 实例
public static <T> int binarySearch(T[] a, T key, Comparator<? super T> c) {
    return binarySearch0(a, 0, a.length, key, c);
}
```

## 5. 元素填充 fill()

使用指定值填充数组：
- `static void fill​(boolean[] a, boolean val)`: Assigns the specified boolean value to each element of the specified array of booleans.
- `static void fill​(boolean[] a, int fromIndex, int toIndex, boolean val)`: Assigns the specified boolean value to each element of the specified range of the specified array of booleans.

## 6. 复制 copyOf()

### 6.1. 基本概念

`static boolean[]	copyOf​(xxx[] original, int newLength)`: 将原数组拷贝到一个长度为 newLength 的新数组中，然后返回该数组。该方法适用于数组的扩容或缩容，在 java.util.ArrayList 类的扩容机制中被广泛使用。

参数说明：
- original - 要复制的数组
- newLength - 要返回的副本的长度
- newType - 要返回的副本的类型

### 6.2. 源码分析

```java
public static <T,U> T[] copyOf(U[] original, int newLength, Class<? extends T[]> newType) {
    @SuppressWarnings("unchecked")
    // 根据 class 的类型来决定是 new 还是反射去构造一个泛型数组
    T[] copy = ((Object)newType == (Object)Object[].class)
        ? (T[]) new Object[newLength]
        : (T[]) Array.newInstance(newType.getComponentType(), newLength);
    // 高效地批量复制元素至新数组中
    System.arraycopy(original, 0, copy, 0,
                      Math.min(original.length, newLength));
    return copy;
}
```

其中，`System.arraycopy()` 方法是一个 System 的 native 函数，该方法高效地批量复制元素至新数组中，由于是对内存直接进行复制，减少了 for 循环过程中的寻址时间，因此效率很高，没有返回值：
```java
public static native void arraycopy(Object src,  int  srcPos,
                                    Object dest, int destPos,
                                    int length);
```
参数说明：
- src - 源数组
- srcPos - 源数组中的起始位置
- dest - 目标数组
- destPos - 目标数据中的起始位置
- length - 要复制的数组元素的数量

## 7. 元素交换 swap()

`static void swap(Object[] x, int a, int b)`：用于数组内指定位置的元素交换。

```java
/**
  * Swaps x[a] with x[b].
  */
private static void swap(Object[] x, int a, int b) {
    Object t = x[a];
    x[a] = x[b];
    x[b] = t;
}
```

## 8. Refer Links

[Java 中的 Arrays 类使用详解](http://blog.csdn.net/liu_yanzhao/article/details/70847050)

[Java 工具类 Arrays 中不得不知的常用方法](https://www.jianshu.com/p/4ed75403edff)

[java 中 Arrays.sort() 实现原理](https://my.oschina.net/liujiest/blog/671292)