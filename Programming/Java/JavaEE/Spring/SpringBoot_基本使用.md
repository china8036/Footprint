- [Spring Boot](#spring-boot)
  - [1. 概述](#1-概述)
  - [2. Spring Boot CLI 安装](#2-spring-boot-cli-安装)
  - [3. 在 IDEA 中创建使用 gradle 的 spring boot 项目](#3-在-idea-中创建使用-gradle-的-spring-boot-项目)
  - [4. 运行 spring boot 项目](#4-运行-spring-boot-项目)
  - [5. 配置风格](#5-配置风格)
  - [6. application.yml / application.propertities](#6-applicationyml--applicationpropertities)
    - [6.1. 常用配置](#61-常用配置)
      - [6.1.1. .properties](#611-properties)
      - [6.1.2. .yml](#612-yml)
  - [7. Spring EL](#7-spring-el)
  - [8. 配置 CORS](#8-配置-cors)
  - [9. 文件上传](#9-文件上传)
    - [9.1. 上传为空导致异常](#91-上传为空导致异常)
  - [10. 日志管理](#10-日志管理)
    - [10.1. 配置](#101-配置)
      - [10.1.1. 配置日志级别【格式：logging.level. 包名 = 级别】](#1011-配置日志级别格式logginglevel-包名--级别)
      - [10.1.2. 配置日志输出文件：](#1012-配置日志输出文件)
      - [10.1.3. 格式化日志](#1013-格式化日志)
      - [10.1.4. 自定义日志框架配置](#1014-自定义日志框架配置)
      - [10.1.5. 代码中使用](#1015-代码中使用)
  - [11. 异常处理](#11-异常处理)
    - [11.1. 使用 `@ControllerAdvice` 进行统一异常处理](#111-使用-controlleradvice-进行统一异常处理)
      - [11.1.1. 实例](#1111-实例)
      - [11.1.2. 实例：处理数据校验异常](#1112-实例处理数据校验异常)
  - [12. 使用 validation 进行数据校验](#12-使用-validation-进行数据校验)
    - [12.1. 捕获异常处理校验失败](#121-捕获异常处理校验失败)
    - [12.2. 使用 BindingResult 处理校验错误](#122-使用-bindingresult-处理校验错误)
    - [12.3. 使用 groups 属性进行分组校验](#123-使用-groups-属性进行分组校验)
    - [12.4. 使用 `@ScriptAssert` 自定义校验逻辑](#124-使用-scriptassert-自定义校验逻辑)
    - [12.5. 自定义校验注解](#125-自定义校验注解)
    - [12.6. 手动校验](#126-手动校验)
  - [13. 静态资源目录](#13-静态资源目录)
  - [14. 模板页面目录](#14-模板页面目录)
  - [15. 部署](#15-部署)
    - [15.1. IDEA 中开启 spring boot 热部署](#151-idea-中开启-spring-boot-热部署)
      - [15.1.1. 使用 spring boot devtools](#1511-使用-spring-boot-devtools)
      - [15.1.2. 使用 JRebel](#1512-使用-jrebel)
    - [15.2. 打包与部署](#152-打包与部署)
      - [15.2.1. 打包为 jar 包](#1521-打包为-jar-包)
        - [15.2.1.1. maven 版本](#15211-maven-版本)
        - [15.2.1.2. gradle 版本](#15212-gradle-版本)
        - [15.2.1.3. 打包为可执行的 jar 包](#15213-打包为可执行的-jar-包)
      - [15.2.2. 打包为 war 包](#1522-打包为-war-包)
      - [15.2.3. 打包为 docker 镜像](#1523-打包为-docker-镜像)
  - [16. 启动时执行](#16-启动时执行)
  - [17. Refer Links](#17-refer-links)

# Spring Boot

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

![image](http://img.cdn.firejq.com/jpg/2017/10/30/6018abea201baec92ce68897f55a7de3.jpg)

下载后解压，将 xxx\spring-boot-cli-xxxx.RELEASE-bin\bin 添加到环境变量即可

验证安装成功：

![image](http://img.cdn.firejq.com/jpg/2017/10/30/a7b5433aa57776d7f28c698ef7407435.jpg)

## 3. 在 IDEA 中创建使用 gradle 的 spring boot 项目

![image](http://img.cdn.firejq.com/jpg/2017/11/14/e31d28feff47bbdf73b514febb75a56e.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/11/14/d3c41c341a0df3530ee6373575e3d01e.jpg)

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

        ![image](http://img.cdn.firejq.com/jpg/2017/11/14/512094e33bac92160172037e9d046c94.jpg)

        ![image](http://img.cdn.firejq.com/jpg/2017/11/14/9febe52aa058b2832eb8db6d401f409f.jpg)

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

![image](http://img.cdn.firejq.com/jpg/2017/10/30/3606cffe1d51ec0a0f19fb5e4489f8df.jpg)

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

![image](http://img.cdn.firejq.com/jpg/2017/11/21/22b8a87d9ae20a9a2d5454aeda792da5.jpg)

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

![image](http://img.cdn.firejq.com/jpg/2017/10/30/b480667fc5a2f1c65991a7eed4dc1bd0.jpg)

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

  ![image](http://img.cdn.firejq.com/jpg/2017/10/30/ffc78353e40411b73721d04c808c5c08.jpg)

  **经过尝试，暂时找不到方法在不使用模板引擎的时候访问 templates 目录页面；** <!-- TODO -->

- 使用各种 spring-boot-start 的模板引擎，会自动配置 viewResolve；
  http://www.jianshu.com/p/85cfe2e061fe

## 15. 部署

http://www.ityouknow.com/springboot/2017/05/09/springboot-deploy.html

### 15.1. IDEA 中开启 spring boot 热部署

#### 15.1.1. 使用 spring boot devtools

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

    ![image](http://img.cdn.firejq.com/jpg/2017/10/30/503526609a902fda6f7ac9a2c212640a.jpg)

    CTRL + SHIFT + A --> 查找 Registry --> 找到并勾选 compiler.automake.allow.when.app.running

    ![image](http://img.cdn.firejq.com/jpg/2017/10/30/f68eef2002759a230f4dca734d0dc63c.jpg)

#### 15.1.2. 使用 JRebel

### 15.2. 打包与部署

#### 15.2.1. 打包为 jar 包

##### 15.2.1.1. maven 版本

在 IDEA 中，打开 project structure

![image](http://img.cdn.firejq.com/jpg/2017/10/30/2079e5202d9c112a760c3ea7bd32058b.jpg)

选择 spring boot 入口类

![image](http://img.cdn.firejq.com/jpg/2017/10/30/349da6a5dee54c7f801c64aaee60d2c9.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/10/30/2abfbe5609f4f86896437776e386e7b8.jpg)

双击 package

![image](http://img.cdn.firejq.com/jpg/2017/10/30/508b412389334268af2ff6fb6e00267d.jpg)

看到下列信息表示打包完成：

![image](http://img.cdn.firejq.com/jpg/2017/10/30/53dfc5e76494b2b6000f8c6f002e41cc.jpg)

在 target 目录下的 jar 包即为打包后的项目文件

![image](http://img.cdn.firejq.com/jpg/2017/10/30/f03b2442d804300806bc4cfe29467bc0.jpg)

在 jar 包目录下，运行项目：
```powershell
java -jar xxxx.jar
```

##### 15.2.1.2. gradle 版本

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

![image](http://img.cdn.firejq.com/jpg/2017/10/30/6fa283f99a1522d4e212621c00597f57.jpg)

在 jar 包目录下，运行项目：
```powershell
java -jar xxxx.jar
```

##### 15.2.1.3. 打包为可执行的 jar 包

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

#### 15.2.2. 打包为 war 包

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

    ![image](http://img.cdn.firejq.com/jpg/2017/11/14/87588085a964930d463d1e5459a74b1f.jpg)

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

#### 15.2.3. 打包为 docker 镜像

https://waylau.com/docker-spring-boot-gradle/

## 16. 启动时执行

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

## 17. Refer Links

[Spring Boot Reference Guide](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/)
