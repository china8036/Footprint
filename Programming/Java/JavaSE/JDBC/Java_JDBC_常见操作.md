- [Java JDBC API: 常见操作](#java-jdbc-api)
	- [1. 一般步骤](#1)
	- [2. 处理大文本和二进制数据](#2)
		- [2.1. 处理 clob 数据](#21--clob)
		- [2.2. 处理 blob 数据](#22--blob)
	- [3. 获取自增主键值](#3)
	- [4. 事务处理](#4)
	- [5. 批处理](#5)
	- [6. Refer Links](#6-refer-links)

# Java JDBC API: 常见操作

## 1. 一般步骤

通过 JDBC 操作数据库一般需要以下步骤：
1. 注册 / 加载 JDBC 驱动程序
  
    注册驱动有三种方式：
    ```java
    // method 1 推荐这种方式，不会对具体的驱动类产生依赖，且驱动注册操作只会进行 1 次
    Class.forName(“com.mysql.jdbc.Driver”);
    // method 2 会对具体的驱动类的 API 产生依赖，且会导致驱动注册操作进行 2 次
    DriverManager.registerDriver(com.mysql.jdbc.Driver);
    // method 3 将驱动程序添加到 Java.lang.System 的属性 jdbc.drivers 中，虽然不会对具体的驱动类产生依赖；但注册不太方便，所以很少使用
    System.setProperty(“jdbc.drivers”, “driver1:driver2”);
    ```
    其中，不同数据库系统的驱动类字符串各有不同，需要根据不同数据库厂商提供的 API 文档来确定。以下是常见的驱动类字符串：
    - MySQL 驱动：`com.mysql.jdbc.Driver`
    - Oracle 驱动：`oracle.jdbc.driver.OracleDriver`
    - SQL Server 驱动：`com.microsoft.sqlserver.jdbc.SQLServerDriver`

1. 建立连接对象 

    通过 Connection 建立连接，Connection 是一个接口类，其功能是与数据库进行连接（会话）：
    ```java
    Connection conn = DriverManager.getConnection(url, user, password);
    ```
    其中：
    - [URL](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-reference-jdbc-url-format.html) 中的 `jdbc:` 是固定的，但其余部分根据不同数据库而不同。以下是常见数据库的连接 URL 格式：
      - MySQL 的连接 URL: `jdbc:mysql://hostname:port/database?field1=value1&field2=value2`
      - Oracle 的连接 URL: `jdbc:oracle:thin:@hostname:port:databasename?field1=value1&field2=value2`
    - user 即为登录数据库的用户名，如 root。
    - password 即为登录数据库的密码，为空就填”。

1. 获取 SQL 执行对象

    SQL 执行接口 Statement 及其派生出的两个接口 PreparedStatement 和 CallableStatement，都可用于执行 SQL 语句，由 Connection 对象产生：
		- `Statement createStatement() throws SQLException;`: 用于创建 Statement 对象执行简单的 SQL 语句（不带参数）。
		- `PreparedStatement prepareStatement(String sql) throws SQLException;`: 用于创建 PreparedStatement 对象执行带一个或多个 IN 参数的 SQL 语句或经常被执行的简单 SQL 语句。
		- `CallableStatement prepareCall(String sql) throws SQLException;`: 用于创建 CallableStatement 对象调用储存过程。

1. 处理执行结果集

    ResultSet 接口的实现类对象负责保存 Statement 执行后所产生的查询结果。

    ResultSet 接口的实现类对象是通过游标来操作的，游标就是一个可控制的、可以指向任意一条记录的指针。通过这个指针，我们可以轻易地指出我们要对结果集中的哪一条记录进行修改、删除，或者要在哪一条记录之前插入数据。一个结果集对象中只包含一个游标。
    ```java
    ResultSet resultSet = statement.executeQuery("SELECT * FROM users");
    // 遍历结果集，得到数据
    while (resultSet.next()) {
        System.out.println(resultSet.getString(1));
        System.out.println(resultSet.getString(2));
    }
    ```

1. 关闭资源对象

    Connection 对象的 close 方法用于关闭连接，并释放和连接相关的资源。关闭资源时，需要判断对象是否存在，后调用的先关闭：
    ```java
    if (resultSet != null) {
        try {
            resultSet.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    if (statement != null) {
        try {
            statement.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    if (connection != null) {
        try {
            connection.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    ```
    **从 JDK7 开始，可通过自动关闭资源的 try 语句来关闭各种数据库资源。Connection、Statement、ResultSet 接口都继承了 AutoClosable 接口，因此都可以由 try 语句来自动关闭**。

## 2. 处理大文本和二进制数据

MySQL 等关系数据库中一般支持大文本数据类型 clob(character long object) 和二进制数据类型 blob(binary long object)，可用于在数据库中存储大文本或文件（图片、音频、视频）数据。若要通过 JDBC 对这种数据进行操作，如果使用 String 类型参数来接收，极有可能会导致内存溢出。

- 插入数据

	在 PreparedStatement 接口中定义了可使用流类型的参数进行赋值的系列方法，因此，可通过 PreparedStatement 对象的这些方法向数据库中插入大文本和二进制数据。
	
	PreparedStatement API:
	- `void setBinaryStream(int parameterIndex, java.io.InputStream x) throws SQLException;`
	- `void setCharacterStream(int parameterIndex, java.io.Reader reader) throws SQLException;`
	- `void setBlob (int parameterIndex, Blob x) throws SQLException;`
	- `void setClob (int parameterIndex, Clob x) throws SQLException;`

- 查询数据
	
	可调用 ResultSet 接口的 getBlob()/getClob() 方法获取一个 Blob/Clob 对象，再调用 Blob/Clob 对象的 getBinaryStream()/getBytes() 方法获取该数据的输入流 / 二进制数据。也可直接调用 getCharacterStream()/getBinaryStream() 方法获取结果集对象的输入流 / 二进制数据。

	ResultSet API:
	- `Blob getBlob(int columnIndex) throws SQLException;`
	- `Blob getBlob(String columnLabel) throws SQLException;`
	- `Clob getClob(int columnIndex) throws SQLException;`
	- `Clob getClob(String columnLabel) throws SQLException;`
	- `java.io.Reader getCharacterStream(int columnIndex) throws SQLException;`
	- `java.io.Reader getCharacterStream(String columnLabel) throws SQLException;`
	- `java.io.InputStream getBinaryStream(int columnIndex) throws SQLException;`
	- `java.io.InputStream getBinaryStream(String columnLabel) throws SQLException;`

### 2.1. 处理 clob 数据

- 向数据库插入 clob 数据
	```java
	@Test
	public void add() {
			try (
					Connection connection = JdbcUtils.getConnection();;
					PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO test2 (bigTest) VALUES(?) ");
					// 获取到文件的路径
					String path = Demo.class.getClassLoader().getResource("BigTest").getPath();
					File file = new File(path);
					FileReader fileReader = new FileReader(file);
			) {
					preparedStatement.setCharacterStream(1, fileReader, file.length());
					if (preparedStatement.executeUpdate() > 0) {
							System.out.println("插入成功");
					}
			}
	}
	```

- 从数据库中获取 clob 数据
	```java
	@Test
	public void read() {
			try (
					Connection connection = JdbcUtils.getConnection();;
					PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM test2");
					ResultSet resultSet = preparedStatement.executeQuery();					
			) {
					if (resultSet.next()) {
						try (
							Reader reader = resultSet.getCharacterStream("bigTest");
							FileWriter fileWriter = new FileWriter("d:\\abc.txt");
						) {
							char[] chars = new char[1024];
							int len = 0;
							while ((len = reader.read(chars)) != -1) {
									fileWriter.write(chars, 0, len);
									fileWriter.flush();
							}
						}
					}
			}
	}
	```

### 2.2. 处理 blob 数据

- 向数据库插入 blob 数据
	```java
	@Test
	public void add() {
			try (
					Connection connection = JdbcUtils.getConnection();;
					PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO test3 (blobtest) VALUES(?)");
			) {
					// 获取文件的路径和文件对象
					String path = Demo6.class.getClassLoader().getResource("1.wmv").getPath();
					File file = new File(path);
					// 调用方法
					preparedStatement.setBinaryStream(1, new FileInputStream(path), (int)file.length());
					if (preparedStatement.executeUpdate() > 0) {
							System.out.println("添加成功");
					}
			}
	}
	```

- 从数据库中获取 blob 数据
	```java
	@Test
	public void read() {
			try (
					Connection connection = JdbcUtils.getConnection();;
					PreparedStatement preparedStatement = connection.prepareStatement("SELECT * FROM test3");
					ResultSet resultSet = preparedStatement.executeQuery();
			) {
					// 如果读取到数据，就把数据写到磁盘下
					if (resultSet.next()) {
							try (
									InputStream inputStream = resultSet.getBinaryStream("blobtest");
									FileOutputStream fileOutputStream = new FileOutputStream("d:\\aa.jpg");								
							) {
									int len = 0;
									byte[] bytes = new byte[1024];
									while ((len = inputStream.read(bytes)) > 0) {
											fileOutputStream.write(bytes, 0, len);
									}
							}
					}
			}
	}
	```

## 3. 获取自增主键值

当向数据库表中插入一条新记录后，很多情况下我们希望获取刚刚插入的记录的自增主键值，此时可通过以下方法：
- 通过 Connection 接口的重载方法`PreparedStatement prepareStatement(String sql, int autoGeneratedKeys)`创建 PreparedStatement 对象，传入 Statement.RETURN_GENERATED_KEYS 常量值，则该对象执行 SQL 插入后会返回自动生成主键值。

- 执行 SQL 插入后，通过 Statement 对象的`ResultSet getGeneratedKeys()`方法，获取自动主键列的值。
	```java
	@Test
	public void test() {
		try (
				Connection connection = JdbcUtils.getConnection();
				PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO test(name) VALUES(?)");
		) {
				preparedStatement.setString(1, "ouzicheng");
				if (preparedStatement.executeUpdate() > 0) {
						// 获取到自动主键列的值
						try (
								ResultSet resultSet = preparedStatement.getGeneratedKeys();
						) {
								if (resultSet.next()) {
										int id = resultSet.getInt(1);
										System.out.println(id);
								}
						}	
				}
		}
	}
	```

## 4. 事务处理

JDBC 提供了数据库的事务支持，由 Connection 对象进行管理。

- 开启 / 关闭事务

	Connection 对象默认关闭了事务支持，可通过调用以下方法关闭自动提交，即开启事务：
	```java
	conn.setAutoCommit(false);
	```
	可通过 Connection 的 `boolean getAutoCommit()` 方法获取该连接是否开启了事务。

- 提交事务

	一旦事务启动后，程序使用 Statement 对象执行 SQL 语句时都不会立即生效，直到程序调用 Connection 的 commit() 方法提交事务，SQL 语句才会真正生效。

- 回滚事务

	Connection 提供了 2 个方法可用于事务回滚：
	- `void rollback()`: 使事务回滚至初始阶段。
	- `void rollback(Savepoint savepoint)`: 使事务回滚至指定中间保存点处。
	实际上，若 Connection 出现一个未捕获的 SQLException 异常时，程序也会自动回滚；但若程序捕获了该异常，则需要进行显式的回滚。

- 创建保存点

	Connection 提供了 2 个方法可用于创建中间保存点：
	- `Savepoint setSavepoint()`: 在当前务中创建一个未命名的中间保存点，并返回该保存点的 Savepoint 对象。
	- `Savepoint setSavepoint(String name)`: 在当前事务中创建一个指定命名的中间保存点，并返回该保存点的 Savepoint 对象。
	实际上，Connection 回滚至指定中间点时是根据 Savepoint 对象而不是名字来回滚的，因此设置中间保存点时没有太大必要指定名称。

- 事务隔离级别

Connection 接口对事务的隔离级别控制也提供了支持：
- `void setTransactionIsolation(int level) throws SQLException;`: 设置事务隔离级别。
	
	对应数据库系统中的 4 个隔离级别，Connection 接口中定义了相应的 4 个常量：
	```java
	int TRANSACTION_READ_UNCOMMITTED = 1; // Serializable【可避免脏读，不可重复读，虚读】

	int TRANSACTION_READ_COMMITTED   = 2; // Repeatable read【可避免脏读，不可重复读】

	int TRANSACTION_REPEATABLE_READ  = 4; // Read committed【可避免脏读】

	int TRANSACTION_SERIALIZABLE     = 8; // Read uncommitted【级别最低，什么都避免不了】
	```

- `int getTransactionIsolation() throws SQLException;`: 获取事务隔离级别。

## 5. 批处理

当需要向数据库发送一系列 SQL 语句执行时，应避免向数据库一条条发送执行，此时可以采用批处理以提升执行效率。Statement 接口及其扩展接口 PreparedStatement 接口都支持批处理操作。

NOTE: 批处理操作不适用于查询语句，若批处理的 SQL 语句中包含了 SELECT 查询语句，则将导致程序出现错误。

例 1：以 Statement 方式实现批处理
```java
try (
		Connection connection = JDBCUtils.getConnection();
		Statement statement = connection.createStatement();
) {
		String sql1 = "UPDATE users SET name='zhongfucheng' WHERE id='3'";
		String sql2 = "INSERT INTO users (id, name, password, email, birthday) VALUES('5','nihao','123','ss@qq.com','1995-12-1')";
		// 将多条 sql 添加到批处理
		statement.addBatch(sql1);
		statement.addBatch(sql2);
		// 执行批处理
		statement.executeBatch();
		// 清空批处理的 sql
		statement.clearBatch();
}
```

例 2：以 PreparedStatement 方式实现批处理
```java
try (
		Connection connection = JDBCUtils.getConnection();
		PreparedStatement preparedStatement = connection.prepareStatement("INSERT INTO test(id,name) VALUES (?,?)");
) {
		for (int i = 1; i <= 205; i++) {
				preparedStatement.setInt(1, i);
				preparedStatement.setString(2, (i + "zhongfucheng"));
				// 将当前的 sql 添加到批处理中
				preparedStatement.addBatch();
				if (i % 2 == 100) {
						// 执行批处理
						preparedStatement.executeBatch();
						// 清空批处理，如果数据量太大，所有数据存入批处理，会导致内存溢出
						preparedStatement.clearBatch();
				}
		}
		// 不是所有的 %2==100，剩下的再执行一次批处理
		preparedStatement.executeBatch();
		preparedStatement.clearBatch();
}
```

例 3：为使批处理操作可以正确处理错误情况，可以使用事务来管理批处理操作。
```java
boolean autoCommit = conn.getAutoCommit(); // 保存原有模式
conn.setAutoCommit(false);
Statement stmt = conn.createStatement();
stmt.addBatch(sql1);
stmt.addBatch(sql2);
//...
stmt.executeBatch();
conn.commit();
conn.setAutoCommit(autoCommit); // 恢复原有模式
```

## 6. Refer Links

[JDBC 第一篇 --【介绍 JDBC、使用 JDBC 连接数据库、简单的工具类】](https://zhongfucheng.bitcron.com/post/jdbc/jdbcdi-yi-pian-jie-shao-jdbc-shi-yong-jdbclian-jie-shu-ju-ku-jian-dan-de-gong-ju-lei)

[JDBC 第二篇 --【PreparedStatment、批处理、处理二进制、自动主键、调用存储过程、函数】](https://zhongfucheng.bitcron.com/post/jdbc/jdbcdi-er-pian-preparedstatment-pi-chu-li-chu-li-er-jin-zhi-zi-dong-zhu-jian-diao-yong-cun-chu-guo-cheng-han-shu)

[JDBC 第三篇 --【事务、元数据、改造 JDBC 工具类】](https://zhongfucheng.bitcron.com/post/jdbc/jdbcdi-san-pian-shi-wu-yuan-shu-ju-gai-zao-jdbcgong-ju-lei)