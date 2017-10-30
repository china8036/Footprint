- [Spring Boot 配置收录](#spring-boot-%E9%85%8D%E7%BD%AE%E6%94%B6%E5%BD%95)
  - [mvc](#mvc)
    - [mvc](#mvc)
    - [messages](#messages)
    - [mobile](#mobile)
    - [view](#view)
    - [multipart](#multipart)
    - [freemarker](#freemarker)
    - [velocity](#velocity)
    - [thymeleaf](#thymeleaf)
    - [mustcache](#mustcache)
    - [groovy 模板](#groovy-%E6%A8%A1%E6%9D%BF)
    - [http](#http)
    - [json](#json)
    - [jersey](#jersey)
  - [server](#server)
    - [server 配置](#server-%E9%85%8D%E7%BD%AE)
    - [ssl 配置](#ssl-%E9%85%8D%E7%BD%AE)
    - [tomcat](#tomcat)
    - [undertow](#undertow)
  - [datasource](#datasource)
    - [datasource](#datasource)
    - [JPA](#jpa)
    - [jooq](#jooq)
    - [h2](#h2)
    - [JTA](#jta)
  - [NoSQL](#nosql)
    - [cache](#cache)
    - [mongodb](#mongodb)
    - [redis](#redis)
    - [springdata](#springdata)
  - [MQ](#mq)
    - [activemq](#activemq)
    - [rabbitmq](#rabbitmq)
    - [hornetq](#hornetq)
    - [jms](#jms)
  - [Security](#security)
  - [Migration](#migration)
    - [flyway](#flyway)
    - [liquibase](#liquibase)
  - [其它](#%E5%85%B6%E5%AE%83)
    - [aop](#aop)
    - [batch](#batch)
    - [jmx](#jmx)
    - [mail](#mail)
    - [sendgrid](#sendgrid)
    - [social](#social)

# Spring Boot 配置收录

原文：https://segmentfault.com/a/1190000004315890 

## mvc

### mvc
```
spring.mvc.async.request-timeout
设定 async 请求的超时时间，以毫秒为单位，如果没有设置的话，以具体实现的超时时间为准，比如 tomcat 的 servlet3 的话是 10 秒。
spring.mvc.date-format
设定日期的格式，比如 dd/MM/yyyy.
spring.mvc.favicon.enabled
是否支持 favicon.ico，默认为：true
spring.mvc.ignore-default-model-on-redirect
在重定向时是否忽略默认 model 的内容，默认为 true
spring.mvc.locale
指定使用的 Locale.
spring.mvc.message-codes-resolver-format
指定 message codes 的格式化策略 (PREFIX_ERROR_CODE,POSTFIX_ERROR_CODE).
spring.mvc.view.prefix
指定 mvc 视图的前缀。
spring.mvc.view.suffix
指定 mvc 视图的后缀。
```

### messages
```
spring.messages.basename
指定 message 的 basename，多个以逗号分隔，如果不加包名的话，默认从 classpath 路径开始，默认：messages
spring.messages.cache-seconds
设定加载的资源文件缓存失效时间，-1 的话为永不过期，默认为 -1
spring.messages.encoding
设定 Message bundles 的编码，默认：UTF-8
```

### mobile
```
spring.mobile.devicedelegatingviewresolver.enable-fallback
是否支持 fallback 的解决方案，默认 false
spring.mobile.devicedelegatingviewresolver.enabled
是否开始 device view resolver，默认为：false
spring.mobile.devicedelegatingviewresolver.mobile-prefix
设定 mobile 端视图的前缀，默认为：mobile/
spring.mobile.devicedelegatingviewresolver.mobile-suffix
设定 mobile 视图的后缀
spring.mobile.devicedelegatingviewresolver.normal-prefix
设定普通设备的视图前缀
spring.mobile.devicedelegatingviewresolver.normal-suffix
设定普通设备视图的后缀
spring.mobile.devicedelegatingviewresolver.tablet-prefix
设定平板设备视图前缀，默认：tablet/
spring.mobile.devicedelegatingviewresolver.tablet-suffix
设定平板设备视图后缀。
spring.mobile.sitepreference.enabled
是否启用 SitePreferenceHandler，默认为：true
```

### view
```
spring.view.prefix
设定 mvc 视图的前缀。
spring.view.suffix
设定 mvc 视图的后缀。
resource
spring.resources.add-mappings
是否开启默认的资源处理，默认为 true
spring.resources.cache-period
设定资源的缓存时效，以秒为单位。
spring.resources.chain.cache
是否开启缓存，默认为：true
spring.resources.chain.enabled
是否开启资源 handling chain，默认为 false
spring.resources.chain.html-application-cache
是否开启 h5 应用的 cache manifest 重写，默认为：false
spring.resources.chain.strategy.content.enabled
是否开启内容版本策略，默认为 false
spring.resources.chain.strategy.content.paths
指定要应用的版本的路径，多个以逗号分隔，默认为：[/**]
spring.resources.chain.strategy.fixed.enabled
是否开启固定的版本策略，默认为 false
spring.resources.chain.strategy.fixed.paths
指定要应用版本策略的路径，多个以逗号分隔
spring.resources.chain.strategy.fixed.version
指定版本策略使用的版本号
spring.resources.static-locations
指定静态资源路径，默认为 classpath:[/META-INF/resources/,/resources/, /static/, /public/] 以及 context:/
```

### multipart
```
multipart.enabled
是否开启文件上传支持，默认为 true
multipart.file-size-threshold
设定文件写入磁盘的阈值，单位为 MB 或 KB，默认为 0
multipart.location
指定文件上传路径。
multipart.max-file-size
指定文件大小最大值，默认 1MB
multipart.max-request-size
指定每次请求的最大值，默认为 10MB
```

### freemarker
```
spring.freemarker.allow-request-override
指定 HttpServletRequest 的属性是否可以覆盖 controller 的 model 的同名项
spring.freemarker.allow-session-override
指定 HttpSession 的属性是否可以覆盖 controller 的 model 的同名项
spring.freemarker.cache
是否开启 template caching.
spring.freemarker.charset
设定 Template 的编码。
spring.freemarker.check-template-location
是否检查 templates 路径是否存在。
spring.freemarker.content-type
设定 Content-Type.
spring.freemarker.enabled
是否允许 mvc 使用 freemarker.
spring.freemarker.expose-request-attributes
设定所有 request 的属性在 merge 到模板的时候，是否要都添加到 model 中。
spring.freemarker.expose-session-attributes
设定所有 HttpSession 的属性在 merge 到模板的时候，是否要都添加到 model 中。
spring.freemarker.expose-spring-macro-helpers
设定是否以 springMacroRequestContext 的形式暴露 RequestContext 给 Spring’s macro library 使用
spring.freemarker.prefer-file-system-access
是否优先从文件系统加载 template，以支持热加载，默认为 true
spring.freemarker.prefix
设定 freemarker 模板的前缀。
spring.freemarker.request-context-attribute
指定 RequestContext 属性的名。
spring.freemarker.settings
设定 FreeMarker keys.
spring.freemarker.suffix
设定模板的后缀。
spring.freemarker.template-loader-path
设定模板的加载路径，多个以逗号分隔，默认：["classpath:/templates/"]
spring.freemarker.view-names
指定使用模板的视图列表。
```
### velocity
```
spring.velocity.allow-request-override
指定 HttpServletRequest 的属性是否可以覆盖 controller 的 model 的同名项
spring.velocity.allow-session-override
指定 HttpSession 的属性是否可以覆盖 controller 的 model 的同名项
spring.velocity.cache
是否开启模板缓存
spring.velocity.charset
设定模板编码
spring.velocity.check-template-location
是否检查模板路径是否存在。
spring.velocity.content-type
设定 ContentType 的值
spring.velocity.date-tool-attribute
设定暴露给 velocity 上下文使用的 DateTool 的名
spring.velocity.enabled
设定是否允许 mvc 使用 velocity
spring.velocity.expose-request-attributes
是否在 merge 模板的时候，将 request 属性都添加到 model 中
spring.velocity.expose-session-attributes
是否在 merge 模板的时候，将 HttpSession 属性都添加到 model 中
spring.velocity.expose-spring-macro-helpers
设定是否以 springMacroRequestContext 的名来暴露 RequestContext 给 Spring’s macro 类库使用
spring.velocity.number-tool-attribute
设定暴露给 velocity 上下文的 NumberTool 的名
spring.velocity.prefer-file-system-access
是否优先从文件系统加载模板以支持热加载，默认为 true
spring.velocity.prefix
设定 velocity 模板的前缀。
spring.velocity.properties
设置 velocity 的额外属性。
spring.velocity.request-context-attribute
设定 RequestContext attribute 的名。
spring.velocity.resource-loader-path
设定模板路径，默认为：classpath:/templates/
spring.velocity.suffix
设定 velocity 模板的后缀。
spring.velocity.toolbox-config-location
设定 Velocity Toolbox 配置文件的路径，比如 /WEB-INF/toolbox.xml.
spring.velocity.view-names
设定需要解析的视图名称。
```
### thymeleaf
```
spring.thymeleaf.cache
是否开启模板缓存，默认 true
spring.thymeleaf.check-template-location
是否检查模板路径是否存在，默认 true
spring.thymeleaf.content-type
指定 Content-Type，默认为：text/html
spring.thymeleaf.enabled
是否允许 MVC 使用 Thymeleaf，默认为：true
spring.thymeleaf.encoding
指定模板的编码，默认为：UTF-8
spring.thymeleaf.excluded-view-names
指定不使用模板的视图名称，多个以逗号分隔。
spring.thymeleaf.mode
指定模板的模式，具体查看 StandardTemplateModeHandlers，默认为：HTML5
spring.thymeleaf.prefix
指定模板的前缀，默认为：classpath:/templates/
spring.thymeleaf.suffix
指定模板的后缀，默认为：.html
spring.thymeleaf.template-resolver-order
指定模板的解析顺序，默认为第一个。
spring.thymeleaf.view-names
指定使用模板的视图名，多个以逗号分隔。
```
### mustcache
```
spring.mustache.cache
是否 Enable template caching.
spring.mustache.charset
指定 Template 的编码。
spring.mustache.check-template-location
是否检查默认的路径是否存在。
spring.mustache.content-type
指定 Content-Type.
spring.mustache.enabled
是否开启 mustcache 的模板支持。
spring.mustache.prefix
指定模板的前缀，默认：classpath:/templates/
spring.mustache.suffix
指定模板的后缀，默认：.html
spring.mustache.view-names
指定要使用模板的视图名。
```
### groovy 模板
```
spring.groovy.template.allow-request-override
指定 HttpServletRequest 的属性是否可以覆盖 controller 的 model 的同名项
spring.groovy.template.allow-session-override
指定 HttpSession 的属性是否可以覆盖 controller 的 model 的同名项
spring.groovy.template.cache
是否开启模板缓存。
spring.groovy.template.charset
指定 Template 编码。
spring.groovy.template.check-template-location
是否检查模板的路径是否存在。
spring.groovy.template.configuration.auto-escape
是否在渲染模板时自动排查 model 的变量，默认为：false
spring.groovy.template.configuration.auto-indent
是否在渲染模板时自动缩进，默认为 false
spring.groovy.template.configuration.auto-indent-string
如果自动缩进启用的话，是使用 SPACES 还是 TAB，默认为：SPACES
spring.groovy.template.configuration.auto-new-line
渲染模板时是否要输出换行，默认为 false
spring.groovy.template.configuration.base-template-class
指定 template base class.
spring.groovy.template.configuration.cache-templates
是否要缓存模板，默认为 true
spring.groovy.template.configuration.declaration-encoding
在写入 declaration header 时使用的编码
spring.groovy.template.configuration.expand-empty-elements
是使用<br/>这种形式，还是<br></br>这种展开模式，默认为：false)
spring.groovy.template.configuration.locale
指定 template locale.
spring.groovy.template.configuration.new-line-string
当启用自动换行时，换行的输出，默认为系统的 line.separator 属性的值
spring.groovy.template.configuration.resource-loader-path
指定 groovy 的模板路径，默认为 classpath:/templates/
spring.groovy.template.configuration.use-double-quotes
指定属性要使用双引号还是单引号，默认为 false
spring.groovy.template.content-type
指定 Content-Type.
spring.groovy.template.enabled
是否开启 groovy 模板的支持。
spring.groovy.template.expose-request-attributes
设定所有 request 的属性在 merge 到模板的时候，是否要都添加到 model 中。
spring.groovy.template.expose-session-attributes
设定所有 request 的属性在 merge 到模板的时候，是否要都添加到 model 中。
spring.groovy.template.expose-spring-macro-helpers
设定是否以 springMacroRequestContext 的形式暴露 RequestContext 给 Spring’s macro library 使用
spring.groovy.template.prefix
指定模板的前缀。
spring.groovy.template.request-context-attribute
指定 RequestContext 属性的名。
spring.groovy.template.resource-loader-path
指定模板的路径，默认为：classpath:/templates/
spring.groovy.template.suffix
指定模板的后缀
spring.groovy.template.view-names
指定要使用模板的视图名称。
```
### http
```
spring.hateoas.apply-to-primary-object-mapper
设定是否对 object mapper 也支持 HATEOAS，默认为：true
spring.http.converters.preferred-json-mapper
是否优先使用 JSON mapper 来转换。
spring.http.encoding.charset
指定 http 请求和相应的 Charset，默认：UTF-8
spring.http.encoding.enabled
是否开启 http 的编码支持，默认为 true
spring.http.encoding.force
是否强制对 http 请求和响应进行编码，默认为 true
```
### json
```
spring.jackson.date-format
指定日期格式，比如 yyyy-MM-dd HH:mm:ss，或者具体的格式化类的全限定名
spring.jackson.deserialization
是否开启 Jackson 的反序列化
spring.jackson.generator
是否开启 json 的 generators.
spring.jackson.joda-date-time-format
指定 Joda date/time 的格式，比如 yyyy-MM-dd HH:mm:ss). 如果没有配置的话，dateformat 会作为 backup
spring.jackson.locale
指定 json 使用的 Locale.
spring.jackson.mapper
是否开启 Jackson 通用的特性。
spring.jackson.parser
是否开启 jackson 的 parser 特性。
spring.jackson.property-naming-strategy
指定 PropertyNamingStrategy (CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES) 或者指定 PropertyNamingStrategy 子类的全限定类名。
spring.jackson.serialization
是否开启 jackson 的序列化。
spring.jackson.serialization-inclusion
指定序列化时属性的 inclusion 方式，具体查看 JsonInclude.Include 枚举。
spring.jackson.time-zone
指定日期格式化时区，比如 America/Los_Angeles 或者 GMT+10.
```
### jersey
```
spring.jersey.filter.order
指定 Jersey filter 的 order，默认为：0
spring.jersey.init
指定传递给 Jersey 的初始化参数。
spring.jersey.type
指定 Jersey 的集成类型，可以是 servlet 或者 filter.
```

## server

### server 配置
```
server.address
指定 server 绑定的地址
server.compression.enabled
是否开启压缩，默认为 false.
server.compression.excluded-user-agents
指定不压缩的 user-agent，多个以逗号分隔，默认值为：text/html,text/xml,text/plain,text/css
server.compression.mime-types
指定要压缩的 MIME type，多个以逗号分隔。
server.compression.min-response-size
执行压缩的阈值，默认为 2048
server.context-parameters.[param name]
设置 servlet context 参数
server.context-path
设定应用的 context-path.
server.display-name
设定应用的展示名称，默认：application
server.jsp-servlet.class-name
设定编译 JSP 用的 servlet，默认：org.apache.jasper
.servlet.JspServlet)

server.jsp-servlet.init-parameters.[param name]
设置 JSP servlet 初始化参数。
server.jsp-servlet.registered
设定 JSP servlet 是否注册到内嵌的 servlet 容器，默认 true
server.port
设定 http 监听端口
server.servlet-path
设定 dispatcher servlet 的监听路径，默认为：/
cookie、session 配置
server.session.cookie.comment
指定 session cookie 的 comment
server.session.cookie.domain
指定 session cookie 的 domain
server.session.cookie.http-only
是否开启 HttpOnly.
server.session.cookie.max-age
设定 session cookie 的最大 age.
server.session.cookie.name
设定 Session cookie 的名称。
server.session.cookie.path
设定 session cookie 的路径。
server.session.cookie.secure
设定 session cookie 的“Secure” flag.
server.session.persistent
重启时是否持久化 session，默认 false
server.session.timeout
session 的超时时间
server.session.tracking-modes
设定 Session 的追踪模式 (cookie, url, ssl).

```

### ssl 配置

```
server.ssl.ciphers
是否支持 SSL ciphers.
server.ssl.client-auth
设定 client authentication 是 wanted 还是 needed.
server.ssl.enabled
是否开启 ssl，默认：true
server.ssl.key-alias
设定 key store 中 key 的别名。
server.ssl.key-password
访问 key store 中 key 的密码。
server.ssl.key-store
设定持有 SSL certificate 的 key store 的路径，通常是一个。jks 文件。
server.ssl.key-store-password
设定访问 key store 的密码。
server.ssl.key-store-provider
设定 key store 的提供者。
server.ssl.key-store-type
设定 key store 的类型。
server.ssl.protocol
使用的 SSL 协议，默认：TLS
server.ssl.trust-store
持有 SSL certificates 的 Trust store.
server.ssl.trust-store-password
访问 trust store 的密码。
server.ssl.trust-store-provider
设定 trust store 的提供者。
server.ssl.trust-store-type
指定 trust store 的类型。
```

### tomcat

```
server.tomcat.access-log-enabled
是否开启 access log ，默认：false)
server.tomcat.access-log-pattern
设定 access logs 的格式，默认：common
server.tomcat.accesslog.directory
设定 log 的目录，默认：logs
server.tomcat.accesslog.enabled
是否开启 access log，默认：false
server.tomcat.accesslog.pattern
设定 access logs 的格式，默认：common
server.tomcat.accesslog.prefix
设定 Log 文件的前缀，默认：access_log
server.tomcat.accesslog.suffix
设定 Log 文件的后缀，默认：.log
server.tomcat.background-processor-delay
后台线程方法的 Delay 大小：30
server.tomcat.basedir
设定 Tomcat 的 base 目录，如果没有指定则使用临时目录。
server.tomcat.internal-proxies
设定信任的正则表达式，默认：“10\.\d{1,3}\.\d{1,3}\.\d{1,3}| 192\.168\.\d{1,3}\.\d{1,3}| 169\.254\.\d{1,3}\.\d{1,3}| 127\.\d{1,3}\.\d{1,3}\.\d{1,3}| 172\.1[6-9]{1}\.\d{1,3}\.\d{1,3}| 172\.2[0-9]{1}\.\d{1,3}\.\d{1,3}|172\.3[0-1]{1}\.\d{1,3}\.\d{1,3}”
server.tomcat.max-http-header-size
设定 http header 的最小值，默认：0
server.tomcat.max-threads
设定 tomcat 的最大工作线程数，默认为：0
server.tomcat.port-header
设定 http header 使用的，用来覆盖原来 port 的 value.
server.tomcat.protocol-header
设定 Header 包含的协议，通常是 X-Forwarded-Proto，如果 remoteIpHeader 有值，则将设置为 RemoteIpValve.
server.tomcat.protocol-header-https-value
设定使用 SSL 的 header 的值，默认 https.
server.tomcat.remote-ip-header
设定 remote IP 的 header，如果 remoteIpHeader 有值，则设置为 RemoteIpValve
server.tomcat.uri-encoding
设定 URI 的解码字符集。
```

### undertow

```
server.undertow.access-log-dir
设定 Undertow access log 的目录，默认：logs
server.undertow.access-log-enabled
是否开启 access log，默认：false
server.undertow.access-log-pattern
设定 access logs 的格式，默认：common
server.undertow.accesslog.dir
设定 access log 的目录。
server.undertow.buffer-size
设定 buffer 的大小。
server.undertow.buffers-per-region
设定每个 region 的 buffer 数
server.undertow.direct-buffers
设定堆外内存
server.undertow.io-threads
设定 I/O 线程数。
server.undertow.worker-threads
设定工作线程数

```

## datasource

### datasource
```
spring.dao.exceptiontranslation.enabled
是否开启 PersistenceExceptionTranslationPostProcessor，默认为 true
spring.datasource.abandon-when-percentage-full
设定超时被废弃的连接占到多少比例时要被关闭或上报
spring.datasource.allow-pool-suspension
使用 Hikari pool 时，是否允许连接池暂停，默认为：false
spring.datasource.alternate-username-allowed
是否允许替代的用户名。
spring.datasource.auto-commit
指定 updates 是否自动提交。
spring.datasource.catalog
指定默认的 catalog.
spring.datasource.commit-on-return
设置当连接被归还时，是否要提交所有还未完成的事务
spring.datasource.connection-init-sql
指定连接被创建，再被添加到连接池之前执行的 sql.
spring.datasource.connection-init-sqls
使用 DBCP connection pool 时，指定初始化时要执行的 sql
spring.datasource.connection-properties.[key]
在使用 DBCP connection pool 时指定要配置的属性
spring.datasource.connection-test-query
指定校验连接合法性执行的 sql 语句
spring.datasource.connection-timeout
指定连接的超时时间，毫秒单位。
spring.datasource.continue-on-error
在初始化数据库时，遇到错误是否继续，默认 false
spring.datasource.data
指定 Data (DML) 脚本
spring.datasource.data-source-class-name
指定数据源的全限定名。
spring.datasource.data-source-jndi
指定 jndi 的地址
spring.datasource.data-source-properties.[key]
使用 Hikari connection pool 时，指定要设置的属性
spring.datasource.db-properties
使用 Tomcat connection pool，指定要设置的属性
spring.datasource.default-auto-commit
是否自动提交。
spring.datasource.default-catalog
指定连接默认的 catalog.
spring.datasource.default-read-only
是否设置默认连接只读。
spring.datasource.default-transaction-isolation
指定连接的事务的默认隔离级别。
spring.datasource.driver-class-name
指定 driver 的类名，默认从 jdbc url 中自动探测。
spring.datasource.fair-queue
是否采用 FIFO 返回连接。
spring.datasource.health-check-properties.[key]
使用 Hikari connection pool 时，在心跳检查时传递的属性
spring.datasource.idle-timeout
指定连接多久没被使用时，被设置为空闲，默认为 10ms
spring.datasource.ignore-exception-on-pre-load
当初始化连接池时，是否忽略异常。
spring.datasource.init-sql
当连接创建时，执行的 sql
spring.datasource.initial-size
指定启动连接池时，初始建立的连接数量
spring.datasource.initialization-fail-fast
当创建连接池时，没法创建指定最小连接数量是否抛异常
spring.datasource.initialize
指定初始化数据源，是否用 data.sql 来初始化，默认：true
spring.datasource.isolate-internal-queries
指定内部查询是否要被隔离，默认为 false
spring.datasource.jdbc-interceptors
使用 Tomcat connection pool 时，指定 jdbc 拦截器，分号分隔
spring.datasource.jdbc-url
指定 JDBC URL.
spring.datasource.jmx-enabled
是否开启 JMX，默认为：false
spring.datasource.jndi-name
指定 jndi 的名称。
spring.datasource.leak-detection-threshold
使用 Hikari connection pool 时，多少毫秒检测一次连接泄露。
spring.datasource.log-abandoned
使用 DBCP connection pool，是否追踪废弃 statement 或连接，默认为：false
spring.datasource.log-validation-errors
当使用 Tomcat connection pool 是否打印校验错误。
spring.datasource.login-timeout
指定连接数据库的超时时间。
spring.datasource.max-active
指定连接池中最大的活跃连接数。
spring.datasource.max-age
指定连接池中连接的最大年龄
spring.datasource.max-idle
指定连接池最大的空闲连接数量。
spring.datasource.max-lifetime
指定连接池中连接的最大生存时间，毫秒单位。
spring.datasource.max-open-prepared-statements
指定最大的打开的 prepared statements 数量。
spring.datasource.max-wait
指定连接池等待连接返回的最大等待时间，毫秒单位。
spring.datasource.maximum-pool-size
指定连接池最大的连接数，包括使用中的和空闲的连接。
spring.datasource.min-evictable-idle-time-millis
指定一个空闲连接最少空闲多久后可被清除。
spring.datasource.min-idle
指定必须保持连接的最小值 (For DBCP and Tomcat connection pools)
spring.datasource.minimum-idle
指定连接维护的最小空闲连接数，当使用 HikariCP 时指定。
spring.datasource.name
指定数据源名。
spring.datasource.num-tests-per-eviction-run
指定运行每个 idle object evictor 线程时的对象数量
spring.datasource.password
指定数据库密码。
spring.datasource.platform
指定 schema 要使用的 Platform(schema-${platform}.sql)，默认为：all
spring.datasource.pool-name
指定连接池名字。
spring.datasource.pool-prepared-statements
指定是否池化 statements.
spring.datasource.propagate-interrupt-state
在等待连接时，如果线程被中断，是否传播中断状态。
spring.datasource.read-only
当使用 Hikari connection pool 时，是否标记数据源只读
spring.datasource.register-mbeans
指定 Hikari connection pool 是否注册 JMX MBeans.
spring.datasource.remove-abandoned
指定当连接超过废弃超时时间时，是否立刻删除该连接。
spring.datasource.remove-abandoned-timeout
指定连接应该被废弃的时间。
spring.datasource.rollback-on-return
在归还连接时，是否回滚等待中的事务。
spring.datasource.schema
指定 Schema (DDL) 脚本。
spring.datasource.separator
指定初始化脚本的语句分隔符，默认：;
spring.datasource.sql-script-encoding
指定 SQL scripts 编码。
spring.datasource.suspect-timeout
指定打印废弃连接前的超时时间。
spring.datasource.test-on-borrow
当从连接池借用连接时，是否测试该连接。
spring.datasource.test-on-connect
创建时，是否测试连接
spring.datasource.test-on-return
在连接归还到连接池时是否测试该连接。
spring.datasource.test-while-idle
当连接空闲时，是否执行连接测试。
spring.datasource.time-between-eviction-runs-millis
指定空闲连接检查、废弃连接清理、空闲连接池大小调整之间的操作时间间隔
spring.datasource.transaction-isolation
指定事务隔离级别，使用 Hikari connection pool 时指定
spring.datasource.url
指定 JDBC URL.
spring.datasource.use-disposable-connection-facade
是否对连接进行包装，防止连接关闭之后被使用。
spring.datasource.use-equals
比较方法名时是否使用 String.equals() 替换 ==.
spring.datasource.use-lock
是否对连接操作加锁
spring.datasource.username
指定数据库名。
spring.datasource.validation-interval
指定多少 ms 执行一次连接校验。
spring.datasource.validation-query
指定获取连接时连接校验的 sql 查询语句。
spring.datasource.validation-query-timeout
指定连接校验查询的超时时间。
spring.datasource.validation-timeout
设定连接校验的超时时间，当使用 Hikari connection pool 时指定
spring.datasource.validator-class-name
用来测试查询的 validator 全限定名。
spring.datasource.xa.data-source-class-name
指定数据源的全限定名。
spring.datasource.xa.properties
指定传递给 XA data source 的属性
```
### JPA
```
spring.jpa.database
指定目标数据库。
spring.jpa.database-platform
指定目标数据库的类型。
spring.jpa.generate-ddl
是否在启动时初始化 schema，默认为 false
spring.jpa.hibernate.ddl-auto
指定 DDL mode (none, validate, update, create, create-drop). 当使用内嵌数据库时，默认是 create-drop，否则为 none.
spring.jpa.hibernate.naming-strategy
指定命名策略
spring.jpa.open-in-view
是否注册 OpenEntityManagerInViewInterceptor，绑定 JPA EntityManager 到请求线程中，默认为：true
spring.jpa.properties
添加额外的属性到 JPA provider.
spring.jpa.show-sql
是否开启 sql 的 log，默认为：false
```
### jooq
```
spring.jooq.sql-dialect
指定 JOOQ 使用的 SQLDialect，比如 POSTGRES.
```
### h2
```
spring.h2.console.enabled
是否开启控制台，默认为 false
spring.h2.console.path
指定控制台路径，默认为：/h2-console
```
### JTA
```
spring.jta.allow-multiple-lrc
是否允许 multiple LRC，默认为：false
spring.jta.asynchronous2-pc
指定两阶段提交是否可以异步，默认为：false
spring.jta.background-recovery-interval
指定多少分钟跑一次 recovery process，默认为：1
spring.jta.background-recovery-interval-seconds
指定多久跑一次 recovery process，默认：60
spring.jta.current-node-only-recovery
是否过滤掉其他非本 JVM 的 recovery，默认为：true
spring.jta.debug-zero-resource-transaction
是否追踪没有使用指定资源的事务，默认为：false
spring.jta.default-transaction-timeout
设定默认的事务超时时间，默认为 60
spring.jta.disable-jmx
是否禁用 jmx，默认为 false
spring.jta.enabled
是否开启 JTA support，默认为：true
spring.jta.exception-analyzer
设置指定的异常分析类
spring.jta.filter-log-status
使用 Bitronix Transaction Manager 时，是否写 mandatory logs，开启的话，可以节省磁盘空间，但是调试会复杂写，默认为 false
spring.jta.force-batching-enabled
使用 Bitronix Transaction Manager 时，是否批量写磁盘，默认为 true.
spring.jta.forced-write-enabled
使用 Bitronix Transaction Manager 时，是否强制写日志到磁盘，默认为 true
spring.jta.graceful-shutdown-interval
当使用 Bitronix Transaction Manager，指定 shutdown 时等待事务结束的时间，超过则中断，默认为 60
spring.jta.jndi-transaction-synchronization-registry-name
当使用 Bitronix Transaction Manager 时，在 JNDI 下得事务同步 registry，默认为：java:comp/TransactionSynchronizationRegistry
spring.jta.jndi-user-transaction-name
指定在 JNDI 使用 Bitronix Transaction Manager 的名称，默认：java:comp/UserTransaction
spring.jta.journal
当使用 Bitronix Transaction Manager，指定 The journal 是否 disk 还是 null 还是一个类的全限定名，默认 disk
spring.jta.log-dir
Transaction logs directory.
spring.jta.log-part1-filename
指定 The journal fragment 文件 1 的名字，默认：btm1.tlog
spring.jta.log-part2-filename
指定 The journal fragment 文件 2 的名字，默认：btm2.tlog
spring.jta.max-log-size-in-mb
指定 journal fragments 大小的最大值。默认：2M
spring.jta.resource-configuration-filename
指定 Bitronix Transaction Manager 配置文件名。
spring.jta.server-id
指定 Bitronix Transaction Manager 实例的 id.
spring.jta.skip-corrupted-logs
是否忽略 corrupted log files 文件，默认为 false.
spring.jta.transaction-manager-id
指定 Transaction manager 的唯一标识。
spring.jta.warn-about-zero-resource-transaction
当使用 Bitronix Transaction Manager 时，是否对没有使用指定资源的事务进行警告，默认为：true
```

## NoSQL

### cache
```
spring.cache.cache-names
指定要创建的缓存的名称，逗号分隔（若该缓存实现支持的话)
spring.cache.ehcache.config
指定初始化 EhCache 时使用的配置文件的位置指定。
spring.cache.guava.spec
指定创建缓存要使用的 spec，具体详见 CacheBuilderSpec.
spring.cache.hazelcast.config
指定初始化 Hazelcast 时的配置文件位置
spring.cache.infinispan.config
指定初始化 Infinispan 时的配置文件位置。
spring.cache.jcache.config
指定 jcache 的配置文件。
spring.cache.jcache.provider
指定 CachingProvider 实现类的全限定名。
spring.cache.type
指定缓存类型
```
### mongodb
```
spring.mongodb.embedded.features
指定要开启的特性，逗号分隔。
spring.mongodb.embedded.version
指定要使用的版本，默认：2.6.10
```
### redis
```
spring.redis.database
指定连接工厂使用的 Database index，默认为：0
spring.redis.host
指定 Redis server host，默认为：localhost
spring.redis.password
指定 Redis server 的密码
spring.redis.pool.max-active
指定连接池最大的活跃连接数，-1 表示无限，默认为 8
spring.redis.pool.max-idle
指定连接池最大的空闲连接数，-1 表示无限，默认为 8
spring.redis.pool.max-wait
指定当连接池耗尽时，新获取连接需要等待的最大时间，以毫秒单位，-1 表示无限等待
spring.redis.pool.min-idle
指定连接池中空闲连接的最小数量，默认为 0
spring.redis.port
指定 redis 服务端端口，默认：6379
spring.redis.sentinel.master
指定 redis server 的名称
spring.redis.sentinel.nodes
指定 sentinel 节点，逗号分隔，格式为 host:port.
spring.redis.timeout
指定连接超时时间，毫秒单位，默认为 0
```
### springdata
```
spring.data.elasticsearch.cluster-name
指定 es 集群名称，默认：elasticsearch
spring.data.elasticsearch.cluster-nodes
指定 es 的集群，逗号分隔，不指定的话，则启动 client node.
spring.data.elasticsearch.properties
指定要配置的 es 属性。
spring.data.elasticsearch.repositories.enabled
是否开启 es 存储，默认为：true
spring.data.jpa.repositories.enabled
是否开启 JPA 支持，默认为：true
spring.data.mongodb.authentication-database
指定鉴权的数据库名
spring.data.mongodb.database
指定 mongodb 数据库名
spring.data.mongodb.field-naming-strategy
指定要使用的 FieldNamingStrategy.
spring.data.mongodb.grid-fs-database
指定 GridFS database 的名称。
spring.data.mongodb.host
指定 Mongo server host.
spring.data.mongodb.password
指定 Mongo server 的密码。
spring.data.mongodb.port
指定 Mongo server port.
spring.data.mongodb.repositories.enabled
是否开启 mongodb 存储，默认为 true
spring.data.mongodb.uri
指定 Mongo database URI. 默认：mongodb://localhost/test
spring.data.mongodb.username
指定登陆 mongodb 的用户名。
spring.data.rest.base-path
指定暴露资源的基准路径。
spring.data.rest.default-page-size
指定每页的大小，默认为：20
spring.data.rest.limit-param-name
指定 limit 的参数名，默认为：size
spring.data.rest.max-page-size
指定最大的页数，默认为 1000
spring.data.rest.page-param-name
指定分页的参数名，默认为：page
spring.data.rest.return-body-on-create
当创建完实体之后，是否返回 body，默认为 false
spring.data.rest.return-body-on-update
在更新完实体后，是否返回 body，默认为 false
spring.data.rest.sort-param-name
指定排序使用的 key，默认为：sort
spring.data.solr.host
指定 Solr host，如果有指定了 zk 的 host 的话，则忽略。默认为：http://127.0.0.1:8983/solr
spring.data.solr.repositories.enabled
是否开启 Solr repositories，默认为：true
spring.data.solr.zk-host
指定 zk 的地址，格式为 HOST:PORT.
```

## MQ

### activemq
```
spring.activemq.broker-url
指定 ActiveMQ broker 的 URL，默认自动生成。
spring.activemq.in-memory
是否是内存模式，默认为 true.
spring.activemq.password
指定 broker 的密码。
spring.activemq.pooled
是否创建 PooledConnectionFactory，而非 ConnectionFactory，默认 false
spring.activemq.user
指定 broker 的用户。
artemis(HornetQ 捐献给 apache 后的版本)
spring.artemis.embedded.cluster-password
指定集群的密码，默认是启动时随机生成。
spring.artemis.embedded.data-directory
指定 Journal 文件的目录。如果不开始持久化则不必要指定。
spring.artemis.embedded.enabled
是否开启内嵌模式，默认 true
spring.artemis.embedded.persistent
是否开启 persistent store，默认 false.
spring.artemis.embedded.queues
指定启动时创建的队列，多个用逗号分隔，默认：[]
spring.artemis.embedded.server-id
指定 Server ID. 默认是一个自增的数字，从 0 开始。
spring.artemis.embedded.topics
指定启动时创建的 topic，多个的话逗号分隔，默认：[]
spring.artemis.host
指定 Artemis broker 的 host. 默认：localhost
spring.artemis.mode
指定 Artemis 的部署模式，默认为 auto-detected（也可以为 native or embedded).
spring.artemis.port
指定 Artemis broker 的端口，默认为：61616
```
### rabbitmq
```
spring.rabbitmq.addresses
指定 client 连接到的 server 的地址，多个以逗号分隔。
spring.rabbitmq.dynamic
是否创建 AmqpAdmin bean. 默认为：true)
spring.rabbitmq.host
指定 RabbitMQ host. 默认为：localhost)
spring.rabbitmq.listener.acknowledge-mode
指定 Acknowledge 的模式。
spring.rabbitmq.listener.auto-startup
是否在启动时就启动 mq，默认：true)
spring.rabbitmq.listener.concurrency
指定最小的消费者数量。
spring.rabbitmq.listener.max-concurrency
指定最大的消费者数量。
spring.rabbitmq.listener.prefetch
指定一个请求能处理多少个消息，如果有事务的话，必须大于等于 transaction 数量。
spring.rabbitmq.listener.transaction-size
指定一个事务处理的消息数量，最好是小于等于 prefetch 的数量。
spring.rabbitmq.password
指定 broker 的密码。
spring.rabbitmq.port
指定 RabbitMQ 的端口，默认：5672)
spring.rabbitmq.requested-heartbeat
指定心跳超时，0 为不指定。
spring.rabbitmq.ssl.enabled
是否开始 SSL，默认：false)
spring.rabbitmq.ssl.key-store
指定持有 SSL certificate 的 key store 的路径
spring.rabbitmq.ssl.key-store-password
指定访问 key store 的密码。
spring.rabbitmq.ssl.trust-store
指定持有 SSL certificates 的 Trust store.
spring.rabbitmq.ssl.trust-store-password
指定访问 trust store 的密码。
spring.rabbitmq.username
指定登陆 broker 的用户名。
spring.rabbitmq.virtual-host
指定连接到 broker 的 Virtual host.
```
### hornetq
```
spring.hornetq.embedded.cluster-password
指定集群的密码，默认启动时随机生成。
spring.hornetq.embedded.data-directory
指定 Journal file 的目录。如果不开启持久化则不必指定。
spring.hornetq.embedded.enabled
是否开启内嵌模式，默认：true
spring.hornetq.embedded.persistent
是否开启 persistent store，默认：false
spring.hornetq.embedded.queues
指定启动是创建的 queue，多个以逗号分隔，默认：[]
spring.hornetq.embedded.server-id
指定 Server ID. 默认使用自增数字，从 0 开始。
spring.hornetq.embedded.topics
指定启动时创建的 topic，多个以逗号分隔，默认：[]
spring.hornetq.host
指定 HornetQ broker 的 host，默认：localhost
spring.hornetq.mode
指定 HornetQ 的部署模式，默认是 auto-detected，也可以指定 native 或者 embedded.
spring.hornetq.port
指定 HornetQ broker 端口，默认：5445
```
### jms
```
spring.jms.jndi-name
指定 Connection factory JNDI 名称。
spring.jms.listener.acknowledge-mode
指定 ack 模式，默认自动 ack.
spring.jms.listener.auto-startup
是否启动时自动启动 jms，默认为：true
spring.jms.listener.concurrency
指定最小的并发消费者数量。
spring.jms.listener.max-concurrency
指定最大的并发消费者数量。
spring.jms.pub-sub-domain
是否使用默认的 destination type 来支持 publish/subscribe，默认：false
```

## Security
```
security.basic.authorize-mode
要使用权限控制模式。
security.basic.enabled
是否开启基本的鉴权，默认为 true
security.basic.path
需要鉴权的 path，多个的话以逗号分隔，默认为 [/**]
security.basic.realm
HTTP basic realm 的名字，默认为 Spring
security.enable-csrf
是否开启 cross-site request forgery 校验，默认为 false.
security.filter-order
Security filter chain 的 order，默认为 0
security.headers.cache
是否开启 http 头部的 cache 控制，默认为 false.
security.headers.content-type
是否开启 X-Content-Type-Options 头部，默认为 false.
security.headers.frame
是否开启 X-Frame-Options 头部，默认为 false.
security.headers.hsts
指定 HTTP Strict Transport Security (HSTS) 模式 (none, domain, all).
security.headers.xss
是否开启 cross-site scripting (XSS) 保护，默认为 false.
security.ignored
指定不鉴权的路径，多个的话以逗号分隔。
security.oauth2.client.access-token-uri
指定获取 access token 的 URI.
security.oauth2.client.access-token-validity-seconds
指定 access token 失效时长。
security.oauth2.client.additional-information.[key]
设定要添加的额外信息。
security.oauth2.client.authentication-scheme
指定传输不记名令牌 (bearer token) 的方式 (form, header, none,query)，默认为 header
security.oauth2.client.authorities
指定授予客户端的权限。
security.oauth2.client.authorized-grant-types
指定客户端允许的 grant types.
security.oauth2.client.auto-approve-scopes
对客户端自动授权的 scope.
security.oauth2.client.client-authentication-scheme
传输 authentication credentials 的方式 (form, header, none, query)，默认为 header 方式
security.oauth2.client.client-id
指定 OAuth2 client ID.
security.oauth2.client.client-secret
指定 OAuth2 client secret. 默认是一个随机的 secret.
security.oauth2.client.grant-type
指定获取资源的 access token 的授权类型。
security.oauth2.client.id
指定应用的 client ID.
security.oauth2.client.pre-established-redirect-uri
服务端 pre-established 的跳转 URI.
security.oauth2.client.refresh-token-validity-seconds
指定 refresh token 的有效期。
security.oauth2.client.registered-redirect-uri
指定客户端跳转 URI，多个以逗号分隔。
security.oauth2.client.resource-ids
指定客户端相关的资源 id，多个以逗号分隔。
security.oauth2.client.scope
client 的 scope
security.oauth2.client.token-name
指定 token 的名称
security.oauth2.client.use-current-uri
是否优先使用请求中 URI，再使用 pre-established 的跳转 URI. 默认为 true
security.oauth2.client.user-authorization-uri
用户跳转去获取 access token 的 URI.
security.oauth2.resource.id
指定 resource 的唯一标识。
security.oauth2.resource.jwt.key-uri
JWT token 的 URI. 当 key 为公钥时，或者 value 不指定时指定。
security.oauth2.resource.jwt.key-value
JWT token 验证的 value. 可以是对称加密或者 PEMencoded RSA 公钥。可以使用 URI 作为 value.
security.oauth2.resource.prefer-token-info
是否使用 token info，默认为 true
security.oauth2.resource.service-id
指定 service ID，默认为 resource.
security.oauth2.resource.token-info-uri
token 解码的 URI.
security.oauth2.resource.token-type
指定当使用 userInfoUri 时，发送的 token 类型。
security.oauth2.resource.user-info-uri
指定 user info 的 URI
security.oauth2.sso.filter-order
如果没有显示提供 WebSecurityConfigurerAdapter 时指定的 Filter order.
security.oauth2.sso.login-path
跳转到 SSO 的登录路径默认为 /login.
security.require-ssl
是否对所有请求开启 SSL，默认为 false.
security.sessions
指定 Session 的创建策略 (always, never, if_required, stateless).
security.user.name
指定默认的用户名，默认为 user.
security.user.password
默认的用户密码。
security.user.role
默认用户的授权角色。
```

## 	Migration
### flyway
```
flyway.baseline-description
对执行迁移时基准版本的描述。
flyway.baseline-on-migrate
当迁移时发现目标 schema 非空，而且包含没有元数据的表时，是否自动执行基准迁移，默认 false.
flyway.baseline-version
开始执行基准迁移时对现有的 schema 的版本打标签，默认值为 1.
flyway.check-location
检查迁移脚本的位置是否存在，默认 false.
flyway.clean-on-validation-error
当发现校验错误时是否自动调用 clean，默认 false.
flyway.enabled
是否开启 flywary，默认 true.
flyway.encoding
设置迁移时的编码，默认 UTF-8.
flyway.ignore-failed-future-migration
当读取元数据表时是否忽略错误的迁移，默认 false.
flyway.init-sqls
当初始化好连接时要执行的 SQL.
flyway.locations
迁移脚本的位置，默认 db/migration.
flyway.out-of-order
是否允许无序的迁移，默认 false.
flyway.password
目标数据库的密码，缺省使用项目数据源的密码
flyway.placeholder-prefix
设置每个 placeholder 的前缀，默认 ${.
flyway.placeholder-replacement
placeholders 是否要被替换，默认 true.
flyway.placeholder-suffix
设置每个 placeholder 的后缀，默认}.
flyway.placeholders.[placeholder name]
设置 placeholder 的 value
flyway.schemas
设定需要 flywary 迁移的 schema，大小写敏感，默认为连接默认的 schema.
flyway.sql-migration-prefix
迁移文件的前缀，默认为 V.
flyway.sql-migration-separator
迁移脚本的文件名分隔符，默认__
flyway.sql-migration-suffix
迁移脚本的后缀，默认为。sql
flyway.table
flyway 使用的元数据表名，默认为 schema_version
flyway.target
迁移时使用的目标版本，默认为 latest version
flyway.url
迁移时使用的 JDBC URL，如果没有指定的话，将使用配置的主数据源
flyway.user
迁移数据库的用户名，缺省使用项目数据源的用户名
flyway.validate-on-migrate
迁移时是否校验，默认为 true.
```
### liquibase
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
## 其它

### aop
```
spring.aop.auto
是否支持 @EnableAspectJAutoProxy，默认为：true
spring.aop.proxy-target-class
true 为使用 CGLIB 代理，false 为 JDK 代理，默认为 false
application
spring.application.admin.enabled
是否启用 admin 特性，默认为：false
spring.application.admin.jmx-name
指定 admin MBean 的名称，默认为：org.springframework.boot:type=Admin,name=SpringApplication
autoconfig
spring.autoconfigure.exclude
配置要排除的 Auto-configuration classes.
```
### batch
```
spring.batch.initializer.enabled
是否在必要时创建 batch 表，默认为 true
spring.batch.job.enabled
是否在启动时开启 batch job，默认为 true
spring.batch.job.names
指定启动时要执行的 job 的名称，逗号分隔，默认所有 job 都会被执行
spring.batch.schema
指定要初始化的 sql 语句路径，默认：classpath:org/springframework/batch/core/schema-@@platform@@.sql)
spring.batch.table-prefix
指定批量处理的表的前缀。
```
### jmx
```
spring.jmx.default-domain
指定 JMX domain name.
spring.jmx.enabled
是否暴露 jmx，默认为 true
spring.jmx.server
指定 MBeanServer bean name. 默认为：mbeanServer)
```
### mail
```
spring.mail.default-encoding
指定默认 MimeMessage 的编码，默认为：UTF-8
spring.mail.host
指定 SMTP server host.
spring.mail.jndi-name
指定 mail 的 jndi 名称
spring.mail.password
指定 SMTP server 登陆密码。
spring.mail.port
指定 SMTP server port.
spring.mail.properties
指定 JavaMail session 属性。
spring.mail.protocol
指定 SMTP server 使用的协议，默认为：smtp
spring.mail.test-connection
指定是否在启动时测试邮件服务器连接，默认为 false
spring.mail.username
指定 SMTP server 的用户名。
```
### sendgrid
```
spring.sendgrid.password
指定 SendGrid password.
spring.sendgrid.proxy.host
指定 SendGrid proxy host.
spring.sendgrid.proxy.port
指定 SendGrid proxy port.
spring.sendgrid.username
指定 SendGrid username.
```
### social
```
spring.social.auto-connection-views
是否开启连接状态的视图，默认为 false
spring.social.facebook.app-id
指定应用 id
spring.social.facebook.app-secret
指定应用密码
spring.social.linkedin.app-id
指定应用 id
spring.social.linkedin.app-secret
指定应用密码
spring.social.twitter.app-id
指定应用 ID.
spring.social.twitter.app-secret
指定应用密码
```