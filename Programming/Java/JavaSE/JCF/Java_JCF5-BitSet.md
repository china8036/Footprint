- [Java JCF BitSet](#java-jcf-bitset)
  - [1. 基本概念](#1-基本概念)
  - [2. 常用 API](#2-常用-api)
  - [3. 源码分析](#3-源码分析)
  - [4. Refer Links](#4-refer-links)

# Java JCF BitSet

## 1. 基本概念

[BitSet](https://docs.oracle.com/javase/9/docs/api/java/util/BitSet.html) 类位于 java.util 中，从 JDK1.0 开始引入，但在多个 jdk 的演变中，bitset 也不断演变。

BitSet 会创建一种特殊类型的数组来保存位值，且数组大小会随需要增加。

Bitmap 的基本操作有：
- 初始化一个 bitset，指定大小
- 清空 bitset
- 反转某一指定位
- 设置某一指定位
- 获取某一位的状态
- 当前 bitset 的 bit 总位数

## 2. 常用 API

- 构造器
  - `BitSet​()`: Creates a new bit set.
  - `BitSet​(int nbits)`: Creates a bit set whose initial size is large enough to explicitly represent bits with indices in the range 0 through nbits-1.
- 置位
  - `void set​(int bitIndex)`: Sets the bit at the specified index to true.
  - `void set​(int bitIndex, boolean value)`: Sets the bit at the specified index to the specified value.
  - `void set​(int fromIndex, int toIndex)`: Sets the bits from the specified fromIndex (inclusive) to the specified toIndex (exclusive) to true.
  - `void set​(int fromIndex, int toIndex, boolean value)`: Sets the bits from the specified fromIndex (inclusive) to the specified toIndex (exclusive) to the specified value.
- 清零
  - `void clear​()`: Sets all of the bits in this BitSet to false.
  - `void clear​(int bitIndex)`: Sets the bit specified by the index to false.
  - `void clear​(int fromIndex, int toIndex)`: Sets the bits from the specified fromIndex (inclusive) to the specified toIndex (exclusive) to false.
- 反转
  - `void flip​(int bitIndex)`: Sets the bit at the specified index to the complement of its current value.
  - `void flip​(int fromIndex, int toIndex)`: Sets each bit from the specified fromIndex (inclusive) to the specified toIndex (exclusive) to the complement of its current value.
- 取值
  - `boolean get​(int bitIndex)`: Returns the value of the bit with the specified index.
  - `BitSet get​(int fromIndex, int toIndex)`: Returns a new BitSet composed of bits from this BitSet from fromIndex (inclusive) to toIndex (exclusive).
- 位运算
  - `void and​(BitSet set)`: Performs a logical AND of this target bit set with the argument bit set.
  - `void or​(BitSet set)`: Performs a logical OR of this bit set with the bit set argument.
  - `void xor​(BitSet set)`: Performs a logical XOR of this bit set with the bit set argument.

eg:
```java
public static void main(String args[]) {
    BitSet bits1 = new BitSet(16);
    BitSet bits2 = new BitSet(16);
    
    // set some bits
    for(int i=0; i<16; i++) {
      if((i%2) == 0) bits1.set(i);
      if((i%5) != 0) bits2.set(i);
    }
    System.out.println("Initial pattern in bits1: ");
    System.out.println(bits1);
    System.out.println("\nInitial pattern in bits2: ");
    System.out.println(bits2);

    // AND bits
    bits2.and(bits1);
    System.out.println("\nbits2 AND bits1: ");
    System.out.println(bits2);

    // OR bits
    bits2.or(bits1);
    System.out.println("\nbits2 OR bits1: ");
    System.out.println(bits2);

    // XOR bits
    bits2.xor(bits1);
    System.out.println("\nbits2 XOR bits1: ");
    System.out.println(bits2);
}
```
运行结果：
```
Initial pattern in bits1:
{0, 2, 4, 6, 8, 10, 12, 14}

Initial pattern in bits2:
{1, 2, 3, 4, 6, 7, 8, 9, 11, 12, 13, 14}

bits2 AND bits1:
{2, 4, 6, 8, 12, 14}

bits2 OR bits1:
{0, 2, 4, 6, 8, 10, 12, 14}

bits2 XOR bits1:
{}
```

## 3. 源码分析

```java
public class BitSet implements Cloneable, java.io.Serializable {

    // ...

    /**
     * The internal field corresponding to the serialField "bits".
     */
    private long[] words;

    // ...
}
```

从源码可知，BitSet 使用 long 数组来作为内部存储结构，因此 Bitset 至少为一个 long 的大小。

## 4. Refer Links

TODO:

[BitSet 数据结构以及 jdk 中实现源码分析](https://blog.csdn.net/cpfeed/article/details/7342480)

[Java BitSet 源码解析 (1)](https://blog.csdn.net/u012005313/article/details/78621918)

[Java BitSet 源码解析 (2)](https://blog.csdn.net/u012005313/article/details/78621952)

[Java BitSet 源码解析 (3)](https://blog.csdn.net/u012005313/article/details/78621970)

[Java BitSet 源码解析 (4)](https://blog.csdn.net/u012005313/article/details/78622048)