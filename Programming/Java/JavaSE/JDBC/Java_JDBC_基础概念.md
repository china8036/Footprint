- [Java JDBC: 基础概念](#java-jdbc)
  - [1. 发展历史](#1)
  - [2. 基本概念](#2)
    - [2.1. JDBC](#21-jdbc)
    - [2.2. 驱动程序](#22)
  - [3. 基本功能](#3)
  - [5. 类型映射](#5)
  - [7. Refer Links](#7-refer-links)

# Java JDBC: 基础概念

## 1. 发展历史

ODBC(openDatabaseConnectivity) 是一种用来在相关或不相关的数据库管理系统（DBMS）中存取数据的，用 C 语言实现的，标准应用程序数据接口。通过 ODBC API，应用程序可以存取保存在多种不同数据库管理系统（DBMS）中的数据，而不论每个 DBMS 使用了何种数据存储格式和编程接口。

自从 Java 语言于 1995 年 5 月正式公布以来，Java 风靡全球。出现大量的用 java 语言编写的程序，其中也包括数据库应用程序。但由于没有一个 Java 语言的 API，编程人员不得不在 Java 程序中加入 C 语言的 ODBC 函数调用。这就使很多 Java 的优秀特性无法充分发挥，比如平台无关性、面向对象特性等。随着越来越多的编程人员对 Java 语言的日益喜爱，越来越多的公司在 Java 程序开发上投入的精力日益增加，对 java 语言接口的访问数据库的 API 的要求越来越强烈。也由于 ODBC 的有其不足之处，比如它并不容易使用，没有面向对象的特性等等，SUN 公司决定开发一 Java 语言为接口的数据库应用程序开发接口。

版本发展历史如下：
- `Since 9 -- new in the JDBC 4.3 API and part of the Java SE platform, version 9`
- `Since 1.8 -- new in the JDBC 4.2 API and part of the Java SE platform, version 8`
- `Since 1.7 -- new in the JDBC 4.1 API and part of the Java SE platform, version 7`
- `Since 1.6 -- new in the JDBC 4.0 API and part of the Java SE platform, version 6`
- `Since 1.4 -- new in the JDBC 3.0 API and part of the J2SE platform, version 1.4`
- `Since 1.2 -- new in the JDBC 2.0 API and part of the J2SE platform, version 1.2`
- `Since 1.1 or no "since" tag -- in the original JDBC 1.0 API and part of the JDK™, version 1.1`

## 2. 基本概念

### 2.1. JDBC

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) (Java DB Connection) Java 数据库连接，是一种可用于在**关系数据库**中执行 SQL 语句的 JAVA API，程序可通过 JDBC API 连接到关系数据库，并使用结构化查询语言 SQL 来完成对数据库的查询、更新。

JDBC 为数据库应用开发人员和数据库前台工具开发人员提供了一种统一的标准的应用程序设计接口，使开发人员可以用纯 JAVA 语言编写完整的数据库应用程序。使用 JDBC 编写的数据库应用程序可以跨平台、跨数据库的运行，即同一程序可以在 Windows/Unix 平台上运行，可以使用 MySQL/Oracle 等数据库，而程序无须进行任何修改，只需根据不同的数据库选择不同的驱动程序即可。

### 2.2. 驱动程序

起初，Sun 公司希望自己开发一组 Java API 来操作所有的数据库系统，但由于各具特性的 DBMS 实在太多，Sun 就只制定了一组标准的 API，它们都是接口，而实现类由各数据库厂商自己提供实现，这样根据同一接口实现的实现类就是驱动程序。实际上，这也是面向接口编程的典型应用。

不同的数据库厂商或者同一厂商的不同数据库版本都会提供不同的驱动（如 [MySQL JDBC 驱动程序](https://dev.mysql.com/downloads/connector/j/)），任何应用程序都是通过这个驱动来操作特定厂商、特定版本的数据库的。使用 JDBC 的第一步就是要注册（加载）这个驱动，驱动程序负责将 JDBC 调用映射成特定的数据库调用。

JDBC 驱动程序的分类：
- 第一类 JDBC 驱动程序是 JDBC-ODBC 桥再加上一个 ODBC 驱动程序。这类驱动是最早实现的 JDBC 驱动程序，主要是为了快速推广 JDBC，它将 JDBC API 映射到 ODBC API 上。一般不用现在的编程应用中，且在 JDK8 中已被删除。
- 第二类 JDBC 驱动程序是部分 JAVA API 代码的驱动程序，用于把 JDBC 调用转换成主流数据库 API 的本机调用，性能最强但使用较为复杂。
- 第三类 JDBC 驱动程序是面向数据库中间件的纯 JAVA 驱动程序，JDBC 调用被转换成一种中间件厂商的协议，中间件再把这些调用转换到数据库 API。这类驱动主要用于 Applet 阶段。
- 第四类 JDBC 驱动程序是直接面向数据库的纯 JAVA 驱动程序，直接与数据库实例交互。这类驱动程序易维护易操作，是使用 JDBC 驱动程序访问数据库的首选方式，也是目前最流行的方式。

## 3. 基本功能

Sun 提供的 JDBC 可以完成以下基本工作：
- 建立与数据库的连接。
- 执行 SQL 语句。
- 获得 SQL 语句的执行结果。

通过这 3 个基本功能，程序可以访问、操作数据库系统。

## 5. 类型映射

根据 JDBC 标准，从 SQL 到 Java 数据类型的映射规范如下：
- `CHAR` ->	`java.lang.String`.
- `VARCHAR` ->	`java.lang.String`.
- `LONGVARCHAR` ->	`java.lang.String`.
- `NUMERIC` ->	`java.math.BigDecimal`.
- `DECIMAL` ->	`java.math.BigDecimal`.
- `BIT` ->	`boolean`.
- `TINYINT` ->	`byte`.
- `SMALLINT` ->	`short`.
- `INTEGER` ->	`int`.
- `BIGINT` ->	`long`.
- `REAL` ->	`float`.
- `FLOAT` ->	`double`.
- `DOUBLE` ->	`double`.
- `BINARY` ->	`byte[]`.
- `VARBINARY` ->	`byte[]`.
- `LONGVARBINARY` ->	`byte[]`.
- `DATE` ->	`java.sql.Date`.
- `TIME` ->	`java.sql.Time`.
- `TIMESTAMP` ->	`java.sql.Timestamp`.
- `BLOB` ->	`java.sql.Blob`.
- `CLOB` ->	`java.sql.Clob`.
- `Array` ->	`java.sql.Array`.
- `REF` ->	`java.sql.Ref`.
- `Struct` ->	`java.sql.Struct`.

注：这种类型匹配不是强制性标准，特定的 JDBC 厂商可能会改变这种类型匹配。例如 Oracle 中的 DATE 类型是包含时分秒，而 java.sql.Date 仅仅支持年月日。

## 7. Refer Links

[MySQL Connector/J 8.0 Developer Guide](https://dev.mysql.com/doc/connector-j/8.0/en/)

[Java3y JDBC](https://zhongfucheng.bitcron.com/tag/JDBC)

[学习 Java JDBC，看这篇就够了](https://blog.csdn.net/ljheee/article/details/50988796)

[维基百科：Java 数据库连接](https://zh.wikipedia.org/wiki/Java%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9E%E6%8E%A5)

[百度百科：JDBC](https://baike.baidu.com/item/JDBC)