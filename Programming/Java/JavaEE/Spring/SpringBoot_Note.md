- [Spring Boot Note](#spring-boot-note)
    - [1. 概述](#1-%E6%A6%82%E8%BF%B0)
    - [2. Spring Boot CLI 安装](#2-spring-boot-cli-%E5%AE%89%E8%A3%85)
    - [3. 在 IDEA 中创建使用 gradle 的 spring boot 项目](#3-%E5%9C%A8-idea-%E4%B8%AD%E5%88%9B%E5%BB%BA%E4%BD%BF%E7%94%A8-gradle-%E7%9A%84-spring-boot-%E9%A1%B9%E7%9B%AE)
    - [4. 运行 spring boot 项目](#4-%E8%BF%90%E8%A1%8C-spring-boot-%E9%A1%B9%E7%9B%AE)
    - [5. 配置风格](#5-%E9%85%8D%E7%BD%AE%E9%A3%8E%E6%A0%BC)
    - [6. application.yml / application.propertities](#6-applicationyml-applicationpropertities)
        - [6.1. 常用配置](#61-%E5%B8%B8%E7%94%A8%E9%85%8D%E7%BD%AE)
            - [6.1.1. .properties](#611-properties)
            - [6.1.2. .yml](#612-yml)
    - [7. Spring EL](#7-spring-el)
    - [8. 配置 CORS](#8-%E9%85%8D%E7%BD%AE-cors)
    - [9. 文件上传](#9-%E6%96%87%E4%BB%B6%E4%B8%8A%E4%BC%A0)
        - [9.1. 上传为空导致异常](#91-%E4%B8%8A%E4%BC%A0%E4%B8%BA%E7%A9%BA%E5%AF%BC%E8%87%B4%E5%BC%82%E5%B8%B8)
    - [10. 日志管理](#10-%E6%97%A5%E5%BF%97%E7%AE%A1%E7%90%86)
        - [10.1. 配置](#101-%E9%85%8D%E7%BD%AE)
            - [10.1.1. 配置日志级别【格式：logging.level. 包名 = 级别】](#1011-%E9%85%8D%E7%BD%AE%E6%97%A5%E5%BF%97%E7%BA%A7%E5%88%AB%E3%80%90%E6%A0%BC%E5%BC%8F%EF%BC%9Alogginglevel-%E5%8C%85%E5%90%8D-%E7%BA%A7%E5%88%AB%E3%80%91)
            - [10.1.2. 配置日志输出文件：](#1012-%E9%85%8D%E7%BD%AE%E6%97%A5%E5%BF%97%E8%BE%93%E5%87%BA%E6%96%87%E4%BB%B6%EF%BC%9A)
            - [10.1.3. 格式化日志](#1013-%E6%A0%BC%E5%BC%8F%E5%8C%96%E6%97%A5%E5%BF%97)
            - [10.1.4. 自定义日志框架配置](#1014-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%97%A5%E5%BF%97%E6%A1%86%E6%9E%B6%E9%85%8D%E7%BD%AE)
            - [10.1.5. 代码中使用](#1015-%E4%BB%A3%E7%A0%81%E4%B8%AD%E4%BD%BF%E7%94%A8)
    - [11. 异常处理](#11-%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86)
        - [11.1. 使用 `@ControllerAdvice` 进行统一异常处理](#111-%E4%BD%BF%E7%94%A8-controlleradvice-%E8%BF%9B%E8%A1%8C%E7%BB%9F%E4%B8%80%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86)
            - [11.1.1. 实例](#1111-%E5%AE%9E%E4%BE%8B)
            - [11.1.2. 实例：处理数据校验异常](#1112-%E5%AE%9E%E4%BE%8B%EF%BC%9A%E5%A4%84%E7%90%86%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C%E5%BC%82%E5%B8%B8)
    - [12. 使用 validation 进行数据校验](#12-%E4%BD%BF%E7%94%A8-validation-%E8%BF%9B%E8%A1%8C%E6%95%B0%E6%8D%AE%E6%A0%A1%E9%AA%8C)
        - [12.1. 捕获异常处理校验失败](#121-%E6%8D%95%E8%8E%B7%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E6%A0%A1%E9%AA%8C%E5%A4%B1%E8%B4%A5)
        - [12.2. 使用 BindingResult 处理校验错误](#122-%E4%BD%BF%E7%94%A8-bindingresult-%E5%A4%84%E7%90%86%E6%A0%A1%E9%AA%8C%E9%94%99%E8%AF%AF)
        - [12.3. 使用 groups 属性进行分组校验](#123-%E4%BD%BF%E7%94%A8-groups-%E5%B1%9E%E6%80%A7%E8%BF%9B%E8%A1%8C%E5%88%86%E7%BB%84%E6%A0%A1%E9%AA%8C)
        - [12.4. 使用 `@ScriptAssert` 自定义校验逻辑](#124-%E4%BD%BF%E7%94%A8-scriptassert-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%A0%A1%E9%AA%8C%E9%80%BB%E8%BE%91)
        - [12.5. 自定义校验注解](#125-%E8%87%AA%E5%AE%9A%E4%B9%89%E6%A0%A1%E9%AA%8C%E6%B3%A8%E8%A7%A3)
        - [12.6. 手动校验](#126-%E6%89%8B%E5%8A%A8%E6%A0%A1%E9%AA%8C)
    - [13. 静态资源目录](#13-%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E7%9B%AE%E5%BD%95)
    - [14. 模板页面目录](#14-%E6%A8%A1%E6%9D%BF%E9%A1%B5%E9%9D%A2%E7%9B%AE%E5%BD%95)
    - [15. 使用 spring boot data JPA](#15-%E4%BD%BF%E7%94%A8-spring-boot-data-jpa)
        - [15.1. 引入依赖 spring-boot-starter-data-jpa](#151-%E5%BC%95%E5%85%A5%E4%BE%9D%E8%B5%96-spring-boot-starter-data-jpa)
        - [15.2. 配置【application.yml】](#152-%E9%85%8D%E7%BD%AE%E3%80%90applicationyml%E3%80%91)
        - [15.3. 编写实体类：](#153-%E7%BC%96%E5%86%99%E5%AE%9E%E4%BD%93%E7%B1%BB%EF%BC%9A)
        - [15.4. 编写数据访问接口：](#154-%E7%BC%96%E5%86%99%E6%95%B0%E6%8D%AE%E8%AE%BF%E9%97%AE%E6%8E%A5%E5%8F%A3%EF%BC%9A)
            - [15.4.1. 接口自动实现的方法](#1541-%E6%8E%A5%E5%8F%A3%E8%87%AA%E5%8A%A8%E5%AE%9E%E7%8E%B0%E7%9A%84%E6%96%B9%E6%B3%95)
            - [15.4.2. 复杂查询方法](#1542-%E5%A4%8D%E6%9D%82%E6%9F%A5%E8%AF%A2%E6%96%B9%E6%B3%95)
                - [15.4.2.1. 分页查询](#15421-%E5%88%86%E9%A1%B5%E6%9F%A5%E8%AF%A2)
                - [15.4.2.2. 排序查询](#15422-%E6%8E%92%E5%BA%8F%E6%9F%A5%E8%AF%A2)
                - [15.4.2.3. 限制查询](#15423-%E9%99%90%E5%88%B6%E6%9F%A5%E8%AF%A2)
                - [15.4.2.4. 自定义 SQL 查询](#15424-%E8%87%AA%E5%AE%9A%E4%B9%89-sql-%E6%9F%A5%E8%AF%A2)
                - [15.4.2.5. 多表查询](#15425-%E5%A4%9A%E8%A1%A8%E6%9F%A5%E8%AF%A2)
    - [16. 使用 spring boot data rest](#16-%E4%BD%BF%E7%94%A8-spring-boot-data-rest)
    - [17. 使用事务](#17-%E4%BD%BF%E7%94%A8%E4%BA%8B%E5%8A%A1)
        - [17.1. 高级使用](#171-%E9%AB%98%E7%BA%A7%E4%BD%BF%E7%94%A8)
            - [17.1.1. 指定不同的事务管理器](#1711-%E6%8C%87%E5%AE%9A%E4%B8%8D%E5%90%8C%E7%9A%84%E4%BA%8B%E5%8A%A1%E7%AE%A1%E7%90%86%E5%99%A8)
            - [17.1.2. 隔离级别控制](#1712-%E9%9A%94%E7%A6%BB%E7%BA%A7%E5%88%AB%E6%8E%A7%E5%88%B6)
            - [17.1.3. 传播行为](#1713-%E4%BC%A0%E6%92%AD%E8%A1%8C%E4%B8%BA)
    - [18. 结合 Mybatis](#18-%E7%BB%93%E5%90%88-mybatis)
        - [18.1. 基本操作](#181-%E5%9F%BA%E6%9C%AC%E6%93%8D%E4%BD%9C)
        - [18.2. mapper 的注解支持](#182-mapper-%E7%9A%84%E6%B3%A8%E8%A7%A3%E6%94%AF%E6%8C%81)
            - [18.2.1. @Insert](#1821-insert)
            - [18.2.2. @Update](#1822-update)
            - [18.2.3. @Delete](#1823-delete)
            - [18.2.4. @Select](#1824-select)
                - [18.2.4.1. 结果映射](#18241-%E7%BB%93%E6%9E%9C%E6%98%A0%E5%B0%84)
                    - [18.2.4.1.1. 普通映射](#182411-%E6%99%AE%E9%80%9A%E6%98%A0%E5%B0%84)
                    - [18.2.4.1.2. 一对一映射](#182412-%E4%B8%80%E5%AF%B9%E4%B8%80%E6%98%A0%E5%B0%84)
                    - [18.2.4.1.3. 一对多映射](#182413-%E4%B8%80%E5%AF%B9%E5%A4%9A%E6%98%A0%E5%B0%84)
        - [18.3. 使用 mybatis-generator](#183-%E4%BD%BF%E7%94%A8-mybatis-generator)
        - [18.4. 多数据源配置](#184-%E5%A4%9A%E6%95%B0%E6%8D%AE%E6%BA%90%E9%85%8D%E7%BD%AE)
        - [18.5. 使用 HikariCP 连接池](#185-%E4%BD%BF%E7%94%A8-hikaricp-%E8%BF%9E%E6%8E%A5%E6%B1%A0)
        - [18.6. 使用 mybatis-plus](#186-%E4%BD%BF%E7%94%A8-mybatis-plus)
    - [19. 使用数据库版本工具](#19-%E4%BD%BF%E7%94%A8%E6%95%B0%E6%8D%AE%E5%BA%93%E7%89%88%E6%9C%AC%E5%B7%A5%E5%85%B7)
        - [19.1. flyway](#191-flyway)
        - [19.2. liquibase](#192-liquibase)
    - [20. 使用 Redis](#20-%E4%BD%BF%E7%94%A8-redis)
    - [21. 使用 Actuator](#21-%E4%BD%BF%E7%94%A8-actuator)
    - [22. 使用 Lombok](#22-%E4%BD%BF%E7%94%A8-lombok)
    - [23. 使用 Swagger](#23-%E4%BD%BF%E7%94%A8-swagger)
    - [24. 使用 JHipster](#24-%E4%BD%BF%E7%94%A8-jhipster)
    - [25. 测试](#25-%E6%B5%8B%E8%AF%95)
        - [25.1. 单元测试](#251-%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
        - [25.2. 集成测试](#252-%E9%9B%86%E6%88%90%E6%B5%8B%E8%AF%95)
    - [26. 部署](#26-%E9%83%A8%E7%BD%B2)
        - [26.1. IDEA 中开启 spring boot 热部署](#261-idea-%E4%B8%AD%E5%BC%80%E5%90%AF-spring-boot-%E7%83%AD%E9%83%A8%E7%BD%B2)
            - [26.1.1. 使用 spring boot devtools](#2611-%E4%BD%BF%E7%94%A8-spring-boot-devtools)
            - [26.1.2. 使用 JRebel](#2612-%E4%BD%BF%E7%94%A8-jrebel)
        - [26.2. 打包与部署](#262-%E6%89%93%E5%8C%85%E4%B8%8E%E9%83%A8%E7%BD%B2)
            - [26.2.1. 打包为 jar 包](#2621-%E6%89%93%E5%8C%85%E4%B8%BA-jar-%E5%8C%85)
                - [26.2.1.1. maven 版本](#26211-maven-%E7%89%88%E6%9C%AC)
                - [26.2.1.2. gradle 版本](#26212-gradle-%E7%89%88%E6%9C%AC)
                - [26.2.1.3. 打包为可执行的 jar 包](#26213-%E6%89%93%E5%8C%85%E4%B8%BA%E5%8F%AF%E6%89%A7%E8%A1%8C%E7%9A%84-jar-%E5%8C%85)
            - [26.2.2. 打包为 war 包](#2622-%E6%89%93%E5%8C%85%E4%B8%BA-war-%E5%8C%85)
            - [26.2.3. 打包为 docker 镜像](#2623-%E6%89%93%E5%8C%85%E4%B8%BA-docker-%E9%95%9C%E5%83%8F)
    - [27. 定时任务](#27-%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1)
        - [27.1. 使用 Schedule](#271-%E4%BD%BF%E7%94%A8-schedule)
        - [27.2. 使用 Quartz](#272-%E4%BD%BF%E7%94%A8-quartz)
    - [28. 使用 Shiro](#28-%E4%BD%BF%E7%94%A8-shiro)
    - [29. 使用 Spring Security](#29-%E4%BD%BF%E7%94%A8-spring-security)
    - [30. 使用 Spring AOP](#30-%E4%BD%BF%E7%94%A8-spring-aop)
    - [31. 启动时执行](#31-%E5%90%AF%E5%8A%A8%E6%97%B6%E6%89%A7%E8%A1%8C)
    - [32. Refer Links](#32-refer-links)

# Spring Boot Note

## 1. 概述

> spring boot 代替了什么，Spring、SpringMVC/Struts2、hibernate/Mybatis？<br/>     
> 个人理解：代替了 spring。用代替 / 取代来解释貌似都不好，更准确的可能是封装了 spring，使搭建 SSH/SSM 更快捷。     

传统的 spring 有很多 xml 配置，例如：dataSource、transactionManager、AOP、bean 等等 xml 的配置。即便用注解，也要在 xml 中配置 component-scan 等。         

但在 spring boot，遵循“约定大于配置”，所以尽可能的避免了 xml 配置。

<!-- TODO: spring 官方提供的前端模板是 thymeleaf，但只要了解即可，不推荐使用，因为在最新的开发模式中，前后端完全分离，后端只需要提供 RESTful 接口，传递如 json 格式的数据给前端即可；而且使用模板会给性能上带来很大的损耗。
具体应该怎么做？？http://blog.didispace.com/springbootweb/ ？
 -->

<!-- TODO: @RestController=@Response+@Controller -->

<!-- TODO: 降耦合原则：项目尽量不要出现任何重复的代码，便于维护 -->

## 2. Spring Boot CLI 安装

https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-gradle-installation 

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/6018abea201baec92ce68897f55a7de3.jpg)

下载后解压，将 xxx\spring-boot-cli-xxxx.RELEASE-bin\bin 添加到环境变量即可

验证安装成功：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/a7b5433aa57776d7f28c698ef7407435.jpg)

## 3. 在 IDEA 中创建使用 gradle 的 spring boot 项目

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/14/e31d28feff47bbdf73b514febb75a56e.jpg)

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/14/d3c41c341a0df3530ee6373575e3d01e.jpg)

勾选所需依赖，finish；

参见 [IDEA 中三种 gradle 模式的区别](https://github.com/firejq/StudyNote/blob/master/Programming/Java/JavaEE/Tool/Gradle-Note.md#idea-%E4%B8%AD%E4%B8%89%E7%A7%8D-gradle-%E6%A8%A1%E5%BC%8F%E7%9A%84%E5%8C%BA%E5%88%AB) 

创建项目后，一般需要做如下修改：

- 修改 gradle 版本：
    - 修改 build.gradle，添加：
        ```
        task wrapper(type: Wrapper) {
            gradleVersion = '4.3.1'
        }
        ```
    - 修改 gradle-wrapper.properties
        
        ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/14/512094e33bac92160172037e9d046c94.jpg)

        ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/14/9febe52aa058b2832eb8db6d401f409f.jpg)

- 修改仓库地址：
    - 修改 build.gradle
        ```
        repositories {
            jcenter()
            maven { url "https://repo.spring.io/snapshot" }
            maven { url "https://repo.spring.io/milestone" }
        }
        ```

- 完整 build.gradle 示例如下：
    ```groovy
    buildscript {
        ext {
            springBootVersion = '1.5.6.RELEASE'
        }
        repositories {
            jcenter()
        }
        dependencies {
            classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
        }
    }

    apply plugin: 'java'
    apply plugin: 'eclipse'
    apply plugin: 'org.springframework.boot'

    version = '0.0.1-SNAPSHOT'
    sourceCompatibility = 1.8

    repositories {	
        jcenter()
    }

    dependencies {
        // spring boot
        compile('org.springframework.boot:spring-boot-starter-web')
        // spring boot test
        testCompile('org.springframework.boot:spring-boot-starter-test')
    }

    task wrapper(type: Wrapper) {
        gradleVersion = '4.3.1'
    }

    jar {
        version = '0.0.1-SNAPSHOT'
        baseName = 'testDemo'
    }

    ```

## 4. 运行 spring boot 项目

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/3606cffe1d51ec0a0f19fb5e4489f8df.jpg)

出现 Started TestdemoApplication in 2.916 seconds (JVM running for 3.43) 字样，表明 spring boot 启动成功；

## 5. 配置风格

在 spring boot 一般采用 Java 配置和注解混合配置，其中：全局配置采用 Java 配置（如数据库、MVC 配置），业务 Bean 的配置采用注解配置（@Service、@Controller 等）。

## 6. application.yml / application.propertities

Spring Boot 采用一个全局的配置文件：application.yml / application.propertities，放置于 src/main/resources 或类路径下的 /config 中。

### 6.1. 常用配置

#### 6.1.1. .properties

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

#### 6.1.2. .yml

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

## 7. Spring EL

Spring EL 即 Spring 表达式语言，支持在 xml 和注解中使用表达式。

Spring 主要在注解 @Value 的参数中使用表达式。

例：
```java
public class ELTest {

    // 字符串
    @Value("I love u")
    private String normal;

    // 系统属性
    @Value("#{systemProperties['os.name']}")
    private String osName;

    // 计算结果
    @Value("#{T(java.lang.Math).random()*100.0}")
    private double randomNumber;

    // 其它 Bean 的属性
    @Value("#{otherBean.something}")
    private String something;

    // 文件资源
    @Value("classpath:com/firejq/web/file.txt")
    private Resouorce testFile;

    // 网址资源
    @Value("http://www.baidu.com")
    private Resource testUrl;

    // 配置文件属性，在 spring boot 中默认读取 application.propertities/application.yml 中的属性
    @Value("${book.name}")
    private String bookName;
}

```

## 8. 配置 CORS

https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-cors

https://spring.io/guides/gs/rest-service-cors/

在 controller 中配置：
```java
@CrossOrigin(origins = "http://domain2.com", maxAge = 3600)
@RestController
@RequestMapping("/account")
public class AccountController {

        @RequestMapping("/{id}")
        public Account retrieve(@PathVariable Long id) {
                // ...
        }

        @RequestMapping(method = RequestMethod.DELETE, path = "/{id}")
        public void remove(@PathVariable Long id) {
                // ...
        }
}
```

全局配置：
```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

        @Override
        public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                        .allowedOrigins("http://domain2.com")
                        .allowedMethods("PUT", "DELETE")
                        .allowedHeaders("header1", "header2", "header3")
                        .exposedHeaders("header1", "header2")
                        .allowCredentials(false).maxAge(3600);
        }
}
```

## 9. 文件上传

http://www.leftso.com/blog/232.html

http://www.028888.net/archives/2016_09_1601.html

https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html

例：

下面的例子演示了上传文件的三种可能方式：
- 单个文件上传 – MultipartFile
- 多个文件上传– MultipartFile[]
- Map file upload to a Model – @ModelAttribute

```java

@RestController
public class RestUploadController {

    private final Logger logger = LoggerFactory.getLogger(RestUploadController.class);

    //Save the uploaded file to this folder
    private static String UPLOADED_FOLDER = "F://temp//";

    // 3.1.1 Single file upload
    @PostMapping("/api/upload")
    // If not @RestController, uncomment this
    //@ResponseBody
    public ResponseEntity<?> uploadFile(
            @RequestParam("file") MultipartFile uploadfile) {

        logger.debug("Single file upload!");

        if (uploadfile.isEmpty()) {
            return new ResponseEntity("please select a file!", HttpStatus.OK);
        }

        try {

            saveUploadedFiles(Arrays.asList(uploadfile));

        } catch (IOException e) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }

        return new ResponseEntity("Successfully uploaded - " +
                uploadfile.getOriginalFilename(), new HttpHeaders(), HttpStatus.OK);

    }

    // 3.1.2 Multiple file upload
    @PostMapping("/api/upload/multi")
    public ResponseEntity<?> uploadFileMulti(
            @RequestParam("extraField") String extraField,
            @RequestParam("files") MultipartFile[] uploadfiles) {

        logger.debug("Multiple file upload!");

        // Get file name
        String uploadedFileName = Arrays.stream(uploadfiles).map(x -> x.getOriginalFilename())
                .filter(x -> !StringUtils.isEmpty(x)).collect(Collectors.joining(" , "));

        if (StringUtils.isEmpty(uploadedFileName)) {
            return new ResponseEntity("please select a file!", HttpStatus.OK);
        }

        try {

            saveUploadedFiles(Arrays.asList(uploadfiles));

        } catch (IOException e) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }

        return new ResponseEntity("Successfully uploaded - "
                + uploadedFileName, HttpStatus.OK);

    }

    // 3.1.3 maps html form to a Model
    @PostMapping("/api/upload/multi/model")
    public ResponseEntity<?> multiUploadFileModel(@ModelAttribute UploadModel model) {

        logger.debug("Multiple file upload! With UploadModel");

        try {

            saveUploadedFiles(Arrays.asList(model.getFiles()));

        } catch (IOException e) {
            return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
        }

        return new ResponseEntity("Successfully uploaded!", HttpStatus.OK);

    }

    //save file
    private void saveUploadedFiles(List<MultipartFile> files) throws IOException {

        for (MultipartFile file : files) {

            if (file.isEmpty()) {
                continue; //next pls
            }

            byte[] bytes = file.getBytes();
            Path path = Paths.get(UPLOADED_FOLDER + file.getOriginalFilename());
            Files.write(path, bytes);

        }

    }
}
```

### 9.1. 上传为空导致异常

http://blog.csdn.net/qq724581322/article/details/51332799

http://1194867672-qq-com.iteye.com/blog/1740406

http://younian.net.cn/article/117

若客户端没有上传文件就提交了表单，会导致 spring 抛出异常：
```
 nested exception is java.lang.IllegalStateException: Cannot convert value of type [java.lang.String] to required type [org.springframework.web.multipart.MultipartFile]: no matching editors or conversion strategy found
```
- 原因在于：

    在同步提交的时候，Spring 把空上传是做 null 来处理的，而在异步提交的时候，Spring 把文件域的值当作空字符串看待的。在 Spring 做请求转换的时候（request–>MultipartHttpServletRequest）底层的 TypeConverter 接口实现对 null 和""是做了不同操作的。

- 解决方法：

    - 设置表单的文件字段名与实体类的 MultipartFile 属性名不一致，在 controller 中利用 HttpServlet 先判空后再手动赋值

    - 在客户端上传之前判断有没有选择文件，没有的话就不添加文件字段；同时在服务端执行文件保存时先判空
        
        客户端
        ```javscript
		if (typeof activityPoster !== 'undefined') {// 若没上传图片，则不添加此字段
			formData.append('activityPoster', activityPoster);
		}
        ```
        
        服务端
        ```java
        if (activity.getActivityPoster() != null) {
            // 保存文件操作
        }
        ```

    - 采用 spring 提供的上传文件的方法

    ```java
    @RequestMapping("springUpload")
    public String  springUpload(HttpServletRequest request) throws IllegalStateException, IOException {
         long  startTime=System.currentTimeMillis();
         // 将当前上下文初始化给  CommonsMutipartResolver （多部分解析器）
        CommonsMultipartResolver multipartResolver=new CommonsMultipartResolver(
                request.getSession().getServletContext());
        // 检查 form 中是否有 enctype="multipart/form-data"
        if(multipartResolver.isMultipart(request)) {
            // 将 request 变成多部分 request
            MultipartHttpServletRequest multiRequest=(MultipartHttpServletRequest)request;
           // 获取 multiRequest 中所有的文件名
            Iterator iter=multiRequest.getFileNames();
             
            while(iter.hasNext()) {
                // 一次遍历所有文件
                MultipartFile file=multiRequest.getFile(iter.next().toString());
                if(file!=null) {
                    String path="E:/springUpload"+file.getOriginalFilename();
                    // 上传
                    file.transferTo(new File(path));
                }    
            }
        }
        long  endTime=System.currentTimeMillis();
        System.out.println("方法三的运行时间："+String.valueOf(endTime-startTime)+"ms");
        return "/success"; 
    }
    ```

## 10. 日志管理

spring boot 支持 Java Util Logging、Log4J、Log4J2、Logback 作为日志框架，默认情况下，Spring Boot 会用 Logback 来记录日志，并用 INFO 级别输出到控制台；

spring boot 的日志管理依赖于 spring-boot-starter-logging，但实际开发中我们不需要直接添加该依赖，因为 spring-boot-starter 其中包含了 spring-boot-starter-logging；

spring boot 对各种支持的日志框架的控制台输出和文件输出做好了默认配置；

### 10.1. 配置

Spring Boot 为我们提供了很多默认的日志配置，所以，只要将 spring-boot-starter-logging 作为依赖加入到当前应用的 classpath，则“开箱即用”，但我们仍可以根据具体需求在【application.properties】中更改配置选项：

#### 10.1.1. 配置日志级别【格式：logging.level. 包名 = 级别】

```
logging.level.org.springframework.web = DEBUG
```
日志级别从低到高分为 TRACE < DEBUG < INFO < WARN < ERROR < FATAL，如果设置为 WARN，则低于 WARN 的信息都不会输出。

Spring Boot 中默认配置 ERROR、WARN 和 INFO 级别的日志输出到控制台。您还可以通过启动您的应用程序–debug 标志来启用“调试”模式（开发的时候推荐开启）；

#### 10.1.2. 配置日志输出文件：

默认情况下，Spring Boot 将日志输出到控制台，不会写到日志文件。如果要编写除控制台输出之外的日志文件，则需在 application.properties 中设置 logging.file 或 logging.path 属性；

```
logging.file，设置文件，可以是绝对路径，也可以是相对路径。如：logging.file=my.log；
logging.path，设置目录，会在该目录下创建 spring.log 文件，并写入日志内容，如：logging.path=/var/log；
```

- 如果只配置 logging.file，会在项目的当前路径下生成一个 xxx.log 日志文件。

- 如果只配置 logging.path，在 /var/log 文件夹生成一个日志文件为 spring.log

注：

- 二者不能同时使用，如若同时使用，则只有 logging.file 生效；

- 默认情况下，日志文件的大小达到 10MB 时会切分一次，产生新的日志文件；

例：
```
logging.file = D:/mylog/log.log
```

#### 10.1.3. 格式化日志
默认的日志输出如下：
```
2016-04-13 08:23:50.120  INFO 37397 --- [main] org.hibernate.Version: HHH000412: Hibernate Core {4.3.11.Final}
```
输出内容元素具体如下：
```
时间日期 — 精确到毫秒

日志级别 — ERROR, WARN, INFO, DEBUG or TRACE

进程 ID

分隔符 — --- 标识实际日志的开始

线程名 — 方括号括起来（可能会截断控制台输出）

Logger 名 — 通常使用源代码的类名

日志内容
```
#### 10.1.4. 自定义日志框架配置

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

#### 10.1.5. 代码中使用
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

## 11. 异常处理

http://blog.didispace.com/springbootexception/

http://blog.csdn.net/kinginblue/article/details/70186586

Spring Boot 提供了一个默认的映射：/error，当处理中抛出异常之后，会转到该请求中处理，并且该请求有一个全局的错误页面用来展示异常内容。

```java
@RequestMapping("/hello")
public String hello() throws Exception {
    throw new Exception("error!");
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/21/22b8a87d9ae20a9a2d5454aeda792da5.jpg)

虽然 Spring Boot 中实现了默认的 error 映射，但是在实际应用中，我们一般需要根据实际需求实现自定义的异常提示界面或者返回 json 格式的异常信息。

### 11.1. 使用 `@ControllerAdvice` 进行统一异常处理

在 spring 3.2 中，新增了 [`@ControllerAdvice`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html) 注解，可以用于定义 @ExceptionHandler、@InitBinder、@ModelAttribute，并应用到所有 @RequestMapping 中。

@ExceptionHandler 拦截了异常，我们可以通过该注解实现自定义异常处理。其中，@ExceptionHandler 配置的 value 指定需要拦截的异常类型，异常的抛出可以是来自 @Service、@Controller 等。

例：
```java

/**
 * controller 增强器
 * @author sam
 * @since 2017/7/17
 */
@ControllerAdvice
public class MyControllerAdvice {

    /**
     * 应用到所有 @RequestMapping 注解方法，在其执行之前初始化数据绑定器
     * @param binder
     */
    @InitBinder
    public void initBinder(WebDataBinder binder) {}

    /**
     * 把值绑定到 Model 中，使全局 @RequestMapping 可以获取到该值
     * @param model
     */
    @ModelAttribute
    public void addAttributes(Model model) {
        model.addAttribute("author", "Magical Sam");
    }

    /**
     * 全局异常捕捉处理
     * @param ex
     * @return
     */
    @ResponseBody
    @ExceptionHandler(value = Exception.class)
    public Map errorHandler(Exception ex) {
        Map map = new HashMap();
        map.put("code", 100);
        map.put("msg", ex.getMessage());
        return map;
    }

}
```

以下以返回 json 格式的异常信息为例，当我们要实现 RESTful API 时，抛出异常时我们需要返回 JSON 格式的异常信息：

#### 11.1.1. 实例

创建统一的 JSON 返回对象，code：消息类型，message：消息内容，url：请求的 url，data：请求返回的数据：
```java
public class ErrorInfo<T> {
    public static final Integer OK = 0;
    public static final Integer ERROR = 100;
    private Integer code;
    private String message;
    private String url;
    private T data;
    // 省略 getter 和 setter
}
```
创建一个自定义异常类，用来实验捕获该异常，并返回 json：
```java
public class MyException extends Exception {
    public MyException(String message) {
        super(message);
    }
    
}
```
在 controller 中抛出 MyException 异常：
```java
@RestController
public class HelloController {
    @RequestMapping("/json")
    public String json() throws MyException {
        throw new MyException("发生错误");
    }
}
```
为 MyException 异常创建对应的统一处理：
```java
@RestControllerAdvice()
public class GlobalExceptionHandler {

    @ExceptionHandler(value = MyException.class)
    public ResponseEntity<?> jsonErrorHandler(HttpServletRequest req, MyException e) throws Exception {
        ErrorInfo<String> r = new ErrorInfo<>();
        r.setMessage(e.getMessage());
        r.setCode(ErrorInfo.ERROR);
        r.setData("Some Data");
        r.setUrl(req.getRequestURL().toString());
        return new ResponseEntity<>(r, HttpStatus.BAD_REQUEST);
    }
}
```
<!-- TODO: 在开发中，统一将异常由内向外抛：DAO->service->controller，最后统一由 handle 包中的 @controllerAdvice 类进行处理。 -->

NOTE:

- 若要渲染到 error.html 中，更改 @ControllerAdvise 即可：
    ```java
    @ControllerAdvice
    class GlobalExceptionHandler {
        public static final String DEFAULT_ERROR_VIEW = "error";
        @ExceptionHandler(value = Exception.class)
        public ModelAndView defaultErrorHandler(HttpServletRequest req, Exception e) throws Exception {
            ModelAndView mav = new ModelAndView();
            mav.addObject("exception", e);
            mav.addObject("url", req.getRequestURL());
            mav.setViewName(DEFAULT_ERROR_VIEW);
            return mav;
        }
    }
    ```

- 优点：将 Controller 层的异常和数据校验的异常进行统一处理，减少模板代码，减少编码量，提升扩展性和可维护性。
- 缺点：只能处理 Controller 层未捕获（往外抛）的异常，对于 Interceptor（拦截器）层的异常，Spring 框架层的异常，就无能为力了。

#### 11.1.2. 实例：处理数据校验异常

在 Dog 类中的字段上的注解数据校验规则：
```java
@Data
public class Dog {

    @NotNull(message = "{Dog.id.non}", groups = {Update.class})
    @Min(value = 1, message = "{Dog.age.lt1}", groups = {Update.class})
    private Long id;

    @NotBlank(message = "{Dog.name.non}", groups = {Add.class, Update.class})
    private String name;

    @Min(value = 1, message = "{Dog.age.lt1}", groups = {Add.class, Update.class})
    private Integer age;
}
```
SpringMVC 中对于 RESTFUL 的 Json 接口来说，数据绑定和校验，是这样的：
```java
@PatchMapping(value = "")
AppResponse update(@Validated(Update.class) @RequestBody Dog dog){
    AppResponse resp = new AppResponse();

    // 执行业务
    Dog newDog = dogService.update(dog);

    // 返回数据
    resp.setData(newDog);

    return resp;
}
```
当使用了 @Validated + @RequestBody 注解但是没有在绑定的数据对象后面跟上 Errors 类型的参数声明的话，Spring MVC 框架会抛出 MethodArgumentNotValidException 异常。
```java
@ControllerAdvice
public class GlobalExceptionHandler {

    // 所有的 Controller 层的异常的日志记录
    private static final Logger LOGGER = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    /**
     * 处理所有不可知的异常
     * @param e
     * @return
     */
    @ExceptionHandler(Exception.class)
    @ResponseBody
    AppResponse handleException(Exception e){
        LOGGER.error(e.getMessage(), e);

        AppResponse response = new AppResponse();
        response.setFail("操作失败！");
        return response;
    }

    /**
     * 处理所有接口数据验证异常
     * @param e
     * @return
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    @ResponseBody
    AppResponse handleMethodArgumentNotValidException(MethodArgumentNotValidException e){
        LOGGER.error(e.getMessage(), e);

        AppResponse response = new AppResponse();
        response.setFail(e.getBindingResult().getAllErrors().get(0).getDefaultMessage());
        return response;
    }
}
```

## 12. 使用 validation 进行数据校验

[使用 spring validation 完成数据后端校验](https://www.cnkirito.moe/2017/08/16/%E4%BD%BF%E7%94%A8spring%20validation%E5%AE%8C%E6%88%90%E6%95%B0%E6%8D%AE%E5%90%8E%E7%AB%AF%E6%A0%A1%E9%AA%8C/)

[Spring boot 使用总结（三）校验](http://www.jianshu.com/p/a9b1e2f7a749)

[Spring4 新特性——集成 Bean Validation 1.1(JSR-349) 到 SpringMVC](http://jinnianshilongnian.iteye.com/blog/1990081)

从 Spring3 起，spring 支持 JSR-303 验证框架，JSR-303 是 Java EE 6 中的一项子规范，也称为 BeanValidation。

### 12.1. 捕获异常处理校验失败

1. 在 POJO 中添加注解定义校验规则：

    常见的校验注解：

    | 限制                        | 说明                                       |
    | ------------------------- | ---------------------------------------- |
    | @Null                     | 限制只能为 null                                |
    | @NotNull                  | 限制必须不为 null                               |
    | @AssertFalse              | 限制必须为 false                               |
    | @AssertTrue               | 限制必须为 true                                |
    | @DecimalMax(value)        | 限制必须为一个不大于指定值的数字                         |
    | @DecimalMin(value)        | 限制必须为一个不小于指定值的数字                         |
    | @Digits(integer,fraction) | 限制必须为一个小数，且整数部分的位数不能超过 integer，小数部分的位数不能超过 fraction |
    | @Future                   | 限制必须是一个将来的日期                             |
    | @Max(value)               | 限制必须为一个不大于指定值的数字                         |
    | @Min(value)               | 限制必须为一个不小于指定值的数字                         |
    | @Past                     | 限制必须是一个过去的日期                             |
    | @Pattern(regex=,flag=)           | 限制必须符合指定的正则表达式                           |
    | @Size(max,min)            | 限制字符长度必须在 min 到 max 之间                       |
    | @Past                     | 验证注解的元素值（日期类型）比当前时间早                     |
    | @NotEmpty                 | 验证注解的元素值不为 null 且不为空（字符串长度不为 0、集合大小不为 0）     |
    | @NotBlank                 | 验证注解的元素值不为空（不为 null、去除首位空格后长度为 0），不同于 @NotEmpty，@NotBlank 只应用于字符串且在比较时会去除字符串的空格 |
    | @Email                    | 验证注解的元素值是 Email，也可以通过正则表达式和 flag 指定自定义的 email 格式 |
    | @Length(min=,max=)        | 被注释的字符串的大小必须在指定的范围内   |
    | @Range(min=,max=,message=)|被注释的元素必须在合适的范围内   |

    例：
    ```java
    @Pattern(regexp="^[a-zA-Z0-9]+$",message="格式错误")
    @Size(min=3,max=20,message="长度错误")

    public class User {
        private Integer id;
        @NotBlank(message = "不能为空")
        private String name;
        private String username;
    }
    ```

    NOTE:
    - 每一个注解都包含了 message 字段，用于校验失败时作为提示信息

    - @NotNull、@NotEmpty、@NotBlank 的区别
        - @NotNull：不能为 null，但可以为 empty
        - @NotEmpty：加了 @NotEmpty 的 String 类、Collection、Map、数组，是不能为 null 并且长度必须大于 0 的（String、Collection、Map 的 isEmpty() 方法）
        - @NotBlank：只能作用在 String 上，不能为 null，而且调用 trim() 后，长度必须大于 0
        - 例子：
            ```
            1.String name = null;

            @NotNull: false

            @NotEmpty:false

            @NotBlank:false

            2.String name = "";

            @NotNull:true

            @NotEmpty: false

            @NotBlank: false

            3.String name = " ";

            @NotNull: true

            @NotEmpty: true

            @NotBlank: false

            4.String name = "Great answer!";

            @NotNull: true

            @NotEmpty:true

            @NotBlank:true

            ```

2. 在 Controller 中请求参数上添加 @Validated 标签开启验证：
    ```java
    @RequestMapping(method = RequestMethod.POST)
    public User create(@RequestBody @Validated User user) {
        return userService.create(user);
    }
    ```

3. 自定义异常处理器，捕获错误信息

    当校验不通过时，[若 controller 参数中使用 @RequestBody 进行 Bean 的绑定，会抛出 MethodArgumentNotValidException 异常；若使用 @ModelAttribute 进行 Bean 的绑定，spring 会抛出 BindException 异常](https://jira.spring.io/browse/SPR-10157)；
    
    需要捕获该异常并进行处理：
    ```java
    @RestControllerAdvice
    @Log4j
    public class GlobalExceptionHandler {
        /**
        * 处理数据验证异常
        * @param e
        * @return
        */
        @ExceptionHandler(BindException.class)
        AppResponse handleMethodArgumentNotValidException(MethodArgumentNotValidException e) {
            log.error(e.getMessage(), e);

            AppResponse response = new AppResponse();
            response.setFail(e.getBindingResult().getAllErrors().get(0).getDefaultMessage());
            return response;
        }
    }
    ```
    NOTE:
    
    spring validation 不会在第一个错误发生后立即停止，而是继续试错，告诉我们所有的错误。

### 12.2. 使用 BindingResult 处理校验错误

在 controller 中：
```java
@Controller
public class FooController {
    @RequestMapping("/foo")
    public String foo(@Validated Foo foo, BindingResult bindingResult) {
        if(bindingResult.hasErrors()){
            for (FieldError fieldError : bindingResult.getFieldErrors()) {
                //...
            }
            return "fail";
        }
        return "success";
    }
}
```
参数 Foo 前加上 @Validated 注解，表明需要 spring 对其进行校验，而校验结果的信息会存放到其后的 BindingResult 中。注意，@Validated 和 BindingResult 必须相邻（一一对应），如果有多个参数需要校验，应一个校验类对应一个校验结果，形式可以如下：
```java
foo(@Validated Foo foo, BindingResult fooBindingResult ，@Validated Bar bar, BindingResult barBindingResult);
```

### 12.3. 使用 groups 属性进行分组校验

如果 Student bean 想要用于两个不同的请求中，每个请求有不同的校验需求，例如一个请求只需要校验 name 字段，一个请求需要校验 name 和 age 两个字段，那该怎么做呢？

使用注解的 groups 属性可以很好的解决这个问题，如下所示：

```java
public class Student {
    // 使用 groups 属性来给分组命名，然后在需要的地方指定命令即可
    @NotBlank(groups=NAME.class)
    private String name;
    @Min(value=3,groups=AGE.class)
    private int age;
    @NotBlank
    private String classess;

    // setter and getter

    public interface NAME{};

    public interface AGE{};
}
```
根据需要在 @Validated 属性中指定需要校验的分组名，可以指定 1 到多个。指定到的分组名会全部进行校验，不指定的不校验：
```java
@RestController
public class ValidateController {

    @RequestMapping(value="testStudent")
    public void testStudent(@Validated Student student) {

    }
    @RequestMapping(value="testStudent1")
    public void testStudent1(@Validated(NAME.class) Student student) {

    }
    @RequestMapping(value="testStudent2")
    public void testStudent2(@Validated({NAME.class, AGE.class}) Student student) {

    }
}

```

### 12.4. 使用 `@ScriptAssert` 自定义校验逻辑

 [@ScriptAssert 文档](https://docs.jboss.org/hibernate/validator/6.0/api/org/hibernate/validator/constraints/ScriptAssert.html) 

如果需要校验的业务逻辑比较复杂，简单的 @NotBlank，@Min 注解已经无法满足需求了，这时可以使用 @ScriptAssert 和 @ParameterScriptAssert  来指定进行校验的方法，通过方法来进行复杂业务逻辑的校验，然后返回 true 或 false 来表明是否校验成功。

@ScriptAssert 注解用于类级别，@ParameterScriptAssert 注解用于[方法级别](https://www.cnblogs.com/resentment/p/6341485.html)。

```java
// 通过 script 属性指定进行校验的方法，传递校验的参数，
// 依然可以通过 groups 属性指定分组名称
@ScriptAssert(lang="javascript",script="com.learn.validate.domain.Student.checkParams(_this.name,_this.age,_this.classes)", groups=CHECK.class)
public class Student {
    @NotBlank(groups=NAME.class)
    private String name;
    @Min(value=3,groups=AGE.class)
    private int age;
    @NotBlank
    private String classess;

    // setter and getter

    public interface NAME{};

    public interface AGE{};

    public interface CHECK{};

    // 注意进行校验的方法要写成静态方法，否则会出现 TypeError: xxx is not a function 的错误
    public static boolean checkParams(String name,int age,String classes) {
        if(name!=null&&age>8&classes!=null) {
            return true;
        } else {
            return false;
        }
    }
}
```
在需要的地方，通过分组名称进行调用
```java
@RestController
public class ValidateController {
    @RequestMapping(value="testStudent3")
    public void testStudent3(@Validated(CHECK.class) Student student) {

    }
}
```

### 12.5. 自定义校验注解

业务需求总是比框架提供的这些简单校验要复杂的多，我们可以自定义校验来满足我们的需求。自定义 spring validation 非常简单，主要分为两步。

1. 添加自定义注解
    ```java
    @Target({METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER})
    @Retention(RUNTIME)
    @Documented
    @Constraint(validatedBy = {CannotHaveBlankValidator.class})// 指定了真正的验证者类
    public @interface CannotHaveBlank {
        // 默认错误消息
        String message() default "不能包含空格";
        // 分组
        Class<?>[] groups() default {};
        // 负载
        Class<? extends Payload>[] payload() default {};
        // 指定多个时使用
        @Target({FIELD, METHOD, PARAMETER, ANNOTATION_TYPE})
        @Retention(RUNTIME)
        @Documented
        @interface List {
            CannotHaveBlank[] value();
        }
    }
    ```

2. 编写校验者类
    ```java
    public class CannotHaveBlankValidator implements ConstraintValidator<CannotHaveBlank, String> {
        @Override
        public void initialize(CannotHaveBlank constraintAnnotation) {
        }
        
        @Override
        // 参数 ConstraintValidatorContext 这个上下文包含了认证中所有的信息，我们可以利用这个上下文实现获取默认错误提示信息，禁用错误提示信息，改写错误提示信息等操作
        public boolean isValid(String value, ConstraintValidatorContext context) {
            //null 时不进行校验
            if (value != null && value.contains(" ")) {
                // 获取默认提示信息
                String defaultConstraintMessageTemplate = context.getDefaultConstraintMessageTemplate();
                System.out.println("default message :" + defaultConstraintMessageTemplate);
                // 禁用默认提示信息
                context.disableDefaultConstraintViolation();
                // 设置提示语
                context.buildConstraintViolationWithTemplate("can not contains blank").addConstraintViolation();
                return false;
            }
            return true;
        }
    }
    ```

### 12.6. 手动校验

可能在某些场景下需要我们手动校验，即使用校验器对需要被校验的实体发起 validate，同步获得校验结果。理论上我们既可以使用 Hibernate Validation 提供 Validator，也可以使用 Spring 对其的封装。在 spring 构建的项目中，提倡使用经过 spring 封装过后的方法：

```java
@Autowired
Validator globalValidator;

@RequestMapping("/validate")
public String validate() {
    Foo foo = new Foo();
    foo.setAge(22);
    foo.setEmail("000");
    Set<ConstraintViolation<Foo>> set = globalValidator.validate(foo);
    for (ConstraintViolation<Foo> constraintViolation : set) {
        System.out.println(constraintViolation.getMessage());
    }
    return "success";
}
```

## 13. 静态资源目录

https://juejin.im/post/58f768458d6d810064a00ad6 

spring boot 默认配置的静态资源目录如下：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/b480667fc5a2f1c65991a7eed4dc1bd0.jpg)

优先级顺序为：`META-INF/resources > resources > static > public`

即：         
服务器启动后，这四个文件夹若同时存在，则他们目录下的所有文件都相当于放在了 web 服务器的 www 中，若存在同名文件，则只访问优先级最高的文件，其它同名文件会被覆盖；

使用 webjar 管理静态资源：http://www.jianshu.com/p/d127c4f78bb8 

## 14. 模板页面目录

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

## 15. 使用 spring boot data JPA
JPA(Java Persistence API) 是 Sun 官方提出的 Java 持久化规范。它为 Java 开发人员提供了一种对象 / 关联映射工具来管理 Java 应用中的关系数据。他的出现主要是为了简化现有的持久化开发工作和整合 ORM 技术，结束现在 Hibernate，TopLink，JDO 等 ORM 框架各自为营的局面。值得注意的是，JPA 是在充分吸收了现有 Hibernate，TopLink，JDO 等 ORM 框架的基础上发展而来的，具有易于使用，伸缩性强等优点。

注意：JPA 是一套规范，不是一套产品，那么像 Hibernate,TopLink,JDO 他们是一套产品，如果说这些产品实现了这个 JPA 规范，那么我们就可以叫他们为 JPA 的实现产品。

### 15.1. 引入依赖 spring-boot-starter-data-jpa
    ```
    compile('org.springframework.boot:spring-boot-starter-data-jpa')
    ```

### 15.2. 配置【application.yml】
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

### 15.3. 编写实体类：
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

### 15.4. 编写数据访问接口：
```java
public interface UserRepository extends JpaRepository<User, Long> {
}
```
说明：

http://www.ityouknow.com/springboot/2016/08/20/springboot(%E4%BA%94)-spring-data-jpa%E7%9A%84%E4%BD%BF%E7%94%A8.html 

这个接口中我们没有定义任何操作方法，而是直接继承于 PagingAndSortingRepository 接口，该接口本身已经实现了创建（save）、更新（save）、删除（delete）、查询（findAll、findOne）等基本操作的函数，因此对于这些基础操作的数据访问就不需要开发者再自己定义。

#### 15.4.1. 接口自动实现的方法

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

 

#### 15.4.2. 复杂查询方法

在实际的开发中我们需要用到分页、删选、连表等查询的时候就需要特殊的方法或者自定义 SQL，可以通过使用 @Query 注解来创建查询，您只需要编写 JPQL 语句，并通过类似“:name”来映射 @Param 指定的参数

##### 15.4.2.1. 分页查询
分页查询在实际使用中非常普遍了，spring data jpa 已经帮我们实现了分页的功能，在查询的方法中，需要传入参数 Pageable , 当查询中有多个参数的时候 Pageable 建议做为最后一个参数传入
```java
Page<User> findALL(Pageable pageable);// 自动实现

Page<User> findByUserName(String userName, Pageable pageable);// 需要自定义
Pageable 是 spring 封装的分页实现类，使用的时候需要传入页数、每页条数和排序规则；
使用 (@RestController 中)：
Page<User> pageUsers = UserRepository.findAll(new PageRequest(1, 2));
return pageUsers;
```

##### 15.4.2.2. 排序查询
```java
List<Person> findAll(Sort sort);// 自动实现
```
使用
```java
List<Person> people =  personRepository.findAll(new Sort(Direction.DESC, “age”));
return people;
```

##### 15.4.2.3. 限制查询
有时候我们只需要查询前 N 个元素，或者支取前一个实体。
```java
ser findFirstByOrderByLastnameAsc();

User findTopByOrderByAgeDesc();

Page<User> queryFirst10ByLastname(String lastname, Pageable pageable);

List<User> findFirst10ByLastname(String lastname, Sort sort);

List<User> findTop10ByLastname(String lastname, Pageable pageable);
```
##### 15.4.2.4. 自定义 SQL 查询
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
##### 15.4.2.5. 多表查询
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

## 16. 使用 spring boot data rest

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

## 17. 使用事务
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

### 17.1. 高级使用
http://blog.didispace.com/springboottransactional/ 

#### 17.1.1. 指定不同的事务管理器
一般我们直接使用默认的事务配置，就可以满足一些基本的事务需求，但是当我们项目较大较复杂时（比如，有多个数据源等），这时候需要在声明事务时，指定不同的事务管理器。在声明事务时，只需要通过 value 属性指定配置的事务管理器名即可，例如：@Transactional(value="transactionManagerPrimary")。

#### 17.1.2. 隔离级别控制
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

#### 17.1.3. 传播行为
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

## 18. 结合 Mybatis

[官方文档](http://www.mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/)

[如何优雅使用 mybatis](http://www.ityouknow.com/springboot/2016/11/06/springboot(%E5%85%AD)-%E5%A6%82%E4%BD%95%E4%BC%98%E9%9B%85%E7%9A%84%E4%BD%BF%E7%94%A8mybatis.html)

### 18.1. 基本操作

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

### 18.2. mapper 的注解支持

官方文档：http://www.mybatis.org/mybatis-3/zh/java-api.html 

#### 18.2.1. @Insert
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

#### 18.2.2. @Update
例：
```java
@Update("UPDATE STUDENTS SET NAME=#{name}, EMAIL=#{email}, PHONE=#{phone} WHERE STUD_ID=#{studId}")
// 将会返回执行了 UPDATE 语句后影响的行数
int updateStudent(Student student);
```
#### 18.2.3. @Delete
例：
```java
@Delete("DELETE FROM STUDENTS WHERE STUD_ID=#{studId}")
// 将会返回执行了 DELETE 语句后影响的行数
int deleteStudent(int studId);
```
#### 18.2.4. @Select
例：
```java
@Select("SELECT STUD_ID AS STUDID, NAME, EMAIL, PHONE FROM STUDENTS WHERE STUD_ID=#{studId}")
Student findStudentById(Integer studId);
```
注：

如果返回了多行结果，将抛出 TooManyResultsException 异常

##### 18.2.4.1. 结果映射

数据库表列与实体类属性映射的四种方法： http://blog.csdn.net/lmy86263/article/details/53150091 

a)	通过 XML 映射文件中的 resultMap；

b)	通过注解 @Results 和 @Result；

c)	通过属性配置完成映射；

d)	通过使用在 SQL 语句中定义别名完成映射；

以下使用的都是注解映射：
###### 18.2.4.1.1. 普通映射
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

###### 18.2.4.1.2. 一对一映射
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

###### 18.2.4.1.3. 一对多映射

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

### 18.3. 使用 mybatis-generator
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

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/ab1e524655371de9b5bd2746800533a9.jpg)

5)	针对实际需要进行修改

    a)	将生成实体类的 setter/getter 及全参构造函数删除，使用 @Data 代替；

    b)	删除 mapper/ 中的 provider 类，以及 mapper 接口中的 provider 方法，只留下基本的增删改查；

### 18.4. 多数据源配置
spring boot mybatis 多数据库源（主从）配置 http://www.ityouknow.com/springboot/2016/11/25/springboot(%E4%B8%83)-springboot+mybatis%E5%A4%9A%E6%95%B0%E6%8D%AE%E6%BA%90%E6%9C%80%E7%AE%80%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88.html 

### 18.5. 使用 HikariCP 连接池
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

### 18.6. 使用 mybatis-plus

官方文档：http://mp.baomidou.com/#/quick-start 

## 19. 使用数据库版本工具

SpringBoot 支持了两种数据库迁移工具，一个是 flyway，一个是 liquibase。其本身也支持 sql script，在初始化数据源之后执行指定的脚本。

### 19.1. flyway

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

### 19.2. liquibase
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

## 20. 使用 Redis

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

## 21. 使用 Actuator

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

## 22. 使用 Lombok

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

## 23. 使用 Swagger
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

## 24. 使用 JHipster

## 25. 测试

### 25.1. 单元测试

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

### 25.2. 集成测试

整体开发完成之后进入集成测试，spring boot 项目的启动入口在 Application 类中，直接运行 run 方法就可以启动项目；

## 26. 部署

http://www.ityouknow.com/springboot/2017/05/09/springboot-deploy.html

### 26.1. IDEA 中开启 spring boot 热部署

#### 26.1.1. 使用 spring boot devtools

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

#### 26.1.2. 使用 JRebel

### 26.2. 打包与部署

#### 26.2.1. 打包为 jar 包

##### 26.2.1.1. maven 版本

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

##### 26.2.1.2. gradle 版本

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

##### 26.2.1.3. 打包为可执行的 jar 包

http://blog.geekidentity.com/spring/spring_boot_production_deploy/

除了使用 java -jar 运行 Spring Boot 应用程序外，还可以为 Unix 系统打包完全可执行的应用程序。 这使得在常见的生产环境中安装和管理 Spring Boot 应用程序非常容易。

Spring Boot 提供了一个 tools 工具，该工具可以方便的让我们将程序部署到生产环境。

- maven 配置：
    ```xml
    <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
            <executable>true</executable>
        </configuration>
    </plugin>
    ```

- gradle 配置

    ```xml
    springBoot {
        executable = true
    }
    ```

这样的配置实际上是 Spring Boot tools 在打包的过程中将 bash 脚本及一些辅助进行启动的 Java 代码打包到我们的项目中，然后，就可以通过键入`./my-application.jar`（其中 my-application 是您工程的 artifact 的名称）来运行应用程序。

在此基础上，还可以通过 Linux 系统中的 init.d 或 systemd 将 Spring Boot 应用程序作为 Unix / Linux 服务启动，从而更加方便和稳定。

#### 26.2.2. 打包为 war 包

https://docs.spring.io/spring-boot/docs/current/reference/html/howto-traditional-deployment.html 

https://my.oschina.net/alexnine/blog/540651 

http://www.jianshu.com/p/b3be5e54d836 

打包为 jar 包时，包含了内置的 tomcat 服务器，若希望使用独立的 tomcat 服务器或者其它容器服务器，则需要将项目打包为 war 包后再部署到容器中；

需要注意的是这样部署的 request url 需要在端口后加上项目的名字才能正常访问。spring-boot 更加强大的一点就是：即便项目是以上配置，依然可以用内嵌的 tomcat 来调试，启动命令和以前没变，还是：`mvn spring-boot:run`。

如果需要在 springboot 中加上 request 前缀，需要在 application.properties 中添加 server.contextPath=/prefix/ 即可。其中 prefix 为前缀名。这个前缀会在 war 包中失效，取而代之的是 war 包名称，如果 war 包名称和 prefix 相同的话，那么调试环境和正式部署环境就是一个 request 地址了。

- 构建项目，生成 war 包
    指定 war 包名：
    ```
    war {
        baseName = "gradle-simple"
    }
    ```
    执行
    ```
    gradle clean war
    ```
    生成的 war 文件名为 gradle-simple.war

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/14/87588085a964930d463d1e5459a74b1f.jpg)

    若要生成 zip 文件：
    ```
    war {
        baseName = "gradle-simple"
        extension = "zip"
    }
    ```
    则生成的 zip 文件名为 gradle-simple.zip，解压 zip 文件至外部容器即可。

NOTE：

若使用`providedRuntime('org.springframework.boot:spring-boot-starter-tomcat')`的依赖设置，会导致开发期间项目无法运行，提示 Unregistering JMX-exposed beans on shutdown，这是因为内置的 tomcat 容器无法启动，因此运行自动停止。

解决方法：开发期间使用`compile('org.springframework.boot:spring-boot-starter-tomcat')`，项目上线时编译才使用`providedRuntime('org.springframework.boot:spring-boot-starter-tomcat')`

#### 26.2.3. 打包为 docker 镜像

https://waylau.com/docker-spring-boot-gradle/ 

## 27. 定时任务

### 27.1. 使用 Schedule

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

### 27.2. 使用 Quartz

https://www.cnblogs.com/javanoob/p/springboot_schedule.html 

https://www.cnblogs.com/lic309/p/4089633.html 

Quartz 是一个功能完善的任务调度框架，特别牛叉的是它支持集群环境下的任务调度，当然代价也很大，需要将任务调度状态序列化到数据库。Quartz 框架需要 10 多张表协同，配置繁多。

## 28. 使用 Shiro

官方网站：http://shiro.apache.org/ 

中文文档：https://waylau.gitbooks.io/apache-shiro-1-2-x-reference/content/ 

Shiro 是 Apache 下的一个开源项目，我们称之为 Apache Shiro。它是一个很易用与 Java 项目的的安全框架，提供了认证、授权、加密、会话管理，与 Spring Security 一样都是做一个权限的安全框架，但是与 Spring Security 相比，在于 Shiro 使用了比较简单易懂易于使用的授权方式。

Apache Shiro 的三大核心组件：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/1f1f5f6d266496add107c8532ab1cac7.jpg)

Apache Shiro 核心通过 Filter 来实现，通过 URL 规则来进行过滤和权限校验，所以我们需要定义一系列关于 URL 的规则和访问权限。

另外我们可以通过 Shiro 提供的会话管理来获取 Session 中的信息。Shiro 也提供了缓存支持，使用 CacheManager 来管理。

1)	引入依赖 shiro-spring
    ```
    compile group: 'org.apache.shiro', name: 'shiro-spring', version: "${ShiroVersion}"
    ```

## 29. 使用 Spring Security

官方教程 https://docs.spring.io/spring-security/site/docs/current/guides/html5/helloworld-boot.html 

## 30. 使用 Spring AOP

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

## 31. 启动时执行

http://blog.csdn.net/qq_35981283/article/details/77826537

Springboot 给我们提供了两种“开机启动”某些方法的方式：ApplicationRunner 接口和 CommandLineRunner 接口。

CommandLineRunner 接口可以用来接收字符串数组的命令行参数，而 ApplicationRunner 是使用 ApplicationArguments 用来接收参数的。

例：
```java
@Component
public class MyCommandLineRunner implements CommandLineRunner{

    @Override
    public void run(String... var1) throws Exception{
        System.out.println("This will be execute when the project was started!");
    }
}
```

```java
@Component
public class MyApplicationRunner implements ApplicationRunner {

    @Override
    public void run(ApplicationArguments var1) throws Exception{
        System.out.println("MyApplicationRunner class will be execute when the project was started!");
    }

}
```

如果想要指定启动方法执行的顺序，可以通过实现 org.springframework.core.Ordered 接口或者使用 org.springframework.core.annotation.Order 注解来实现。

例：
```java
@Component
public class MyApplicationRunner implements ApplicationRunner,Ordered{

    @Override
    public int getOrder(){
        return 1;// 通过设置这里的数字来指定顺序
    }

    @Override
    public void run(ApplicationArguments var1) throws Exception{
        System.out.println("MyApplicationRunner1!");
    }

}
```

```java
@Component
@Order(value = 1)
public class MyApplicationRunner implements ApplicationRunner{

    @Override
    public void run(ApplicationArguments var1) throws Exception{
        System.out.println("MyApplicationRunner1!");
    }

}
```

## 32. Refer Links

官方教程 https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/ 
