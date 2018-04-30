- [MySQL 备份与恢复](#mysql)
  - [1. 导出数据](#1)
    - [1.1. SELECT ... INTO OUTFILE](#11-select-into-outfile)
    - [1.2. mysqldump](#12-mysqldump)
    - [1.3. mysql 命令重定向](#13-mysql)
    - [1.4. 示例：定时导出数据后删除](#14)
      - [1.4.1. SELECT … INTO OUTFILE+ 存储过程 + 事件](#141-select--into-outfile)
      - [1.4.2. mysql 命令重定向 +PowerShell+ 定时任务](#142-mysql--powershell)
      - [1.4.3. mysql 命令重定向 +shell+crontab](#143-mysql--shellcrontab)
      - [1.4.4. python 实现](#144-python)
  - [2. 导入数据](#2)
    - [2.1. LOAD DATA INFILE](#21-load-data-infile)
    - [2.2. source](#22-source)
    - [2.3. mysqldump](#23-mysqldump)
    - [2.4. mysqlimport](#24-mysqlimport)

# MySQL 备份与恢复

## 1. 导出数据

### 1.1. SELECT ... INTO OUTFILE

使用 SELECT...INTO OUTFILE 语句可以将查询结果数据导出为纯文本，到 csv、xls、sql、txt 文件中；

示例：MySQL 命令行中
```sql
SELECT 
* 
FROM 
`table_name` 
WHERE 
`id` BETWEEN 1 AND 1000 
INTO OUTFILE 
“D:/test_file.csv”
[
FIELDS 
TERMINATED BY ', ' -- 指定字段分割符
ENCLOSED BY ''  -- 指定字段包围符号
LINES 
	TERMINATED BY '\n'  -- 指定换行符
] -- 方括号内为可选项
```
效果：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/22cd2d35b6428a5ca4d9fbb59f8bc4f7.jpg)

NOTE:
- 指定的 OUTFILE 路径需要与 my.ini 中配置的 secure-file-priv 一致，使用 SHOW VARIABLES LIKE "secure_file_priv"; 查看该配置项，如：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/600c045403ed30898ec202bf7ad4f221.jpg)

- 即使在 Windows 下，路径中的“\”都要换成“/”。

- 导出时可明确指定导出格式：使用 FIELDS 和 LINES 子句，两个子句都是可选的，但是如果两个同时被指定，FIELDS 子句必须出现在 LINES 子句之前；如果用户指定一个 FIELDS 子句，它的子句 （TERMINATED BY、[OPTIONALLY] ENCLOSED BY 和 ESCAPED BY) 也是可选的，不过，用户必须至少指定它们中的一个。

- 导出时若不指定字段分隔符，默认是不使用分隔符，各字段都连在一起。
  - 使用“, ”即逗号 + 空格作为分隔符时，excel 可以识别各字段自动归档到各个单元格。
  - 使用其它符号（如空格等) 作为分隔符时，excel 无法识别出各字段，无法将各个字段完美匹配到各个单元格。

- 导出的文件名一定不能已经存在。

- 导出时登录的 mysql 账号需要有 FILE 权限。

- null 值会被处理成`\N`。

- 导出的 csv 文件中没有列名，只有数据。

### 1.2. mysqldump

mysqldump 将数据库表的结构和数据导出为 SQL 脚本，到 sql、txt 文件中，其中包含从头重新创建数据库和插入数据所需的命令 CREATE TABLE 和 INSERT 等。

格式：Cmd/Powershell/Bash 中

导出一个数据表
```
mysqldump -u<username> -p <db_name> <table_name> --where/-w=“筛选条件” > <file_path>
```
导出一个数据库
```
mysqldump -u<username> -p <db_name> > <file_path>
```
导出所有数据库：
```
mysqldump -u<username> -p --all-databases > <file_path>
```

参数说明：
- `--where/-w`: 用来设定数据导出的条件，使用方式和 SQL 查询命令中中的 WHERE 基本上相同。
- `-d` 不导出数据只导出结构。
- `--add-drop-table`: 在每个 create 语句之前增加一个 drop table。
- `--add-locks`：在每个表导出之前增加 LOCK TABLES 并且之后 UNLOCK  TABLE（默认为打开状态，使用 --skip-add-locks 取消选项)。
- `--lock-all-tables,  -x`：提交请求锁定所有数据库中的所有表，以保证数据的一致性。这是一个全局读锁，并且自动关闭 --single-transaction 和 --lock-tables 选项。
- `--lock-tables`,  -l：开始导出前，锁定所有表。用 READ  LOCAL 锁定表以允许 MyISAM 表并行插入。对于支持事务的表例如 InnoDB 和 BDB，--single-transaction 是一个更好的选择，因为它根本不需要锁定表。
- `--xml, -X`：导出 XML 格式。
- `--debug`：输出 debug 信息，用于调试；。

例：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/aa5560f4908a523f2cef47367a4e37e6.jpg)

效果：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/fc08f845c3e09bab7409c83b17d1ad3f.jpg)

注意：
- null 值会被处理成、N。
- 导出的文件名是固定的。

### 1.3. mysql 命令重定向

PowerShell 中
```
mysql -u 用户名 -p 密码 -e “SELECT * FROM test WHERE id BETWEEN 1 AND 1000;” 数据库名 > 文件路径名。csv
```

NOTE:
- null 值会被处理成字符串"NULL"。
- 可以和 Windows powershell/Linux shell 结合使用，灵活性比较大，其中在 Windows 下时，使用 powershell 可以自动在各字段间添加分隔符，以被 excel 识别，而若使用 dos，各字段间没有分隔符。
- 导出的 csv 文件默认包含列名，若不要列名，加上 -N 参数即可。

### 1.4. 示例：定时导出数据后删除

有表 test_1200，数据增量基本在 0.5 条记录 / 秒，要求能在每天的 0 点将表中最早的 1000 条记录导出到 csv 文件中备份，然后将这 1000 条记录在表中删除。

要取得最早的 1000 条记录，由于存在自增 id，因此只需取得 id 值前 1000 条即可，实现方案有以下几种：

#### 1.4.1. SELECT … INTO OUTFILE+ 存储过程 + 事件

存储过程定义：
```sql
DROP PROCEDURE IF EXISTS backup_then_delete;
DELIMITER $ 
CREATE PROCEDURE backup_then_delete(IN number INT) -- 参数：要操作的记录条数
BEGIN
	-- 定义 id 的起始值，即最早 / 小的 id
	SELECT
		`id` INTO @begin_id
	FROM
		`test_1200`
	ORDER BY
		`id`
	LIMIT 1;

	-- 定义 id 的终止值
	SET @last_id = @begin_id + number - 1;
	-- 将 id 从起始值到终止值的记录导出到 csv 文件中
	SELECT
		*
	FROM
		`test_1200`
	WHERE
		`id` BETWEEN @begin_id AND @last_id
	INTO OUTFILE 
		"D:/test_1200.csv" -- TODO 以日期动态生成文件路径名
	-- 设置输出到文件时的编码「在 win 系统使用该 csv 文件时需要写上，否则会出现中文乱码，若无中文则不需要」
CHARACTER SET gbk
FIELDS 	
		TERMINATED BY ", " 
		ENCLOSED BY "" 
	LINES 
		TERMINATED BY "\n";
	-- 删除 id 从起始值到终止值的记录
	DELETE 
FROM 
`test_1200` 
WHERE
`id` BETWEEN @begin_id AND @last_id;
END$
DELIMITER ;
```

事件定义：
```sql
DROP EVENT IF EXISTS backup_event;
SELECT concat(ADDDATE(CURRENT_DATE(),1),' 00:00:00') INTO @dayruntime;
DELIMITER $
CREATE EVENT backup_event
ON SCHEDULE
　　EVERY 1 DAY STARTS @dayruntime
COMMENT 
'SAVES THE RECORDS THEN DELETE THEM EACH DAY'
DO
BEGIN
	-- 调用存储过程
	CALL backup_then_delete(1000);
END $
DELIMITER ;
```

缺点：导出的 csv 文件没有列名。

#### 1.4.2. mysql 命令重定向 +PowerShell+ 定时任务

1. 编写 ps1 脚本文件：
    ```sql
    $number=$args[0]
    $now_date=Get-Date -Format 'yyyy_MM_dd_HH_mm_ss'
    $file_name="D:/"+$now_date+"_backup.csv"
    $sql="SELECT id INTO @begin_id FROM test_1200 ORDER BY id LIMIT 1; SET @last_id = @begin_id+"+$number+"-1; SELECT * FROM test_1200 WHERE id BETWEEN @begin_id AND @last_id;DELETE FROM test_1200 WHERE id BETWEEN @begin_id AND @last_id;"
    mysql -u 用户名 -p 密码 -e $sql 数据库名 > $file_name
    ```

1. 定义定时任务：

    打开“任务计划程序”，新建任务：
    
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/4/30/14e7ef97ea49bfdb7b13cb53d43f6914.jpg)

    在操作板块新建操作，程序或脚本选择本机 PowerShell.exe 的绝对路径；添加参数填写 ps1 脚本的绝对路径 + 参数，如：“C:\Users\firej\Desktop\backup_and_delete.ps1 1000”；在触发器板块新建触发器，设定好时间即可。

优点：导出的 csv 文件有列名。

NOTE: 相同的思路若采用 DOS 编程操作，输出的 csv 文件各字段是没有分隔的，因此要采用 powershell。

#### 1.4.3. mysql 命令重定向 +shell+crontab

<!-- TODO: -->

Linux：
```
mysql -uroot -p -e "select name_txt,source,taxon_id,name_class from names" thematic | sed -e"s/^/\"/" -e "s/[\t]/\",\"/g" -e "s/$/\"\r/">/home/csvdata/name.csv
```

#### 1.4.4. python 实现

https://zmobi.github.io/2016/09/27/export-csv-and-charset.html 

## 2. 导入数据

### 2.1. LOAD DATA INFILE

使用 LOAD DATA INFILE 语句将文本文件 (txt、csv、xls 等) 中的数据导入到指定的数据表中：
```sql
LOAD DATA INFILE '/tmp/test123.txt' INTO TABLE `test123`;
```

NOTE:
- LOAD DATA 默认情况下是按照数据文件中列的顺序插入数据的，如果数据文件中的列与插入表中的列不一致，则需要指定列的顺序。如，在数据文件中的列顺序是 a,b,c，但要使插入表的列顺序为 b,c,a，则：
  ```sql
  LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl (b, c, a);
  ```

### 2.2. source

在 MySQL 命令行中，可以使用 source 命令导入 sql 脚本文件：
```
mysql>source d:\wcnc_db.sql
```

### 2.3. mysqldump

使用 mysqldump 导入一个 sql 脚本，可完整的创建一个数据表结构并插入预定义的数据；

格式：
```
$ mysqldump -u root -p <database_name> < <file_path>
```

NOTE:
- 导入前需要先确认数据库已经创建。
- 可使用 -h 直接导入到远程的服务器：
  ```
  $ mysqldump -u root -p database_name | mysql -h other-host.com database_name
  ```

### 2.4. mysqlimport

mysqlimport 客户端提供了 LOAD DATA INFILEQL 语句的一个命令行接口。mysqlimport 的大多数选项直接对应 LOAD DATA INFILE 子句。从文件 dump.txt 中将数据导入到 mytbl 数据表中，可以使用以下命令：
```
$ mysqlimport -u root -p --local database_name dump.txt
```

NOTE:
- `mysqlimport` 语句中使用 --columns 选项来设置列的顺序：
  ```
  $ mysqlimport -u root -p --local --columns=b,c,a database_name dump.txt
  ```
- `mysqlimport`命令可以指定选项来设置指定格式：
  ```
  $ mysqlimport -u root -p --local --fields-terminated-by=":" --lines-terminated-by="\r\n"  database_name dump.txt
  ```
- `-l or -lock-tables`：数据被插入之前锁住表，这样就防止了， 你在更新数据库时，用户的查询和更新受到影响；
