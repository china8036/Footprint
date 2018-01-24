- [Tomcat 与 Nginx 搭配部署](#tomcat-%E4%B8%8E-nginx-%E6%90%AD%E9%85%8D%E9%83%A8%E7%BD%B2)
  - [背景与意义](#%E8%83%8C%E6%99%AF%E4%B8%8E%E6%84%8F%E4%B9%89)
  - [Tomcat 设置](#tomcat-%E8%AE%BE%E7%BD%AE)
  - [Nginx 设置](#nginx-%E8%AE%BE%E7%BD%AE)
    - [nginx.conf](#nginxconf)
    - [负载均衡策略](#%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E7%AD%96%E7%95%A5)
  - [分布式 / 集群 session 共享机制](#%E5%88%86%E5%B8%83%E5%BC%8F-%E9%9B%86%E7%BE%A4-session-%E5%85%B1%E4%BA%AB%E6%9C%BA%E5%88%B6)
    - [Session 保持（黏性 session）](#session-%E4%BF%9D%E6%8C%81%EF%BC%88%E9%BB%8F%E6%80%A7-session%EF%BC%89)
    - [Session 复制](#session-%E5%A4%8D%E5%88%B6)
    - [Session 共享](#session-%E5%85%B1%E4%BA%AB)
  - [Refer Links](#refer-links)

# Tomcat 与 Nginx 搭配部署

## 背景与意义

Tomcat 服务器作为一个 Web 服务器，其并发数在 300-500 之间，如果超过 500 的并发数会出现 Tomcat 不能响应新的请求的情况，严重影响网站的运行。同时如果访问量非常大的情况下，Tomcat 的线程数会不断增加。因此会占据大量内存，严重时出现内存溢出的现象，这时需要重启 Tomcat 以释放内存，阻断了网站的运行。所以对 Tomcat 做负载均衡便很有必要。

目前可以和 Tomcat 做负载均衡的主流服务器是 Apache 和 nginx，其中 Nginx 由于功能多、配置简单等优点成为很多负载均衡服务器的首选。Nginx 的并发数可达到 50000，所以理论上可以和 Tomcat 以 1：100 的比例来配置，这边可以很好的解决网站并发瓶颈问题。

在实际生产中，Tomcat 服务器一般不单独使用在项目中，对于静态资源的响应 Nginx 表现的比较好，另外由于 nginx 是专门用于反向代理的服务器，所以很容易实现将 java 的请求转发到后端交给 tomcat 容器处理，而本身用来处理静态资源。

nginx 主要是通过反向代理的方法将 jsp,jspx 后缀或者是 javaee 框架设置的特定的页面 (.do，.action) 请求来交给 tomcat 处理，自己处理。html,.css 或者是一些图片和 flash。

优势：
- 静态分离，加快用户访问网站的速度。
- 整个负载均衡层和 Web 层的工作流程为 LVS/DR+Keeaplived→Nginx 反向代理（动静分离）→Tomcat 集群，可以保证整个网站不会因为某一台 LVS 或 Nginx+tomcat 机器挂掉而影响网站的运营。
- Nginx 稳定，宕机的可能性微乎其乎。

## Tomcat 设置

分别配置多个 tomcat：
- 修改 HTTP/1.1 监听不同的端口，如 8081、8082 等；
- 修改 AJP 监听不同的端口，如 8009、8010； 

同时启动多个 tomcat；

## Nginx 设置

### nginx.conf

1. 配置服务器组，在 http{}节点之间添加 upstream 配置。（注意不要写 localhost，不然会增加地址解析时间，降低访问速度）
    ```conf
    upstream nginxDemo {
        server 127.0.0.1:8081;   #服务器地址 1
        server 127.0.0.1:8082;   #服务器地址 2
    }
    ```

1. 修改 nginx 监听的端口号 80，改为 8080。
    
    ```conf
    server {
        listen       8080;
        ......
    }
    ```

1. 在 location\{}中，利用 proxy_pass 配置反向代理地址；此处“http://”不能少，**后面的地址要和第一步 upstream 定义的名称保持一致**。
    
    ```conf
    location / {
            root   html; #根目录
            index  index.html index.htm; #默认首页，按顺序匹配
            proxy_pass http://nginxDemo; #配置反向代理地址
            
            #以下配置不是必须，视情况添加
            #proxy_redirect    off; 
              #以下三条使后端的 Web 服务器可以通过 X-Forwarded-For 获取用户真实 IP
            #proxy_set_header   Host $host; 
            #proxy_set_header   X-Real-IP $remote_addr; 
            #proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for; 
            #client_max_body_size   10m; #允许客户端请求的最大单文件字节数
            #client_body_buffer_size   128k; #缓冲区代理缓冲用户端请求的最大字节数
            #proxy_connect_timeout   90; # Nginx 跟后端服务器连接超时时间
            #proxy_send_timeout   90; #连接成功后，请求后端服务器的超时时间
            #proxy_read_timeout   90; #连接成功后，后端服务器响应的超时时间
            #proxy_buffer_size   4k; #设置代理服务器保存用户头信息的缓冲区大小
            #proxy_buffers   6 32k; #proxy_buffers 缓冲区
            #proxy_busy_buffers_size   64k; #高负荷下缓冲大小
            #proxy_temp_file_write_size  64k;#设定缓存文件夹大小
    }
    ```

    测试：访问 localhost:

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/85ab518347db0fb7e9cb1184cd2d6924.jpg)

1. 配置动静分离

    Nginx 的静态处理能力和效率远高于 Tomcat，因此将对动态文件（如 jsp）的请求交予 tomcat 处理，而对静态文件的请求直接由 nginx 处理；

    将第三步中的 location / {……}全部注释，在同一层级加入下边两个 location：
    ```conf
    location ~ \.jsp$ { #用正则表达式将所有 JSP 的请求匹配到该 location 中
        index  index.jsp;
    proxy_pass http://127.0.0.1:8080; #配置反向代理地址

      proxy_redirect    off; 
      proxy_set_header   Host $host; #以下三条使后端的 Web 服务器可以通过 X-Forwarded-For 获取用户真实 IP
      proxy_set_header   X-Real-IP $remote_addr; 
      proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for; 
      client_max_body_size   10m; #允许客户端请求的最大单文件字节数
      client_body_buffer_size   128k; #缓冲区代理缓冲用户端请求的最大字节数
      proxy_connect_timeout   90; # Nginx 跟后端服务器连接超时时间
      proxy_send_timeout   90; #连接成功后，请求后端服务器的超时时间
      proxy_read_timeout   90; #连接成功后，后端服务器响应的超时时间
      proxy_buffer_size   4k; #设置代理服务器保存用户头信息的缓冲区大小
      proxy_buffers   6 32k; #proxy_buffers 缓冲区
      proxy_busy_buffers_size   64k; #高负荷下缓冲大小
      proxy_temp_file_write_size  64k;#设定缓存文件夹大小
    }

    location ~ \.(gif|jpg|png|bmp|swf)$ { #用正则表达式将所有静态资源的请求匹配到该 location 中
      expires 30d;#使用 expires 缓存模块，缓存到客户端 30 天
      root D:\software\web\apache-tomcat-9.0.0.M21-windows-x64\webapps\ROOT; #将匹配到的请求都直接到指定目录下查找，不经过 tomcat 服务器处理
    }
    location ~ \.(html|js|css)$ {
      expires 1h;
      root D:\software\web\apache-tomcat-9.0.0.M21-windows-x64\webapps\ROOT;
    }   
    ``` 
    测试：

    访问 localhost，显示 nginx 的 404 页面，因为没有对 / 的访问做匹配；

    访问 localhost/index.jsp，正常显示 tomcat/webapp/ROOT 项目：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/264e3c3945bc1da50e44f436a15d2340.jpg)

    当点击页面上的其他链接时，会显示 nginx 的 404 页面，因为当前只配置了 ROOT 项目的目录

    注意：
    - 权限问题：
      
      如果配置完仍然发现无法读取静态文件，看看访问 http://localhost/tomcat.png 时是否显示 403 forbidden。如果是的话就是因为权限问题导致的，这里简单的解决办法是把 nginx.conf 首行的 user 设为 root:
      
      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/f15b68fb917aba9280011a9a42631a19.jpg)

      如果不想使用 root 用户运行，可以通过修改目录访问权限解决 403 问题，但不能把目录放在 root 用户宿主目录下，放在任意一个位置并给它 755，或者通过 chown 改变它的拥有者与 Nginx 运行身份一致也可以解决权限问题。

### 负载均衡策略

- 轮询（默认）

  每个 web 请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器 down 掉，能自动剔除。
  
  ```conf
  upstream nginxDemo {
      server 127.0.0.1:8081;
      server 127.0.0.1:8082;
  }
  ```

- 最少链接

  web 请求会被转发到连接数最少的服务器上。
  
  ```conf
  upstream nginxDemo {
      least_conn;
      server 127.0.0.1:8081;
      server 127.0.0.1:8082;
  }
  ```

- weight 权重

  指定轮询几率，weight 和访问比率成正比，用于后端服务器性能不均的情况，weight 默认是 1。
  
  ```conf
  #服务器 A 和服务器 B 的访问比例为：2-1; 比如有 3 个请求，前两个会访问 A，三个访问 B，其它规则和轮询一样。
  upstream nginxDemo {
      server 127.0.0.1:8081 weight=2; #服务器 A
      server 127.0.0.1:8082; #服务器 B
  }
  ```

- ip_hash

  每个请求按访问 ip 的 hash 值分配，这样同一客户端连续的 Web 请求都会被分发到同一服务器进行处理，可以解决 session 的问题。当后台服务器宕机时，会自动跳转到其它服务器。
  
  ```conf
  upstream nginxDemo {
      ip_hash;
      server 127.0.0.1:8081 weight=2; #服务器 A
      server 127.0.0.1:8082; #服务器 B
  }
  ```
  基于 weight 的负载均衡和基于 ip_hash 的负载均衡可以组合在一起使用。

- url_hash（第三方）
  
  url_hash 是 nginx 的第三方模块，nginx 本身不支持，需要打补丁。
  
  nginx 按访问 url 的 hash 结果来分配请求，使每个 url 定向到同一个后端服务器，后端服务器为缓存服务器、文件服务器、静态服务器时比较有效。缺点是当后端服务器宕机的时候，url_hash 不会自动跳转的其他缓存服务器，而是返回给用户一个 503 错误。
  
  ```conf
  upstream nginxDemo {
      server 127.0.0.1:8081; #服务器 A
      server 127.0.0.1:8082; #服务器 B
      hash $request_url;
  }
  ```

- fair（第三方）
  
  按后端服务器的响应时间来分配请求，响应时间短的优先分配。
  
  ```conf
  upstream nginxDemo {
      server 127.0.0.1:8081; #服务器 A
      server 127.0.0.1:8082; #服务器 B
      fair;
  }
  ```

## 分布式 / 集群 session 共享机制

在搭建完集群或者分布式环境之后，如果不做任何处理的话，网站将频繁的出现用户未登录的现象。比如：集群中有 A、B 两台服务器，用户第一次访问网站时，Nginx 将用户请求分发到 A 服务器，这时 A 服务器给用户创建了一个 Session，当用户第二次访问网站时，假设 Nginx 将用户请求分发到了 B 服务器上，而这时 B 服务器并不存在用户的 Session，所以就会出现用户未登录的情况，这对用户来说是不可忍受的。

因此，在搭建集群 / 分布式环境之后，必须考虑的一个问题就是用户访问产生的 session 如何处理，即 session 的共享机制。

处理 Session 的方式大致分为三种：Session 保持（黏性 Session）、Session 复制、Session 共享。

### Session 保持（黏性 session）

Session 保持（会话保持）就是将用户锁定到某一个服务器上。比如上面说的例子，用户第一次请求时，负载均衡器（Nginx）将用户的请求分发到了 A 服务器上，如果负载均衡器（Nginx）设置了 Session 保持的话，那么用户以后的每次请求都会分发到 A 服务器上，相当于把用户和 A 服务器粘到了一块，这就是 Session 保持的原理。Session 保持方案在所有的负载均衡器都有对应的实现。而且这是在负载均衡这一层就可以解决 Session 问题。

优点：非常简单，不需要对 session 做任何处理。

缺点：
- 负责不绝对均衡了：由于使用了 Session 保持，很显然就无法保证负载绝对的均衡。
- 缺乏容错性：如果后端某台服务器宕机，那么这台服务器的 Session 丢失，被分配到这台服务请求的用户还是需要重新登录，所以没有彻底的解决问题。

适用场景：发生故障对客户产生的影响较小；服务器发生故障是低概率事件。

实现方式：以 Nginx 为例，在 upstream 模块配置 ip_hash 属性即可实现粘性 Session。

### Session 复制

针对 Session 保持的容错性缺点，我们可以在所有服务器上都保存一份用户的 Session 信息。这种将每个服务器中的 Session 信息复制到其它服务器上的处理办法就称为会话复制。当任何一台服务器上的 session 发生改变时，该节点会把 session 的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要 session，以此来保证 Session 同步。

优点：可容错，各个服务器间的 Session 能够实时响应。

缺点：
- 将 session 广播同步给成员，会对网络负荷造成一定压力；
- 如果 session 量大的话可能会造成网络堵塞，拖慢服务器性能，特别是 session 保存的对象较大，并且对象变化较快时；从而也使应用水平扩展受到限制；
- session 内容的序列化，也消耗了系统的性能；
- 实现方式局限，必须在同一种组件之间实现（比如用的 tomcat，则必须全部用 tomcat）。

实现方式：tomcat 本身已支持该功能，可以参考官方文档。而且这种处理方式，**在大的集群中也不推荐生产环境使用**。

Tomcat 的会话复制分为两种：
- 全局复制 (DeltaManager)：复制会话中的变更信息到集群中的所有其他节点。
- 非全局复制（BackupManager）：它会把 Session 复制给一个指定的备份节点。

### Session 共享

Session 共享的实现方式有很多种，比如 memcached、Redis、DB 等；核心思想是修改 tomcat 的 session 存储机制，使之能够把 session 序列化，然后存放到 memcached 中。

使用 Session 共享可以分为两种机制：
- 黏性 Session 处理方式：
  
  Tomcat 本地 Session 为主 Session，Memcached 中的 Session 为备 Session。用户访问时首先在 tomcat 中创建 session，然后将 session 复制一份放到对应的 memcahed 上。memcache 只起备份作用，读写都在 tomcat 上。当某一个 tomcat 挂掉后，集群将用户的访问定位到备 tomcat 上，然后根据 cookie 中存储的 SessionId 找 session，找不到时，再去相应的 memcached 上取 session，找到之后将其复制到备 tomcat 上。

- 非黏性 Session 处理方式：
  
  Tomcat 本地 Session 为中转 Session，Memcached 为主备 Session。创建的 session 都往 memcached 上写，读取都从 memcached 读取，tomcat 本身不存储 session。

实现方式：[Tomcat+Nginx+MSM+memcached](https://my.oschina.net/bgq365/blog/870569) 

[关于 tomcat 集群中 session 共享的三种方法](http://ezerg.iteye.com/blog/2068557) 
- 使用 filter 方法存储
- 使用 tomcat session manager 方法存储
- 使用 terracotta 服务器共享

## Refer Links

[同时启动多个 tomcat](https://my.oschina.net/bgq365/blog/870155)

[nginx+tomcat 配置负载均衡集群](https://my.oschina.net/bgq365/blog/870569)

[tomcat + MSM：Manager 标签属性说明](https://my.oschina.net/bgq365/blog/879833) 

http://blog.csdn.net/cxm19881208/article/details/65441865 
