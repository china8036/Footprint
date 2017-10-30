- [Spring Boot Note](#spring-boot-note)
  - [概述](#%E6%A6%82%E8%BF%B0)
  - [Spring Boot CLI 安装](#spring-boot-cli-%E5%AE%89%E8%A3%85)
  - [运行 spring boot 项目](#%E8%BF%90%E8%A1%8C-spring-boot-%E9%A1%B9%E7%9B%AE)
  - [Spring Boot 配置选项](#spring-boot-%E9%85%8D%E7%BD%AE%E9%80%89%E9%A1%B9)
    - [常用配置](#%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE)
      - [.properties](#properties)
      - [.yml](#yml)
  - [日志管理](#%E6%97%A5%E5%BF%97%E7%AE%A1%E7%90%86)
    - [配置](#%E9%85%8D%E7%BD%AE)
      - [配置日志级别【格式：logging.level. 包名 = 级别】：](#%E9%85%8D%E7%BD%AE%E6%97%A5%E5%BF%97%E7%BA%A7%E5%88%AB%E3%80%90%E6%A0%BC%E5%BC%8F%EF%BC%9Alogginglevel-%E5%8C%85%E5%90%8D-%E7%BA%A7%E5%88%AB%E3%80%91%EF%BC%9A)
      - [配置日志输出文件：](#%E9%85%8D%E7%BD%AE%E6%97%A5%E5%BF%97%E8%BE%93%E5%87%BA%E6%96%87%E4%BB%B6%EF%BC%9A)
      - [格式化日志](#%E6%A0%BC%E5%BC%8F%E5%8C%96%E6%97%A5%E5%BF%97)
      - [自定义日志框架配置](#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%97%A5%E5%BF%97%E6%A1%86%E6%9E%B6%E9%85%8D%E7%BD%AE)
      - [代码中使用](#%E4%BB%A3%E7%A0%81%E4%B8%AD%E4%BD%BF%E7%94%A8)
  - [异常处理](#%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86)
  - [静态资源目录](#%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E7%9B%AE%E5%BD%95)
  - [模板页面目录](#%E6%A8%A1%E6%9D%BF%E9%A1%B5%E9%9D%A2%E7%9B%AE%E5%BD%95)
  - [使用 spring boot data JPA](#%E4%BD%BF%E7%94%A8-spring-boot-data-jpa)
    - [引入依赖 spring-boot-starter-data-jpa](#%E5%BC%95%E5%85%A5%E4%BE%9D%E8%B5%96-spring-boot-starter-data-jpa)
    - [配置【application.yml】](#%E9%85%8D%E7%BD%AE%E3%80%90applicationyml%E3%80%91)
    - [编写实体类：](#%E7%BC%96%E5%86%99%E5%AE%9E%E4%BD%93%E7%B1%BB%EF%BC%9A)
    - [编写数据访问接口：](#%E7%BC%96%E5%86%99%E6%95%B0%E6%8D%AE%E8%AE%BF%E9%97%AE%E6%8E%A5%E5%8F%A3%EF%BC%9A)
      - [接口自动实现的方法](#%E6%8E%A5%E5%8F%A3%E8%87%AA%E5%8A%A8%E5%AE%9E%E7%8E%B0%E7%9A%84%E6%96%B9%E6%B3%95)
      - [复杂查询方法](#%E5%A4%8D%E6%9D%82%E6%9F%A5%E8%AF%A2%E6%96%B9%E6%B3%95)
        - [分页查询](#%E5%88%86%E9%A1%B5%E6%9F%A5%E8%AF%A2)
        - [排序查询](#%E6%8E%92%E5%BA%8F%E6%9F%A5%E8%AF%A2)
        - [限制查询](#%E9%99%90%E5%88%B6%E6%9F%A5%E8%AF%A2)
        - [自定义 SQL 查询](#%E8%87%AA%E5%AE%9A%E4%B9%89-sql-%E6%9F%A5%E8%AF%A2)
        - [多表查询](#%E5%A4%9A%E8%A1%A8%E6%9F%A5%E8%AF%A2)
  - [使用 spring boot data rest](#%E4%BD%BF%E7%94%A8-spring-boot-data-rest)
  - [使用事务](#%E4%BD%BF%E7%94%A8%E4%BA%8B%E5%8A%A1)
    - [高级使用](#%E9%AB%98%E7%BA%A7%E4%BD%BF%E7%94%A8)
      - [指定不同的事务管理器](#%E6%8C%87%E5%AE%9A%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86%E5%99%A8)
      - [隔离级别控制](#%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB%E6%8E%A7%E5%88%B6)
      - [传播行为](#%E4%BC%A0%E6%92%AD%E8%A1%8C%E4%B8%BA)
  - [结合 Mybatis](#%E7%BB%93%E5%90%88-mybatis)
    - [基本操作](#%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
    - [mapper 的注解支持](#mapper-%E7%9A%84%E6%B3%A8%E8%A7%A3%E6%94%AF%E6%8C%81)
      - [@Insert](#insert)
      - [@Update](#update)
      - [@Delete](#delete)
      - [@Select](#select)
        - [结果映射](#%E7%BB%93%E6%9E%9C%E6%98%A0%E5%B0%84)
          - [普通映射](#%E6%99%AE%E9%80%9A%E6%98%A0%E5%B0%84)
          - [一对一映射](#%E4%B8%80%E5%AF%B9%E4%B8%80%E6%98%A0%E5%B0%84)
          - [一对多映射](#%E4%B8%80%E5%AF%B9%E5%A4%9A%E6%98%A0%E5%B0%84)
    - [使用 mybatis-generator](#%E4%BD%BF%E7%94%A8-mybatis-generator)
    - [多数据源配置](#%E5%A4%9A%E6%95%B0%E6%8D%AE%E6%BA%90%E9%85%8D%E7%BD%AE)
    - [使用 HikariCP 连接池](#%E4%BD%BF%E7%94%A8-hikaricp-%E8%BF%9E%E6%8E%A5%E6%B1%A0)
    - [使用 mybatis-plus](#%E4%BD%BF%E7%94%A8-mybatis-plus)
  - [使用数据库版本工具](#%E4%BD%BF%E7%94%A8%E6%95%B0%E6%8D%AE%E5%BA%93%E7%89%88%E6%9C%AC%E5%B7%A5%E5%85%B7)
    - [flyway](#flyway)
    - [liquibase](#liquibase)
  - [使用 Redis](#%E4%BD%BF%E7%94%A8-redis)
  - [使用 Actuator](#%E4%BD%BF%E7%94%A8-actuator)
  - [使用 Lombok](#%E4%BD%BF%E7%94%A8-lombok)
  - [使用 Swagger](#%E4%BD%BF%E7%94%A8-swagger)
  - [使用 JHipster](#%E4%BD%BF%E7%94%A8-jhipster)
  - [测试](#%E6%B5%8B%E8%AF%95)
    - [单元测试](#%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
    - [集成测试](#%E9%9B%86%E6%88%90%E6%B5%8B%E8%AF%95)
  - [部署](#%E9%83%A8%E7%BD%B2)
    - [IDEA 中开启 spring boot 热部署](#idea-%E4%B8%AD%E5%BC%80%E5%90%AF-spring-boot-%E7%83%AD%E9%83%A8%E7%BD%B2)
      - [使用 spring boot devtools](#%E4%BD%BF%E7%94%A8-spring-boot-devtools)
      - [使用 JRebel](#%E4%BD%BF%E7%94%A8-jrebel)
    - [打包与部署](#%E6%89%93%E5%8C%85%E4%B8%8E%E9%83%A8%E7%BD%B2)
      - [打包为 jar 包](#%E6%89%93%E5%8C%85%E4%B8%BA-jar-%E5%8C%85)
        - [maven 版本](#maven-%E7%89%88%E6%9C%AC)
        - [gradle 版本](#gradle-%E7%89%88%E6%9C%AC)
      - [打包为 war 包](#%E6%89%93%E5%8C%85%E4%B8%BA-war-%E5%8C%85)
      - [打包为 docker 镜像](#%E6%89%93%E5%8C%85%E4%B8%BA-docker-%E9%95%9C%E5%83%8F)
  - [定时任务](#%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1)
    - [使用Schedule](#%E4%BD%BF%E7%94%A8schedule)
    - [使用Quartz](#%E4%BD%BF%E7%94%A8quartz)
  - [使用Shiro](#%E4%BD%BF%E7%94%A8shiro)
  - [使用Spring Security](#%E4%BD%BF%E7%94%A8spring-security)
  - [Refer Links](#refer-links)

# Spring Boot Note

## 概述

> spring boot 代替了什么，Spring、SpringMVC/Struts2、hibernate/Mybatis？<br/>     
> 个人理解：代替了 spring。用代替 / 取代来解释貌似都不好，更准确的可能是封装了 spring，使搭建 SSH/SSM 更快捷。     
传统的 spring 有很多 xml 配置，例如：dataSource、transactionManager、AOP、bean 等等 xml 的配置。即便用注解，也要在 xml 中配置 component-scan 等。         
但在 spring boot，遵循“约定大于配置”，所以尽可能的避免了 xml 配置。

<!-- TODO spring 官方提供的前端模板是 thymeleaf，但只要了解即可，不推荐使用，因为在最新的开发模式中，前后端完全分离，后端只需要提供 RESTful 接口，传递如 json 格式的数据给前端即可；而且使用模板会给性能上带来很大的损耗。
具体应该怎么做？？http://blog.didispace.com/springbootweb/ ？
 -->

<!-- TODO @RestController=@Response+@Controller -->

<!-- TODO 降耦合原则：项目尽量不要出现任何重复的代码，便于维护 -->

## Spring Boot CLI 安装

https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-gradle-installation 

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/6018abea201baec92ce68897f55a7de3.jpg)

下载后解压，将 xxx\spring-boot-cli-xxxx.RELEASE-bin\bin 添加到环境变量即可

验证安装成功：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/a7b5433aa57776d7f28c698ef7407435.jpg)

## 运行 spring boot 项目

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/3606cffe1d51ec0a0f19fb5e4489f8df.jpg)

出现 Started TestdemoApplication in 2.916 seconds (JVM running for 3.43) 字样，表明 spring boot 启动成功；

## Spring Boot 配置选项

### 常用配置

#### .properties

```ini
#disbale Spring banner
#spring.main.banner-mode=off
banner.charset=UTF-8
banner.location=classpath:banner.txt

# 服务器配置
server.port=8080

# 数据库配置
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/wincc?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC&useSSL=false
spring.datasource.username=root
spring.datasource.password=root
# 数据库连接池配置
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
spring.datasource.hikari.maximum-pool-size=20
spring.datasource.hikari.max-lifetime=60000
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.data-source-properties.prepStmtCacheSize=250
spring.datasource.hikari.data-source-properties.prepStmtCacheSqlLimit=2048
spring.datasource.hikari.data-source-properties.cachePrepStmts=true
spring.datasource.hikari.data-source-properties.useServerPrepStmts=true

# Mybatis 配置
mybatis.configuration.map-underscore-to-camel-case=true
mybatis.configuration.default-fetch-size=100
mybatis.configuration.default-statement-timeout=30

# 数据库版本管理 flyway 配置
#flyway.enabled=false
flyway.baseline-on-migrate=true

# spring boot starter actutor 配置
management.security.enabled=true

```

#### .yml

```yaml
# 服务器配置
server:
    port: 8080

spring:
    # 数据库连接配置
    datasource:
        driver-class-name: com.mysql.jdbc.Driver
        password: root
        url: jdbc:mysql://127.0.0.1:3306/wincc?useUnicode=true&characterEncoding=utf8&serverTimezone=UTC&useSSL=false
        username: root
        # 数据库连接池 hikariCP 配置
        type: com.zaxxer.hikari.HikariDataSource
        hikari:
            data-source-properties:
                cachePrepStmts: true
                prepStmtCacheSize: 250
                prepStmtCacheSqlLimit: 2048
                useServerPrepStmts: true
            idle-timeout: 30000
            max-lifetime: 60000
            maximum-pool-size: 20

# Mybatis 配置
mybatis:
    configuration:
        default-fetch-size: 100
        default-statement-timeout: 30
        map-underscore-to-camel-case: true

# Flyway 配置
flyway:
    baseline-on-migrate: true

# Actuator 配置
management:
    security:
        enabled: true

# 其它配置
banner:
    charset: UTF-8
    location: classpath:banner.txt

```

## 日志管理

spring boot 支持 Java Util Logging、Log4J、Log4J2、Logback 作为日志框架，默认情况下，Spring Boot 会用 Logback 来记录日志，并用 INFO 级别输出到控制台；

spring boot 的日志管理依赖于 spring-boot-starter-logging，但实际开发中我们不需要直接添加该依赖，因为 spring-boot-starter 其中包含了 spring-boot-starter-logging；

spring boot 对各种支持的日志框架的控制台输出和文件输出做好了默认配置；

### 配置

Spring Boot 为我们提供了很多默认的日志配置，所以，只要将 spring-boot-starter-logging 作为依赖加入到当前应用的 classpath，则“开箱即用”，但我们仍可以根据具体需求在【application.properties】中更改配置选项：

#### 配置日志级别【格式：logging.level. 包名 = 级别】：
```
logging.level.org.springframework.web = DEBUG
```
日志级别从低到高分为 TRACE < DEBUG < INFO < WARN < ERROR < FATAL，如果设置为 WARN，则低于 WARN 的信息都不会输出。

Spring Boot 中默认配置 ERROR、WARN 和 INFO 级别的日志输出到控制台。您还可以通过启动您的应用程序–debug 标志来启用“调试”模式（开发的时候推荐开启）；

#### 配置日志输出文件：
默认情况下，Spring Boot 将日志输出到控制台，不会写到日志文件。如果要编写除控制台输出之外的日志文件，则需在 application.properties 中设置 logging.file 或 logging.path 属性；
```
logging.file，设置文件，可以是绝对路径，也可以是相对路径。如：logging.file=my.log；
logging.path，设置目录，会在该目录下创建 spring.log 文件，并写入日志内容，如：logging.path=/var/log；
```

如果只配置 logging.file，会在项目的当前路径下生成一个 xxx.log 日志文件。

如果只配置 logging.path，在 /var/log 文件夹生成一个日志文件为 spring.log

注：

二者不能同时使用，如若同时使用，则只有 logging.file 生效；

默认情况下，日志文件的大小达到 10MB 时会切分一次，产生新的日志文件；

例：
```
logging.file = D:/mylog/log.log
```

#### 格式化日志
默认的日志输出如下：
```
2016-04-13 08:23:50.120  INFO 37397 --- [main] org.hibernate.Version: HHH000412: Hibernate Core {4.3.11.Final}
```
输出内容元素具体如下：

时间日期 — 精确到毫秒

日志级别 — ERROR, WARN, INFO, DEBUG or TRACE

进程 ID

分隔符 — --- 标识实际日志的开始

线程名 — 方括号括起来（可能会截断控制台输出）

Logger 名 — 通常使用源代码的类名

日志内容

#### 自定义日志框架配置
由于日志服务一般都在 ApplicationContext 创建前就初始化了，它并不是必须通过 Spring 的配置文件控制。因此通过系统属性和传统的 Spring Boot 外部配置文件依然可以很好的支持日志控制和管理。

根据不同的日志系统，你可以在 src/main/resources 下，按如下规则组织配置文件名，就能被正确加载：
```
Logback：logback-spring.xml, logback-spring.groovy, logback.xml, logback.groovy
Log4j：log4j-spring.properties, log4j-spring.xml, log4j.properties, log4j.xml
Log4j2：log4j2-spring.xml, log4j2.xml
JDK (Java Util Logging)：logging.properties
```
Spring Boot 官方推荐优先使用带有 -spring 的文件名作为你的日志配置（如使用 logback-spring.xml，而不是 logback.xml），命名为 logback-spring.xml 的日志配置文件，spring boot 可以为它添加一些 spring boot 特有的配置项；

也可在【application.properties】中直接指定：
```
logging.config=classpath:logging-config.xml
```

logback-spring.xml 例子：

具体解释见 http://tengj.top/2017/04/05/springboot7/ 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration  scan="true" scanPeriod="60 seconds" debug="false">
    <contextName>logback</contextName>
    <property name="log.path" value="E:\\test\\logback.log" />
    <!-- 输出到控制台 -->
    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
       <!-- <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>-->
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 输出到文件 -->
    <appender name="file" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${log.path}</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logback.%d{yyyy-MM-dd}.log</fileNamePattern>
        </rollingPolicy>
        <encoder>
            <pattern>%d{HH:mm:ss.SSS} %contextName [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="info">
        <appender-ref ref="console" />
        <appender-ref ref="file" />
    </root>

    <!-- logback 为 java 中的包 -->
    <logger name="com.dudu.controller"/>
    <!--logback.LogbackDemo：类的全路径 -->
    <logger name="com.dudu.controller.LearnController" level="WARN" additivity="false">
        <appender-ref ref="console"/>
    </logger>
</configuration>
```

#### 代码中使用
只需在类中
```
private Logger logger = LoggerFactory.getLogger(this.getClass());
```
即可使用
```
this.logger.trace(“xxxxx”);
this.logger.debug(“xxxxx”);
this.logger.info(“xxxxx”)
this.logger.warn(“xxxxx”)
this.logger.error(“xxxxx”)
```

## 异常处理

统一异常处理：

异常由内向外抛：DAO->service->controller，最后统一由 handle 包中的 @controllerAdvice 类处理

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/7a9ba77e239ea9fdac86f7f8b270372e.jpg)

## 静态资源目录

https://juejin.im/post/58f768458d6d810064a00ad6 

spring boot 默认配置的静态资源目录如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/b480667fc5a2f1c65991a7eed4dc1bd0.jpg)

优先级顺序为：`META-INF/resources > resources > static > public`

即：         
服务器启动后这四个文件夹若同时存在，则他们目录下的所有文件都相当于放在了 web 服务器的 www 中，若存在同名文件，则只访问优先级最高的文件，其它同名文件会被覆盖；

使用 webjar 管理静态资源：http://www.jianshu.com/p/d127c4f78bb8 

## 模板页面目录

templates 文件夹下的模板文件无法直接访问，要访问这些页面，有两种方法：

- 不使用模板引擎，自己配置 spring mvc 的 viewResolve；         
  https://segmentfault.com/a/1190000007008637 
  https://stackoverflow.com/questions/29953245/configure-viewresolver-with-spring-boot-and-annotations-gives-no-mapping-found-f 

  ```java
  @Configuration
  @EnableWebMvc
  public class MvcConfiguration extends WebMvcConfigurerAdapter{
      @Bean
      public ViewResolver getViewResolver() {
          InternalResourceViewResolver resolver = new InternalResourceViewResolver();
          resolver.setPrefix("/WEB-INF/");
          resolver.setSuffix(".html");
          return resolver;
      }

      @Override
      public void configureDefaultServletHandling(
              DefaultServletHandlerConfigurer configurer) {
          configurer.enable();
      }    
  }

  ```

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/ffc78353e40411b73721d04c808c5c08.jpg)

  **经过尝试，暂时找不到方法在不使用模板引擎的时候访问 templates 目录页面；** <!-- TODO -->

- 使用各种 spring-boot-start 的模板引擎，会自动配置 viewResolve；      
  http://www.jianshu.com/p/85cfe2e061fe 

## 使用 spring boot data JPA
JPA(Java Persistence API) 是 Sun 官方提出的 Java 持久化规范。它为 Java 开发人员提供了一种对象 / 关联映射工具来管理 Java 应用中的关系数据。他的出现主要是为了简化现有的持久化开发工作和整合 ORM 技术，结束现在 Hibernate，TopLink，JDO 等 ORM 框架各自为营的局面。值得注意的是，JPA 是在充分吸收了现有 Hibernate，TopLink，JDO 等 ORM 框架的基础上发展而来的，具有易于使用，伸缩性强等优点。

注意：JPA 是一套规范，不是一套产品，那么像 Hibernate,TopLink,JDO 他们是一套产品，如果说这些产品实现了这个 JPA 规范，那么我们就可以叫他们为 JPA 的实现产品。

### 引入依赖 spring-boot-starter-data-jpa
    ```
    compile('org.springframework.boot:spring-boot-starter-data-jpa')
    ```

### 配置【application.yml】
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

### 编写实体类：
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

### 编写数据访问接口：
```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```
说明：

http://www.ityouknow.com/springboot/2016/08/20/springboot(%E4%BA%94)-spring-data-jpa%E7%9A%84%E4%BD%BF%E7%94%A8.html 

这个接口中我们没有定义任何操作方法，而是直接继承于 PagingAndSortingRepository 接口，该接口本身已经实现了创建（save）、更新（save）、删除（delete）、查询（findAll、findOne）等基本操作的函数，因此对于这些基础操作的数据访问就不需要开发者再自己定义。

#### 接口自动实现的方法

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/d958459a5671411daa55a05c0a69aee4.jpg)

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

 

#### 复杂查询方法

在实际的开发中我们需要用到分页、删选、连表等查询的时候就需要特殊的方法或者自定义 SQL，可以通过使用 @Query 注解来创建查询，您只需要编写 JPQL 语句，并通过类似“:name”来映射 @Param 指定的参数

##### 分页查询
分页查询在实际使用中非常普遍了，spring data jpa 已经帮我们实现了分页的功能，在查询的方法中，需要传入参数 Pageable , 当查询中有多个参数的时候 Pageable 建议做为最后一个参数传入
```java
Page<User> findALL(Pageable pageable);// 自动实现

Page<User> findByUserName(String userName, Pageable pageable);// 需要自定义
Pageable 是 spring 封装的分页实现类，使用的时候需要传入页数、每页条数和排序规则；
使用 (@RestController 中)：
Page<User> pageUsers = UserRepository.findAll(new PageRequest(1, 2));
return pageUsers;
```

##### 排序查询
```java
List<Person> findAll(Sort sort);// 自动实现
```
使用
```java
List<Person> people =  personRepository.findAll(new Sort(Direction.DESC, “age”));
return people;
```

##### 限制查询
有时候我们只需要查询前 N 个元素，或者支取前一个实体。
```java
ser findFirstByOrderByLastnameAsc();

User findTopByOrderByAgeDesc();

Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);

List<User> findFirst10ByLastname(String lastname, Sort sort);

List<User> findTop10ByLastname(String lastname, Pageable pageable);
```
##### 自定义 SQL 查询
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
##### 多表查询
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

## 使用 spring boot data rest

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

## 使用事务
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

### 高级使用
http://blog.didispace.com/springboottransactional/ 

#### 指定不同的事务管理器
一般我们直接使用默认的事务配置，就可以满足一些基本的事务需求，但是当我们项目较大较复杂时（比如，有多个数据源等），这时候需要在声明事务时，指定不同的事务管理器。在声明事务时，只需要通过 value 属性指定配置的事务管理器名即可，例如：@Transactional(value="transactionManagerPrimary")。

#### 隔离级别控制
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

#### 传播行为
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

## 结合 Mybatis

官方文档 http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/ 

如何优雅使用 mybatis：http://www.ityouknow.com/springboot/2016/11/06/springboot（六)- 如何优雅的使用 mybatis.html 

### 基本操作

1)  引入依赖 mybatis-spring-boot-starter 和 mysql-connector-java【build.gradle】
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

    
    常用的配置：
    ```
    # 使用《驼峰命名法》进行数据库表列与实体类属性的映射
    mybatis.configuration.map-underscore-to-camel-case=true

    mybatis.configuration.default-fetch-size=100
    mybatis.configuration.default-statement-timeout=30
    ```

4)	编写实体类
    注意：实体类必须有无参构造函数，否则 mybatis 无法完成映射；

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

### mapper 的注解支持

官方文档：http://www.mybatis.org/mybatis-3/zh/java-api.html 

#### @Insert
基本使用：
```java
public interface StudentMapper
{
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

#### @Update
例：
```java
@Update("UPDATE STUDENTS SET NAME=#{name}, EMAIL=#{email}, PHONE=#{phone} WHERE STUD_ID=#{studId}")
// 将会返回执行了 UPDATE 语句后影响的行数
int updateStudent(Student student);
```
#### @Delete
例：
```java
@Delete("DELETE FROM STUDENTS WHERE STUD_ID=#{studId}")
// 将会返回执行了 DELETE 语句后影响的行数
int deleteStudent(int studId);
```
#### @Select
例：
```java
@Select("SELECT STUD_ID AS STUDID, NAME, EMAIL, PHONE FROM STUDENTS WHERE STUD_ID=#{studId}")
Student findStudentById(Integer studId);
```
注：

如果返回了多行结果，将抛出 TooManyResultsException 异常

##### 结果映射

数据库表列与实体类属性映射的四种方法： http://blog.csdn.net/lmy86263/article/details/53150091 

a)	通过 XML 映射文件中的 resultMap；

b)	通过注解 @Results 和 @Result；

c)	通过属性配置完成映射；

d)	通过使用在 SQL 语句中定义别名完成映射；

以下使用的都是注解映射：
###### 普通映射
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

###### 一对一映射
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

###### 一对多映射

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

### 使用 mybatis-generator
http://www.jianshu.com/p/188622950cc6 

由于 mybatis-generator 官方使用的是 maven plugin，因此针对 gradle，使用第三方解决方案 https://plugins.gradle.org/plugin/com.arenagod.gradle.MybatisGenerator ，即使用第三方插件 https://github.com/kimichen13/mybatis-generator-plugin ；

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

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/ab1e524655371de9b5bd2746800533a9.jpg)

5)	针对实际需要进行修改

    a)	将生成实体类的 setter/getter 及全参构造函数删除，使用 @Data 代替；

    b)	删除 mapper/ 中的 provider 类，以及 mapper 接口中的 provider 方法，只留下基本的增删改查；

### 多数据源配置
spring boot mybatis 多数据库源（主从）配置 http://www.ityouknow.com/springboot/2016/11/25/springboot(%E4%B8%83)-springboot+mybatis%E5%A4%9A%E6%95%B0%E6%8D%AE%E6%BA%90%E6%9C%80%E7%AE%80%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.html 

### 使用 HikariCP 连接池
SpringBoot 默认使用 org.apache.tomcat.jdbc.pool.DataSource 连接池，通过更改配置可使用其它第三方连接池；
1)	引入依赖
    ```
    compile group: 'com.zaxxer', name: 'HikariCP', version: '2.6.3'
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

### 使用 mybatis-plus

官方文档：http://mp.baomidou.com/#/quick-start 

## 使用数据库版本工具

SpringBoot 支持了两种数据库迁移工具，一个是 flyway，一个是 liquibase。其本身也支持 sql script，在初始化数据源之后执行指定的脚本。

### flyway

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

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/4fbd5d0090538643cf22e9445d3ed383.jpg)

3)	创建 Migrations 脚本

    Flyway 加载 Migrations 的默认 Locations 为 classpath:db/migration，其加载是在 Runtime 自动递归地执行的；

    除了需要指定 Location 外，Flyway 对 Migrations 的扫描还必须遵从一定的命名模式，Migration 主要分为两类：Versioned 和 Repeatable：

    a)	Versioned migrations

    一般常用的是 Versioned 类型，用于版本升级，每一个版本都有一个唯一的标识并且只能被应用一次，并且不能再修改已经加载过的 Migrations，因为 Metadata 表会记录其 Checksum 值。其中的 version 标识版本号，由一个或多个数字构成，数字之间的分隔符可以采用点或下划线，在运行时下划线其实也是被替换成点了，每一部分的前导零会被自动忽略。

    b)	Repeatable migrations

    Repeatable 是指可重复加载的 Migrations，其每一次的更新会影响 Checksum 值，然后都会被重新加载，并不用于版本升级。对于管理不稳定的数据库对象的更新时非常有用。Repeatable 的 Migrations 总是在 Versioned 之后按顺序执行，但开发者必须自己维护脚本并且确保可以重复执行，通常会在 sql 语句中使用 CREATE OR REPLACE 来保证可重复执行。

    Migrations 命名规则：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/360a244c369d71c54e7668e7ef99de64.jpg)

###	liquibase
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

## 使用 Redis

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

## 使用 Actuator

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

## 使用 Lombok

lombok 是一套代码模板解决方案，将极大提升开发的效率；

1)	引入依赖
    ```
    compile group: 'org.projectlombok', name: 'lombok', version: "${LombokVersion}"
    ```
2)	IDEA 中设置

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/edb3d0efcea761a2f742ad9bd388debe.jpg)

3)	使用
    
    官网说明：https://projectlombok.org/features/all 

    | 注解                                       | 说明                                       |
    | ---------------------------------------- | ---------------------------------------- |
    | @Data                                    | 自动生成 @ToString, @EqualsAndHashCode, @Getter on all fields, and @Setter on all  non-final fields, and @RequiredArgsConstructor；  注意：不会自动生成全参 / 无参构造方法，因此通常要与 @AllArgsConstructor、@NoArgsConstructor 一起使用； |
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

## 使用 Swagger
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

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/e88dcf9de6a2feaf7b91a60e38c173f3.jpg)

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

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/486cad5ff3fdb4abbcaf21c2590b3d05.jpg)

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

## 使用 JHipster

## 测试

### 单元测试

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

### 集成测试

整体开发完成之后进入集成测试，spring boot 项目的启动入口在 Application 类中，直接运行 run 方法就可以启动项目；

## 部署

http://www.ityouknow.com/springboot/2017/05/09/springboot-deploy.html

###	IDEA 中开启 spring boot 热部署

#### 使用 spring boot devtools

在 spring boot 运行期间，对代码做出任何修改会触发 IDEA 自动重启 spring boot；

1)	spring boot 项目中加入 spring-boot-devtools 依赖
    maven：
    ```xml
    /* 依赖 */
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>

    /* 开启热部署 */
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>// 该配置必须
                </configuration>
            </plugin>
        </plugins>
    </build>
    ```
    Gradle: 在 dependencies {} 中加入：
    ```groovy
    /* 热部署 */
    runtime('org.springframework.boot:spring-boot-devtools')
    ```
2)	IDEA 中配置

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/503526609a902fda6f7ac9a2c212640a.jpg)

    CTRL + SHIFT + A --> 查找 Registry --> 找到并勾选 compiler.automake.allow.when.app.running

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/f68eef2002759a230f4dca734d0dc63c.jpg)

#### 使用 JRebel

### 打包与部署

#### 打包为 jar 包

##### maven 版本

在 IDEA 中，打开 project structure

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/2079e5202d9c112a760c3ea7bd32058b.jpg)

选择 spring boot 入口类

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/349da6a5dee54c7f801c64aaee60d2c9.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/2abfbe5609f4f86896437776e386e7b8.jpg)

双击 package

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/508b412389334268af2ff6fb6e00267d.jpg)

看到下列信息表示打包完成：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/53dfc5e76494b2b6000f8c6f002e41cc.jpg)

在 target 目录下的 jar 包即为打包后的项目文件

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/f03b2442d804300806bc4cfe29467bc0.jpg)

在 jar 包目录下，运行项目：
```powershell
java -jar xxxx.jar
```

##### gradle 版本

在 build.gradle 中加入
```groovy
jar {
    baseName = 'gs-rest-service'// 这里就是 jar 包名字
    version =  '0.1.0'// 版本号
}
```
在命令行中切换到程序目录，或者直接在 IDEA 中 ALT + F12 进行命令行后执行：
```powershell
gradle clean build
```

执行完毕后，在 build/libs/ 目录下可看到打包后的 jar 文件

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/6fa283f99a1522d4e212621c00597f57.jpg)

在 jar 包目录下，运行项目：
```powershell
java -jar xxxx.jar
```

#### 打包为 war 包

打包为 jar 包时，包含了内置的 tomcat 服务器，若希望使用专门的 tomcat 服务器或者其它服务器，可将项目打包为 war 包后部署；

https://docs.spring.io/spring-boot/docs/current/reference/html/howto-traditional-deployment.html 

https://my.oschina.net/alexnine/blog/540651 

http://www.jianshu.com/p/b3be5e54d836 

#### 打包为 docker 镜像

https://waylau.com/docker-spring-boot-gradle/ 


## 定时任务

### 使用Schedule

1)	引入依赖
    spring boot的定时任务依赖于springboot starter，但若使用了springboot starter web则无须再添加（已包含）；

2)	启动类启用定时任务
    在启动类上面加上@EnableScheduling即可开启定时任务；

3)	创建定时任务
    在能被spring扫描后注入的类（如@Service、@Component）中创建定时任务，并用@Scheduled注解标记，项目启动会自动执行使用了该注解的方法；
    
    例：

    ```java
    @Component
    public class SchedulerTask {

        private int count=0;

        @Scheduled(cron="*/6 * * * * ?")
        private void process(){
            System.out.println("this is scheduler task runing  "+(count++));
        }

    }
    ```

    @Scheduled的参数可接收两种类型：

    - cron表达式：

      注：cron一共有7位，但是最后一位是年，可以留空，所以我们可以写6位：
      * 第一位，表示秒，取值0-59
      * 第二位，表示分，取值0-59
      * 第三位，表示小时，取值0-23
      * 第四位，日期天/日，取值1-31
      * 第五位，日期月份，取值1-12
      * 第六位，星期，取值1-7，星期一，星期二...，注：不是第1周，第二周的意思，1 是星期天，2是星期一……
      * 第7为，年份，可以留空，取值1970-2099

      特殊符号：
      -	(*)星号：可以理解为每的意思，每秒，每分，每天，每月，每年...；
      -	(?)问号：问号只能出现在日期和星期这两个位置，表示这个位置的值不确定，每天3点执行，所以第六位星期的位置，我们是不需要关注的，就是不确定的值。同时：日期和星期是两个相互排斥的元素，通过问号来表明不指定值。比如，1月10日，比如是星期1，如果在星期的位置是另指定星期二，就前后冲突矛盾了；
      -	(-)减号：表达一个范围，如在小时字段中使用“10-12”，则表示从10到12点，即10,11,12；
      -	(,)逗号：表达一个列表值，如在星期字段中使用“1,2,4”，则表示星期一，星期二，星期四；
      -	(/)斜杠：如：x/y，x是开始值，y是步长，比如在第一位（秒） 0/15就是，从0秒开始，每15秒，最后就是0，15，30，45，60    另：*/y，等同于0/y；

      例：
      ```
      0 0 3 * * ?     每天3点执行
      0 5 3 * * ?     每天3点5分执行
      0 5 3 ? * *     每天3点5分执行，与上面作用相同
      0 5/10 3 * * ?  每天3点的 5分，15分，25分，35分，45分，55分这几个时间点执行
      0 10 3 ? * 1    每周星期天，3点10分 执行，注：1表示星期天    
      0 10 3 ? * 1#3  每个月的第三个星期，星期天 执行，#号只能出现在星期的位置
      ```

    - fixedRate风格，单位是毫秒：    
    
      例：
      ```
      @Scheduled(fixedRate = 6000) ：上一次开始执行时间点之后6秒再执行，即每隔多久执行一次，不论你业务执行花费了多少时间
      @Scheduled(fixedDelay = 6000) ：上一次执行完毕时间点之后6秒再执行，即每次执行完毕后多久再执行一次
      @Scheduled(initialDelay=1000, fixedRate=6000) ：第一次延迟1秒后执行，之后按fixedRate的规则每6秒执行一次
      ```

      注：http://412887952-qq-com.iteye.com/blog/2369020 

      若是在应用服务器集群中，spring Schedule会出现任务多次被调度执行的情况，因为集群的节点之间是不会共享任务信息的，每个节点上的任务都会按时执行。比如我们部署了3个实例，三个实例一启动，就会把定时任务都启动，那么在同一个时间点，定时任务会一起执行，也就是会执行3次，这样很可能会导致我们的业务出现错误。
      
      解决方案：     
      逻辑分离，就是我们将真正要定时任务处理的逻辑，写成rest服务，或者rpc服务，然后我们可以新建一个单独的定时任务项目，这个项目应该是没有任何的业务代码的，他纯粹只有定时任务功能，几点启动，或者每隔多少时间启动，启动后，通过rest或者rpc的方式，调用真正处理逻辑的服务。同时，我们甚至可以不用新建一个项目，我们通过linux的cron就可以进行。同时，这种方式还有一个好处，比如有些时候，我们的定时任务也会因为某些原因出现问题，没有执行，那么我们就可以通过curl 或者wget等等很多方式，再次定时任务的执行。


### 使用Quartz

https://www.cnblogs.com/javanoob/p/springboot_schedule.html 

https://www.cnblogs.com/lic309/p/4089633.html 

Quartz是一个功能完善的任务调度框架，特别牛叉的是它支持集群环境下的任务调度，当然代价也很大，需要将任务调度状态序列化到数据库。Quartz框架需要10多张表协同，配置繁多。

## 使用Shiro

官方网站：http://shiro.apache.org/ 

中文文档：https://waylau.gitbooks.io/apache-shiro-1-2-x-reference/content/ 

Shiro是Apache下的一个开源项目，我们称之为Apache Shiro。它是一个很易用与Java项目的的安全框架，提供了认证、授权、加密、会话管理，与 Spring Security 一样都是做一个权限的安全框架，但是与Spring Security 相比，在于 Shiro 使用了比较简单易懂易于使用的授权方式。

Apache Shiro 的三大核心组件:     
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/1f1f5f6d266496add107c8532ab1cac7.jpg)

Apache Shiro 核心通过 Filter 来实现，通过URL规则来进行过滤和权限校验，所以我们需要定义一系列关于URL的规则和访问权限。

另外我们可以通过Shiro 提供的会话管理来获取Session中的信息。Shiro 也提供了缓存支持，使用 CacheManager 来管理。


1)	引入依赖shiro-spring
    ```
    compile group: 'org.apache.shiro', name: 'shiro-spring', version: "${ShiroVersion}"
    ```

## 使用Spring Security

官方教程 https://docs.spring.io/spring-security/site/docs/current/guides/html5/helloworld-boot.html 




## Refer Links

官方教程 https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/ 
