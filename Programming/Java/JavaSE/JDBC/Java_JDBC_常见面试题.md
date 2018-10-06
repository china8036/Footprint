- [Java JDBC: 常见面试题](#java-jdbc-常见面试题)
	- [1. JDBC 是如何实现 Java 程序和 JDBC 驱动的松耦合的？](#1-jdbc-是如何实现-java-程序和-jdbc-驱动的松耦合的)
	- [2. JDBC 操作数据库的步骤？](#2-jdbc-操作数据库的步骤)
	- [3. execute，executeQuery，executeUpdate 的区别是什么？](#3-executeexecutequeryexecuteupdate-的区别是什么)
	- [4. JDBC 中的 Statement 和 PreparedStatement，CallableStatement 的区别？](#4-jdbc-中的-statement-和-preparedstatementcallablestatement-的区别)
	- [5. PreparedStatement 的缺点是什么？怎么解决这个问题？](#5-preparedstatement-的缺点是什么怎么解决这个问题)
	- [6. JDBC 如何进行事务的处理？](#6-jdbc-如何进行事务的处理)
	- [7. 数据库连接池工作原理和实现方案？](#7-数据库连接池工作原理和实现方案)
	- [8. JAVA 访问数据库，从数据库连接池开始到数据库返回结果，都发生了哪些事情？](#8-java-访问数据库从数据库连接池开始到数据库返回结果都发生了哪些事情)
	- [9. 常见的 JDBC 异常有哪些？](#9-常见的-jdbc-异常有哪些)
	- [10. java.util.Date 和 java.sql.Date 有什么区别？](#10-javautildate-和-javasqldate-有什么区别)
	- [11. JDBC 的 RowSet 是什么，有哪些不同的 RowSet？](#11-jdbc-的-rowset-是什么有哪些不同的-rowset)
	- [12. Refer Links](#12-refer-links)

# Java JDBC: 常见面试题

## 1. JDBC 是如何实现 Java 程序和 JDBC 驱动的松耦合的？

通过制定符合 JDBC 规范的接口，要求数据库厂商来实现。程序员只要通过接口调用即可。

随便看一个简单的 JDBC 示例，你会发现所有操作都是通过 JDBC 接口完成的，而驱动只有在通过 `Class.forName` 反射机制来加载的时候才会出现。

## 2. JDBC 操作数据库的步骤？

1. 注册数据库驱动。
1. 建立数据库连接。
1. 创建一个 Statement。
1. 执行 SQL 语句。
1. 处理结果集。
1. 关闭数据库连接。

## 3. execute，executeQuery，executeUpdate 的区别是什么？

- Statement 的 execute(String query) 方法用来执行任意的 SQL 查询，如果查询的结果是一个 ResultSet，这个方法就返回 true。如果结果不是 ResultSet，比如 insert 或者 update 查询，它就会返回 false。我们可以通过它的 getResultSet 方法来获取 ResultSet，或者通过 getUpdateCount() 方法来获取更新的记录条数。

- Statement 的 executeQuery(String query) 接口用来执行 select 查询，并且返回 ResultSet。即使查询不到记录返回的 ResultSet 也不会为 null。我们通常使用 executeQuery 来执行查询语句，这样的话如果传进来的是 insert 或者 update 语句的话，它会抛出错误信息为 “executeQuery method can not be used for update”的 java.util.SQLException。

- Statement 的 executeUpdate(String query) 方法用来执行 insert 或者 update/delete（DML）语句，或者什么也不返回的 DDL 语句。返回值是 int 类型，如果是 DML 语句的话，它就是更新的条数，如果是 DDL 的话，就返回 0。

- 只有当不确定是什么语句的时候才应该使用 execute() 方法，否则应该使用 executeQuery 或者 executeUpdate 方法。

## 4. JDBC 中的 Statement 和 PreparedStatement，CallableStatement 的区别？

- PreparedStatement 对象避免了数据库对 SQL 语句的重复编译。当需要多次执行同一结构的 SQL 语句时，可以显著地提高效率。

- PreparedStatement 对象在设置 SQL 参数后会自动把内部单引号转义后再添加单引号，因此可以防止 SQL 注入。

- PreparedStatement 对象支持对大文本 Clob、二进制 Blob 数据进行处理，而 Statement 对象不支持。

- PreparedStatement 提高了代码的可读性和可维护性，将参数与 SQL 语句分离出来，这样就可以方便对程序的更改和扩展，同时避免了对字符串参数的多次拼接，可以减少不必要的错误。 

- CallableStatement 适用于执行存储过程。

## 5. PreparedStatement 的缺点是什么？怎么解决这个问题？

[PreparedStatement 的缺点是什么？](https://www.nowcoder.com/questionTerminal/1db85bbc03f7413abc64cfe23b096534?orderByHotValue=1&questionTypes=111111&page=7&onlyReference=false)

[JDBC PreparedStatement 批量查询 IN 的实现 方案](https://blog.csdn.net/suifeng3051/article/details/24033237)

我们经常会有这种业务需求，根据一个条件集合去查询一张表的数据，比如：
```sql
SELECT * FROM all_element t WHERE t.task_id IN (List <taskids>);
```
PreparedStatement 的一个缺点就是，由于 SQL 中占位符的数量一旦写好就是固定的，因此我们无法直接用它来执行不定参数数量的 IN 条件语句。

为解决该问题，可以有以下迂回方案：
- 将 IN 条件语句转换成 OR 条件语句：
	```sql
	select * from all_element t where t.task_id in (123 ,456, 789);
	-- 转换为
	select * from all_element t where t.task_id=123 or t.task_id= 456 t.task_id= 789;
	```
	由于数据库的 SQL 预编译功能必须在 SQL 语句完全一致的情况下才能使缓存生效，因此，这种方案会导致数据库的 SQL 缓存无法被有效使用（taskid 值会变化）。

- 将 IN 条件语句拆分成多条等值查询语句：
	```sql
	select * from all_element t where t.task_id in (123 ,456, 789);
	```
	拆分为
	```java
	String QUERY = "select * from all_element where taskId = ?";
	PrepareStatement ps = con.prepareStatement(QUERY);
	for(String taskId : taskIds) {
			ps.setInt(1, taskId);
			rs = ps .executeQuery();
	}
	```
	这种方案虽然可以很好的利用数据库的 SQL 缓存，但缺点也很明显，就是每一个 Id 都需要查询数据库一次，这样效率是极低的。

- 使用存储过程实现，但这取决于数据库的实现，不是所有数据库都支持。

- 动态拼接 SQL 生成 PreparedStatement
	```java
	// 对于我们传过来的集合参数，我们可以动态地创建一个 PrepareStatement
	private static void createQuery(List<String> taskIds) {
			String query = "select * from all_element t where t.task_id in (";
			StringBuilder queryBuilder = new StringBuilder(query);
				for ( int i = 0; i < taskIds.size(); i++) {
					queryBuilder.append( " ?");
						if (i != taskIds.size() - 1)
								queryBuilder.append( ",");
			}
			queryBuilder.append( ")");
				ps = con .prepareStatement(query);
				for ( int i = 1; i <= taskIds.size(); i++) {
						ps.setInt(i, taskIds.get(i - 1));
			}
				rs = ps .executeQuery();
	}
	```
	这种方案依旧存在问题，虽然 taskId 值的变化不会导致数据库对 SQL 重新编译了，但若 taskId 的数量发生变化，相应的 SQL 也发生了变化，则数据库还是会对 SQL 重新编译，无法有效利用数据库缓存。

- 预定义参数个数的分批次查询

	这种方案兼顾了前边几种方案，基本思想是：预先定义好几个每次要查询参数的个数，然后把参数集合按这些定义好的值划分成小组。比如我们的前台传过来一个数量为 75 的 taskId 的集合，预定义的批量查询参数的个数分别为：
	```java
	SINGLE_BATCH = 1; // 注意：为了把参数集合完整划分，这个值为 1 的批量数是必须的
	SMALL_BATCH = 4;
	MEDIUM_BATCH = 11;
	LARGE_BATCH = 51;
	```
	那么我们第一次会查询 51 条数据，还剩下 24 个没有查询，那么第二次批量查询 11 条数据，还剩下 13 条未查询，第三次再批量查询 11 条数据，最后还剩 2 条未查询，那么我们再分两批次，每批次仅查询一条，这样，最终一个 75 条的数据分 5 批次即可查询完成，减少了查询次数，而且还利用到了数据库缓存。

	例：
	```java
	public static final int SINGLE_BATCH = 1; // 注意：为了把参数集合完整划分，这个值为 1 的批量数是必须的
	public static final int SMALL_BATCH = 4;
	public static final int MEDIUM_BATCH = 11;
	public static final int LARGE_BATCH = 51;
	static int totalNumberOfValuesLeftToBatch=75;
	public static List<Integer>  getBatchSize( int totalNumberOfValuesLeftToBatch){
			List<Integer> batches= new ArrayList<Integer>();
				while ( totalNumberOfValuesLeftToBatch > 0 ) {
						int batchSize = SINGLE_BATCH;
						if ( totalNumberOfValuesLeftToBatch >= LARGE_BATCH ) {
						batchSize = LARGE_BATCH;
					} else if ( totalNumberOfValuesLeftToBatch >= MEDIUM_BATCH ) {
						batchSize = MEDIUM_BATCH;
					} else if ( totalNumberOfValuesLeftToBatch >= SMALL_BATCH ) {
						batchSize = SMALL_BATCH;
					}
					batches.add(batchSize);
					totalNumberOfValuesLeftToBatch -= batchSize;
					System. out.println(batchSize);
			}
				return batches;
	}
	```

- 在 PreparedStatement 查询中使用 NULL 值，如果知道输入变量的最大个数的话，这是个不错的办法。

## 6. JDBC 如何进行事务的处理？

Connection 类中提供了 4 个事务处理方法：
- setAutoCommit(Boolean autoCommit): 设置是否自动提交事务，默认为自动提交，即为 true, 通过设置 false 禁止自动提交事务。
- commit(): 提交事务。
- rollback(): 回滚事务。
- savepoint: 保存点。

## 7. 数据库连接池工作原理和实现方案？

## 8. JAVA 访问数据库，从数据库连接池开始到数据库返回结果，都发生了哪些事情？

利用 JDBC driver 和 MySQL 数据库建立 TCP 连接之后的连接对象放在池中，当需要操作数据库的时候从池中取出一个连接，发送 SQL 到 MySQL，MySQL 经过 SQL 语法解析、查询优化、生成实际物理计划及执行、连接处理与类型处理等一系列的过程之后返回要查询的数据给 JDBC driver 的 ResultSet。

关闭数据库连接对象后再把连接对象重新放到池子中。

## 9. 常见的 JDBC 异常有哪些？

- java.sql.SQLException：是 JDBC 异常的基类。
- java.sql.BatchUpdateException：当批处理操作执行失败的时候可能会抛出这个异常。这取决于具体的 JDBC 驱动的实现，它也可能直接抛出基类异常 java.sql.SQLException。
- java.sql.SQLWarning：SQLWarning 是 SQLException 的子类，通过 Connection、Statement、Result 的 getWarnings() 方法都可以获取到它。 SQLWarning 不会中断查询语句的执行，只是用来提示用户存在相关的警告信息。
- java.sql.DataTruncation：字段值由于某些非正常原因被截断了（不是因为超过对应字段类型的长度限制）。

## 10. java.util.Date 和 java.sql.Date 有什么区别？

java.util.Date 包含日期和时间，而 java.sql.Date 只包含日期信息，而没有具体的时间信息。

## 11. JDBC 的 RowSet 是什么，有哪些不同的 RowSet？

RowSet 用于存储查询的数据结果，和 ResultSet 相比，它更具灵活性。RowSet 继承自 ResultSet，因此 ResultSet 能干的，它们也能，而 ResultSet 做不到的，它们还是可以。RowSet 接口定义在 javax.sql 包里。

RowSet 分为两大类：
- 连接型 RowSet: 这类对象与数据库进行连接，和 ResultSet 很类似。JDBC 接口只提供了一种连接型 RowSet，javax.sql.rowset.JdbcRowSet，它的标准实现是 com.sun.rowset.JdbcRowSetImpl。
- 离线型 RowSet: 这类对象不需要和数据库进行连接，因此它们更轻量级，更容易序列化。它们适用于在网络间传递数据。

## 12. Refer Links

[JDBC 面试题都在这里](https://zhongfucheng.bitcron.com/post/jdbc/jdbcmian-shi-ti-du-zai-zhe-li)

[JAVA 访问数据库，从数据库连接池开始到数据库返回结果，都发生了哪些事情？](https://www.zhihu.com/question/53589525)