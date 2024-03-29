- [SpringBoot 搭配使用](#springboot-搭配使用)
  - [1. 使用 spring boot data JPA](#1-使用-spring-boot-data-jpa)
    - [1.1. 引入依赖 spring-boot-starter-data-jpa](#11-引入依赖-spring-boot-starter-data-jpa)
    - [1.2. 配置【application.yml】](#12-配置applicationyml)
    - [1.3. 编写实体类：](#13-编写实体类)
    - [1.4. 编写数据访问接口：](#14-编写数据访问接口)
      - [1.4.1. 接口自动实现的方法](#141-接口自动实现的方法)
      - [1.4.2. 复杂查询方法](#142-复杂查询方法)
        - [1.4.2.1. 分页查询](#1421-分页查询)
        - [1.4.2.2. 排序查询](#1422-排序查询)
        - [1.4.2.3. 限制查询](#1423-限制查询)
        - [1.4.2.4. 自定义 SQL 查询](#1424-自定义-sql-查询)
        - [1.4.2.5. 多表查询](#1425-多表查询)
  - [2. 使用 spring boot data rest](#2-使用-spring-boot-data-rest)
  - [3. 使用事务](#3-使用事务)
    - [3.1. 高级使用](#31-高级使用)
      - [3.1.1. 指定不同的事务管理器](#311-指定不同的事务管理器)
      - [3.1.2. 隔离级别控制](#312-隔离级别控制)
      - [3.1.3. 传播行为](#313-传播行为)
  - [4. 结合 Mybatis](#4-结合-mybatis)
    - [4.1. 基本操作](#41-基本操作)
    - [4.2. mapper 的注解支持](#42-mapper-的注解支持)
      - [4.2.1. @Insert](#421-insert)
      - [4.2.2. @Update](#422-update)
      - [4.2.3. @Delete](#423-delete)
      - [4.2.4. @Select](#424-select)
        - [4.2.4.1. 结果映射](#4241-结果映射)
          - [4.2.4.1.1. 普通映射](#42411-普通映射)
          - [4.2.4.1.2. 一对一映射](#42412-一对一映射)
          - [4.2.4.1.3. 一对多映射](#42413-一对多映射)
    - [4.3. 使用 mybatis-generator](#43-使用-mybatis-generator)
    - [4.4. 多数据源配置](#44-多数据源配置)
    - [4.5. 使用 HikariCP 连接池](#45-使用-hikaricp-连接池)
    - [4.6. 使用 mybatis-plus](#46-使用-mybatis-plus)
  - [5. 使用数据库版本工具](#5-使用数据库版本工具)
    - [5.1. flyway](#51-flyway)
    - [5.2. liquibase](#52-liquibase)
  - [6. 使用 Redis](#6-使用-redis)
  - [7. 使用 Actuator](#7-使用-actuator)
  - [8. 使用 Lombok](#8-使用-lombok)
  - [9. 使用 Swagger](#9-使用-swagger)
  - [10. 使用 JHipster](#10-使用-jhipster)
  - [11. 测试](#11-测试)
    - [11.1. 单元测试](#111-单元测试)
    - [11.2. 集成测试](#112-集成测试)
  - [12. 定时任务](#12-定时任务)
    - [12.1. 使用 Schedule](#121-使用-schedule)
    - [12.2. 使用 Quartz](#122-使用-quartz)
  - [13. 使用 Shiro](#13-使用-shiro)
  - [14. 使用 Spring Security](#14-使用-spring-security)
  - [15. 使用 Spring AOP](#15-使用-spring-aop)
  - [16. Spring RestTemplate](#16-spring-resttemplate)
  - [17. Refer Links](#17-refer-links)

# SpringBoot 搭配使用

## 1. 使用 spring boot data JPA

JPA(Java Persistence API) 是 Sun 官方提出的 Java 持久化规范。它为 Java 开发人员提供了一种对象 / 关联映射工具来管理 Java 应用中的关系数据。他的出现主要是为了简化现有的持久化开发工作和整合 ORM 技术，结束现在 Hibernate，TopLink，JDO 等 ORM 框架各自为营的局面。值得注意的是，JPA 是在充分吸收了现有 Hibernate，TopLink，JDO 等 ORM 框架的基础上发展而来的，具有易于使用，伸缩性强等优点。

注意：JPA 是一套规范，不是一套产品，那么像 Hibernate,TopLink,JDO 他们是一套产品，如果说这些产品实现了这个 JPA 规范，那么我们就可以叫他们为 JPA 的实现产品。

### 1.1. 引入依赖 spring-boot-starter-data-jpa

```
compile('org.springframework.boot:spring-boot-starter-data-jpa')
```

### 1.2. 配置【application.yml】
    ```yaml
    spring:
        # database configure
        datasource:
            driver-class-name: com.mysql.jdbc.Driver
            password: root
            url: jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC&useSSL=false
            username: root
        #jpa configure
        jpa:
            hibernate:
                ddl-auto: update
            show-sql: true # 在控制台输出被执行的 sql 语句
    jackson:
              serialization: true # 美化输出的 json 字符串
    ```
    说明：hibernate 提供了根据实体类自动维护数据表的功能，可通过 jpa.hibernate.ddl-auto 来配置维护的策略，该配置项可使用以下值：
    - create：启动时删除上一次生成的表，则清空数据；
    - create-drop：启动时生成表，sessionFactory 关闭时表会被删除；
    - update：最常用的属性，第一次加载 hibernate 时根据 model 类会自动建立起表的结构（前提是先建立好数据库），以后加载 hibernate 时根据 model 类自动更新表结构，即使表结构改变了但表中的行仍然存在不会删除以前的行。要注意的是当部署到服务器后，表结构是不会被马上建立起来的，是要等应用第一次运行起来后才会。在开发初期使用此项；
    - validate：每次加载 hibernate 时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值，在开发稳定后使用此项；
    - none：启动时不做任何措施；

### 1.3. 编写实体类：
```java
@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {

    @Id
    @GeneratedValue
    private Long id;

    private String email;

    private String name;
}
```
可用注解：http://itindex.net/detail/53173-jpa-hibernate
- @Entity：注解表明 User 是一个 JPA 实体，程序启动时，jpa 会自动在连接的数据库中校验并创建相应的数据表；
- @Column：映射数据表字段名和实体类属性名，若不设置则默认使用驼峰映射，如属性 phoneNumber 映射字段 phone_number；
- @id：定义了映射到数据库表的主键的属性，一个实体只能有一个属性被映射为主键；
- @Basic(fetch=FetchType,optional=true) ：表示一个简单的属性到数据库表的字段的映射；
- @GeneratedValue(strategy=GenerationType,generator="")：strategy: 表示主键生成策略，有 AUTO,INDENTITY,SEQUENCE 和 TABLE 4 种，分别表示让 ORM 框架自动选择，根据数据库的 Identity 字段生成，根据数据库表的 Sequence 字段生成，以有根据一个额外的表生成主键，默认为 AUTO；generator: 表示主键生成器的名称，这个属性通常和 ORM 框架相关，例如，Hibernate 可以指定 uuid 等主键生成方式；
- @Transient：表示该属性并非一个到数据库表的字段的映射，ORM 框架将忽略该属性，如果一个属性并非数据库表的字段映射，就务必将其标示 - @Transient, 否则，ORM 框架默认其注解为 @Basic；
- @JoinColumn：描述一个关联字段，
- @ManyToOne(fetch=FetchType,cascade=CascadeType)：表示多对一的映射，该注解标注的属性通常是数据库表的外键；optional: 是否允许该字段为 null, 该属性应该根据数据库表的外键约束来确定，默认为 true；fetch: 表示抓取策略，默认为 FetchType.EAGER；cascade: 表示默认的级联操作策略，可以指定为 ALL,PERSIST,MERGE,REFRESH 和 REMOVE 中的若干组合，默认为无级联操；targetEntity: 表示该属性关联的实体类型。该属性通常不必指定，ORM 框架根据属性类型自动判断 targetEntity；
  例：
  ```java
  // 订单 Order 和用户 User 是一个 ManyToOne 的关系
  // 在 Order 类中定义
  @ManyToOne()
  @JoinColumn(name="USER")
  public User getUser() {
  return user;
  }
  ```
- @OneToMany(fetch=FetchType,cascade=CascadeType)：描述一个一对多的关联，该属性应该为集体类型，在数据库中并没有实际字段；fetch: 表示抓取策略，默认为 FetchType.LAZY, 因为关联的多个对象通常不必从数据库预先读取到内存；cascade: 表示级联操作策略，对于 OneToMany 类型的关联非常重要，通常该实体更新或删除时，其关联的实体也应当被更新或删除；
- @OneToOne(fetch=FetchType,cascade=CascadeType)：描述一个一对一的关联；fetch: 表示抓取策略，默认为 FetchType.LAZY；cascade: 表示级联操作策略；
- @ManyToMany：描述一个多对多的关联。多对多关联上是两个一对多关联，但是在 ManyToMany 描述中，中间表是由 ORM 框架自动处理  ；targetEntity: 表示多对多关联的另一个实体类的全名，例如：package.Book.class；mappedBy: 表示多对多关联的另一个实体类的对应集合属性名称；
- @Enumerated(EnumType.STRING)：使用枚举的时候，我们希望数据库中存储的是枚举对应的 String 类型，而不是枚举的索引值；
  例：
  ```java
  @Enumerated(EnumType.STRING)
  @Column(nullable = true)
  private UserType type;
  ```

- 验证注解：

  | 注解       | 使用类型   | 说明               | 示例                         |
  | -------- | ------ | ---------------- | -------------------------- |
  | @Pattern | String | 通过正则表达式来验证字符串    | @Pattern(regex="[a-z]{6}") |
  | @Length  | String | 验证字符串的长度         | @length(min=3,max=20)      |
  | @Email   | String | 验证一个 Email 地址是否有效  |                            |
  | @Range   | Long   | 验证一个整型是否在有效的范围内  | @Range(min=0,max=100)      |
  | @Min     | Long   | 验证一个整型必须不小于指定值   | @Min(value=10)             |
  | @Max     | Long   | 验证一个整型必须不大于指定值   | @Max(value=20)             |
  | @Size    | 集合或数组  | 集合或数组的大小是否在指定范围内 | @Size(min=1,max=255)       |

  注意：以上每个注解都可能性有一个 message 属性，用于在验证失败后向用户返回的消息；

### 1.4. 编写数据访问接口：
```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```
说明：

http://www.ityouknow.com/springboot/2016/08/20/springboot(%E4%BA%94)-spring-data-jpa%E7%9A%84%E4%BD%BF%E7%94%A8.html

这个接口中我们没有定义任何操作方法，而是直接继承于 PagingAndSortingRepository 接口，该接口本身已经实现了创建（save）、更新（save）、删除（delete）、查询（findAll、findOne）等基本操作的函数，因此对于这些基础操作的数据访问就不需要开发者再自己定义。

#### 1.4.1. 接口自动实现的方法

![image](http://img.cdn.firejq.com/jpg/2017/10/30/d958459a5671411daa55a05c0a69aee4.jpg)

在我们实际开发中，JpaRepository 接口定义的接口往往还不够或者性能不够优化，我们需要进一步实现更复杂一些的查询或操作，如：
```java
User findByNameAndAge(String name, Integer age)
```

该接口方法实现了按 name 和 age 查询 User 实体，可以看到我们这里没有任何类 SQL 语句就完成了两个条件查询方法。这就是 Spring-data-jpa 的一大特性：通过解析方法名创建查询，即根据方法名来自动生成 SQL：
主要的语法是 findXXBy,readAXXBy,queryXXBy,countXXBy, getXXBy 后面跟属性名称：
```java
User findByUserName(String userName);
```
也使用一些加一些关键字 And、 Or
```java
User findByUserNameOrEmail(String username, String email);
```
修改、删除、统计也是类似语法
```java
Long deleteById(Long id);
Long countByUserName(String userName)
```
基本上 SQL 体系中的关键词都可以使用，例如：LIKE、 IgnoreCase、 OrderBy。
```java
List<User> findByEmailLike(String email);

User findByUserNameIgnoreCase(String userName);

List<User> findByUserNameOrderByEmailDesc(String email);
```

详细对应关系：

| **Keyword**       | **Sample**                              | **JPQL snippet**                         |
| ----------------- | --------------------------------------- | ---------------------------------------- |
| And               | findByLastnameAndFirstname              | … where x.lastname = ?1 and x.firstname = ?2 |
| Or                | findByLastnameOrFirstname               | … where x.lastname = ?1 or x.firstname = ?2 |
| Is,Equals         | findByFirstnameIs,findByFirstnameEquals | … where x.firstname = ?1                 |
| Between           | findByStartDateBetween                  | … where x.startDate between ?1 and ?2    |
| LessThan          | findByAgeLessThan                       | … where x.age < ?1                       |
| LessThanEqual     | findByAgeLessThanEqual                  | … where x.age ⇐ ?1                       |
| GreaterThan       | findByAgeGreaterThan                    | … where x.age > ?1                       |
| GreaterThanEqual  | findByAgeGreaterThanEqual               | … where x.age >= ?1                      |
| After             | findByStartDateAfter                    | … where x.startDate > ?1                 |
| Before            | findByStartDateBefore                   | … where x.startDate < ?1                 |
| IsNull            | findByAgeIsNull                         | … where x.age is null                    |
| IsNotNull,NotNull | findByAge(Is)NotNull                    | … where x.age not null                   |
| Like              | findByFirstnameLike                     | … where x.firstname like ?1              |
| NotLike           | findByFirstnameNotLike                  | … where x.firstname not like ?1          |
| StartingWith      | findByFirstnameStartingWith             | … where x.firstname like ?1 (parameter bound with  appended %) |
| EndingWith        | findByFirstnameEndingWith               | … where x.firstname like ?1 (parameter bound with  prepended %) |
| Containing        | findByFirstnameContaining               | … where x.firstname like ?1 (parameter bound  wrapped in %) |
| OrderBy           | findByAgeOrderByLastnameDesc            | … where x.age = ?1 order by x.lastname desc |
| Not               | findByLastnameNot                       | … where x.lastname <> ?1                 |
| In                | findByAgeIn(Collection ages)            | … where x.age in ?1                      |
| NotIn             | findByAgeNotIn(Collection age)          | … where x.age not in ?1                  |
| TRUE              | findByActiveTrue()                      | … where x.active = true                  |
| FALSE             | findByActiveFalse()                     | … where x.active = false                 |
| IgnoreCase        | findByFirstnameIgnoreCase               | … where UPPER(x.firstame) = UPPER(?1)    |



#### 1.4.2. 复杂查询方法

在实际的开发中我们需要用到分页、删选、连表等查询的时候就需要特殊的方法或者自定义 SQL，可以通过使用 @Query 注解来创建查询，您只需要编写 JPQL 语句，并通过类似“:name”来映射 @Param 指定的参数

##### 1.4.2.1. 分页查询
分页查询在实际使用中非常普遍了，spring data jpa 已经帮我们实现了分页的功能，在查询的方法中，需要传入参数 Pageable , 当查询中有多个参数的时候 Pageable 建议做为最后一个参数传入
```java
Page<User> findALL(Pageable pageable);// 自动实现

Page<User> findByUserName(String userName, Pageable pageable);// 需要自定义
Pageable 是 spring 封装的分页实现类，使用的时候需要传入页数、每页条数和排序规则；
使用 (@RestController 中)：
Page<User> pageUsers = UserRepository.findAll(new PageRequest(1, 2));
return pageUsers;
```

##### 1.4.2.2. 排序查询
```java
List<Person> findAll(Sort sort);// 自动实现
```
使用
```java
List<Person> people =  personRepository.findAll(new Sort(Direction.DESC, “age”));
return people;
```

##### 1.4.2.3. 限制查询
有时候我们只需要查询前 N 个元素，或者支取前一个实体。
```java
ser findFirstByOrderByLastnameAsc();

User findTopByOrderByAgeDesc();

Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);

List<User> findFirst10ByLastname(String lastname, Sort sort);

List<User> findTop10ByLastname(String lastname, Pageable pageable);
```
##### 1.4.2.4. 自定义 SQL 查询
其实 Spring data 觉大部分的 SQL 都可以根据方法名定义的方式来实现，但是由于某些原因我们想使用自定义的 SQL 来查询，spring data 也是完美支持的；在 SQL 的查询方法上面使用 @Query 注解，如涉及到删除和修改在需要加上 @Modifying. 也可以根据需要添加 @Transactional 对事务的支持，查询超时的设置等；
```java
@Modifying
@Query("update User u set u.userName = ?1 where c.id = ?2")// 使用序号参数，“?1“ 表示 userName，”?2“ 表示 id
int modifyByIdAndUserId(String  userName, Long id);

@Transactional
@Modifying
@Query("delete from User where id = :id")// 使用命名参数，“:id“ 表示 id
void deleteByUserId(Long id);

@Transactional(timeout = 10)
@Query("select u from User u where u.emailAddress = ?1")
    User findByEmailAddress(String emailAddress);
```
##### 1.4.2.5. 多表查询
多表查询在 spring data jpa 中有两种实现方式，第一种是利用 hibernate 的级联查询来实现，第二种是创建一个结果集的接口来接收连表查询后的结果，这里主要第二种方式。
首先需要定义一个结果集的接口类。
```java
public interface HotelSummary {

    City getCity();

    String getName();

    Double getAverageRating();

    default Integer getAverageRatingRounded() {
        return getAverageRating() == null ? null : (int) Math.round(getAverageRating());
    }

}
```
查询的方法返回类型设置为新创建的接口
```java
@Query("select h.city as city, h.name as name, avg(r.rating) as averageRating "
        - "from Hotel h left outer join h.reviews r where h.city = ?1 group by h")
Page<HotelSummary> findByCity(City city, Pageable pageable);

@Query("select h.name as name, avg(r.rating) as averageRating "
        - "from Hotel h left outer join h.reviews r  group by h")
Page<HotelSummary> findByCity(Pageable pageable);
使用

Page<HotelSummary> hotels = this.hotelRepository.findByCity(new PageRequest(0, 10, Direction.ASC, "name"));
for(HotelSummary summay:hotels){
        System.out.println("Name" +summay.getName());
    }
```
在运行中 Spring 会给接口（HotelSummary）自动生产一个代理类来接收返回的结果，代码汇总使用 getXX 的形式来获取

至此，JPA 数据库访问层编写完毕，只需在需要访问数据的地方使用 @Autowired 注入 UserRepository（自动注册成 Bean），即可访问数据库；

## 2. 使用 spring boot data rest

中文文档 https://springcloud.cc/spring-data-rest-zhcn.html#dependencies.spring-boot

Spring Data REST 构建在 Spring Data repositories 之上，并自动将其导出为 REST 资源。它利用超媒体来允许客户端查找存储库暴露的功能，并将这些资源自动集成到相关的超媒体功能中。

1)	引入依赖
    ```java
    compile('org.springframework.boot:spring-boot-starter-data-jpa')
    compile("org.springframework.boot:spring-boot-starter-data-rest")
    ```

2)	配置

    在【application.properties】中可通过以 spring.data.rest 为前缀的配置项对 Spring Data REST 进行配置；
    - 更改根 URI
      默认情况下，Spring Data REST 以根 URI“/”提供 REST 资源，可在【application.properties】中修改：
      ```
      spring.data.rest.basePath = /api
      ```
      则可通过 GET 方法访问 http://localhost:8080/api/ 表名 /2
    - 更改节点路径
      spring data REST 默认使用实体类名 + ‘s’ 形成节点路径，如对于实体类 person，则生成的 API URI 为 /persons/；
      若要更改，需在实体类的 Repo 接口上添加注解：
      ```java
      @RepositoryRestResource(path = "people")
      ```
      则生成的 API URI 为 /people/；
    - 其它可配置项
      ```
      defaultPageSize
      更改在单个页面中投放的默认项目数量
      maxpagesize
      更改单个页面中的最大项目数量
      pageParamName
      更改用于选择页面的查询参数的名称
      limitParamName
      更改页面中要显示的项目的查询参数的名称
      sortParamName
      更改用于排序的查询参数的名称
      defaultMediaType
      更改默认介质类型以在没有指定时使用
      returnBodyOnCreate
      如果在创建一个新实体时应该返回一个主体，那么请改变
      returnBodyOnUpdate
      如果在更新一个实体时应该返回一个机构，则更改
      ```

3)	使用
  在编写完 Jpa 的实体类和数据访问接口后，Spring Data Rest 会检测数据表暴露的接口，自动在 Spring MVC 中绑定响应的 HTTP 路由方法，并创建 User 对象的 RESTful 访问端点 /users：
  如：
  ```
  GET /users User 列表信息（数据来源于 UserRepository 对应的关系型数据库，下同）
  POST /users 创建一个 User 对象
  GET /users/{id} 获取单个 User 对象
  PUT /users/{id} 更新单个 User 对象
  DELETE /users/{id} 删除单个 User 对象
  ```

## 3. 使用事务
Spring Data JPA 默认对所有的方法开启了事务支持，且查询类事务默认启用 readOnly == true；因此，在 spring boot data jpa 项目中，无需使用 @EnableTransactionManagement，直接在需要使用事务的方法 / 类上标注 @Transactional 即可；

通常我们单元测试为了保证每个测试之间的数据独立，会使用 @Rollback 注解让每个单元测试都能在结束时回滚；

真正在开发业务逻辑时，我们通常在 service 层接口中使用 @Transactional 来对一整个业务逻辑进行事务管理的配置

例：
```java
public interface {

    @Transactional
    User login(String name, String password);

}
```

### 3.1. 高级使用
http://blog.didispace.com/springboottransactional/

#### 3.1.1. 指定不同的事务管理器
一般我们直接使用默认的事务配置，就可以满足一些基本的事务需求，但是当我们项目较大较复杂时（比如，有多个数据源等），这时候需要在声明事务时，指定不同的事务管理器。在声明事务时，只需要通过 value 属性指定配置的事务管理器名即可，例如：@Transactional(value="transactionManagerPrimary")。

#### 3.1.2. 隔离级别控制
隔离级别是指若干个并发的事务之间的隔离程度，与我们开发时候主要相关的场景包括：脏读取、重复读、幻读。

支持的五种隔离级别：
```java
public enum Isolation {
    DEFAULT(-1),
    READ_UNCOMMITTED(1),
    READ_COMMITTED(2),
    REPEATABLE_READ(4),
    SERIALIZABLE(8);
}
```

- DEFAULT：这是默认值，表示使用底层数据库的默认隔离级别。对大部分数据库而言，通常这值就是：READ_COMMITTED。

- READ_UNCOMMITTED：该隔离级别表示一个事务可以读取另一个事务修改但还没有提交的数据。该级别不能防止脏读和不可重复读，因此很少使用该隔离级别。

- READ_COMMITTED：该隔离级别表示一个事务只能读取另一个事务已经提交的数据。该级别可以防止脏读，这也是大多数情况下的推荐值。

- REPEATABLE_READ：该隔离级别表示一个事务在整个过程中可以多次重复执行某个查询，并且每次返回的记录都相同。即使在多次查询之间有新增的数据满足该查询，这些新增的记录也会被忽略。该级别可以防止脏读和不可重复读。

- SERIALIZABLE：所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

指定方法：通过使用 isolation 属性设置，例如：
```java
@Transactional(isolation = Isolation.DEFAULT)
```

#### 3.1.3. 传播行为
所谓事务的传播行为是指，如果在开始当前事务之前，一个事务上下文已经存在，此时有若干选项可以指定一个事务性方法的执行行为。
```java
public enum Propagation {
    REQUIRED(0),
    SUPPORTS(1),
    MANDATORY(2),
    REQUIRES_NEW(3),
    NOT_SUPPORTED(4),
    NEVER(5),
    NESTED(6);
}
```

- REQUIRED：如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。

- SUPPORTS：如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。

- MANDATORY：如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。

- REQUIRES_NEW：创建一个新的事务，如果当前存在事务，则把当前事务挂起。

- NOT_SUPPORTED：以非事务方式运行，如果当前存在事务，则把当前事务挂起。

- NEVER：以非事务方式运行，如果当前存在事务，则抛出异常。

- NESTED：如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于 REQUIRED。

指定方法：通过使用 propagation 属性设置，例如：
```java
@Transactional(propagation = Propagation.REQUIRED)
```

## 4. 结合 Mybatis

[官方文档](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)

[如何优雅使用 mybatis](http://www.ityouknow.com/springboot/2016/11/06/springboot(%E5%85%AD)-%E5%A6%82%E4%BD%95%E4%BC%98%E9%9B%85%E7%9A%84%E4%BD%BF%E7%94%A8mybatis.html)

### 4.1. 基本操作

1)  引入依赖 mybatis-spring-boot-starter 和 mysql-connector-java，【build.gradle】
    ```
    compile group: 'org.mybatis.spring.boot', name: 'mybatis-spring-boot-starter', version: '1.3.1'

    compile group: 'mysql', name: 'mysql-connector-java'
    ```

2)  配置数据库连接【appication.properties】
    ```
    spring.datasource.url=jdbc:mysql://127.0.0.1:3306/wincc?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC&useSSL=false
    spring.datasource.username=root
    spring.datasource.password=root
    ```

3)  配置 mybatis【appication.properties】

    可用配置项见[这里](http://www.mybatis.org/mybatis-3/zh/configuration.html#settings)

    使用前缀为 mybatis 进行配置，可用配置如下：

    | **Property**               | **Description**                          |
    | -------------------------- | ---------------------------------------- |
    | `config-location`          | Location of MyBatis xml config file.     |
    | `check-config-location`    | Indicates whether perform presence check of  the MyBatis xml config file. |
    | `mapper-locations`         | Locations of Mapper xml config file.     |
    | `type-aliases-package`     | Packages to search for type aliases. (Package  delimiters are "`,; \t\n`") |
    | `type-handlers-package`    | Packages to search for type handlers. (Package  delimiters are "`,; \t\n`") |
    | `executor-type`            | Executor type: `SIMPLE`, `REUSE`, `BATCH`. |
    | `configuration-properties` | Externalized properties for MyBatis  configuration. Specified properties can be used as placeholder on MyBatis  config file and Mapper file. For detail see the [MyBatis reference  page](http://www.mybatis.org/mybatis-3/configuration.html#properties) |
    | `configuration`            | A MyBatis `Configuration` bean. About available properties see  the [MyBatis reference  page](http://www.mybatis.org/mybatis-3/configuration.html#settings). **NOTE** This  property cannot be used at the same time with the `config-location` |
    | `autoMappingBehavior`      |	指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示取消自动映射；PARTIAL 只会自动映射没有定义嵌套结果集映射的结果集。 FULL 会自动映射任意复杂的结果集（无论是否嵌套）。默认值为 PARTIAL。|
    | `autoMappingUnknownColumnBehavior`| 指定发现自动映射目标未知列（或者未知属性类型）的行为。NONE: 不做任何反应；WARNING: 输出提醒日志 ('org.apache.ibatis.session.AutoMappingUnknownColumnBehavior' 的日志等级必须设置为 WARN)；FAILING: 映射失败 （抛出 SqlSessionException）。默认值为 NONE。|


    常用的配置：
    ```
    # 使用《驼峰命名法》进行数据库表列与实体类属性的映射
    mybatis.configuration.map-underscore-to-camel-case=true

    mybatis.configuration.default-fetch-size=100
    mybatis.configuration.default-statement-timeout=30
    ```

4)	编写实体类

    注意：**实体类必须有无参构造函数，否则 mybatis 无法完成映射**；

5)	编写 mapper 接口【使用注解的方式】
    ```java
    @Mapper
    public interface TestMapper {
      @Select("SELECT * FROM test_1200 WHERE id = #{id}")
      Record findById(@Param("id") Long id);
    }
    ```
    编写完 mapper 接口即完成了 DAO 层的开发工作；
    注意：
    - 要使得 mapper 接口被 spring 扫描到，可采取两种方式：
      - 一是在每个 mapper 接口类上都注解 @Mapper；
      - 二是在 Application 启动类上注解`@MapperScan(“com.firejq.mapper”)`

6)	使用 mapper 接口进行数据库操作
    ```java
    @RunWith(SpringRunner.class)
    // 使用该注解，会自动配置 spring boot 上下文环境并使用 Transactional 开启 Rollback，若要关闭回滚，@Rollback(false)
    @SpringBootTest
    public class UserMapperTest {
      @Autowired
      private UserMapper UserMapper;
      // 将 mapper 接口直接注入后即可使用

      @Test
      public void testQuery() throws Exception {
        List<UserEntity> users = UserMapper.getAll();
        System.out.println(users.toString());
      }

    }
    ```

### 4.2. mapper 的注解支持

官方文档：http://www.mybatis.org/mybatis-3/zh/java-api.html

#### 4.2.1. @Insert
基本使用：
```java
public interface StudentMapper {
    @Insert("INSERT INTO STUDENTS(STUD_ID,NAME,EMAIL,ADDR_ID, PHONE)
        VALUES(#{studId},#{name},#{email},#{address.addrId},#{phone})")
    // 使用了 @Insert 注解的 insertMethod() 方法将返回 insert 语句执行后影响的行数
    int insertStudent(Student student);
}
```

自动插入自增主键值后赋值给 POJO：
```java
@Insert("INSERT INTO STUDENTS(NAME,EMAIL,ADDR_ID, PHONE)
VALUES(#{name},#{email},#{address.addrId},#{phone})")
@Options(useGeneratedKeys = true, keyProperty = "studId")
int insertStudent(Student student);
// 这里 STUD_ID 列值将会在插入记录时通过 MySQL 数据库自动生成，并且在插入操作执行完毕后，将该自增值赋值给 student 类的 studId 属性
```
例：
```java
mapper.insertStudent(student);
int studentId = student.getStudId();
```

使用主键序列来设置主键值：

可以使用 @SelectKey 注解来为任意 SQL 语句来指定主键值，作为主键列的值。

假设我们有一个名为 STUD_ID_SEQ 的序列来生成 STUD_ID 主键值
```java
@Insert("INSERT INTO STUDENTS(STUD_ID,NAME,EMAIL,ADDR_ID, PHONE)
VALUES(#{studId},#{name},#{email},#{address.addrId},#{phone})")
@SelectKey(statement="SELECT STUD_ID_SEQ.NEXTVAL FROM DUAL", keyProperty="studId", resultType=int.class, before=true)
// 会在执行 INSERT 语句之前，先执行 SELECT 语句，从 STUD_ID_SEQ 中取得 STUD_ID 后，将其赋值给 student 类的 studId 属性，然后再执行插入
int insertStudent(Student student);
```
注：如果在数据库中设置了 AFTER 触发器，每次插入后都将主键值存储到 STUD_ID_SEQ 中，可将 before=false，则会在插入完毕后，从 STUD_ID_SEQ 中执行 SELECT 语句获取刚刚插入的主键值，赋值给 studId；

#### 4.2.2. @Update
例：
```java
@Update("UPDATE STUDENTS SET NAME=#{name}, EMAIL=#{email}, PHONE=#{phone} WHERE STUD_ID=#{studId}")
// 将会返回执行了 UPDATE 语句后影响的行数
int updateStudent(Student student);
```
#### 4.2.3. @Delete
例：
```java
@Delete("DELETE FROM STUDENTS WHERE STUD_ID=#{studId}")
// 将会返回执行了 DELETE 语句后影响的行数
int deleteStudent(int studId);
```
#### 4.2.4. @Select
例：
```java
@Select("SELECT STUD_ID AS STUDID, NAME, EMAIL, PHONE FROM STUDENTS WHERE STUD_ID=#{studId}")
Student findStudentById(Integer studId);
```
注：

如果返回了多行结果，将抛出 TooManyResultsException 异常

##### 4.2.4.1. 结果映射

数据库表列与实体类属性映射的四种方法： http://blog.csdn.net/lmy86263/article/details/53150091

a)	通过 XML 映射文件中的 resultMap；

b)	通过注解 @Results 和 @Result；

c)	通过属性配置完成映射；

d)	通过使用在 SQL 语句中定义别名完成映射；

以下使用的都是注解映射：
###### 4.2.4.1.1. 普通映射
将查询结果通过别名或者是 @Results 注解与 JavaBean 属性映射起来，一个 @Result 对应 Java bean 的一个属性赋值，映射失败的属性保持默认值：

例：
```java
@Select("SELECT * FROM STUDENTS")
// column 对于数据表中的列名，property 对于 JavaBean 的属性名
@Results({
    @Result(id = true, column = "stud_id", property = "studId"),
    @Result(column = "name", property = "name"),
    @Result(column = "email", property = "email"),
    @Result(column = "addr_id", property = "address.addrId")
})
List<Student> findAllStudents();
```
可通过在配置中开启驼峰命名映射，自动将所有 axxx_bxx_cxx 的列名与 axxxBxxCxx 的属性名相映射；

###### 4.2.4.1.2. 一对一映射
当使用嵌套的 SELECT 语句时，可使用 @One 来进行一对一的数据映射

例：Student 类中有一个属性为 Address 对象，该对象属性无法与 STUDENTS 表中的列名直接进行映射
```java
@Select("SELECT ADDR_ID AS ADDRID, STREET, CITY, STATE, ZIP, COUNTRY FROM ADDRESSES WHERE ADDR_ID=#{id}")//b
Address findAddressById(int id);

@Select("SELECT * FROM STUDENTS WHERE STUD_ID=#{studId} ")//a
@Results({
@Result(id = true, column = "stud_id", property = "studId"),
@Result(column = "name", property = "name"),
@Result(column = "email", property = "email"),
@Result(column = "addr_id",
one = @One(select = "com.xxx.mappers.StudentMapper.findAddressById")),
property = "address"
})
Student selectStudentWithAddress(int studId);
```

当执行 selectStudentWithAddress 时，会先执行 SQL 语句 a，从 STUDENTS 表中查询 stud_id 为 studId 的记录赋值到 java bean 中（但 Address 对象属性无法映射），然后再将 STUEDNTS 表中列 addr_id 的值作为输入参数传递给 findAddressById() 方法，执行 SQL 语句 b，并将查询到的 Address 对象赋值给之前 java bean 中的 Address 属性；

```java
Student student = studentMapper.selectStudentWithAddress(studId);
System.out.println("Student :"+student.toString);
System.out.println("Address :"+student.getAddress().toString);
```

###### 4.2.4.1.3. 一对多映射

MyBatis 还提供了 @Many 注解，用来使用嵌套 Select 语句加载一对多关联查询；

例：Tutor 类中有一个对象列表属性为**`List<Course>`类型**，该属性无法与数据表中的列名直接进行映射
```java
@Select("select * from courses where tutor_id=#{tutorId}")
@Results({
@Result(id = true, column = "course_id", property = "courseId"),
@Result(column = "name", property = "name"),
@Result(column = "description", property = "description"),
@Result(column = "start_date" property = "startDate"),
@Result(column = "end_date" property = "endDate")
})
List<Course> findCoursesByTutorId(int tutorId);

@Select("SELECT tutor_id, name as tutor_name, email, addr_id FROM tutors where tutor_id=#{tutorId}")
@Results({
@Result(id = true, column = "tutor_id", property = "tutorId"),
@Result(column = "tutor_name", property = "name"),
@Result(column = "email", property = "email"),
@Result(column = "tutor_id",
many = @Many(select = "com.xxx.mappers.TutorMapper.findCoursesByTutorId")),
property = "courses"
})
Tutor findTutorById(int tutorId);
```

### 4.3. 使用 mybatis-generator
http://www.jianshu.com/p/188622950cc6

由于 mybatis-generator 官方使用的是 maven plugin，因此针对 gradle，使用[第三方解决方案](https://plugins.gradle.org/plugin/com.arenagod.gradle.MybatisGenerator) ，即使用[第三方插件](https://github.com/kimichen13/mybatis-generator-plugin) ；

1)	配置【build.gradle】
    添加以下内容：
    ```groovy
    //mybatis generator plugin ------ start
    buildscript {
        repositories {
            maven {
                url "https://plugins.gradle.org/m2/"
            }
        }
        dependencies {
            classpath "gradle.plugin.com.arenagod.gradle:mybatis-generator-plugin:1.4"
        }
    }

    apply plugin: "com.arenagod.gradle.MybatisGenerator"

    configurations {
        mybatisGenerator
    }

    mybatisGenerator {
        verbose = true
        configFile = 'src/main/resources/mybatis/mybatis-generator.xml'
    }
    //mybatis generator plugin ------ end
    ```

2)	创建目录
    ```
    com.firejq.demo.dao.mapper
    com.firejq.demo.entity
    ```

3)	配置【mybatis-generator.xml】

    在 resources 目录下新建 mybatis/mybatis-generator.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE generatorConfiguration PUBLIC
            "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
            "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
    <generatorConfiguration>

        <context id="context" targetRuntime="MyBatis3">
            <!-- 生成的 Java 文件的编码 -->
            <property name="javaFileEncoding" value="UTF-8"/>

            <!-- 对注释进行控制 -->
            <commentGenerator>
                <!-- 是否生成注释时间 -->
                <property name="suppressDate" value="true"/>
                <!-- 是否取消注释 -->
                <property name="suppressAllComments" value="true"/>
            </commentGenerator>

            <!-- jdbc 的数据库连接配置 -->
            <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                            connectionURL="jdbc:mysql://127.0.0.1:3306/test?useUnicode=true"
                            userId="root" password="root"/>

            <!-- java 类型处理器，用于处理 DB 中的类型到 Java 中的类型，默认使用 JavaTypeResolverDefaultImpl；
                注意，默认会先尝试使用 Integer，Long，Short 等来对应 DECIMAL 和 NUMERIC 数据类型；
            -->
            <javaTypeResolver>
                <!--
                  true：使用 BigDecimal 对应 DECIMAL 和 NUMERIC 数据类型
                  false：默认，
                      scale>0;length>18：使用 BigDecimal;
                      scale=0;length[10,18]：使用 Long；
                      scale=0;length[5,9]：使用 Integer；
                      scale=0;length<5：使用 Short；
                -->
                <property name="forceBigDecimals" value="false"/>
            </javaTypeResolver>

            <!-- java 模型创建器，是必须要的元素
              负责：1，key 类；2，java 类；3，查询类
              targetPackage：生成的类要放的包，真实的包受 enableSubPackages 属性控制；
              targetProject：目标项目，指定一个存在的目录下，生成的内容会放到指定目录中，如果目录不存在，MBG 不会自动建目录
            -->
            <javaModelGenerator targetPackage="com.firjq.demo.entity" targetProject="src/main/java">
                <!-- 是否允许子包，即 targetPackage.schemaName.tableName -->
                <property name="enableSubPackages" value="true"/>
                <!-- 是否对 model 添加构造函数 -->
                <property name="constructorBased" value="false"/>
                <!-- 是否对类 CHAR 类型的列的数据进行 trim 操作 -->
                <property name="trimStrings" value="true"/>
                <!-- 建立的 Model 对象是否不可改变，即生成的 Model 对象不会有 setter 方法，只有构造方法 -->
                <property name="immutable" value="false"/>
            </javaModelGenerator>

            <!-- 对于 mybatis 来说，即生成 Mapper 接口，如果没有配置该元素，那么默认不会生成 Mapper 接口
                targetPackage/targetProject: 同 javaModelGenerator
                type：生成 mapper 类型：
                      1，ANNOTATEDMAPPER：会生成使用 Mapper 接口 +Annotation 的方式创建（SQL 生成在 annotation 中），不会生成对应的 XML；
                      2，MIXEDMAPPER：使用混合配置，会生成 Mapper 接口，并适当添加合适的 Annotation，但是 XML 会生成在 XML 中；
                      3，XMLMAPPER：会生成 Mapper 接口，接口完全依赖 XML；
            -->
            <javaClientGenerator targetPackage=" com.firejq.demo.dao.mapper" type="ANNOTATEDMAPPER" targetProject="src/main/java">
                <!-- 在 targetPackage 的基础上，根据数据库的 schema 再生成一层 package，最终生成的类放在这个 package 下，默认为 false -->
                <property name="enableSubPackages" value="true"/>
            </javaClientGenerator>

            <!-- tableName="%"表示指定对所有数据库表生成实体类 -->
            <table tableName="user" domainObjectName="User " enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>
    <table tableName="order " domainObjectName="Order " enableCountByExample="false"     enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>

        </context>
    </generatorConfiguration>
    ```

4)	启动 mybatis-generator

    ![image](http://img.cdn.firejq.com/jpg/2017/10/30/ab1e524655371de9b5bd2746800533a9.jpg)

5)	针对实际需要进行修改

    a)	将生成实体类的 setter/getter 及全参构造函数删除，使用 @Data 代替；

    b)	删除 mapper/ 中的 provider 类，以及 mapper 接口中的 provider 方法，只留下基本的增删改查；

### 4.4. 多数据源配置
spring boot mybatis 多数据库源（主从）配置 http://www.ityouknow.com/springboot/2016/11/25/springboot(%E4%B8%83)-springboot+mybatis%E5%A4%9A%E6%95%B0%E6%8D%AE%E6%BA%90%E6%9C%80%E7%AE%80%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.html

### 4.5. 使用 HikariCP 连接池
SpringBoot 默认使用 org.apache.tomcat.jdbc.pool.DataSource 连接池，通过更改配置可使用其它第三方连接池；
1)	引入依赖
    ```
    compile group: 'com.zaxxer', name: 'HikariCP', version: '2.7.6'
    ```

2)	更改配置【application.properties】：

    ```ini
    # 数据库连接池配置
    spring.datasource.type=com.zaxxer.hikari.HikariDataSource
    # 最大连接数量
    spring.datasource.hikari.maximum-pool-size=20
    # 生命周期
    spring.datasource.hikari.max-lifetime=30000
    # 连接超时
    spring.datasource.hikari.idle-timeout=30000
    spring.datasource.hikari.data-source-properties.prepStmtCacheSize=250
    spring.datasource.hikari.data-source-properties.prepStmtCacheSqlLimit=2048
    spring.datasource.hikari.data-source-properties.cachePrepStmts=true
    spring.datasource.hikari.data-source-properties.useServerPrepStmts=true
    ```

### 4.6. 使用 mybatis-plus

官方文档：http://mp.baomidou.com/#/quick-start

## 5. 使用数据库版本工具

SpringBoot 支持了两种数据库迁移工具，一个是 flyway，一个是 liquibase。其本身也支持 sql script，在初始化数据源之后执行指定的脚本。

### 5.1. flyway

flyway 官方教程 / 工作原理说明 https://flywaydb.org/getstarted/how

中文工作原理说明：https://blog.waterstrong.me/flyway-in-practice/

Flyway 是一个用 Java 编写的开源数据库版本管理工具，或者说是数据库结构变更工具，旨在帮助开发和运维更容易地管理数据库演进过程中的各个版本。

在 spring boot 中使用 flyway：

1)	引入依赖
    ```
    compile "org.flywaydb:flyway-core:4.2.0"
    ```
    spring boot 会自动注入 flyway 并使用 flyway 的默认配置，在项目启动时自动调用 flyway；

2)	配置 flyway【application.properties】

    spring boot 为 flyway 提供了一系列默认配置，但可通过覆盖来修改自定义配置；

注意：

若原本已有数据库和数据表，但没有 flyway 的元数据表，会导致项目无法启动，要加上配置：
```
flyway.baseline-on-migrate=true
```
这种情况下，初次启动要用于 flyway 的初始化，导致第一个版本的 Migrations 脚本中的 SQL 语句不会被执行，因此这种情况下直接建立一个空的`V1__Initialize.sql`，再把要执行的内容放到`V1.1__Migrations.sql`中即可；

![image](http://img.cdn.firejq.com/jpg/2017/10/30/4fbd5d0090538643cf22e9445d3ed383.jpg)

3)	创建 Migrations 脚本

    Flyway 加载 Migrations 的默认 Locations 为 classpath:db/migration，其加载是在 Runtime 自动递归地执行的；

    除了需要指定 Location 外，Flyway 对 Migrations 的扫描还必须遵从一定的命名模式，Migration 主要分为两类：Versioned 和 Repeatable：

    a)	Versioned migrations

    一般常用的是 Versioned 类型，用于版本升级，每一个版本都有一个唯一的标识并且只能被应用一次，并且不能再修改已经加载过的 Migrations，因为 Metadata 表会记录其 Checksum 值。其中的 version 标识版本号，由一个或多个数字构成，数字之间的分隔符可以采用点或下划线，在运行时下划线其实也是被替换成点了，每一部分的前导零会被自动忽略。

    b)	Repeatable migrations

    Repeatable 是指可重复加载的 Migrations，其每一次的更新会影响 Checksum 值，然后都会被重新加载，并不用于版本升级。对于管理不稳定的数据库对象的更新时非常有用。Repeatable 的 Migrations 总是在 Versioned 之后按顺序执行，但开发者必须自己维护脚本并且确保可以重复执行，通常会在 sql 语句中使用 CREATE OR REPLACE 来保证可重复执行。

    Migrations 命名规则：

    ![image](http://img.cdn.firejq.com/jpg/2017/10/30/360a244c369d71c54e7668e7ef99de64.jpg)

### 5.2. liquibase
可用配置：
```
liquibase.change-log
Change log 配置文件的路径，默认值为 classpath:/db/changelog/db.changelog-master.yaml
liquibase.check-change-log-location
是否坚持 change log 的位置是否存在，默认为 true.
liquibase.contexts
逗号分隔的运行时 context 列表。
liquibase.default-schema
默认的 schema.
liquibase.drop-first
是否首先 drop schema，默认为 false
liquibase.enabled
是否开启 liquibase，默认为 true.
liquibase.password
目标数据库密码
liquibase.url
要迁移的 JDBC URL，如果没有指定的话，将使用配置的主数据源。
liquibase.user
目标数据用户名
```

## 6. 使用 Redis

spring-boot-starter-data-redis 依赖

```
spring.redis.database=0
spring.redis.host=127.0.0.1
spring.redis.password=
spring.redis.pool.max-active=-1
spring.redis.pool.max-idle=100
spring.redis.port=6379
spring.redis.timeout=0
```

```java
@AutowiredRedisTemplate redisTemplate;
```

## 7. 使用 Actuator

Actuator 是 spring boot 提供的对应用系统的自省和监控的集成功能，可以对应用系统进行配置查看、相关功能统计等。

1)	添加依赖
    ```
    compile group: 'org.flywaydb', name: 'flyway-core', version: "${FlywayVersion}"
    ```

2)	关闭安全模式

    Actutor 默认开启了安全模式，使得需要授权的端点都会返回 401（只有 /info 和 /health 是无需授权的），在生产环境下必须开启安全模式然后通过安全认证访问端点，或者直接卸载 Actuator；

    开发环境下关闭安全模式：【application.properties】
    ```
    management.security.enabled=true
    ```

3)	访问监控端点

    Actuator 提供了以下监控端点供开发者使用：

    | HTTP 方法 | 路径              | 描述            | 需要鉴权  |
    | ------ | --------------- | ------------- | ----- |
    | GET    | /autoconfig     | 查看自动配置的使用情况   | true  |
    | GET    | /configprops    | 查看配置属性，包括默认配置 | true  |
    | GET    | /beans          | 查看 bean 及其关系列表  | true  |
    | GET    | /dump           | 打印线程栈         | true  |
    | GET    | /env            | 查看所有环境变量      | true  |
    | GET    | /env/{name}     | 查看具体变量值       | true  |
    | GET    | /health         | 查看应用健康指标      | false |
    | GET    | /info           | 查看应用信息        | false |
    | GET    | /mappings       | 查看所有 url 映射     | true  |
    | GET    | /metrics        | 查看应用基本指标      | true  |
    | GET    | /metrics/{name} | 查看具体指标        | true  |
    | POST   | /shutdown       | 关闭应用          | true  |
    | GET    | /trace          | 查看基本追踪信息      | true  |

## 8. 使用 Lombok

lombok 是一套代码模板解决方案，将极大提升开发的效率；

1)	引入依赖
    ```
    compile group: 'org.projectlombok', name: 'lombok', version: "${LombokVersion}"
    ```
2)	IDEA 中设置

    ![image](http://img.cdn.firejq.com/jpg/2017/10/30/edb3d0efcea761a2f742ad9bd388debe.jpg)

3)	使用

    官网说明：https://projectlombok.org/features/all

    | 注解                                       | 说明                                       |
    | ---------------------------------------- | ---------------------------------------- |
    | @Data                                    | 自动生成 @ToString, @EqualsAndHashCode, @Getter on all fields, and @Setter on all  non-final fields, and @RequiredArgsConstructor；  <br>注意：**不会自动生成全参 / 无参构造方法，因此通常要与 @AllArgsConstructor、@NoArgsConstructor 一起使用**； |
    | @Getter                                  | 自动生成 Getter 方法                             |
    | @Setter                                  | 自动生成 Setter 方法                             |
    | @ToString                                | 自动覆盖 toString 方法                           |
    | @NonNull                                 | 标识对象是否为空，为空则抛出异常                         |
    | @EqualsAndHashCode                       | 自动覆盖 equal 和 hashCode 方法                     |
    | @Slf4j                                   | 默认使用 slf4j 的日志对象    protected final Logger logger =  LoggerFactory.getLogger(this.getClass()); |
    | @CleanUp                                 | 自动资源管理：不用再在 finally 中添加资源的 close 方法          |
    | @NoArgsConstructor  /@RequiredArgsConstructor  /@AllArgsConstructor | 自动生成构造方法                                 |
    | @Value                                   | 用于注解 final 类                               |
    | @Builder                                 | 产生复杂的构建器 api 类                             |
    | @Synchronized                            | 同步方法安全的转化                                |

    例：
    ```java
    import lombok.Data;
    import lombok.extern.slf4j.Slf4j;

    @Slf4j
    @Data
    @AllArgsConstructor
    public class TestBean {
      private String name;
      private int age;
    }
    ```

## 9. 使用 Swagger
Swagger 是一个规范和完整的框架，用于生成、描述、调用和可视化 RESTful 风格的 Web 服务。

1)	引入依赖
    ```
    compile group: 'io.springfox', name: 'springfox-swagger2', version: "${SwaggerVersion}"
    compile group: 'io.springfox', name: 'springfox-swagger-ui', version: "${SwaggerVersion}"
    ```
2)	编写配置类

    完整配置说明：https://springfox.github.io/springfox/docs/current/#configuration-explained

    ```java
    @Configuration
    @EnableSwagger2
    public class SwaggerConfigure {
        @Bean
        public Docket createRestApi() {
            return new Docket(DocumentationType.SWAGGER_2)
                    .apiInfo(apiInfo())
                    .select()
    // 指定 Swagger 扫描的包名，假如不指定此项， 在 Spring Boot 项目中， 会生成 base-err-controller 的 api 接口
                    .apis(RequestHandlerSelectors.basePackage("com.didispace.web"))
                    .paths(PathSelectors.any())
                    .build();
    }

    // 创建该 Api 的基本信息
        private ApiInfo apiInfo() {
            return new ApiInfoBuilder()
                    .title("文档标题")
                    .description("文档描述")
                    .termsOfServiceUrl("xxxxx")
                    .contact(new Contact("作者", "访问地址", "联系方式"))
                    .version("版本号")
                    .build();
        }
    }
    ```
    此时，启动项目，访问 http://localhost:8080/v2/api-docs，可以看到生成的文档的 json 数据结构；访问 http://localhost:8080/swagger-ui.html，可以看到生成的 API 文档的基本页面，但还没有 API 的具体描述；

    ![image](http://img.cdn.firejq.com/jpg/2017/10/30/e88dcf9de6a2feaf7b91a60e38c173f3.jpg)

3)	在 controller 中添加注解

    可用注解：

    ```
    @Api：修饰整个类，描述 Controller 的作用
    @ApiOperation：描述一个类的一个方法，或者说一个接口
    @ApiParam：单个参数描述
    @ApiModel：用对象来接收参数
    @ApiProperty：用对象接收参数时，描述对象的一个字段

    @ApiResponse：HTTP 响应其中 1 个描述
    @ApiResponses：HTTP 响应整体描述
    @ApiIgnore：使用该注解忽略这个 API

    @ApiClass
    @ApiError
    @ApiErrors

    @ApiParamImplicit
    @ApiParamsImplicit
    ```

    例：

    ```java
    @Api(description = "《对整个 Controller 的定义做解释》文章操作相关接口")
    @RestController
    @RequestMapping(value="/users")//Restful 设计：增删改查对于同一个地址
    public class UserController {
    static Map<Long, User> users = Collections.synchronizedMap(new HashMap<Long, User>());

        @ApiOperation(value="《功能说明》获取用户列表", notes="《注意事项》")
        @RequestMapping(value={""}, method=RequestMethod.GET)
        public List<User> getUserList() {
            List<User> r = new ArrayList<User>(users.values());
            return r;
    }

        @ApiOperation(value="创建用户", notes="根据 User 对象创建用户")
        @ApiImplicitParam(name = "user", value = "用户详细实体 user", required = true, dataType = "User")
        @RequestMapping(value="", method=RequestMethod.POST)
        public String postUser(@RequestBody User user) {
            users.put(user.getId(), user);
            return "success";
    }

        @ApiOperation(value="获取用户详细信息", notes="根据 url 的 id 来获取用户详细信息", response = User.class)
        @ApiImplicitParam(name = "id", value = "用户 ID", required = true, dataType = "Long")
        @RequestMapping(value="/{id}", method=RequestMethod.GET)
        public User getUser(@PathVariable Long id) {
            return users.get(id);
    }

        @ApiOperation(value="更新用户详细信息", notes="根据 url 的 id 来指定更新对象，并根据传过来的 user 信息来更新用户详细信息")
        @ApiImplicitParams({
                @ApiImplicitParam(name = "id", value = "用户 ID", required = true, dataType = "Long"),
                @ApiImplicitParam(name = "user", value = "用户详细实体 user", required = true, dataType = "User")
    })
        @RequestMapping(value="/{id}", method=RequestMethod.PUT)
        public String putUser(@PathVariable Long id, @RequestBody User user) {
            User u = users.get(id);
            u.setName(user.getName());
            u.setAge(user.getAge());
            users.put(id, u);
            return "success";
    }

        @ApiOperation(value="删除用户", notes="根据 url 的 id 来指定删除对象")
        @ApiImplicitParam(name = "id", value = "用户 ID", required = true, dataType = "Long")
        @RequestMapping(value="/{id}", method=RequestMethod.DELETE)
        public String deleteUser(@PathVariable Long id) {
            users.remove(id);
            return "success";
        }
    }
    ```

    注：@ApiOperation(value="获取用户详细信息", notes="根据 url 的 id 来获取用户详细信息", response = User.class)

    其中，response = User.class 表示返回值使用 JsonResult 的子类 User 来展示更详细的内容，可以使用 @ApiModel 和 @ApiModelProperty 注解该 Model 类；如果不指定，则使用返回值 JsonResult 来生成文档；

    JsonResult.java

    ```java
    @ApiModel
    public class JsonResult {
        @ApiModelProperty(value = "状态码", example="40001", required = true, position=-2)
        private String code;
        @ApiModelProperty(value = "返回消息", example="恭喜，成功！", required = true, position=-1)
        private String message;
        @ApiModelProperty(value = "具体数据")
        private Object data;

        //constructors, getters, setters 略。..
    }
    ```

    User.java

    ```java
    @ApiModel
    public class User extends JsonResult {
        @ApiModelProperty(value = "专题详情", required = true)
        private Topic data;

        //constructors, getters, setters 略。..
    }
    ```

4)	访问 /swagger-ui.html，即可阅读完整文档

    ![image](http://img.cdn.firejq.com/jpg/2017/10/30/486cad5ff3fdb4abbcaf21c2590b3d05.jpg)

5)	设定访问 API doc 的路由

    在配置文件中，application.yml 中声明：
    ```
    springfox.documentation.swagger.v2.path: /api-docs
    ```
    这个 path 就是 json 的访问 request mapping. 可以自定义，防止与自身代码冲突。

    API doc 的显示路由是：http://localhost:8080/swagger-ui.html

    如果项目是一个 webservice，通常设定 home / 重定向指向这里：

    ```java
    @Controller
    public class HomeController {

        @RequestMapping(value = "/swagger")
        public String index() {
            System.out.println("swagger-ui.html");
            return "redirect:swagger-ui.html";
        }
    }
    ```

6)	安全隐患

    整合 spring Security 实现访问 API 页面输入用户名密码

    maven 依赖：spring-boot-starter-security

    配置文件添加：

    ```
    security.basic.path=/swagger-ui.html
    security.basic.enabled=true
    security.user.name=lovnx
    security.user.password=123456
    ```

    Swagger 存在一定安全漏洞，需要使用官方最新版并且自己完全安全认证措施；

## 10. 使用 JHipster

## 11. 测试

### 11.1. 单元测试

http://www.ityouknow.com/springboot/2017/05/09/springboot-deploy.html

1)	引入依赖

    spring boot 的单元测试依赖于 spring-boot-starter-test；

2)	编写测试类

    ```java
    @RunWith(SpringRunner.class)
    @SpringBootTest
    public class ApplicationTests {

      @Test
      public void hello() {
        System.out.println("hello world");
      }

    }
    ```

    spring-boot-starter-test 还增加了对 Controller 层测试的支持：

    ```java
    // 简单验证结果集是否正确
    Assert.assertEquals(3, userMapper.getAll().size());

    // 验证结果集，提示

    Assert.assertTrue("错误，正确的返回值为 200", status == 200);
    Assert.assertFalse("错误，正确的返回值为 200", status != 200);
    ```

    引入了 MockMvc 支持了对 Controller 层的测试，简单示例如下：

    ```java
    public class HelloControlerTests {

        private MockMvc mvc;

        // 初始化执行
        @Before
        public void setUp() throws Exception {
            mvc = MockMvcBuilders.standaloneSetup(new HelloController()).build();
        }

        // 验证 controller 是否正常响应并打印返回结果
        @Test
        public void getHello() throws Exception {
            mvc.perform(MockMvcRequestBuilders.get("/hello").accept(MediaType.APPLICATION_JSON))
                    .andExpect(MockMvcResultMatchers.status().isOk())
                    .andDo(MockMvcResultHandlers.print())
                    .andReturn();
        }

        // 验证 controller 是否正常响应并判断返回结果是否正确
        @Test
        public void testHello() throws Exception {
            mvc.perform(MockMvcRequestBuilders.get("/hello").accept(MediaType.APPLICATION_JSON))
                    .andExpect(status().isOk())
                    .andExpect(content().string(equalTo("Hello World")));
        }

    }
    ```

### 11.2. 集成测试

整体开发完成之后进入集成测试，spring boot 项目的启动入口在 Application 类中，直接运行 run 方法就可以启动项目；

## 12. 定时任务

### 12.1. 使用 Schedule

1)	引入依赖

    spring boot 的定时任务依赖于 springboot starter，但若使用了 springboot starter web 则无须再添加（已包含）；

2)	启动类启用定时任务

    在 Spring Boot 的主类中加入 @EnableScheduling 注解，启用定时任务的配置；

3)	创建定时任务

    在能被 spring 扫描后注入的类（如 @Service、@Component）中创建定时任务，并用 @Scheduled 注解标记，项目启动会自动执行使用了该注解的方法；

    例：

    ```java
    @Component
    public class SchedulerTask {

        private int count=0;

        @Scheduled(cron="*/6 * * * * ?")
        private void process() {
            System.out.println("this is scheduler task runing  "+(count++));
        }

    }
    ```

    @Scheduled 的参数可接收两种类型：

    - cron 表达式：

      注：cron 一共有 7 位，但是最后一位是年，可以留空，所以我们可以写 6 位：
      * 第一位，表示秒，取值 0-59
      * 第二位，表示分，取值 0-59
      * 第三位，表示小时，取值 0-23
      * 第四位，日期天 / 日，取值 1-31
      * 第五位，日期月份，取值 1-12
      * 第六位，星期，取值 1-7，星期一，星期二。..，注：不是第 1 周，第二周的意思，1 是星期天，2 是星期一……
      * 第 7 为，年份，可以留空，取值 1970-2099

      特殊符号：
      -	(*) 星号：可以理解为每的意思，每秒，每分，每天，每月，每年。..；
      -	(?) 问号：问号只能出现在日期和星期这两个位置，表示这个位置的值不确定，每天 3 点执行，所以第六位星期的位置，我们是不需要关注的，就是不确定的值。同时：日期和星期是两个相互排斥的元素，通过问号来表明不指定值。比如，1 月 10 日，比如是星期 1，如果在星期的位置是另指定星期二，就前后冲突矛盾了；
      -	(-) 减号：表达一个范围，如在小时字段中使用“10-12”，则表示从 10 到 12 点，即 10,11,12；
      -	(,) 逗号：表达一个列表值，如在星期字段中使用“1,2,4”，则表示星期一，星期二，星期四；
      -	(/) 斜杠：如：x/y，x 是开始值，y 是步长，比如在第一位（秒） 0/15 就是，从 0 秒开始，每 15 秒，最后就是 0，15，30，45，60    另：*/y，等同于 0/y；

      例：
      ```
      0 0 3 * * ?     每天 3 点执行
      0 5 3 * * ?     每天 3 点 5 分执行
      0 5 3 ? * *     每天 3 点 5 分执行，与上面作用相同
      0 5/10 3 * * ?  每天 3 点的 5 分，15 分，25 分，35 分，45 分，55 分这几个时间点执行
      0 10 3 ? * 1    每周星期天，3 点 10 分 执行，注：1 表示星期天
      0 10 3 ? * 1#3  每个月的第三个星期，星期天 执行，#号只能出现在星期的位置
      ```

    - fixedRate 风格，单位是毫秒：

      例：
      ```
      @Scheduled(fixedRate = 6000) ：上一次开始执行时间点之后 6 秒再执行，即每隔多久执行一次，不论你业务执行花费了多少时间
      @Scheduled(fixedDelay = 6000) ：上一次执行完毕时间点之后 6 秒再执行，即每次执行完毕后多久再执行一次
      @Scheduled(initialDelay=1000, fixedRate=6000) ：第一次延迟 1 秒后执行，之后按 fixedRate 的规则每 6 秒执行一次
      ```

4) 更改执行任务的时间

    http://blog.csdn.net/qq_34125349/article/details/77430956

5) 停止、启动任务

    http://blog.csdn.net/qq_34125349/article/details/77430956

NOTE：
- 集群环境中定时任务存在的问题

    http://412887952-qq-com.iteye.com/blog/2369020

    若是在应用服务器集群中，spring Schedule 会出现任务多次被调度执行的情况，因为集群的节点之间是不会共享任务信息的，每个节点上的任务都会按时执行。比如我们部署了 3 个实例，三个实例一启动，就会把定时任务都启动，那么在同一个时间点，定时任务会一起执行，也就是会执行 3 次，这样很可能会导致我们的业务出现错误。

    解决方案：

    逻辑分离，就是我们将真正要定时任务处理的逻辑，写成 rest 服务，或者 rpc 服务，然后我们可以新建一个单独的定时任务项目，这个项目应该是没有任何的业务代码的，他纯粹只有定时任务功能，几点启动，或者每隔多少时间启动，启动后，通过 rest 或者 rpc 的方式，调用真正处理逻辑的服务。同时，我们甚至可以不用新建一个项目，我们通过 linux 的 cron 就可以进行。同时，这种方式还有一个好处，比如有些时候，我们的定时任务也会因为某些原因出现问题，没有执行，那么我们就可以通过 curl 或者 wget 等等很多方式，再次定时任务的执行。

- 多个定时任务默认运行在同一个线程池的同一个线程中

    https://www.jianshu.com/p/0db083bf4d39

    Spring Boot 中默认所有的定时任务都是在同一个线程池中的同一个线程来完成的。在实际开发过程中，我们当然不希望所有的任务都运行在一个线程中。

    解决方案：

    通过 ScheduleConfig 配置文件实现 SchedulingConfigurer 接口，并重写 setSchedulerfang 方法：
    ```java
    @Configuration
    public class ScheduleConfig implements SchedulingConfigurer {
        @Override
        public void configureTasks(ScheduledTaskRegistrar taskRegistrar) {

            taskRegistrar.setScheduler(Executors.newScheduledThreadPool(5));
        }
    }
    ```

### 12.2. 使用 Quartz

https://www.cnblogs.com/javanoob/p/springboot_schedule.html

https://www.cnblogs.com/lic309/p/4089633.html

Quartz 是一个功能完善的任务调度框架，特别牛叉的是它支持集群环境下的任务调度，当然代价也很大，需要将任务调度状态序列化到数据库。Quartz 框架需要 10 多张表协同，配置繁多。

## 13. 使用 Shiro

官方网站：http://shiro.apache.org/

中文文档：https://waylau.gitbooks.io/apache-shiro-1-2-x-reference/content/

Shiro 是 Apache 下的一个开源项目，我们称之为 Apache Shiro。它是一个很易用与 Java 项目的的安全框架，提供了认证、授权、加密、会话管理，与 Spring Security 一样都是做一个权限的安全框架，但是与 Spring Security 相比，在于 Shiro 使用了比较简单易懂易于使用的授权方式。

Apache Shiro 的三大核心组件：
![image](http://img.cdn.firejq.com/jpg/2017/10/30/1f1f5f6d266496add107c8532ab1cac7.jpg)

Apache Shiro 核心通过 Filter 来实现，通过 URL 规则来进行过滤和权限校验，所以我们需要定义一系列关于 URL 的规则和访问权限。

另外我们可以通过 Shiro 提供的会话管理来获取 Session 中的信息。Shiro 也提供了缓存支持，使用 CacheManager 来管理。

1)	引入依赖 shiro-spring
    ```
    compile group: 'org.apache.shiro', name: 'shiro-spring', version: "${ShiroVersion}"
    ```

## 14. 使用 Spring Security

官方教程 https://docs.spring.io/spring-security/site/docs/current/guides/html5/helloworld-boot.html

## 15. 使用 Spring AOP

[官方文档](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/aop.html)

[拦截规则写法](https://www.jianshu.com/p/def4c497571c)

http://blog.csdn.net/gfd54gd5f46/article/details/77001551

http://blog.csdn.net/clementad/article/details/52035199

https://my.oschina.net/itblog/blog/211693

系列教程：http://jinnianshilongnian.iteye.com/blog/1418596

使用 @Aspect 注解将一个 Java 类定义为切面类

- @Pointcut 定义一个切入点，可以是一个规则表达式，比如下例中某个 package 下的所有函数，也可以是一个注解等。

- @Before 在切入点开始处切入内容

- @After 在切入点结尾处切入内容

- @AfterReturning 在切入点 return 内容之后切入内容（可以用来对处理返回值做一些加工处理）

- @Around 在切入点前后切入内容，并自己控制何时执行切入点自身的内容

- @AfterThrowing 用来处理当切入内容部分抛出异常之后的处理逻辑

具体执行顺序：
- 在 @Around 方法中调用了`proceedingJoinPoint.proceed()`：
    - 进入 @Around 方法
    - 执行 @Around 方法中的`proceedingJoinPoint.proceed()`
    - 进入 @Before 方法
    - 退出 @Before 方法
    - 执行目标方法
    - 退出 @Around 方法
    - 进入 @After 方法
    - 退出 @After 方法
    - 进入 @AfterReturning 方法
    - 退出 @AfterReturning 方法

- 在 @Around 方法中没有调用`proceedingJoinPoint.proceed()`：
    - 进入 @Around 方法
    - 退出 @Around 方法
    - 进入 @After 方法
    - 退出 @After 方法
    - 进入 @AfterReturning 方法
    - 退出 @AfterReturning 方法

例：拦截指定包下所有类的所有方法
```java
@Aspect
@Order(5)
@Component
public class WebLogAspect {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    private ThreadLocal<Long> startTime = new ThreadLocal<>();

    /**
     * 定义拦截规则
     */
    @Pointcut("execution(public * com.demo.demo.controller..*.*(..))")
    public void webLog(){}

    @Around("webLog()")
    public Object Interceptor(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        logger.info("enter Around");

        Object result = null;
        //		result = new String("firejq test");

        if (result == null) {
            // 一切正常的情况下，继续执行被拦截的方法
            result = proceedingJoinPoint.proceed();
        }

        logger.info("exit Around");

        return result;
    }

    @Before("webLog()")
    public void doBefore(JoinPoint joinPoint) throws Throwable {
        logger.info("enter before");

        // 接收到请求，记录请求内容
        startTime.set(System.currentTimeMillis());

        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder
                .getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();

        // 记录请求内容
        logger.info("URL : " + request.getRequestURL().toString());
        logger.info("HTTP_METHOD : " + request.getMethod());
        logger.info("IP : " + request.getRemoteAddr());
        logger.info("CLASS_METHOD : " + joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName());
        logger.info("ARGS : " + Arrays.toString(joinPoint.getArgs()));

        logger.info("exit before");
    }

    // 声明例外通知
    @AfterThrowing(pointcut = "webLog()", throwing = "e")
    public void doAfterThrowing(Exception e){
        logger.info("enter afterThrowing");
        logger.info("exit afterThrowing");
    }

    @After("webLog()")
    public void doAfter(JoinPoint joinPoint) throws Throwable {
        logger.info("enter after");
        logger.info("exit after");
    }

    @AfterReturning(pointcut = "webLog()", returning = "returnValue")
    public void doAfterReturning(JoinPoint joinPoint, Object returnValue) throws Throwable {
        logger.info("enter afterReturn");

        // 处理完请求，返回内容
        if (returnValue != null) {
            logger.info("RESPONSE : " + returnValue);
            logger.info("SPEND TIME : " +
                                (System.currentTimeMillis() - startTime.get()));
        }

        logger.info("exit afterReturn");
    }

}
```
```java
@RestController
public class UserController {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    @GetMapping(value = "/test")
    public String test() {
        logger.info("enter controller");
        return "test123";
    }
}
```

例：拦截指定注解标注的方法
```java
@Target(ElementType.METHOD)// 字段注解 , 用于描述方法
@Retention(RetentionPolicy.RUNTIME)// 在运行期保留注解信息
public @interface LogAop {
    String name() default "Log";
}

@Component
@Aspect
public class LogAspect {

    private static final Logger LOG = LoggerFactory.getLogger(LogAspect.class);

    @Before("@annotation(log)")
    public void beforeTest(JoinPoint joinPoint, LogAop log) throws Throwable {
        LOG.info("进入：" + log.name());
        LOG.info("CLASS_METHOD : " + joinPoint.getSignature().getDeclaringTypeName() + "."
                + joinPoint.getSignature().getName());
        LOG.info("参数 : " + Arrays.toString(joinPoint.getArgs()));
    }

    @After("@annotation(log)")
    public void afterTest(JoinPoint point, LogAop log) {
        LOG.info("退出：" + log.name());
    }
}

@RestController
@RequestMapping("/aop")
public class AopController {

    @LogAop(name="/aop/aop.action") // 添加了注解之后才会被拦截
    @RequestMapping("/aop")
    public String aop(){
        return "hello world!";
    }

    @RequestMapping("/noAop")    // 这个方法是不会被拦截的
    public String noAop(){
        return "hello world!";
    }
}
```

## 16. Spring RestTemplate

TODO:

https://www.jianshu.com/p/c9644755dd5e

http://www.cnblogs.com/tomcatandjerry/p/5899722.html

http://www.baeldung.com/rest-template

https://blog.csdn.net/u012702547/article/details/77917939

## 17. Refer Links