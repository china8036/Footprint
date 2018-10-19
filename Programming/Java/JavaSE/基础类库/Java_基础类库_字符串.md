- [Java 基础类库：字符串](#java-基础类库字符串)
    - [1. CharSequence 接口](#1-charsequence-接口)
    - [2. String 实现类](#2-string-实现类)
        - [2.1. 常用 API](#21-常用-api)
        - [2.2. 源码分析](#22-源码分析)
            - [2.2.1. 类定义](#221-类定义)
            - [2.2.2. 内部属性](#222-内部属性)
            - [2.2.3. hashCode](#223-hashcode)
            - [2.2.4. charAt](#224-charat)
            - [2.2.5. compareTo](#225-compareto)
            - [2.2.6. indexOf](#226-indexof)
            - [2.2.7. concat](#227-concat)
    - [3. AbstractStringBuilder 抽象类](#3-abstractstringbuilder-抽象类)
    - [4. StringBuffer 实现类](#4-stringbuffer-实现类)
        - [4.1. 常用 API](#41-常用-api)
        - [4.2. 源码分析](#42-源码分析)
    - [5. StringBuilder 实现类](#5-stringbuilder-实现类)
        - [5.1. 常用 API](#51-常用-api)
        - [5.2. 源码分析](#52-源码分析)
            - [5.2.1. 类定义](#521-类定义)
            - [5.2.2. 构造方法](#522-构造方法)
            - [5.2.3. append](#523-append)
            - [5.2.4. toString](#524-tostring)
    - [6. 区别](#6-区别)
    - [7. Refer Links](#7-refer-links)

# Java 基础类库：字符串

## 1. CharSequence 接口

CharSequence 接口包含了`charAt()`、`length()`、`subSequence()`、`toString()`方法，java.lang 包下的 String 类、StringBuffer 类和 StringBuilder 类都实现了该接口。

```java
public interface CharSequence {
    int length();

    char charAt(int index);

    CharSequence subSequence(int start, int end);

    public String toString();
}
```

## 2. String 实现类

java.lang.[String](https://docs.oracle.com/javase/9/docs/api/java/lang/String.html) 类用于创建和操作字符串，是 Java 中最常用的基础类之一。

### 2.1. 常用 API

- 构造器
  - `String​()`: Initializes a newly created String object so that it represents an empty character sequence.
  - `String​(byte[] bytes)`: Constructs a new String by decoding the specified array of bytes using the platform's default charset.
  - `String​(byte[] bytes, int offset, int length)`: Constructs a new String by decoding the specified subarray of bytes using the platform's default charset.
  - `String​(byte[] bytes, int offset, int length, String charsetName)`: Constructs a new String by decoding the specified subarray of bytes using the specified charset.
  - `String​(byte[] bytes, int offset, int length, Charset charset)`: Constructs a new String by decoding the specified subarray of bytes using the specified charset.
  - `String​(byte[] bytes, String charsetName)`: Constructs a new String by decoding the specified array of bytes using the specified charset.
  - `String​(byte[] bytes, Charset charset)`: Constructs a new String by decoding the specified array of bytes using the specified charset.
  - `String​(char[] value)`: Allocates a new String so that it represents the sequence of characters currently contained in the character array argument.
  - `String​(char[] value, int offset, int count)`: Allocates a new String that contains characters from a subarray of the character array argument.
  - `String​(int[] codePoints, int offset, int count)`: Allocates a new String that contains characters from a subarray of the Unicode code point array argument.
  - `String​(String original)`: Initializes a newly created String object so that it represents the same sequence of characters as the argument; in other words, the newly created string is a copy of the argument string.
  - `String​(StringBuffer buffer)`: Allocates a new string that contains the sequence of characters currently contained in the string buffer argument.
  - `String​(StringBuilder builder)`: Allocates a new string that contains the sequence of characters currently contained in the string builder argument.
- 字符串长度
   - `int length​()`: 
- 连接字符串
  - `String concat​(String str)`: Concatenates the specified string to the end of this string.
  - 直接使用 `+` 操作符
- 格式化字符串
  - `static String format​(String format, Object... args)`: Returns a formatted string using the specified format string and arguments.
  - `static String format​(Locale l, String format, Object... args)`: Returns a formatted string using the specified locale, format string, and arguments.
  
  eg:
  ```java
  System.out.printf("浮点型变量的值为 " +
                    "%f, 整型变量的值为 " +
                    "%d, 字符串变量的值为 " +
                    "%s", floatVar, intVar, stringVar);
  // equals to
  String fs = String.format("浮点型变量的值为 " +
                   "%f, 整型变量的值为 " +
                   "%d, 字符串变量的值为 " +
                   "%s", floatVar, intVar, stringVar);
  ```
- `public native String intern()`: 如果池中已经包含一个与该 String 相同的字符串字面量，则返回该字符串字面量；否则，将此 String 对象添加到常量池中，并返回此字面量的引用。
  
  eg:
  ```java
  String str1 = "hello";                // 字面量只会在常量池中创建对象
  String str2 = str1.intern();
  System.out.println(str1==str2);       // true
  
  String str3 = new String("world");    // new 关键字只会在堆中创建对象
  String str4 = str3.intern();
  System.out.println(str3 == str4);     // false
  
  String str5 = str1 + str2;            // 变量拼接的字符串，会在常量池中和堆中都创建对象
  String str6 = str5.intern();          // 这里由于池中已经有对象了，直接返回的是对象本身，也就是堆中的对象
  System.out.println(str5 == str6);     // true
  
  String str7 = "hello1" + "world1";    // 常量拼接的字符串，只会在常量池中创建对象
  String str8 = str7.intern();
  System.out.println(str7 == str8);     // true
  ```

NOTE:
- String 类是不可改变的，所以你一旦创建了 String 对象，那它的值就无法改变了。
- 从 JDK 7 开始，在 switch 语句中可以直接使用 String 作为 case 条件。

### 2.2. 源码分析

#### 2.2.1. 类定义

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {/* ... */}
```

String 类是一个用 final 声明的常量类，不能被任何类所继承，且一旦 String 对象被创建，包含在这个对象中的字符序列是不可改变的。

#### 2.2.2. 内部属性

```java
/** 用来存储字符串，可见一个 String 字符串实际上是一个 char 数组  */
private final char value[];

/** 缓存字符串的哈希码 */
private int hash; // Default to 0

/** 实现序列化的标识 */
private static final long serialVersionUID = -6849794470754667710L;
```

#### 2.2.3. hashCode

```java
public int hashCode() {
    int h = hash;
    if (h == 0 && value.length > 0) {
        char val[] = value;

        for (int i = 0; i < value.length; i++) {
            h = 31 * h + val[i];
        }
        hash = h;
    }
    return h;
}
```

String 类的 hashCode 算法计算公式如下：
```
s[0]*31^(n-1) + s[1]*31^(n-2) + ... + s[n-1]
```

为什么选择 31 作为乘积因子，而且没有用一个常量来声明？
- 31 是一个不大不小的质数，是作为 hashCode 乘子的优选质数之一。
- 31 可以被 JVM 优化，如 `31 * i = (i << 5) - i`，其中移位运算比乘法更快更省性能。

#### 2.2.4. charAt

```java
public char charAt(int index) {
    // 如果传入的索引大于字符串的长度或者小于 0，直接抛出索引越界异常
    if ((index < 0) || (index >= value.length)) {
        throw new StringIndexOutOfBoundsException(index);
    }
    return value[index];// 返回指定索引的单个字符
}
```

#### 2.2.5. compareTo

```java
public int compareTo(String anotherString) {
    int len1 = value.length;
    int len2 = anotherString.value.length;
    int lim = Math.min(len1, len2);
    char v1[] = value;
    char v2[] = anotherString.value;

    int k = 0;
    while (k < lim) {
        char c1 = v1[k];
        char c2 = v2[k];
        if (c1 != c2) {
            return c1 - c2;
        }
        k++;
    }
    return len1 - len2;
}
```

> If the argument string is equal to this string; a value less than {@code 0} if this string is lexicographically less than the string argument; and a value greater than {@code 0} if this string is lexicographically greater than the string argument.

该方法按字母顺序比较两个字符串：
- 当两个字符串某个位置的字符不同时，返回的是这一位置的字符 Unicode 值之差。
- 当两个字符串所有位置都相同时，返回两个字符串长度之差。

除此之外，String 类还提供了 compareToIgnoreCase() 方法，它在 compareTo 方法的基础上忽略了大小写进行比较。

#### 2.2.6. indexOf

```java
public int indexOf(int ch) {
    return indexOf(ch, 0);
}
public int indexOf(int ch, int fromIndex) {
    final int max = value.length;//max 等于字符的长度
    if (fromIndex < 0) {// 指定索引的位置如果小于 0，默认从 0 开始搜索
        fromIndex = 0;
    } else if (fromIndex >= max) {
        // 如果指定索引值大于等于字符的长度（因为是数组，下标最多只能是 max-1），直接返回 -1
        return -1;
    }

    if (ch < Character.MIN_SUPPLEMENTARY_CODE_POINT) {// 一个 char 占用两个字节，如果 ch 小于 2 的 16 次方（65536），绝大多数字符都在此范围内
      final char[] value = this.value;
      for (int i = fromIndex; i < max; i++) {//for 循环依次判断字符串每个字符是否和指定字符相等
        if (value[i] == ch) {
               return i;// 存在相等的字符，返回第一次出现该字符的索引位置，并终止循环
         }
       }
       return -1;// 不存在相等的字符，则返回 -1
    } else {// 当字符大于 65536 时，处理的少数情况，该方法会首先判断是否是有效字符，然后依次进行比较
       return indexOfSupplementary(ch, fromIndex);
    }
}
public int indexOf(String str) {
    return indexOf(str, 0);
}
public int indexOf(String str, int fromIndex) {
    return indexOf(value, 0, value.length,
            str.value, 0, str.value.length, fromIndex);
}

/* 静态方法，供 String 和 StringBuffer 类内部调用 */
static int indexOf(char[] source, int sourceOffset, int sourceCount,
        String target, int fromIndex) {
    return indexOf(source, sourceOffset, sourceCount,
                    target.value, 0, target.value.length,
                    fromIndex);
}

/* 使用暴力匹配方法进行子串搜索，时间复杂度为 O(mn) */
static int indexOf(char[] source, int sourceOffset, int sourceCount,
        char[] target, int targetOffset, int targetCount,
        int fromIndex) {
    if (fromIndex >= sourceCount) {
        return (targetCount == 0 ? sourceCount : -1);
    }
    if (fromIndex < 0) {
        fromIndex = 0;
    }
    if (targetCount == 0) {
        return fromIndex;
    }

    char first = target[targetOffset];
    int max = sourceOffset + (sourceCount - targetCount);

    for (int i = sourceOffset + fromIndex; i <= max; i++) {
        /* Look for first character. */
        if (source[i] != first) {
            while (++i <= max && source[i] != first);
        }

        /* Found first character, now look at the rest of v2 */
        if (i <= max) {
            int j = i + 1;
            int end = j + targetCount - 1;
            for (int k = targetOffset + 1; j < end && source[j]
                    == target[k]; j++, k++);

            if (j == end) {
                /* Found whole string. */
                return i - sourceOffset;
            }
        }
    }
    return -1;
}
```
String.indexOf(String str) 方法内部所使用的算法其实就是字符串匹配的暴力朴素算法，因此在字符串长度很长时，匹配效率较低。

#### 2.2.7. concat

```java
public String concat(String str) {
    int otherLen = str.length();
    if (otherLen == 0) {
        return this;
    }
    int len = value.length;
    char buf[] = Arrays.copyOf(value, len + otherLen);
    str.getChars(buf, len);
    return new String(buf, true);
}
```

NOTE: 返回值是 `new String(buf, true)`，也就是重新通过 new 关键字创建了一个新的字符串，原字符串是不变的。一旦一个 String 对象被创建，包含在这个对象中的字符序列是不可改变的。

## 3. AbstractStringBuilder 抽象类

AbstractStringBuilder 抽象类实现了 CharSequence 接口，是 StringBuffer 和 StringBuilder 的父类。由于 AbstractStringBuilder 已经实现了大部分需要的方法，因此，StringBuilder 和 StringBuffer 只需要封装调用即可。
```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * The value is used for character storage.
     */
    char[] value;

    /**
     * The count is the number of characters used.
     */
    int count;

    /**
     * This no-arg constructor is necessary for serialization of subclasses.
     */
    AbstractStringBuilder() {
    }

    /**
     * Creates an AbstractStringBuilder of the specified capacity.
     */
    AbstractStringBuilder(int capacity) {
        value = new char[capacity];
    }

    /* ... */
}
```
**AbstractStringBuilder 内部用一个 `char[]` 数组保存字符串，可以在构造的时候指定初始容量方法。在调用 toString() 方法时，AbstractStringBuilder 才会将 char 数组转换成 String 返回，因此解决 String 类中字符串字面值不可变的问题**。

- 扩容
  ```java
  public void ensureCapacity(int minimumCapacity) {
      if (minimumCapacity > 0)
          ensureCapacityInternal(minimumCapacity);
  }
  private void ensureCapacityInternal(int minimumCapacity) {
      // overflow-conscious code
      if (minimumCapacity - value.length > 0)
          expandCapacity(minimumCapacity);
  }
  void expandCapacity(int minimumCapacity) {
      int newCapacity = value.length * 2 + 2;
      if (newCapacity - minimumCapacity < 0)
          newCapacity = minimumCapacity;
      if (newCapacity < 0) {
          if (minimumCapacity < 0) // overflow
              throw new OutOfMemoryError();
          newCapacity = Integer.MAX_VALUE;
      }
      value = Arrays.copyOf(value, newCapacity);
  }
  ```
  扩容的方法最终是由 `expandCapacity()` 实现的，在这个方法中首先把容量扩大为原来的容量加 2，如果此时仍小于指定的容量，那么就把新的容量设为 minimumCapacity。然后判断是否溢出，如果溢出了，把容量设为 `Integer.MAX_VALUE`。最后把 value 值进行拷贝，这显然是耗时操作。

- append
  ```java
  public AbstractStringBuilder append(String str) {
      if (str == null)
          return appendNull();
      int len = str.length();
      ensureCapacityInternal(count + len);
      str.getChars(0, len, value, count);
      count += len;
      return this;
  }
  ```
  append() 是最常用的方法，它有很多形式的重载。上面是其中一种，用于追加字符串。如果 str 是 null, 则会调用 appendNull() 方法。这个方法其实是追加了'n'、'u'、'l'、'l'这几个字符。如果不是 null，则首先扩容，然后调用 String 的 getChars() 方法将 str 追加到 value 末尾。最后返回对象本身，所以 append() 可以连续调用。

## 4. StringBuffer 实现类

> A thread-safe, mutable sequence of characters.
> 
> A string buffer is like a {@link String}, but can be modified. At any point in time it contains some particular sequence of characters, but the length and content of the sequence can be changed through certain method calls.

java.lang.[StringBuffer](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuffer.html) 类从 JDK 1.0 就存在，是一个**线程安全的、可变的**字符串操作类。

### 4.1. 常用 API

TODO:

### 4.2. 源码分析

```java
 public final class StringBuffer
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence {/* ... */}
```

为实现同步，StringBuffer 中很多方法使用 Synchronized 修饰：
```java
public synchronized int length() {
        return count;
}
public synchronized StringBuffer append(String str) {
    toStringCache = null;
    super.append(str);
    return this;
}
public synchronized void setLength(int newLength) {
    toStringCache = null;
    super.setLength(newLength);
}
```

## 5. StringBuilder 实现类

> A mutable sequence of characters.  This class provides an API compatible with {@code StringBuffer}, but with no guarantee of synchronization. This class is designed for use as a drop-in replacement for {@code StringBuffer} in places where the string buffer was being used by a single thread (as is generally the case).   Where possible, it is recommended that this class be used in preference to {@code StringBuffer} as it will be faster under most implementations.
 
JDK 1.0 引入的 StringBuffer 虽然保证了线程安全，但由于同步也使得其各个操作的效率都比较低。因此，在 JDK 1.5 中引入了 java.lang.[StringBuilder](https://docs.oracle.com/javase/9/docs/api/java/lang/StringBuidler.html) 类，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的，不能同步访问，但其操作速度更快。

**由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类**，然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。

### 5.1. 常用 API

TODO:

### 5.2. 源码分析

#### 5.2.1. 类定义

```java
public final class StringBuilder
    extends AbstractStringBuilder
    implements java.io.Serializable, CharSequence
{/* ... */}
```

#### 5.2.2. 构造方法

StringBuilder 默认的容量大小为 16。
```java
public StringBuilder() {
    super(16);
}
public StringBuilder(int capacity) {
    super(capacity);
}
public StringBuilder(String str) {
    super(str.length() + 16);
    append(str);
}
public StringBuilder(CharSequence seq) {
    this(seq.length() + 16);
    append(seq);
}
```

#### 5.2.3. append

append() 的重载方法很多，但都是直接调用的父类 AbstractStringBuilder 中的方法。

```java
public StringBuilder append(String str) {
    super.append(str);
    return this;
}
public StringBuilder append(CharSequence s) {
    super.append(s);
    return this;
}
// ...
```

#### 5.2.4. toString

```java
public String toString() {
    // Create a copy, don't share the array
    return new String(value, 0, count);
}
```

toString() 方法返回了一个新的 String 对象，与原来的对象不共享内存。

## 6. 区别

- StringBuilder 和 StringBuffer 都是可变字符串，前者线程不安全，后者线程安全。
- StringBuilder 和 StringBuffer 的大部分方法均调用父类 AbstractStringBuilder 的实现。其扩容机制首先是把容量变为原来容量的 2 倍加 2。最大容量是 Integer.MAX_VALUE，也就是 0x7fffffff。
- StringBuilder 和 StringBuffer 的默认容量都是 16，最好预先估计好字符串的大小避免扩容带来的时间消耗。

## 7. Refer Links

[Runoob: Java String 类](http://www.runoob.com/java/java-string.html)

[Java StringBuilder 和 StringBuffer 源码分析](https://segmentfault.com/a/1190000004261063)