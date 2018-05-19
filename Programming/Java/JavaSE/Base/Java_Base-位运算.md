- [Java 基础：位运算](#java)
  - [1. 基本概念](#1)
  - [2. 位运算符](#2)
  - [3. 基本原理](#3)
  - [4. 基本技巧](#4)
  - [5. 常见运用](#5)
  - [6. 其它应用](#6)
  - [7. Refer Links](#7-refer-links)

# Java 基础：位运算

## 1. 基本概念

在计算机中所有数据都是以二进制的形式储存的。位运算实质上就是直接对整数在内存中的二进制位进行操作，因此处理数据的速度非常快。

使用位运算，主要目的是节约内存，使程序运行速度更快，还有就是在对内存要求苛刻的地方使用。

位运算是优化代码的实用技巧，因此编写程序需要熟悉位运算及其常见技巧，同时也需要熟悉位运算的手工运算。

## 2. 位运算符

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

- `>>>`：按位右移补零操作符（无符号右移）。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。

  e.g. A>>>2 得到 15 即 0000 1111.

NOTE:
- 在位运算操作符中，只有`~`取反是单目操作符，其它都是双目操作符。
- 位操作只能用于整形数据，对 float 和 double 类型进行位操作会报错。
- 位操作符的运算优先级比较低，因此应尽量使用括号来确保运算顺序，否则很可能会得到莫明其妙的结果。
- 类型算术运算符，位操作还有一些复合操作符，如 &=、|=、 ^=、<<=、>>=。

## 3. 基本原理

进行位操作时，有以下基本原理（1s 表示全 1 串，0s 表示全 0 串）：
- x ^ 0s = x
- x ^ 1s = ~x
- x ^ x = 0
- x & 0s = 0
- x & 1s = x
- x & x = x
- x | 0s = x
- x | 1s = 1s
- x | x = x

## 4. 基本技巧

位运算应用口诀
> 清零取反要用与，某位置一可用或
> 若要取反和交换，轻轻松松用异或

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

## 5. 常见运用

- 判断 int 型变量 a 是奇数还是偶数
  ```
  a & 1 == 0 偶数
  a & 1 == 1 奇数
  ```

- 求 int 整数的平均值

  对于两个整数 x 和 y，如果用 `(x + y) / 2` 求平均值，由于 x+y 可能会大于 Integer.MAX_VALUE，因此可能会产生溢出。但若知道它们的平均值是肯定不会溢出的，可以采用 `(x & y) + ((x ^ y) >> 1)` 来求得平均值。

- 不用 temp 交换两个 int 整数
  ```java
  void swap(int x , int y) {
      x ^= y;
      y ^= x;
      x ^= y;
  }
  ```

- 计算绝对值
  ```java
  int abs(int x) {
      return (x ^ (x >> 31)) - (x >> 31) ;
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

## 6. 其它应用

- 签到系统
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
      state |= 1 << days[0];// 第 1 天
      state |= 1 << days[1];// 第 2 天
      state |= 1 << days[2];// 第 3 天
      state |= 1 << days[3];// 第 4 天
      state |= 1 << days[6];// 第 7 天
      for (int day : days) {
          boolean flag = (state >>> day & 1) == 1;
          System.err.println("第" + day + "天的签到状态：" + flag);
      }
  }
  ```

- 权限控制

  在一个系统中，用户一般有查询 (Select)、新增 (Insert)、修改 (Update)、删除 (Delete) 四种权限，四种权限有多种组合方式，也就是有 16 中不同的权限状态（2 的 4 次方）。使用位掩码的方式，只需要用一个大于或等于 0 且小于 16 的整数即可表示所有的 16 种权限的状态。
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
  usage
  ```java
  // 添加一项 Update 权限：
  permission.enable(NewPermission.ALLOW_UPDATE);
  // 添加 Insert、Update、Delete 三项权限：
  permission.enable(NewPermission.ALLOW_INSERT 
    | NewPermission.ALLOW_UPDATE | NewPermission.ALLOW_DELETE);

  ```

## 7. Refer Links

[位运算有什么奇技淫巧？](https://www.zhihu.com/question/38206659) 

[在写代码的过程中使用位运算的好处？](https://www.zhihu.com/question/34021773)

[位运算总结 取模 取余](https://blog.csdn.net/yasin_lee/article/details/5700384)

[移位运算实现加减乘除详解以及 java 源码实现](https://blog.csdn.net/tingting256/article/details/52550188)

[Java 位运算在程序设计中的使用：位掩码（BitMask）](http://xxgblog.com/2013/09/15/java-bitmask/)

[九章算法：干货！史上最强位运算面试题大总结！](https://mp.weixin.qq.com/s?__biz=MzA5MzE4MjgyMw==&mid=2649456688&idx=1&sn=467f105e3306a47b5b4768d12d7f6fe7&chksm=887eee38bf09672ec7725ac7edac6f8e3801a1c1cfcb072c16dda99b36d21bcdc9cb08fef8c7&mpshare=1&scene=1&srcid=0317fQV1z43SxmGxHAuVKZWv&key=32ff7e6b073562f9d3e5c6a75b00a1e44ef11b1f30bb189e6e6cac5dbe218b1c0cbfff9d9766ee1fafb419fff7e5dfcc9cf3b892a88a76935043aa6861379c6d572ad08119cbcf3a19bc6da910f843a7&ascene=0&uin=MTUyMzg3NjAwMA%3D%3D&devicetype=iMac+MacBookAir7%2C1+OSX+OSX+10.12.3+build(16D32)&version=12020010&nettype=WIFI&fontScale=100&pass_ticket=0AiIToHJN8yqpuqRAsA5PaaQMJr8KtvlnZ2EqkX0zx%2BEZweRvHKyF%2ByjmycpUbVn)