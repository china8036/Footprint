- [位运算](#位运算)
  - [1. 基本概念](#1-基本概念)
  - [2. 位运算符](#2-位运算符)
  - [3. 常用技巧](#3-常用技巧)
  - [4. 常见运用](#4-常见运用)
  - [5. 实际问题](#5-实际问题)
    - [5.1. 签到系统](#51-签到系统)
    - [5.2. 权限控制](#52-权限控制)
    - [5.3. 找出配对序列中缺少的数](#53-找出配对序列中缺少的数)
    - [5.4. 找出连续序列中缺少的数](#54-找出连续序列中缺少的数)
    - [5.5. 确定需要几位可以将整数 A 转成整数 B](#55-确定需要几位可以将整数-a-转成整数-b)
  - [6. Refer Links](#6-refer-links)

# 位运算

## 1. 基本概念

在计算机中所有数据都是以二进制的形式储存的。**位运算实质上就是直接对整数在内存中的二进制位进行操作，因此处理数据的速度非常快**。

使用位运算，主要目的是节约内存，使程序运行速度更快，还有就是在对内存要求苛刻的地方使用。

**位运算是优化代码的实用技巧，因此编写程序需要熟悉位运算及其常见技巧，同时也需要熟悉位运算的手工运算**。

## 2. 位运算符

常见的位运算符包含以下几种（Java & C++ 共有）：
- `&`：按位与。

  e.g. 假设 A = 0011 1100，B = 0000 1101，则 A & B 得到 12，即 0000 1100.

- `|`：按位或。

  e.g. A | B 得到 61，即 0011 1101.

- `^`：按位异或。

  e.g. A ^ B 得到 49，即 0011 0001.

- `~`：按位非。

  e.g. ~A 得到 -61，即 1100 0011.

- `<<`：按位左移运算符。左操作数按位左移右操作数指定的位数。

  e.g. A << 2 得到 240，即 1111 0000.

- `>>`：按位右移运算符。左操作数按位右移右操作数指定的位数。

  e.g. A >> 2 得到 15 即 1111.

NOTE:
- 在位运算操作符中，只有`~`取反是单目操作符，其它都是双目操作符。
- **位操作只能用于整形数据，对 float 和 double 类型进行位操作会报错**。
- 位操作符的运算优先级比较低，因此应尽量使用括号来确保运算顺序，否则很可能会得到莫明其妙的结果。
- 类型算术运算符，位操作还有一些复合操作符，如 &=、|=、 ^=、<<=、>>=。

## 3. 常用技巧

进行位操作时，有以下基本原理（1s 表示全 1 串，0s 表示全 0 串）：
- `x ^ 0s = x`
- `x ^ 1s = ~x`
- `x ^ x = 0`
- `x & 0s = 0`
- `x & 1s = x`
- `x & x = x`
- `x | 0s = x`
- `x | 1s = 1s`
- `x | x = x`

其中，对于异或 (XOR) 运算，具有以下性质：
- 异或运算满足交换律和结合律。若 a XOR b = c，则 a XOR c = b、b XOR c = a。
- 若有 `1, 2, 3, ...n` 连续 n 个数的序列，若对该序列进行连续的 XOR 运算，即 `1 XOR 2 XOR ... XOR n`，则具有以下规律：
  - 若 `n % 4 == 0`，则结果为 n。
  - 若 `n % 4 == 1`，则结果为 1。
  - 若 `n % 4 == 2`，则结果为 n+1。
  - 若 `n % 4 == 3`，则结果为 0。

位运算应用口诀：
> 清零取反要用与，某位置一可用或
> 若要取反和交换，轻轻松松用异或

常见技巧：
- 获取指定位的值
  ```java
  boolean getBit(int num, int i) {
      return (num & (1 << i)) != 0;
  }
  ```

- 将指定位置 1
  ```java
  int setBit(int num, int i) {
      return num | (1 << i);
  }
  ```

- 将指定位置 0
  ```java
  int clearBit(int num, int i) {
      return num & ~(1 << i);
  }
  ```

- 将 num 最高位至 i 位（包含 i 位）置 0
  ```java
  int clearBitsMSBthroughtI(int num, int i) {
      return num & ((1 << i) - 1);
  }
  ```

- 将 num 第 i 位至 0 位（包含 0 位）置 0
  ```java
  int clearBitsIthrought0(int num, int i) {
      return num & ~((1 << (i + 1)) - 1);
  }
  ```

- 将指定位置为指定值
  ```java
  int updateBit(int num, int i, int v) {
      return (num & ~(1 << i)) | (v << i);
  }
  ```

## 4. 常见运用

- 判断 int 型变量 a 是奇数还是偶数
  ```
  a & 1 == 0 偶数
  a & 1 == 1 奇数
  ```

- 求 int 整数的平均值

  对于两个整数 x 和 y，如果用 `(x + y) / 2` 求平均值，由于 x+y 可能会大于 Integer.MAX_VALUE 而产生溢出。但若知道它们的平均值是肯定不会溢出的，可以采用 `(x & y) + ((x ^ y) >> 1)` 来求得平均值。

- 不用临时变量交换两个整数
  ```java
  void swap(int x , int y) {
      x ^= y;
      y ^= x;
      x ^= y;
  }
  ```
  除此之外，还有以下方法：
  ```java
  void swap(int x, int y) {
    x = x - y;
    y = x + y;
    x = y - x;
  }
  ```

- 计算绝对值
  ```java
  int abs(int x) {
      return (x ^ (x >> 31)) - (x >> 31);
  }
  ```

- 用位运算表示取模运算 （在不产生溢出的情况下)
  ```
  a % (2^n) 
  ```
  等价于 
  ```
  a & (2^n - 1)
  ```
  例：a % 2 == a & 1

- 用位运算表示乘法运算 （在不产生溢出的情况下)
  ```
  a * (2^n)
  ```
  等价于 
  ```
  a << n
  ```

- 用位运算表示除法运算 （在不产生溢出的情况下)
  ```
  a / (2^n)
  ```
  等价于 
  ```
  a >> n
  ```
  例：12 / 8 == 12 >> 3

- 用位运算表示相反数
  ```
  ~n + 1 == -n // 由于计算机中使用补码表示法表示正负数，因此要取相反数，先求反再加 1 即可
  ```

- 按 8 对齐（当 n 不为 8 的倍数时，需要将 n 调整为大于 n 的最小的 8 的倍数）
  ```
  int align8(int n) {
    if (n & 0x7 == 0) // 相当于 n % 8，但效率更高
      return n;
    return (n >> 3 + 1) << 3;
  }
  ```

- 用位运算判断 n 是否为 2 的某次方或 n 为 0
  ```
  n & (n - 1) == 0
  ```
  原理：**n 中低位的 0 在 n-1 中变为 1，n 中最低有效位的 1 在 n-1 中变为 0**。即：
  ```
  if        n = abcde1000
  then  n - 1 = abcde0111
  if        n = abcde0001 
  then  n - 1 = abcde0000
  ```
  因此，要使得 `n & (n - 1) == 0`，n 必须像 `00001000`，因此 n 必须是 2 的某次方或者 n 为 0。

## 5. 实际问题

### 5.1. 签到系统

```java
public static int[] days = new int[]{1, 2, 3, 4, 5, 6, 7, 8, 9};

public static void main(String[] args) {
    int state = 0;
    for (int day : days) {
        boolean flag = (state >> day & 1) == 1;
        System.err.println("第" + day + "天的签到状态：" + flag);
    }
    System.err.println("============");
    
    // 将 0 的二进制位从低位一直向高位填 1
    state |= 1 << days[0]; // 第 1 天签到，将第 1 个位置 1
    state |= 1 << days[1]; // 第 2 天签到，将第 2 个位置 1
    state |= 1 << days[2]; // 第 3 天签到，将第 3 个位置 1
    state |= 1 << days[3]; // 第 4 天签到，将第 4 个位置 1
    state |= 1 << days[6]; // 第 7 天签到，将第 7 个位置 1
    for (int day : days) {
        boolean flag = (state >>> day & 1) == 1;
        System.err.println("第" + day + "天的签到状态：" + flag);
    }
}
```

### 5.2. 权限控制

在一个系统中，用户一般有查询 (Select)、新增 (Insert)、修改 (Update)、删除 (Delete) 四种权限，四种权限有多种组合方式，也就是有 16 中不同的权限状态（2 的 4 次方）。

**使用位掩码的方式，只需要用一个 0 <= n < 16 的整数 n 就可以表示所有的 16 种权限的状态**。

实现：
```java
public class Permission {
    // 是否允许查询，二进制第 1 位，0 表示否，1 表示是
    public static final int ALLOW_SELECT = 1 << 0; // 0001
    
    // 是否允许新增，二进制第 2 位，0 表示否，1 表示是
    public static final int ALLOW_INSERT = 1 << 1; // 0010
    
    // 是否允许修改，二进制第 3 位，0 表示否，1 表示是
    public static final int ALLOW_UPDATE = 1 << 2; // 0100
    
    // 是否允许删除，二进制第 4 位，0 表示否，1 表示是
    public static final int ALLOW_DELETE = 1 << 3; // 1000
    
    // 存储目前的权限状态，可表示所有的 16 种权限的状态
    private int flag;
    
    // 重新设置权限
    public void setPermission(int permission) {
        flag = permission;
    }

    // 添加一项或多项权限
    public void enable(int permission) {
        flag |= permission;
    }
    
    // 删除一项或多项权限
    public void disable(int permission) {
        flag &= ~permission;
    }
    
    // 是否拥某些权限
    public boolean isAllow(int permission) {
        return (flag & permission) == permission;
    }
    
    // 是否禁用了某些权限
    public boolean isNotAllow(int permission) {
        return (flag & permission) == 0;
    }
    
    // 是否仅仅拥有某些权限
    public boolean isOnlyAllow(int permission) {
        return flag == permission;
    }
}
```

Usage:
```java
// 添加一项 Update 权限：
permission.enable(NewPermission.ALLOW_UPDATE);
// 添加 Insert、Update、Delete 三项权限：
permission.enable(NewPermission.ALLOW_INSERT 
  | NewPermission.ALLOW_UPDATE | NewPermission.ALLOW_DELETE);
```

### 5.3. 找出配对序列中缺少的数

一个数组里有 n 个数，其中每个数都出现了 2 次，只有一个数出现了 1 次，如 `1 2 1 4 3 4 2`。怎么使用 O(n) 的时间效率、O(1) 的空间效率找出这个只出现 1 次的数？

解决思路：**使用异或**。从编号序列的第 1 个开始，将第 1 个与第 2 个进行异或，再将其结果与第 3 个进行异或，以此类推，直到序列最后一个。
- 若所有数字都能两两配对，则最终的异或结果必定是 0。
- 若其中缺少了某个数，则最终的异或结果就是这个数。

举例论证：对于序列 `1 1 2 2 3 3`，最终异或的结果为 0。由于**异或运算满足交换律和结合律**，因此将序列元素的顺序打乱后（如 `1 2 1 3 3 2`），其异或结果依旧为 0。而如果其中缺少了某个配对的元素，结果必然为 1。

类似：现有多对筷子，配对的筷子有相同的编号，如：`1 1 2 2 3 3 4 4`。若现有 n 对筷子，其中可能有一对筷子少了一只，即编号形如：`1 3 4 2 2 1 4`。要求：使用 O(n) 的时间效率、O(1) 的空间效率，确定哪一对筷子缺少了一只。

延伸：若给定的数组是有序的，如 `1 1 2 2 3 4 4`，同样的问题要求，能如何优化？

解决思路：使用**二分查找**的思想。首先看序列中间的两个数（偶数索引和上一个配对，奇数索引和下一个配对）是否相同：
- 如果相同：说明左边的序列都是两两配对的，因此只需继续对序列右边的子序列递归即可。
- 如果不同：说明左边的序列中无法两两配对，即缺少的数在左边的序列中，因此只需继续对序列左边的子序列递归即可。

### 5.4. 找出连续序列中缺少的数

问题 1：有一个数组存有 **1-1000000000**的连续 n 个乱序的整数，其中缺失了一个整数（**缺失的用 0 代替**），怎样快速找出缺失的数？

问题 2：有一个数组存有 **1-99999999999**的连续 n 个乱序的整数，其中缺失了一个整数（**缺失的用 0 代替**），怎样快速找出缺失的数？

- 方法一：求和

  使用等差数列求和公式求出从 1 加到 n 的和，即 s = n*(n+1)/2，然后遍历磁盘中的整数序列，在遍历中用 sum 逐个减去，最后得到的值就是缺失的数。

  整体时间复杂度为求和过程的 O(n) ，空间复杂度为 O(1)。

- 方法二：异或

  首先确定 2 个规律：
  - 对于序列 0-99、0-199、0-299、0-1999、0-2999... 逐个异或的结果为 0。
  - 对于序列 0-100、0-200、0-300、0-1000、0-2000... 逐个异或的结果为 1。

  因此，当序列中缺少某个数时：
  - **对于第一种序列，那么全部异或的结果正好就是那个数**。

    假如有四个数：0001,0010,0101,0110 排列成一个 matrix.
    ```
    bits:     1     2    3      4
              0     0    0      1
              0     0    1      0
              0     1    0      1
              0     1    1      0
    全部异或： 0     0    0      0
    ```
    可知：**要全部异或的结果为 0，那么所有 bit 位上的 1 的个数必须为偶数**。反过来说：如果其中有一个数不存在了（比如 0001），那么少 0 的的 bit 位上的值不变（因为 1 的个数还是偶数），而少 1 的 bit 位上的值就变成了 1（1 的个数为奇数了）。因此，全部异或的结果正好就是缺少的那个数。

  - **对于第二种序列，那么全部异或的结果再与最后一个数异或后，就是那个数**。

    根据异或运算的性质，由于 1000000000 % 4 = 0，因此 EXC1 = 1 XOR 2 ... XOR 1000000000 = 1000000000。

    现对目标整数序列进行依次 XOR，即 EXC2 = 1 XOR 2 XOR ... XOR 0 XOR ... XOR 1000000000，其中缺失的数用 0 代替，由于 x XOR 0 = x，因此不影响整体结果。 

    设缺失的数是 M，则有 EXC2 XOR M = EXC1，进而有 M = EXC2 XOR EXC1 = EXC2 XOR 1000000000。

  时间复杂度也是 O(n)，具体在实现的时候也可以借用上面的方法一边从文件读入数据一边异或运算，则空间复杂度也可以是 O(1)。

  ```cpp
  /* 查找 1-1000000 中缺失的数 -- 异或运算法 */
  int FindLostNumber(int *Data, int N) {
    int Exc1, Exc2 = 0, M;	
    switch (N % 4) {
      case 0: Exc1 = N; break;
      case 1: Exc1 = 1; break;
      case 2: Exc1 = N + 1; break;
      case 3: Exc1 = 0; break;
    }
    for (int i = 0; i < N; ++i) {
      Exc2 ^= Data[i];
    }
    return Exc2 ^ Exc1;
  }
  ```

- 方法三：位图

  首先需要 10 亿 bit 位，约 120MB，并将其全部初始化为 0。遍历一边原数组，以整数的值作为索引将对应 bit 位置为 1。遍历位图的 1-10 亿位，如果 bit 位为 0 则输出对应位置即为缺失的数。

  该方法的时间复杂度也是 O(n)，不过空间复杂度为 O(n)。但是其适用面更广，可以找出不止一个缺失的数，并且也相当于将原数组排了序，所以有时候在对空间要求比较高的排序过程中也可以使用。

### 5.5. 确定需要几位可以将整数 A 转成整数 B

问题：编写程序确定需要改变几个位，才能将整数 A 转成整数 B？

要解决这个问题，只要找出 2 个数之间有哪些位不相同即可，因此可以用异或 (XOR) 操作，然后统计异或结果中有多少个 1 即可：
```java
public int bitSwapRequired(int a, int b) {
  int cnt = 0;
  for (int i = a ^ b; i != 0; i >>= 1)
    cnt += i & 1;
  return cnt;
}
```

在对异或结果进行遍历统计时，可以**不断翻转最低有效位，计算要多少次 i 才会变成 0，这个次数就是 1 的个数**，从而减少循环遍历的次数，达到优化的效果：
```java
public int bitSwapRequired(int a, int b) {
  int cnt = 0;
  for (int i = a ^ b; i != 0; i &= (i - 1))
    cnt++;
  return cnt;
}
```
`n&=(n-1)` 可以用于翻转最低有效位，如 `n=01000100`，`n-1=01000011`，执行 1 次 `n&=(n-1)` 后 `n=01000000`，执行 2 次 `n&=(n-1)` 后 `n=00000000`。这也是一个常见的位操作技巧。

## 6. Refer Links

[位运算有什么奇技淫巧？](https://www.zhihu.com/question/38206659) 

[在写代码的过程中使用位运算的好处？](https://www.zhihu.com/question/34021773)

[位运算总结 取模 取余](https://blog.csdn.net/yasin_lee/article/details/5700384)

[移位运算实现加减乘除详解以及 java 源码实现](https://blog.csdn.net/tingting256/article/details/52550188)

[Java 位运算在程序设计中的使用：位掩码（BitMask）](http://xxgblog.com/2013/09/15/java-bitmask/)

[九章算法：干货！史上最强位运算面试题大总结！](https://mp.weixin.qq.com/s?__biz=MzA5MzE4MjgyMw==&mid=2649456688&idx=1&sn=467f105e3306a47b5b4768d12d7f6fe7&chksm=887eee38bf09672ec7725ac7edac6f8e3801a1c1cfcb072c16dda99b36d21bcdc9cb08fef8c7&mpshare=1&scene=1&srcid=0317fQV1z43SxmGxHAuVKZWv&key=32ff7e6b073562f9d3e5c6a75b00a1e44ef11b1f30bb189e6e6cac5dbe218b1c0cbfff9d9766ee1fafb419fff7e5dfcc9cf3b892a88a76935043aa6861379c6d572ad08119cbcf3a19bc6da910f843a7&ascene=0&uin=MTUyMzg3NjAwMA%3D%3D&devicetype=iMac+MacBookAir7%2C1+OSX+OSX+10.12.3+build(16D32)&version=12020010&nettype=WIFI&fontScale=100&pass_ticket=0AiIToHJN8yqpuqRAsA5PaaQMJr8KtvlnZ2EqkX0zx%2BEZweRvHKyF%2ByjmycpUbVn)

[快速求出 10 亿整数中缺失的数](https://blog.csdn.net/jxzh1023/article/details/53823001)

TODO:

[【腾讯】连续数打乱判断出少了哪些数？](http://hxraid.iteye.com/blog/618153)