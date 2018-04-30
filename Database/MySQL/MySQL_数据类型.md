- [MySQL 数据类型](#mysql)
  - [1. 文本](#1)
    - [1.1. char](#11-char)
      - [1.1.1. char](#111-char)
      - [1.1.2. varchar](#112-varchar)
    - [1.2. text](#12-text)
      - [1.2.1. tinytext](#121-tinytext)
      - [1.2.2. text](#122-text)
      - [1.2.3. mediumtext](#123-mediumtext)
      - [1.2.4. longtext](#124-longtext)
    - [1.3. 比较](#13)
      - [1.3.1. char、varchar 比较](#131-charvarchar)
      - [1.3.2. varchar、text 比较](#132-varchartext)
      - [1.3.3. char、varchar、text 比较](#133-charvarchartext)
  - [2. 数字](#2)
    - [2.1. tinyint](#21-tinyint)
    - [2.2. smallint](#22-smallint)
    - [2.3. int](#23-int)
    - [2.4. bigint](#24-bigint)
  - [3. 二进制](#3)
    - [3.1. binary](#31-binary)
    - [3.2. varbinary](#32-varbinary)
    - [3.3. bit](#33-bit)
    - [3.4. tinyblob、blob、mediumblob、longblob](#34-tinyblobblobmediumbloblongblob)
  - [4. 类型选择](#4)
    - [4.1. 选择原则](#41)
    - [4.2. 时间](#42)
      - [4.2.1. 存入 timestamp](#421--timestamp)
      - [4.2.2. 存储类型](#422)
    - [4.3. 手机号码](#43)
    - [4.4. IP 地址](#44-ip)
    - [4.5. 枚举](#45)

# MySQL 数据类型

Mysql 中提供了约 25 种数据类型。

## 1. 文本

### 1.1. char

#### 1.1.1. char

- char 是一种固定长度的类型。
- char(M) 类型的数据列里，每个值都占用 M 个字节，如果某个长度小于 M，MySQL 就会在它的右边用空格字符补足．（在检索操作中那些填补出来的空格字符将被去掉）。
- 若最大数据的长度小于 50 byte，一般考虑使用 char；若超过 50 byte，则不宜使用 char，可采用 varchar。

#### 1.1.2. varchar

- varchar 则是一种可变长度的类型。

- 在 varchar(M) 类型的数据列里，每个值只占用刚好够用的字节再加上一个用来记录其长度的字节（即总长度为 L+1 字节）。

- varchar(255) 表示最大长度是 255 的可变字符类型。

- 存储：varchar 字段是将实际内容单独存储在聚簇索引之外，内容开头用 1 到 2 个字节表示实际长度（长度超过 255 时需要 2 个字节），因此最大长度不能超过 65535。

- 编码长度（UTF-8：一个汉字 = 3 个字节，英文是一个字节；GBK： 一个汉字 = 2 个字节，英文是一个字节）：
  - 在 utf-8 状态下，汉字最多可以存 21844 个字符，英文也为 21844 个字符。
  - 在 gbk 状态下，汉字最多可以存 32766 个字符，英文也为 32766 个字符。

- 行长度限制：
  
  MySQL 要求一个行的定义长度不能超过 65535。若定义的表长度超过这个值会抛出 warning。

### 1.2. text

#### 1.2.1. tinytext

#### 1.2.2. text

#### 1.2.3. mediumtext

#### 1.2.4. longtext

### 1.3. 比较

#### 1.3.1. char、varchar 比较

- char（n）或 varchar（n）中括号中 n 代表字符的个数，并不代表字节个数，所以当使用了中文的时候 (UTF8) 意味着可以插入 m 个中文，但是实际会占用 m*3 个字节。
- 超过 char 和 varchar 的 n 设置后，字符串会被截断。

#### 1.3.2. varchar、text 比较

区别：
- 从 ISO SQL:2003 上讲 VARCHAR 是一个标准型，但 TEXT 不是（包括 tinytext）。
- 在 MySQL 中，text 不允许有默认值；varchar 允许有默认值。
- 大于 varchar（255）变为 tinytext，大于 varchar（500）变为 text，大于 varchar（20000）变为 mediumtext，并抛出 warning。
- varchar(255) 不只是 255byte , 实质上有可能占用的更多。
- varchar 大字段一样会降低性能，所以在设计中有一个原则就是大字段要拆出去，主表还是要尽量的瘦小。
- varchar 会使用 1-3 个字节来存储长度，text 不会。

选择：
- 如果需要非空的默认值，就只能使用 varchar。

- 若长度不足 255，使用 varchar 并合理限制最大上限。

- 若长度超过 255，使用 varchar 和 text 没有本质区别，在存储空间和性能效率上都没有明显区别。
  
  但推荐使用 varchar，因为 varchar 可以限制最大上限，而且有自动截断机制，可以保证字段的最大值可控，如果使用 text 那么如果 code 有漏洞很有可能就写入数据库一个很大的内容，会造成风险

#### 1.3.3. char、varchar、text 比较

- char，存定长，速度快，存在空间浪费的可能，会处理尾部空格，上限 255。
- varchar，存变长，速度慢，不存在空间浪费，不处理尾部空格，上限 65535，但是有存储长度实际 65532 最大可用。
- text，存变长大数据，速度慢，不存在空间浪费，不处理尾部空格，上限 65535，会用额外空间存放数据长度，顾可以全部使用 65535。
- char 在存储的时候会截断尾部的空格，varchar 和 text 不会。

## 2. 数字

### 2.1. tinyint

从 0 到 255 的整型数据。存储大小为 1 字节。

### 2.2. smallint

从 -2^15 (-32,768) 到 2^15 – 1 (32,767) 的整型数据。存储大小为 2 个字节。 

### 2.3. int

- 从 -2^31 (-2,147,483,648) 到 2^31 – 1 (2,147,483,647) (10 位数字) 的整型数据。存储大小为 4 个字节。int 的 SQL-92 同义字为 integer。
- int(M) 在 integer 数据类型中，M 表示最大显示宽度。在 int(M) 中，M 的值跟 int(M) 所占多少存储空间并无任何关系。和数字位数也无关系 int(3)、int(4)、int(8) 在磁盘上都是占用 4 btyes 的存储空间。

### 2.4. bigint

从 -2^63 (-9223372036854775808) 到 2^63-1 (9223372036854775807) 的整型数据（所有数字）。存储大小为 8 个字节。 
注意：bigint 已经有长度了，在 mysql 建表中的 length，只是用于显示的位数。

## 3. 二进制

### 3.1. binary

binary 类型的长度是固定的，在创建表时就指定了。不足最大长度的空间由‘\0’补全。举个例子，binary(50) 就是指定 binary 类型的长度为 50。

### 3.2. varbinary

varbinary 类型的长度是可变的，在创建表时指定了最大长度。指定好了 varbinary 类型的最大值以后，其长度可以在 0 到最大长度之间。

### 3.3. bit

bit(M)，其中，‘M’指定了该二进制数的最大字节长度为 M，M 的最大值为 64。举个例子，bit(4) 就是数据类型为 bit 类型，最大长度为 4。若字段的类型 bit(4)，存储的数据值（十进制）是从 0~~15。

### 3.4. tinyblob、blob、mediumblob、longblob

blob 类型是一种特殊的变长二进制类型。blob 可以用来保存数据量很大的二进制数据，如图片等。blob 类型包括 tinyblob，blob，mediumblob，longblob。这几种 blob 类型最大的区别就是能够保存的最大长度不同。longblob 的长度最大，tinyblob 的长度最小。blob 类型与 text 类型很类似，不同点在于 blob 类型用于存储二进制数据，blob 类型数据是根据其二进制编码进行比较和排序的，而 text 类型是文本模式进行比较和排序的。

## 4. 类型选择

### 4.1. 选择原则

当一个列可以选择多种数据类型时，应该优先考虑数字类型，其次是日期类型或二进制类型，最后是字符类型。

对于相同级别的数据类型，应该优先选择占用空间小的数据类型：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/39ba2f7857769b6b17d671cd8901db27.jpg)

### 4.2. 时间

#### 4.2.1. 存入 timestamp

基于“数据的存储和显示相分离”的设计原则，存储时间应采用基于绝对时间戳的时间存储。只要把表示绝对时间的时间戳（无论是 Long 型还是 Float）存入数据库，在显示的时候根据用户设置的时区格式化为正确的字符串，完全不用管数据库自己提供的 DATETIME 或 TIMESTAMP。

优点：
- 将时区存储问题转变为一个显示问题。
- 两个时间的比较就是数值的比较，根本不涉及时区问题，极其简单。
- 时间的筛选也是两个数值之间筛选，写出 SQL 就是 between(?, ?)。
- 显示时间时，把 Long 或 Float 传到页面，无论用服务端脚本还是用 JavaScript 都能简单而正确地显示时间。唯一需要编写的两个辅助函数就是 String->Long 和 Long->String。String->Long 的作用是把用户输入的时间字符串按照用户指定时区转换成 Long 存进数据库。
缺点：
- 数据库查询你看到的不是时间字符串，而是类似 1413266801750 之类的数字。

#### 4.2.2. 存储类型

时间精度要求不高，采用 int(10)，最大 2147483647，即 10 位的 unix 时间戳（基于秒），表示 2038-01-19 11:14:07，占 4 个字节；

时间精度要求较高，采用 bigint，最大 9223372036854775807，即 19 位 unix 时间戳（基于纳秒），表示 2262-04-12 07:47:16，占 8 个字节；

NOTE:
- 具体采用的存储类型，应根据项目实际需求而定；
- 若考虑到项目长期使用，应考虑到“2038 年问题”，即用 int 存储的时间戳最大值只能表示到 2038 年 1 月 19 日 3 时 14 分 07 秒，超过这个时间后，时间戳将被表示成一个负数，导致程序时间无法正常执行，因此，考虑到此问题，存储时间戳应采用 bigint；

P.S. char 和 datetime 存储时间的区别：
- 存储类型，char 的本质是定长字符串，datetime 表现是时间类型，本质是 int。精度要求相同时，char 占用的空间更大。
- char 可以存储时间的长度和精度可以完全由程序决定，datetime 则由数据库本身决定；

从 MySQL 5.6 开始，建议优先选择 DATETIME 存储日期时间，因为它的可用范围比 TIMESTAMP 更大，物理存储上仅比 TIMESTAMP 多 1 个字节，整体性能上的损失并不大。

### 4.3. 手机号码

- 国内手机号码为 11 位数字，因此无法用 int，int 最大可以表示 10 位数字。
- 若业务只能确定数据最大长度是 11 位数字，采用 varchar(11)，数据长度不确定故采用变长类型以节省存储空间。
- 若业务确定所有数据都是 11 位数字，采用 char(11)，定长数据的查找速度要优于变长数据。
- 若业务包含了国外手机号码，最大长度无法确定，采用 varchar(20)，长度预留几个字符。

### 4.4. IP 地址

- varchar(15)，占用 15 个字节，传统存储方式，空间效率低；

- unsigned int，占用 4 个字节，采用 ip 地址转换为 long/int 型数字的算法：
  ```
  A.B.C.D -> A*256*256*256+B*256*256+C*256+D
  ```
  采用程序应用层控制转换：
  ```java
  // 将 IP 地址转换为 long 的 JAVA 方法：
  public static long ip2Long(String ipStr) {  
          String[] ip = ipStr.split("\\.");  
          return (Long.valueOf(ip[0]) << 24) + (Long.valueOf(ip[1]) << 16)  
                  + (Long.valueOf(ip[2]) << 8) + Long.valueOf(ip[3]);  
  }  
  // 将 long 转换为 IP 字符串的 JAVA 方法：
  public static String long2Ip(long ipLong) {  
          StringBuilder ip = new StringBuilder();  
          ip.append(ipLong >>> 24).append(".");  
          ip.append((ipLong >>> 16) & 0xFF).append(".");  
          ip.append((ipLong >>> 8) & 0xFF).append(".");  
          ip.append(ipLong & 0xFF);  
          return ip.toString();  
  }  
  ```

  采用数据库函数控制转换
  ipv4：inet_aton: 将 ip 地址转换成数字型；inet_ntoa: 将数字型转换成 ip 地址；
  ipv6：INET6_ATON 和 INET6_NTOA；
  ```
  mysql> select inet_aton('192.168.0.1');  
  mysql> select inet_ntoa(3232235521);  
  ```

- varbinary(4)
  
  将 ipv4 地址分段，每 4 位占一个字段。

### 4.5. 枚举

枚举类型可以使用 ENUM，ENUM 的内部存储机制是采用 TINYINT 或 SMALLINT（并非 CHAR/VARCHAR），性能一点都不差，记住千万别用 CHAR/VARCHAR 来存储枚举数据。
