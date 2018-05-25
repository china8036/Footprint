- [Java JDBC: 数据库连接池](#java-jdbc)
    - [1. 基本概念](#1)
    - [2. 开源连接池](#2)
    - [3. 连接池实现](#3)
    - [4. Refer Links](#4-refer-links)

# Java JDBC: 数据库连接池

## 1. 基本概念

数据库连接的建立和关闭是极耗费资源的操作，在多层结构的应用环境中，这种资源的耗费对系统性能影响尤为明显。

类比线程池，产生了数据库连接池的解决方案：当应用程序启动时，系统主动建立足够的数据库连接，并将这些连接组成一个连接池。每次应用程序请求数据库连接时，无须重新打开连接，而是从连接池中取出已有的连接使用，使用完后不再关闭数据库连接，而是直接将连接归还给连接池。通过使用连接池，将大大提高程序的运行效率。

实际上，这是一种通用的设计模式：资源池模式，用于解决资源的频繁请求、释放所造成的性能下降问题。

数据库连接池常用的参数如下：
- 数据库的初始连接数
- 连接池的最大连接数
- 连接池的最小连接数
- 连接池的扩容幅度

除了节省连接建立、关闭的资源消耗，数据库连接池还可以提供以下功能：
- 缓存 PreparedStatement 以便更快的执行。
- 可以设置连接超时时间。
- 提供日志记录的功能。
- 设置 ResultSet 大小的最大阈值设置。
- 通过 JNDI 的支持，可以为 servlet 容器提供连接池的功能。

JDBC 2.0 规范引入了数据库连接池，以作为 Connection 对象的工厂。数据库连接池使用 javax.sql.[DataSource](https://docs.oracle.com/javase/9/docs/api/javax/sql/DataSource.html) 接口表示，为 JDBC 提供连接池都必须实现该接口，如商用服务器 WebLogic、WebSphere 的实现，开源组织的实现 DBCP、C3P0、HikariCP、Druid 等。

## 2. 开源连接池

例：使用 C3P0 连接池
```java
/** 步骤：
  * 1. 导入开发包【c3p0-0.9.2-pre1.jar】和【mchange-commons-0.2.jar】
  * 2. 导入 XML 配置文件【可以在程序中自己一个一个配，C3P0 的 doc 中的 Configuration 有 XML 文件的事例】
  * 3. new 出 ComboPooledDataSource 对象
  */
private static ComboPooledDataSource comboPooledDataSource = null;

static {
    // 通过静态代码块确保整个应用只产生一个连接池实例
    comboPooledDataSource = new ComboPooledDataSource();
    comboPooledDataSource.setDriverClass("com.mysql.jdbc.Driver");
    comboPooledDataSource.setJdbcUrl("jdbc://....");
    comboPooledDataSource.setUser("root");
    comboPooledDataSource.setPassword("toor");
    comboPooledDataSource.setMaxPoolSize(50);
    comboPooledDataSource.setMinPoolSize(2);
    comboPooledDataSource.setInitialPoolSize(10);
    comboPooledDataSource.setMaxStatement(180);
}

public static Connection getConnection() throws SQLException {
    return comboPooledDataSource.getConnection();
}
```

## 3. 连接池实现

实现一个 JDBC 规范的连接池，至少需要完成以下步骤：
1. 实现 java.sql.DataSource 接口。
1. 创建批量的 Connection 用 LinkedList 保存（LinkedList 底层是链表，对增删性能更好）。
1. 实现 getConnetion()，让 getConnection() 每次调用，都是在 LinkedList 中取一个 Connection 返回给用户。
1. 使调用 Connection 对象的 close() 方法时，Connection 返回给 LinkedList 连接池而不是关闭连接。

其中，要使调用 Connection 对象的 close() 方法时 Connection 连接不关闭，一般有以下实现思路：
- 使用内聚方式，实现一个 Connection 包装类，增强 close() 方法。

  步骤如下：
  1. 写一个类，实现与被增强对象的相同接口，即 Connection 接口
  1. 定义一个内部变量，指向被增强的对象
  1. 定义构造方法，接收被增强的对象
  1. 覆盖想增强的方法，在该方法中加入自定义逻辑，并调用内部被增强对象的原有逻辑
  1. 对于不想增强的方法，直接调用被增强对象的方法
  思路本身没有大的问题，但是实现接口时，大量的不想增强的方法都需要实现，编写效率很低。

- 使用继承方式，实现一个 Connection 子类，覆盖 close() 方法。

  Connection 对象是通过具体的数据库驱动加载的，保存了数据的信息。因此，即便实现了 Connection 的子类，却无法直接继承父类的私有数据信息，因此子类是无法连接数据库的，更别说覆盖 close() 方法实现其它逻辑了。

- 使用动态代理模式，拦截 close() 方法的调用，对 close() 增强。
因此，通常要使调用 Connection 对象的 close() 方法时 Connection 连接不关闭，会采用动态代理模式，对 close() 方法进行增强。

e.g.
```java
public class ConnectionPool implement DataSource {
    private static LinkedList<Connection> list = new LinkedList<>();
    
    // 获取连接只需要一次就够了，所以用 static 代码块
    static {
        // 读取文件配置
        InputStream inputStream = Demo1.class.getClassLoader().getResourceAsStream("db.properties");

        Properties properties = new Properties();
        try {
            properties.load(inputStream);
            String url = properties.getProperty("url");
            String username = properties.getProperty("username");
            String driver = properties.getProperty("driver");
            String password = properties.getProperty("password");

            // 加载驱动
            Class.forName(driver);

            // 获取多个连接，保存在 LinkedList 集合中
            for (int i = 0; i < 10; i++) {
                Connection connection = DriverManager.getConnection(url, username, password);
                list.add(connection);
            }
            
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }

    // 重写 Connection 方法，用户获取连接应该从 LinkedList 中给他
    @Override
    public Connection getConnection() throws SQLException {
        if (list.size() > 0) {
            final Connection connection = list.removeFirst();
            // 看看池的大小
            System.out.println(list.size());
            // 返回一个动态代理对象
            return (Connection) Proxy.newProxyInstance(ConnectionPool.class.getClassLoader(), 
                                                      connection.getClass().getInterfaces(), 
                                                      new InvocationHandler() {
                @Override
                public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                    // 如果不是调用 close 方法，就按照正常的来调用
                    if (!method.getName().equals("close")) {
                        method.invoke(connection, args);
                    } else {
                        list.add(connection);
                        // 再看看池的大小
                        System.out.println(list.size());
                    }
                    return null;
                }
            });
        }
        return null;
    }
}
```

## 4. Refer Links

[JDBC 第四篇 --【数据库连接池、DbUtils 框架、分页】](https://zhongfucheng.bitcron.com/post/jdbc/jdbcdi-si-pian-shu-ju-ku-lian-jie-chi-dbutilskuang-jia-fen-ye)

[自己动手开发 Java DataSource](http://www.open-open.com/lib/view/open1338106062370.html)

<!-- todo: -->
[MyBatis 源码分析（5）——内置 DataSource 实现](http://www.cnblogs.com/jabnih/p/5738432.html)