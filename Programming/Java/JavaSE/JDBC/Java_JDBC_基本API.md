- [Java JDBC API: 基本 API](#java-jdbc-api---api)
    - [1. Driver 接口](#1-driver)
    - [2. DriverManager 类](#2-drivermanager)
        - [2.1. 主要 API](#21--api)
        - [2.2. 类定义](#22)
            - [2.2.1. 内部属性](#221)
            - [2.2.2. 方法实现](#222)
    - [3. Connection 接口](#3-connection)
    - [4. Statement 接口](#4-statement)
        - [4.1. PreparedStatement 接口](#41-preparedstatement)
            - [4.1.1. 常用 API](#411--api)
            - [4.1.2. 类定义](#412)
            - [4.1.3. PreperedStatement 和 Statement 的区别](#413-preperedstatement--statement)
            - [4.1.4. MySQL 的预编译](#414-mysql)
        - [4.2. CallableStatement 接口](#42-callablestatement)
            - [4.2.1. 常用 API](#421--api)
            - [4.2.2. 类定义](#422)
    - [5. ResultSet 接口](#5-resultset)
        - [5.1. 基本特性](#51)
        - [5.2. 类定义](#52)
    - [6. 元数据接口](#6)
        - [6.1. ParameterMetaData 接口](#61-parametermetadata)
        - [6.2. ResultSetMetaData 接口](#62-resultsetmetadata)
        - [6.3. DataBaseMetaData 接口](#63-databasemetadata)
    - [7. RowSet 接口](#7-rowset)
        - [7.1. JdbcRowSet 接口](#71-jdbcrowset)
        - [7.2. CachedRowSet 接口](#72-cachedrowset)
            - [7.2.1. WebRowSet 接口](#721-webrowset)
            - [7.2.2. JoinRowSet 接口](#722-joinrowset)
            - [7.2.3. FilteredRowSet 接口](#723-filteredrowset)
    - [8. MySQL Connector/J 实现类](#8-mysql-connector-j)
        - [8.1. ConnectionImpl 类](#81-connectionimpl)
        - [8.2. NonRegisteringDriver 类](#82-nonregisteringdriver)
        - [8.3. Driver 类](#83-driver)
    - [9. Refer Links](#9-refer-links)

# Java JDBC API: 基本 API

JDBC API 主要位于 JDK 中的 java.sql 包中（之后扩展的内容位于 javax.sql 包中），主要包括：
- DriverManager：负责加载各种不同驱动程序（Driver），并根据不同的请求，向调用者返回相应的数据库连接（Connection）。
- SQLException：代表在数据库连接的建立和关闭和 SQL 语句的执行过程中发生了例外情况（即错误）。
- Driver：驱动程序，会将自身加载到 DriverManager 中去，并处理相应的请求并返回相应的数据库连接（Connection）。
- Connection：数据库连接，负责进行与数据库间的通讯，SQL 执行以及事务处理都是在某个特定 Connection 环境中进行的。可以产生用以执行 SQL 的 Statement。
- Statement：用以执行 SQL 查询和更新（针对静态 SQL 语句和单次执行）。
- PreparedStatement：用以执行包含动态参数的 SQL 查询和更新（在服务器端编译，允许重复执行以提高效率）。
- CallableStatement：用以调用数据库中的存储过程。

其中，**除了 DriverManager 类和 SQLException 类在 JDK 中已包含了具体实现外，其它均为接口，需要具体数据库的驱动程序提供者来实现**。

## 1. Driver 接口

https://docs.oracle.com/javase/9/docs/api/java/sql/Driver.html

```java
public interface Driver {
    // 执行连接逻辑
    Connection connect(String url, java.util.Properties info) throws SQLException;
    // Retrieves whether the driver thinks that it can open a connection to the given URL.
    boolean acceptsURL(String url) throws SQLException;
    // Gets information about the possible properties for this driver.
    DriverPropertyInfo[] getPropertyInfo(String url, java.util.Properties info)
                         throws SQLException;
    // Retrieves the driver's major version number. Initially this should be 1.
    int getMajorVersion();
    // Gets the driver's minor version number. Initially this should be 0.
    int getMinorVersion();
    // Reports whether this driver is a genuine JDBC Compliant&trade; driver.
    boolean jdbcCompliant();
    //------------------------- JDBC 4.1 -----------------------------------
    public Logger getParentLogger() throws SQLFeatureNotSupportedException;
}
```

## 2. DriverManager 类

> The basic service for managing a set of JDBC drivers.

[DriverManager](https://docs.oracle.com/javase/9/docs/api/java/sql/DriverManager.html) 是管理一个 JDBC Driver 的基础服务类。主要用于获取 Connection 对象。

### 2.1. 主要 API

- getConnection​
  - `static Connection	getConnection​(String url)`: Attempts to establish a connection to the given database URL.
  - `static Connection	getConnection​(String url, String user, String password)`: Attempts to establish a connection to the given database URL.
  - `static Connection	getConnection​(String url, Properties info)`: Attempts to establish a connection to the given database URL.

- registerDriver​
  - `static void	registerDriver​(Driver driver)`: Registers the given driver with the DriverManager.
  - `static void	registerDriver​(Driver driver, DriverAction da)`: Registers the given driver with the DriverManager.

- deregisterDriver​
  - `static void	deregisterDriver​(Driver driver)`: Removes the specified driver from the DriverManager's list of registered drivers.

### 2.2. 类定义

```java
public class DriverManager {}
```

#### 2.2.1. 内部属性

```java
// 内部属性 registeredDrivers 存放着已注册的驱动
private final static CopyOnWriteArrayList<DriverInfo> registeredDrivers = new CopyOnWriteArrayList<>();

private static volatile int loginTimeout = 0;
private static volatile java.io.PrintWriter logWriter = null;
private static volatile java.io.PrintStream logStream = null;
// Used in println() to synchronize logWriter
private final static  Object logSync = new Object();

// ...
```

#### 2.2.2. 方法实现

- registerDriver
  ```java
  public static synchronized void registerDriver(java.sql.Driver driver)
      throws SQLException {
      registerDriver(driver, null);
  }
  public static synchronized void registerDriver(java.sql.Driver driver,
          DriverAction da)
      throws SQLException {
      // 注册驱动的过程其实就是把具体的 Driver 添加到内部属性 registeredDrivers 中
      if (driver != null) {
          registeredDrivers.addIfAbsent(new DriverInfo(driver, da));
      } else {
          // This is for compatibility with the original DriverManager
          throw new NullPointerException();
      }
      println("registerDriver: " + driver);
  }
  ```

- getConnection​

  ```java
  @CallerSensitive
  public static Connection getConnection(String url,
      java.util.Properties info) throws SQLException {

      return (getConnection(url, info, Reflection.getCallerClass()));
  }

  @CallerSensitive
  public static Connection getConnection(String url,
      String user, String password) throws SQLException {
      java.util.Properties info = new java.util.Properties();

      if (user != null) {
          info.put("user", user);
      }
      if (password != null) {
          info.put("password", password);
      }

      return (getConnection(url, info, Reflection.getCallerClass()));
  }

  @CallerSensitive
  public static Connection getConnection(String url)
      throws SQLException {

      java.util.Properties info = new java.util.Properties();
      return (getConnection(url, info, Reflection.getCallerClass()));
  }

  //  前三个公共方法最终都调用了该私有方法进行实现
  private static Connection getConnection(
      String url, java.util.Properties info, Class<?> caller) throws SQLException {
      /*
        * When callerCl is null, we should check the application's
        * (which is invoking this class indirectly)
        * classloader, so that the JDBC driver class outside rt.jar
        * can be loaded from here.
        */
      ClassLoader callerCL = caller != null ? caller.getClassLoader() : null;
      synchronized(DriverManager.class) {
          // synchronize loading of the correct classloader.
          if (callerCL == null) {
              callerCL = Thread.currentThread().getContextClassLoader();
          }
      }

      if(url == null) {
          throw new SQLException("The url cannot be null", "08001");
      }

      println("DriverManager.getConnection(\"" + url + "\")");

      // Walk through the loaded registeredDrivers attempting to make a connection.
      // Remember the first exception that gets raised so we can reraise it.
      SQLException reason = null;
      // 遍历 registeredDrivers 中存放的所有的 Driver，直到找到可与 URL 中指定的数据库进行连接的驱动程序为止，然后进行连接
      for(DriverInfo aDriver : registeredDrivers) {
          if(isDriverAllowed(aDriver.driver, callerCL)) {
              try {
                  println("    trying " + aDriver.driver.getClass().getName());
                  // 尝试连接具体的 URL
                  Connection con = aDriver.driver.connect(url, info);
                  if (con != null) {
                      // 连接成功，返回连接对象
                      println("getConnection returning " + aDriver.driver.getClass().getName());
                      return (con);
                  }
              } catch (SQLException ex) {
                  if (reason == null) {
                      reason = ex;
                  }
              }

          } else {
              println("    skipping: " + aDriver.getClass().getName());
          }
      }

      // 连接失败
      if (reason != null)    {
          println("getConnection failed: " + reason);
          throw reason;
      }

      println("getConnection: no suitable driver found for "+ url);
      throw new SQLException("No suitable driver found for "+ url, "08001");
  }
  ```

## 3. Connection 接口

[Connection](https://docs.oracle.com/javase/9/docs/api/java/sql/Connection.html) 接口的实现类对象代表一个与数据库的物理连接会话。

一个应用程序可与单个数据库有一个或多个连接，也可与许多数据库有连接。客户端与数据库所有的交互都是通过 Connection 对象来完成的，该接口的实现类由具体的数据库驱动程序提供。

```java
public interface Connection  extends Wrapper, AutoCloseable {
    // 创建向数据库发送 sql 的 statement 对象
    Statement createStatement() throws SQLException;

    // 创建向数据库发送预编译 sql 的 PrepareSatement 对象
    PreparedStatement prepareStatement(String sql) throws SQLException;

    // 创建执行存储过程的 callableStatement 对象，用于调用存储过程
    CallableStatement prepareCall(String sql) throws SQLException;
    
    // Converts the given SQL statement into the system's native SQL grammar.
    String nativeSQL(String sql) throws SQLException;

    // ...

		/* 控制事务的相关方法 */
    // 设置事务自动提交
    void setAutoCommit(boolean autoCommit) throws SQLException;
		// 创建一个保存点
		Savepoint	setSavepoint​() throws SQLException;
		Savepoint	setSavepoint​(String name) throws SQLException;
    // 提交事务
    void commit() throws SQLException;
		// 设置事务隔离级别
		void setTransactionIsolation​(int level) throws SQLException;
    // 回滚事务 （到指定保存点)
    void rollback() throws SQLException;
		void rollback​(Savepoint savepoint) throws SQLException;

    // ...    
		
		/* JDK7 新增 */
		// 控制 / 获取该 Connection 对象访问的数据库 Schema
		void setSchema​(String schema) throws SQLException;
		String getSchema​() throws SQLException;
		// 控制 / 获取连接的超时行为
		void setNetworkTimeout​(Executor executor, int milliseconds) throws SQLException;
		int	getNetworkTimeout​() throws SQLException;

    // ...    
}

public interface Wrapper {
    <T> T unwrap(java.lang.Class<T> iface) throws java.sql.SQLException;
    boolean isWrapperFor(java.lang.Class<?> iface) throws java.sql.SQLException;
}

public interface AutoCloseable {
    void close() throws Exception;
}
```

## 4. Statement 接口

[Statement](https://docs.oracle.com/javase/9/docs/api/java/sql/Statement.html) 接口的实现类对象用于向数据库发送 sql 语句，对数据库的增删改查都可以通过该对象发送 sql 语句完成，该接口的实现类由具体的数据库驱动程序提供。

```java
public interface Statement extends Wrapper, AutoCloseable {
    /* 执行 SQL 语句 */
		// 用于执行查询语句，返回查询结果集
    ResultSet executeQuery(String sql) throws SQLException;
    // 用于执行 DML 语句（增删改)，返回受影响的结果行数；也可用于执行 DDL 语句，返回 0
    int executeUpdate(String sql) throws SQLException;
    // 可用于执行任意 sql 语句，若第一个结果为 ResultSet 则返回 true，否则返回 false
    boolean execute(String sql) throws SQLException;

		/* 获取执行结果 */
		ResultSet	getResultSet​() throws SQLException; // 获取执行查询语句后返回的 ResultSet 对象
		int	getUpdateCount​() throws SQLException; // 获取执行 DML 语句后所影响的记录行数

		/* 批处理相关方法 */
    void addBatch( String sql ) throws SQLException;
    int[] executeBatch() throws SQLException; // 返回一个 int[] 数组，该数组代表各句 SQL 的返回值
		void clearBatch() throws SQLException;
		
		/* JDK7 新增 */
		// 当所有依赖于该 Statement 对象的 ResultSet 关闭后，该 Statement 对象会自动关闭
		void closeOnCompletion​() throws SQLException;
		boolean	isCloseOnCompletion​() throws SQLException;

		/* JDK8 新增 */
		// 返回 long，当 DML 语句影响的记录行数超过 Integer.MAX_VALUE 时，应使用 executeLargeXxx() 方法
		long[] executeLargeBatch​()	throws SQLException;
		long executeLargeUpdate​(String sql)	throws SQLException;
		long executeLargeUpdate​(String sql, int autoGeneratedKeys)	throws SQLException;
		long executeLargeUpdate​(String sql, int[] columnIndexes)	throws SQLException;
		long executeLargeUpdate​(String sql, String[] columnNames)	throws SQLException;

    // ...
}
```
		
### 4.1. PreparedStatement 接口

[PreparedStatement](https://docs.oracle.com/javase/9/docs/api/java/sql/PreparedStatement.html) 接口由 Statement 接口扩展而来，是一个预编译的 Statement 对象，允许数据库预编译 SQL 语句（通常带有参数），以后每次只改变 SQL 语句的参数即可重复使用。

#### 4.1.1. 常用 API

- 通过 Connection 对象获取 PreparedStatement 对象
	```java
	PreparedStatementstr = con.prepareStatement("update user set id=? where username=?");
	```
	NOTE: 占位符参数只能代替普通值，不能代替表明、列名等数据库对象。

- PreparedStatement 在其父类 Statement 接口的基础上新增了 setXxx​ 方法：
	````java
	void setXxx​(int parameterIndex, Xxx value)
	```
	该系列方法将指定类型的传入值根据参数索引赋值给 SQL 语句中指定位置的参数。若不确定参数的数据类型，可使用`setObject()`方法进行赋值，由 PreparedStatement 对象负责类型转换。

	另外，setXxx 系列方法中包含了多个为 stream 对象设计的接口方法，使得程序能够支持对大文本 Clob、二进制 Blob 数据进行处理。

- PreparedStatement 接口中同样包含了 executeQuery、executeUpdate 和 execute 方法，但这三个方法无须传入 SQL 语句作为参数，因为在获取 PreparedStatement 对象已预编译了 SQL 命令：
	- `ResultSet executeQuery() throws SQLException;`
	- `int executeUpdate() throws SQLException;`
	- `boolean execute() throws SQLException;`

例：
```java
// 模拟查询 id 为 2 的信息
String id = "2";
Connection connection = JDBCUtils.getConnection();
PreparedStatement preparedStatement = connection.preparedStatement("SELECT * FROM users WHERE id = ?"); // ? 为占位符
preparedStatement.setString(1, id);
ResultSet resultSet = preparedStatement.executeQuery(); // executeXxx 方法不再需要传 SQL 参数
if (resultSet.next()) {
		System.out.println(resultSet.getString("name"));
}
// 释放资源
JDBCUtils.release(connection, preparedStatement, resultSet);
```

#### 4.1.2. 类定义

```java
public interface PreparedStatement extends Statement {

}
```

#### 4.1.3. PreperedStatement 和 Statement 的区别

（这个问题在面试时经常问到)
- PreparedStatement 对象避免了数据库对 SQL 语句的重复编译。当需要多次执行同一结构的 SQL 语句时，可以显著地提高效率。

- PreparedStatement 对象在设置 SQL 参数后会自动把内部单引号转义后再添加单引号，因此可以防止 SQL 注入。
	- NOTE: [PreparedStatement 并不能完全防止 SQL 注入](https://www.9sok.com/forum.php?mod=viewthread&tid=199)。例：PreparedStatement 不会对 `%` 进行转义过滤，因此用户可以利用模糊查询耗尽系统资源，从而演化成拒绝服务攻击。

- PreparedStatement 对象支持对大文本 Clob、二进制 Blob 数据进行处理，而 Statement 对象不支持。

- 提高了代码的可读性和可维护性。将参数与 SQL 语句分离出来，这样就可以方便对程序的更改和扩展，同时避免了对字符串参数的多次拼接，可以减少不必要的错误。 

因此，通常推荐使用 PreperedStatement 而不是 Statement 接口对象来执行 SQL 语句。

#### 4.1.4. MySQL 的预编译

https://c0d3p1ut0s.github.io/%E7%AE%80%E5%8D%95%E8%AF%B4%E8%AF%B4MySQL-Prepared-Statement/

https://blog.csdn.net/c929833623lvcha/article/details/44517245

MySQL 官网在 Connector/J 5.0.5 的变更中有如下内容：
> Important change: Due to a number of issues with the use of server-side prepared statements, Connector/J 5.0.5 has disabled their use by default. The disabling of server-side prepared statements does not affect the operation of the connector in any way.
> 
> To enable server-side prepared statements, add the following configuration property to your connector string:
> 
> useServerPrepStmts=true
> 
> The default value of this property is false (that is, Connector/J does not use server-side prepared statements).

MySQL 服务端是在 Connector/J 4.1 版本之后才开始支持预编译的，之后的版本都默认开启预编译。但从 Connector/J 5.05 版本开始，默认情况下 useServerPrepStmts 的值是 false，即默认关闭了服务端预编译。因此，**若我们使用的 MySQL JDBC 驱动是 5.05 之后的版本，要打开预编译功能需要在 JDBC 连接 URL 中设置 useServerPrepStmts 参数值为 true，否则即使客户端使用 PreparedStatement 来执行 SQL 语句，最终到了服务端同样没有预编译的效果**。

除此之外，还可以在 JDBC 连接 URL 中设置`cachePrepStmts=true`，从而开启了缓存预编译的功能，即使 PreparedStatement 对象关闭，之前预编译的 SQL 语句在缓存有效期间同样可以起作用。在测试案例中，开启预编译并且打开预编译缓存的效果明显比不打开预编译的查询性能好 30% 左右。

### 4.2. CallableStatement 接口

[CallableStatement](https://docs.oracle.com/javase/9/docs/api/java/sql/CallableStatement.html) 接口由 PreparedStatement 接口扩展而来，其对象用于执行 SQL 储存过程，即一组可通过名称来调用（类型函数调用）的 SQL 语句。CallableStatement 对象从 PreparedStatement 中继承了用于处理 IN 参数的方法，而且还增加了用于处理 OUT 参数和 INOUT 参数的方法。

#### 4.2.1. 常用 API

- 通过 Connection 对象获取 CallableStatement 对象
	```java
	CallableStatement = con.prepareCall("{call demoSp(?,?)}");
	```
	其中，JDBC API 提供了一个 SQL 存储过程的转义语法：
	```
	{call <procedure-name>[<arg1>,<arg2>, ...]}
	```
	- procedure-name：是所要执行的 SQL 存储过程的名字。
	- [<arg1>,<arg2>, ...]：是相对应的 SQL 存储过程所需要的参数。

- `void setXxx​(int parameterIndex, Xxx value)`: 类似 PreparedStatement 接口中的参数赋值方法，CallableStatement 中同样通过该系列方法将指定类型的传入值根据参数索引赋值给 SQL 语句中指定位置的参数。若不确定参数的数据类型，可使用`setObject()`方法进行赋值，由 PreparedStatement 对象负责类型转换。

- `void	registerOutParameter​(int parameterIndex, int sqlType)`: 若参数是 out 类型，需要在执行存储过程之前进行注册。
	
	例：
	```java
	callableStatement.registerOutParameter(2, Types.VARCHAR);
	```

- `boolean execute() throws SQLException;`: 调用该方法执行存储过程。
	
- `Xxx getXxx​(int parameterIndex)`: 执行完毕后，可通过该方法获取指定传出参数的值（必须在存储过程调用之前注册过才能获取）。

例：
```java
/*
	delimiter $$
			CREATE PROCEDURE demoSp(IN inputParam VARCHAR(255), INOUT inOutParam varchar(255))
			BEGIN
					SELECT CONCAT('zyxw---', inputParam) into inOutParam;
			END $$
	delimiter ;
*/
public static void main(String[] args) {
		try (
				Connection connection = JdbcUtils.getConnection();
				CallableStatement callableStatement = connection.prepareCall("{call demoSp(?,?)}");
		) {
				// 设置输入参数
				callableStatement.setString(1, "nihaoa");
				// 注册输出参数，类型是 VARCHAR
				callableStatement.registerOutParameter(2, Types.VARCHAR);
				// 调用存储过程
				callableStatement.execute();
				// 获取传出参数的值
				System.out.println(callableStatement.getString(2));
				e.printStackTrace();
		}
}
```

#### 4.2.2. 类定义

```java
public interface CallableStatement extends PreparedStatement {

}
```

## 5. ResultSet 接口

[ResultSet](https://docs.oracle.com/javase/9/docs/api/java/sql/ResultSet.html) 接口的实现类对象代表 Sql 语句的执行结果，当 Statement 对象执行 executeQuery() 方法时，会返回一个 ResultSet 对象。

### 5.1. 基本特性

ResultSet 接口的实现类对象具有以下 2 个特性：
- 可滚动

	ResultSet 可滚动指的是 ResultSet 对象中包含了多个常用方法可用来移动记录指针，并通过列索引或列名获取列数据。

- 可更新

	ResultSet 可更新指的是可通过 `updateXxx(int columnIndex, Xxx value)` 来修改记录指针指向的记录、特定列的值，并调用 `updateRow()` 方法提交修改到数据库中。

	以默认方式获取的 ResultSet 结果集对象是不可更新的，但若在创建 Statement/PreparedStatement 对象时传入额外参数，则该对象执行 SQL 语句后返回的 ResultSet 结果集对象就是可更新的：
	- `Statement createStatement(int resultSetType, int resultSetConcurrency) throws SQLException;`
	- `PreparedStatement prepareStatement(String sql, int resultSetType, int resultSetConcurrency) throws SQLException;`
	其中，额外传入的 2 个参数为：
	- resultSetType: 控制 ResultSet 类型，可取以下 3 个值：
		- ResultSet.TYPE_FORWARD_ONLY: 该常量控制记录指针只能向前移动。
    - ResultSet.TYPE_SCROLL_INSENSITIVE: 该常量控制记录指针可以自由移动，但底层数据的改变不会影响 ResultSet 的内容。
		- ResultSet.TYPE_SCROLL_SENSITIVE: 该常量控制记录指针可以自由移动，且底层数据的改变会影响 ResultSet 的内容。
		
	- resultSetConcurrency: 控制 ResultSet 的并发类型，可取以下 2 个值：
		- ResultSet.CONCUR_READ_ONLY: 该常量指示 ResultSet 是只读的并发模式（默认）。
    - ResultSet.CONCUR_UPDATABLE: 该常量指示 ResultSet 是可更新的并发模式。

	需要注意的是，可更新的结果集对象必须要满足以下条件：
	- 所有数据都来自同一个数据表。
	- 选出的数据集必须包含主键。
	若不满足以上任一条件，执行更新时将会引起更新失败。

### 5.2. 类定义

```java
public interface ResultSet extends Wrapper, AutoCloseable {
    // 获取任意类型的数据
    Object getObject(String columnLabel) throws SQLException;

    // 获取指定类型的数据
    String getString(String columnLabel) throws SQLException;

    // 关闭结果集资源
    void close() throws SQLException;

    /* 记录指针操作的相关方法 */
    boolean next() throws SQLException; // 将指针移动到下一行；若移动后指针指向一条有效记录，则返回 true
    boolean previous() throws SQLException; // 将指针移动到上一行；若移动后指针指向一条有效记录，则返回 true
    boolean absolute( int row ) throws SQLException; // 将指针移动到第 row 行，若 row 为负数，则移动到倒数第 row 行；若移动后指针指向一条有效记录，则返回 true
    void beforeFirst() throws SQLException; // 将指针移动到首行之前（即恢复到初始状态）
		boolean first() throws SQLException; // 将指针移动到首行；若移动后指针指向一条有效记录，则返回 true
		boolean last() throws SQLException; // 将指针移动到末行；若移动后指针指向一条有效记录，则返回 true
    void afterLast() throws SQLException; // 将指针移动到末行之后

		/* 当记录指针移动到指定行之后，可通过 getXxx(int columnIndex) 或 getXxx(String columnLabel) 方法来获取当前行、指定列的值 */
		String getString(int columnIndex) throws SQLException;
		String getString(String columnLabel) throws SQLException;
		boolean getBoolean(int columnIndex) throws SQLException;
		boolean getBoolean(String columnLabel) throws SQLException;
		byte getByte(int columnIndex) throws SQLException;
		byte getByte(String columnLabel) throws SQLException;
    // ...
}
```

## 6. 元数据接口

元数据是描述其它数据的数据，即数据库、表、列的定义信息，JDBC 中提供了 3 个可用于获取元数据的接口：
- `ParameterMetaData`: 参数的元数据，封装了描述 Parameter 的数据。
- `ResultSetMetaData`: 结果集的元数据，封装了描述 ResultSet 对象的数据。
- `DataBaseMetaData`: 数据库的元数据，封装了描述 Database 的数据。

### 6.1. ParameterMetaData 接口

https://docs.oracle.com/javase/9/docs/api/java/sql/ParameterMetaData.html

### 6.2. ResultSetMetaData 接口

通过使用 ResultSet 接口类的对象我们可以获取查询结果集中的数据，但 ResultSet 功能有限，如果我们想要得到诸如查询结果集中有多少列、每个列的名称以及每个数据类的数据类型等描述信息，就必须使用 [ResultSetMetaData](https://docs.oracle.com/javase/9/docs/api/java/sql/ResultSetMetaData.html) 接口了。

NOTE: 使用 ResultSetMetaData 需要一定的系统开销，因此如果在编程过程中已经知道 ResultSet 中的元数据信息，就没有必要使用 ResultSetMetaData 来分析该 ResultSet 对象了。

- 通过 ResultSet 对象中的 getMataData() 方法，即可获取该对象对应的 ResultSetMetaData 对象。
	```java
	ResultSetMetaData rsmd = rst.getMetaData();
	```

- 类定义

	```java
	public interface ResultSetMetaData extends Wrapper {
			// 返回所有字段的数目
			public int getColumCount() throws SQLException;

			// 根据字段的索引值取得字段的名称
			public String getColumName (int colum) throws SQLException;

			// 根据字段的索引值取得字段的类型
			public String getColumType (int colum) throws SQLException;

			// ...
	}
	```

### 6.3. DataBaseMetaData 接口

https://docs.oracle.com/javase/9/docs/api/java/sql/DatabaseMetaData.html

通过 Connection 接口的方法获取 DataBaseMetaData 对象：
```java
DatabaseMetaData getMetaData() throws SQLException;
```

## 7. RowSet 接口

javax.sql.[RowSet](https://docs.oracle.com/javase/9/docs/api/javax/sql/RowSet.html) 接口继承自 ResultSet 接口，并在 javax.sql.rowset 包下包含了多个常用子接口，主要包括 CachedRowSet，WebRowSet，FilteredRowSet，JoinRowSet 和 JdbcRowSet。其中，除了 JdbcRowSet 需要保持与数据源的连接之外，其余四个都是离线 RowSet，即无须保持与数据库的连接。

- 发展历史
	
	RowSet 接口及 javax.sql.rowset 包中的子接口自 JDK 1.4 引入，从 JDK 5.0 开始在 com.sun.rowset 包下提供了参考实现，但由于这些参考实现属于内部 API，且可能在未来版本中删除，若使用会导致代码与 JDK 版本的高度耦合，RowSet 系列接口一直得不到广泛应用。
	
	从 JDK7 开始，新增了 [RowSetProvider](https://docs.oracle.com/javase/9/docs/api/javax/sql/rowset/RowSetProvider.html) 类和 [RowSetFactory](https://docs.oracle.com/javase/9/docs/api/javax/sql/rowset/RowSetFactory.html) 接口，通过工厂设计模式将程序与 RowSet 实现类分离开，避免直接使用非公开的实现类，有利于后期升级和维护，从而使得 RowSet 系列接口得以广泛使用。 

- RowSet 规范的接口类图：

	![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/5/16/6f06a5e694b29ffc34472dfd91f9a73a.jpg)

- 与 ResultSet 接口的区别：
	
	- RowSet 默认是可滚动、可更新、可序列化的结果集，且可作为 JavaBean 使用，因而能方便地在网络上进行传输，用于同步两端的数据。
	
	- 在使用 ResultSet 的时代，程序查询到 ResultSet 之后必须立即读取或处理它包含了记录，否则一旦 Connection 关闭，就无法再通过 ResultSet 读取记录。在这种模式下，JDBC 编程中对 ResultSet 的处理通常有以下 2 种方式：
		- 直接将 ResultSet 传到视图层 / 逻辑层进行处理，但要求底层 Connection 必须保持打开状态，否则会导致异常。这种方式不仅不安全，对性能也有很大影响。
		- 迭代访问 ResultSet 中的记录，并将这些记录转换成 JavaBean，再将多个 JavaBean 封装为一个 List 集合。转换完成后才可以关闭 Connection 等资源，之后将 JavaBean 集合传到视图层 / 逻辑层进行进一步的业务处理。这种方式比较安全，但非常繁琐。
		
		对于离线 RowSet ( CachedRowSet 接口及其子接口 ) 而言，程序在创建 RowSet 对象时已将数据从底层数据库读取到了内存中，封装成 RowSet 对象，从而可以充分利用计算机内存，降低与数据库服务器保持长连接的负载消耗，提高程序性能。而且，RowSet 对象可以直接作为 JavaBean 使用，因此不仅安全，且编程十分简洁方便。

- 创建 RowSet 对象

	使用 JDK7 新增的工厂模式 API 进行创建：
	```java
	ResultSet rs = stmt.executeQuery(sql);
	RowSetFactory factory = RowSetProvider.newFactory(); // 使用 RowSetProvider 的静态方法创建 RowSet 工厂类
	CachedRowSet cachedRs = factory.createCachedRowSet(); // 使用 RowSet 工厂类创建 RowSet 实现类对象
	cachedRs.populate(rs); // 使用 ResultSet 装填 RowSet
	```
	RowSetFactory 接口方法：
	- `CachedRowSet	createCachedRowSet​()`:	Creates a new instance of a CachedRowSet.
	- `FilteredRowSet	createFilteredRowSet​()`:	Creates a new instance of a FilteredRowSet.
	- `JdbcRowSet	createJdbcRowSet​()`:	Creates a new instance of a JdbcRowSet.
	- `JoinRowSet	createJoinRowSet​()`:	Creates a new instance of a JoinRowSet.
	- `WebRowSet	createWebRowSet​()`:	Creates a new instance of a WebRowSet.

### 7.1. JdbcRowSet 接口

[JdbcRowSet](https://docs.oracle.com/javase/9/docs/api/javax/sql/rowset/JdbcRowSet.html) 接口是对 ResultSet 的一个封装，使其能够作为 JavaBeans 被使用，是唯一一个保持数据库连接的 RowSet。

### 7.2. CachedRowSet 接口

[CachedRowSet](https://docs.oracle.com/javase/9/docs/api/javax/sql/rowset/CachedRowSet.html) 接口最常用的一种 RowSet，WebRowSet、FilteredRowSet、JoinRowSet 都是直接或间接继承于它并进行了扩展。

CachedRowSet 具有以下特性：
- 提供了对数据库的离线操作，可以将数据读取到内存中进行增删改查，再同步到数据源。
- 可串行化，可作为 JavaBeans 在网络间传输。
- 支持事件监听。
- 支持分页。由于 CachedRowSet 会将数据记录直接装载到内存中，因此若 SQL 查询结果过大，可能会导致内存溢出的问题。为此，CachedRowSet 提供了分页的支持（setPageSize(int pageSize)、previousPage()、nextPage() 等方法），即每次只装载 ResultSet 中的某一部分记录，从而避免 CachedRowSet 占用过多内存的问题。

#### 7.2.1. WebRowSet 接口

[WebRowSet](https://docs.oracle.com/javase/9/docs/api/javax/sql/rowset/WebRowSet.html) 接口继承自 CachedRowSet，并可以将 WebRowSet 写到 XML 文件中，也可以用符合规范的 XML 文件来填充 WebRowSet。

#### 7.2.2. JoinRowSet 接口

[JoinRowSet](https://docs.oracle.com/javase/9/docs/api/javax/sql/rowset/JoinRowSet.html) 接口继承自 CachedRowSet，提供类似 SQL JOIN 的功能，将不同的 RowSet 中的数据组合起来。

#### 7.2.3. FilteredRowSet 接口

[FilteredRowSet](https://docs.oracle.com/javase/9/docs/api/javax/sql/rowset/FilteredRowSet.html) 接口继承自 CachedRowSet，通过设置 Predicate（在 javax.sql.rowset 包中），提供数据过滤的功能，可以根据不同的条件对 RowSet 中的数据进行筛选和过滤。

## 8. MySQL Connector/J 实现类

https://github.com/mysql/mysql-connector-j/tree/release/8.0

### 8.1. ConnectionImpl 类

ConnectionImpl 类是 MySQL 连接的直接封装类。数据库连接的本质其实就是客户端维持了一个和远程 MySQL 服务器的一个 **TCP 长连接**，并且在此连接上维护了一些信息。
```java
public class ConnectionImpl implements JdbcConnection, SessionEventListener, Serializable {
    // ... 
    public ConnectionImpl(HostInfo hostInfo) throws SQLException {
        try {
            // Stash away for later, used to clone this connection for Statement.cancel and Statement.setQueryTimeout().
            // ... 省略一堆内部属性赋值操作
        } catch (CJException e1) {
            throw SQLExceptionsMapping.translateException(e1, getExceptionInterceptor());
        }

        try {
            createNewIO(false);
            unSafeQueryInterceptors();
            NonRegisteringDriver.trackConnection(this);
        } catch (SQLException ex) {
            cleanup(ex);
            // don't clobber SQL exceptions
            throw ex;
        } catch (Exception ex) {
            cleanup(ex);
            throw SQLError
                    .createSQLException(
                            this.propertySet.getBooleanReadableProperty(PropertyDefinitions.PNAME_paranoid).getValue() ? Messages.getString("Connection.0")
                                    : Messages.getString("Connection.1",
                                            new Object[] { this.session.getHostInfo().getHost(), this.session.getHostInfo().getPort() }),
                            MysqlErrorNumbers.SQL_STATE_COMMUNICATION_LINK_FAILURE, ex, getExceptionInterceptor());
        }

    }
    public void createNewIO(boolean isForReconnect) {
        synchronized (getConnectionMutex()) {
            // Synchronization Not needed for *new* connections, but defintely for connections going through fail-over, since we might get the new connection up
            // and running *enough* to start sending cached or still-open server-side prepared statements over to the backend before we get a chance to
            // re-prepare them...

            try {
                if (!this.autoReconnect.getValue()) {
                    connectOneTryOnly(isForReconnect);
                    return;
                }

                connectWithRetries(isForReconnect);
            } catch (SQLException ex) {
                throw ExceptionFactory.createException(UnableToConnectException.class, ex.getMessage(), ex);
            }
        }
    }
    public void unSafeQueryInterceptors() throws SQLException {
        this.queryInterceptors = this.queryInterceptors.stream().map(u -> ((NoSubInterceptorWrapper) u).getUnderlyingInterceptor())
                .collect(Collectors.toList());

        if (this.session != null) {
            this.session.setQueryInterceptors(this.queryInterceptors);
        }
    }

    // 获取 MySQL 连接的实例对象，在 NonRegisteringDriver 类的 connect() 方法中调用
    public static JdbcConnection getInstance(HostInfo hostInfo) throws SQLException {
        return new ConnectionImpl(hostInfo);
    }

    // ...
}
```

### 8.2. NonRegisteringDriver 类

> The Java SQL framework allows for multiple database drivers. Each driver should supply a class that implements the Driver interface. The DriverManager will try to load as many drivers as it can find and then for any given connection request, it will ask each driver in turn to try to connect to the target URL.It is strongly recommended that each Driver class should be small and standalone so that the Driver class can be loaded and queried without bringing in vastquantities of supporting code.When a Driver class is loaded, it should create an instance of itself and register it with the DriverManager. This means that a user can load and register adriver by doing Class.forName("foo.bah.Driver").

```java
public class NonRegisteringDriver implements java.sql.Driver {
    // ....

    // 执行连接逻辑
    public java.sql.Connection connect(String url, Properties info) throws SQLException {
        try {
            ConnectionUrl conStr = ConnectionUrl.getConnectionUrlInstance(url, info);
            if (conStr.getType() == null) {
                /*
                 * According to JDBC spec:
                 * The driver should return "null" if it realizes it is the wrong kind of driver to connect to the given URL. This will be common, as when the
                 * JDBC driver manager is asked to connect to a given URL it passes the URL to each loaded driver in turn.
                 */
                return null;
            }

            switch (conStr.getType()) {
                case LOADBALANCE_CONNECTION:
                    return LoadBalancedConnectionProxy.createProxyInstance((LoadbalanceConnectionUrl) conStr);

                case FAILOVER_CONNECTION:
                    return FailoverConnectionProxy.createProxyInstance(conStr);

                case REPLICATION_CONNECTION:
                    return ReplicationConnectionProxy.createProxyInstance((ReplicationConnectionUrl) conStr);

                case XDEVAPI_SESSION:
                    // TODO test it
                    //return new XJdbcConnection(conStr.getProperties());

                default:
                    // 获取 MySQL 连接的实例对象
                    return com.mysql.cj.jdbc.ConnectionImpl.getInstance(conStr.getMainHost());

            }

        } catch (CJException ex) {
            throw ExceptionFactory.createException(UnableToConnectException.class,
                    Messages.getString("NonRegisteringDriver.17", new Object[] { ex.toString() }), ex);
        }
    }
}
```
### 8.3. Driver 类

```java
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    // 在 Driver 的静态代码块里，调用了 DriverManager 的 registerDriver 方法来进行注册
    static {
        try {
            java.sql.DriverManager.registerDriver(new Driver());
        } catch (SQLException E) {
            throw new RuntimeException("Can't register driver!");
        }
    }

    public Driver() throws SQLException {
        // Required for Class.forName().newInstance()
    }
}
```
因此，**当我们在程序中调用 Class.forName("com.mysql.jdbc.Driver") 后，com.mysql.jdbc.Driver 类就会被加载，同时也在静态代码块中完成了向 DriverManager 的注册**。

## 9. Refer Links

[JDBC 第一篇 --【介绍 JDBC、使用 JDBC 连接数据库、简单的工具类】](https://zhongfucheng.bitcron.com/post/jdbc/jdbcdi-yi-pian-jie-shao-jdbc-shi-yong-jdbclian-jie-shu-ju-ku-jian-dan-de-gong-ju-lei)

[JDBC 第二篇 --【PreparedStatment、批处理、处理二进制、自动主键、调用存储过程、函数】](https://zhongfucheng.bitcron.com/post/jdbc/jdbcdi-er-pian-preparedstatment-pi-chu-li-chu-li-er-jin-zhi-zi-dong-zhu-jian-diao-yong-cun-chu-guo-cheng-han-shu)

[JDBC 源码解析（一）：示例 +Driver 注册流程](https://www.jianshu.com/p/ae7efd78202f?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)

[JDBC 源码解析（二）：获取 connection](https://www.jianshu.com/p/e2d267497cf6)

[MySQL 的 JDBC 驱动源码解析](https://blog.csdn.net/c929833623lvcha/article/details/44517245)