- [Nginx 配置](#nginx-配置)
  - [1. 配置文件结构](#1-配置文件结构)
    - [1.1. main 模块](#11-main-模块)
    - [1.2. events 模块](#12-events-模块)
    - [1.3. http 模块](#13-http-模块)
    - [1.4. server 模块](#14-server-模块)
    - [1.5. location 模块](#15-location-模块)
    - [1.6. upstram 模块](#16-upstram-模块)
  - [2. 常用配置](#2-常用配置)
    - [2.1. 优化配置](#21-优化配置)
    - [2.2. 安全配置](#22-安全配置)
    - [2.3. 虚拟主机配置](#23-虚拟主机配置)
  - [3. Refer Links](#3-refer-links)

# Nginx 配置

更改配置文件后，可以通过 `nginx -t` 命令来测试配置文件的语法是否正确。

对比 Apache 的配置文件，它的相对比较清晰和简单。

## 1. 配置文件结构

nginx 配置文件主要分为六个区域：
- main（全局设置）
- events（nginx 工作模式）
- http（http 设置）
- sever（主机设置）
- location（URL 匹配）
- upstream（负载均衡服务器设置）

```
main

events   {
  ....
}

http        {
  ....

  upstream myproject {
    .....
  }

  server  {
    ....

    location {
        ....
    }
  }
  server  {
    ....
    location {
        ....
    }
  }
  ....
}

```

### 1.1. main 模块

e.g.
```
user nobody nobody;
worker_processes 2;

error_log  /usr/local/var/log/nginx/error.log  notice;
pid        /usr/local/var/run/nginx/nginx.pid;

worker_rlimit_nofile 1024;
```

其中：
- user 来指定 Nginx Worker 进程运行用户以及用户组，默认由 nobody 账号运行。
- worker_processes 来指定了 Nginx 要开启的子进程数。每个 Nginx 进程平均耗费 10M~12M 内存。根据经验，一般指定 1 个进程就足够了，如果是多核 CPU，建议指定和 CPU 的数量一样的进程数即可。我这里写 2，那么就会开启 2 个子进程，总共 3 个进程。
- error_log 用来定义全局错误日志文件。日志输出级别有 debug、info、notice、warn、error、crit 可供选择，其中，debug 输出日志最为最详细，而 crit 输出日志最少。
- pid 用来指定进程 id 的存储文件位置。
- worker_rlimit_nofile 用于指定一个 nginx 进程可以打开的最多文件描述符数目，这里是 65535，需要使用命令“ulimit -n 65535”来设置。

### 1.2. events 模块

events 模块来用指定 nginx 的工作模式和工作模式及连接数上限。

e.g.
```
events {
    use kqueue; #mac 平台
    worker_connections  1024;
}
```

其中：
- use 用来指定 Nginx 的工作模式。Nginx 支持的工作模式有 select、poll、kqueue、epoll、rtsig 和 /dev/poll。其中 select 和 poll 都是标准的工作模式，kqueue 和 epoll 是高效的工作模式，不同的是 epoll 用在 Linux 平台上，而 kqueue 用在 BSD 系统中，因为 Mac 基于 BSD, 所以 Mac 也得用这个模式，对于 Linux 系统，epoll 工作模式是首选。
- worker_connections 用于定义 Nginx 每个进程的最大连接数，即接收前端的最大请求数，默认是 1024。最大客户端连接数由 worker_processes 和 worker_connections 决定，即`Max_clients = worker_processes * worker_connections`，在作为反向代理时，Max_clients 变为：`Max_clients = worker_processes * worker_connections/4`。
- 进程的最大连接数受 Linux 系统进程的最大打开文件数限制，在执行操作系统命令“ulimit -n 65536”后 worker_connections 的设置才能生效。

### 1.3. http 模块

**http 模块是 Nginx 配置中最核心的模块，它负责 HTTP 服务器相关属性的配置**。

e.g.
```
http{
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /usr/local/var/log/nginx/access.log  main;

    sendfile        on;
    tcp_nopush      on;
    tcp_nodelay     on;

    keepalive_timeout  10;

    #gzip  on;

    upstream myproject {
       .....
    }

    server {
       ....
    }
}
```
- include 来用设定文件的 mime 类型，类型在配置文件目录下的 mime.type 文件定义，来告诉 nginx 来识别文件类型。
- default_type 设定了默认的类型为二进制流，也就是当文件类型未定义时使用这种方式，例如在没有配置 asp 的 locate 环境时，Nginx 是不予解析的，此时，用浏览器访问 asp 文件就会出现下载了。
- log_format 用于设置日志的格式，和记录哪些参数，这里设置为 main，刚好用于 access_log 来纪录这种类型。
- main 的类型日志如下：也可以增删部分参数。
  ```
  127.0.0.1 - - [21/Apr/2015:18:09:54 +0800] "GET /index.php HTTP/1.1" 200 87151 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.76 Safari/537.36"
  ```
- access_log 用来纪录每次的访问日志的文件地址，后面的 main 是日志的格式样式，对应于 log_format 的 main。
- sendfile 参数用于开启高效文件传输模式。将 tcp_nopush 和 tcp_nodelay 两个指令设置为 on 用于防止网络阻塞。
- keepalive_timeout 设置客户端连接保持活动的超时时间。在超过这个时间之后，服务器会关闭该连接。

### 1.4. server 模块

sever 模块是 http 的子模块，它用来定一个虚拟主机。

e.g.
```
server {
    listen       8080;
    server_name  localhost 192.168.12.10 www.baidu.com;
    # 全局定义，如果都是这一个目录，这样定义最简单
    root   /Users/yangyi/www;
    index  index.php index.html index.htm;

    charset utf-8;
    access_log  usr/local/var/log/host.access.log  main;
    aerror_log  usr/local/var/log/host.error.log  error;

    ....
}
```

其中：
- server 标志定义虚拟主机开始。
- listen 用于指定虚拟主机的服务端口。
- server_name 用来指定 IP 地址或者域名，多个域名之间用空格分开。
- root 表示在这整个 server 虚拟主机内，全部的 root web 根目录。注意要和 locate {}下面定义的区分开来。
- index 全局定义访问的默认首页地址。注意要和 locate {}下面定义的区分开来。
- charset 用于设置网页的默认编码格式。
- access_log 用来指定此虚拟主机的访问日志存放路径，最后的 main 用于指定访问日志的输出格式。

### 1.5. location 模块

location 模块是 nginx 中用的最多的，也是最重要的模块了，什么负载均衡啊、反向代理啊、虚拟域名啊都与它相关。

location 根据它字面意思就知道是来定位的，定位 URL，解析 URL，所以，它也提供了强大的正则匹配功能，也支持条件判断匹配，用户可以通过 location 指令实现 Nginx 对动、静态网页进行过滤处理。

设定默认首页和虚拟机目录：
```
location / {
            root   /Users/yangyi/www;
            index  index.php index.html index.htm;
        }
```
- location / 表示匹配访问根目录。
- root 指令用于指定访问根目录时，虚拟主机的 web 目录，这个目录可以是相对路径（相对路径是相对于 nginx 的安装目录）。也可以是绝对路径。
- index 用于设定我们只输入域名后访问的默认首页地址，有个先后顺序：index.php index.html index.htm，如果没有开启目录浏览权限，又找不到这些默认首页，就会报 403 错误。

正则匹配：location 还有一种方式就是正则匹配，开启正则匹配这样：`location ~`。

e.g.
```
location ~ \.php$ {
            root           /Users/yangyi/www;
            fastcgi_pass   127.0.0.1:9000;
            fastcgi_index  index.php;
            include        fastcgi.conf;
        }
```
- `\.php$` 是匹配 `.php` 结尾的 URL，用来解析 php 文件。里面的 root 也是一样，用来表示虚拟主机的根目录。
- fast_pass 链接的是 php-fpm 的地址。

location 还有其他用法。

### 1.6. upstram 模块

upstream 模块负债负载均衡模块，通过一个简单的调度算法来实现客户端 IP 到后端服务器的负载均衡。

e.g.
```
upstream iyangyi.com{
    ip_hash;
    server 192.168.12.1:80;
    server 192.168.12.2:80 down;
    server 192.168.12.3:8080  max_fails=3  fail_timeout=20s;
    server 192.168.12.4:8080;
}
```
在上面的例子中，通过 upstream 指令指定了一个负载均衡器的名称 iyangyi.com。这个名称可以任意指定，在后面需要的地方直接调用即可。里面是 ip_hash 这是其中的一种负载均衡调度算法，紧接着就是各种服务器了，用 server 关键字表识，后面接 ip。

Nginx 的负载均衡模块目前支持 4 种调度算法：

- weight 轮询（默认）

  每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某台服务器宕机，故障系统被自动剔除，使用户访问不受影响。weight。指定轮询权值，weight 值越大，分配到的访问机率越高，主要用于后端每个服务器性能不均的情况下。

- ip_hash

  每个请求按访问 IP 的 hash 结果分配，这样来自同一个 IP 的访客固定访问一个后端服务器，有效解决了动态网页存在的 session 共享问题。

- fair

  比上面两个更加智能的负载均衡算法。此种算法可以依据页面大小和加载时间长短智能地进行负载均衡，也就是根据后端服务器的响应时间来分配请求，响应时间短的优先分配。Nginx 本身是不支持 fair 的，如果需要使用这种调度算法，必须下载 Nginx 的 upstream_fair 模块。

- url_hash

  按访问 url 的 hash 结果来分配请求，使每个 url 定向到同一个后端服务器，可以进一步提高后端缓存服务器的效率。Nginx 本身是不支持 url_hash 的，如果需要使用这种调度算法，必须安装 Nginx 的 hash 软件包。

在 HTTP Upstream 模块中，可以通过 server 指令指定后端服务器的 IP 地址和端口，同时还可以设定每个后端服务器在负载均衡调度中的状态。常用的状态有：
- down，表示当前的 server 暂时不参与负载均衡。
- backup，预留的备份机器。当其他所有的非 backup 机器出现故障或者忙的时候，才会请求 backup 机器，因此这台机器的压力最轻。
- max_fails，允许请求失败的次数，默认为 1。当超过最大次数时，返回 proxy_next_upstream 模块定义的错误。
- fail_timeout，在经历了 max_fails 次失败后，暂停服务的时间。max_fails 可以和 fail_timeout 一起使用。

注意 当负载调度算法为 ip_hash 时，后端服务器在负载均衡调度中的状态不能是 weight 和 backup。

## 2. 常用配置

### 2.1. 优化配置

- 增加 worker 进程数和并发连接数

  Nginx 默认 worker 进程数为 1，默认并发连接数为 1024，但这在高并发的场景是明显不够的。
  ```
  worker_processes  4; # worker 进程数，受限于 CPU 核数
  events {
    worker_connections  10240; # 每个进程允许打开的最大连接数，但受限于操作系统的配置，需要同步更改才能起作用
    multi_accept on; # 允许与客户端一次建立多个连接
    use epoll; # 使用 epoll 网络模型
  }
  ```

- 启用后端 Server 的长连接

  Nginx 与客户端浏览器之间默认启用了长连接，但与后端 Server 之间的长连接并没有默认开启。

  ![image](http://img.cdn.firejq.com/jpg/2018/12/20/bf68465c6a0dbb7b11f97514f1296b81.jpg)

- 启用缓存、压缩

  ![image](http://img.cdn.firejq.com/jpg/2018/12/20/cc2ea512c54c23a858d703ac1913d3e9.jpg)

- 减少内核态与用户态的文件拷贝
  ```
  sendfile on;
  ```

- 避免发生零碎的小数据
  ```
  tcp_nopush on; # 启用当数据包达到一定大小后再发送
  tcp_nodelay off; # 关闭随时发送数据包
  ```

- 操作系统优化
  - TCP 协议参数优化 `/etc/sysctl.conf`
    ```
    # 防止一个 socket 在有过多试图连接到达时引起过载
    sysctl -w net.ipv4.tcp_syncookies=1
    # 连接等待队列，默认为 128
    sysctl -w net.core.somaxconn=1024
    # 减少 TIMEWAIT 状态的超时时间，默认为 120
    sysctl -w net.ipv4.tcp_fin_timeout=10
    # 允许复用 TIMEWAIT 连接
    sysctl -w net.ipv4.tcp_tw_reuse=1
    # 回收禁用
    sysctl -w net.ipv4.tcp_tw_recycle=0
    ```
  - 进程限制优化 `/etc/security/limits.conf`
    ```
    hard nofile 204800
    soft nofile 204800
    soft core unlimited
    soft stack 204800
    ```

### 2.2. 安全配置

- 消除目录浏览漏洞

  nginx 默认不允许目录浏览，但最好检查目录浏览的相关配置，确保没有目录浏览漏洞，确保 autoindex 的配置为 off:
  ```
  autoindex off
  ```

- 开启访问日志

  开启日志有助于发生安全事件后回溯分析整个事件的原因及定位攻击者。默认情况下，nginx 已开启访问日志记录，但最好在 nginx 配置文件中确认已开启访问日志：
  ```
  access_log /backup/nginx_logs/access.log combined;
  ```

- 目录安全配置

  如果 Nginx 以 nobody 用户启动，则黑客通过网站漏洞入侵服务器后，将获得 nginx 的 nobody 权限，因此需要确认网站 web 目录和文件的属主与 nginx 启动用户不同，防止网站被黑客恶意篡改和删除。网站 web 目录和文件的属主可以设置为 root 等（非 nginx 启动用户）。

  Web 目录权限应统一设置为 755，web 文件权限应统一设置为 644。只有上传目录等需要可读可写权限的目录可以设置为 777 。但是，黑客可以在 777 权限目录中上传或者写入 web 木马，因此应该严格保证 777 权限的目录没有执行脚本的权限。

  安全配置如下：
  - 对于使用 php 的业务，在 nginx 配置文件中添加如下配置，即可禁止 log 目录执行 php 脚本：
    ```
    location ~* ^/data/cgisvr/log/.*\.(php|php5)$ {
      deny all;
    }
    ```
	- 对于使用非 php 的业务（如 python、二进制 cgi 等），则需禁止外部访问 777 目录，配置用例如下：
    ```
    location ~ ^/log/ {
      deny all;
    }
    ```

- 管理目录安全配置

  对于管理目录，需要做到只允许合法 ip 可以访问，nginx 限制白名单 ip 访问的配置日下：
  ```
  location ~ ^/private/ {
    allow 192.168.1.0/24;
    deny all;
  }
  ```
  管理目录建议启用密码认证，密码认证依靠 Web 应用自身的认证机制。如果 Web 应用无认证机制，可启用 nginx 的密码认证机制，配置如下：
  ```
  location ^~ /soft/ {
    location ~ .*\.(php|php5)?$ {
    fastcgi_pass unix:/tmp/php-cgi.sock; # custom settings
    fastcgi_index index.php;
    include fcgi.conf;
    }
    auth_basic "Authorized users only";
    auth_basic_user_file your_file_path;
  }
  ```
- 删除默认页面和默认目录

  删除 nginx 默认首页 index.html，可以自行创建默认首页代替，且删除首页 index.html 后，新建其他首页内容不允许出现诸如 `Welcome to Nginx` 的首页内容。

  删除默认目录：`/doc` 和 `/images`，删除如下配置信息：
  ```
  location /doc {
    root /usr/share;
    autoindex on;
    allow 127.0.0.1;
    deny all;
  }

  location /images {
    root /usr/share;
    autoindex off;
  }
  ```

- 隐藏 nginx 版本信息

  使得攻击者在信息收集时候无法判断程序版本，增加防御系数：
  ```
  Server_tokens off;
  ```

- 禁用非必要的请求方法（如 TRACE 请求用于网络诊断，会暴露信息）

  只允许 GET、HEAD、POST 请求，其他请求直接返回 444 状态码 （444 是 nginx 定义的响应状态码，会立即断开连接，没有响应正文，TRACE 请求 nginx 内置 405 拒绝）
  ```
  if ($request_method !~ ^(GET|HEAD|POST)$ ) { return 444; }
  ```

- X-powered-by 隐藏信息

  nginx 反向代理配置很多站点，主机信息会被泄露，因此需要隐藏信息，配置在 http 区段：
  ```
  proxy_hide_header X-Powered-By;
  fastcgi_hide_header X-Powered-By;
  ```

### 2.3. 虚拟主机配置

Nginx 虚拟主机分为基于 IP、基于域名、基于端口的虚拟主机。

e.g.
1. 在 server 模块下边再添加两个（自定义多少个）server 模块，如下：
    ```
    #virtualhost_1 blog.firejq.me
    server {
        listen 80;
        server_name blog.firejq.me;

        location / {
            root html/blog;
            index index.html index.php;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        location ~ \.php$ {
            root            html/blog;
            fastcgi_pass    127.0.0.1:9000;
            fastcgi_index   index.php;
        #   fastcgi_param   SCRIPT_FILENAME  /scripts$fastcgi_script_name;
            fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include         fastcgi_params;
        }

    }

    #virtualhost_2 lab.firejq.me
    server {
        listen 80;
        server_name lab.firejq.me;
        location / {
            root html/lab;
            index index.html index.php;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        location ~ \.php$ {
            root            html/lab;
            fastcgi_pass    127.0.0.1:9000;
            fastcgi_index   index.php;
        #   fastcgi_param   SCRIPT_FILENAME  /scripts$fastcgi_script_name;
            fastcgi_param   SCRIPT_FILENAME $document_root$fastcgi_script_name;
            include         fastcgi_params;
        }
    }
    ```

1. 创建虚拟主机目录
    ```
    mkdir /usr/local/nginx/html/blog;
    mkdir /usr/local/nginx/html/lab;
    ```

1. 创建测试页面

    /usr/local/nginx/html/blog/index.php
    ```php
    <?php echo “blog indx”;	?>
    ```
    vim /usr/local/nginx/html/lab/index.php
    ```php
    <?php echo “lab indx”;	?>
    ```

1. 重启服务器
```
killall nginx;
/usr/local/nginx/sbin/nginx
```

1. 测试结果，分别访问 blog.firejq.me 和 lab.firejq.me

P.S.
- 还可以将基于端口的配置信息独立出来成 virtualport.conf，同样放在 extra 目录下，然后在 nginx.conf 配置文件中添加`include extra/virtualport.conf`，但容易出现地址的相对路径错误的情况，可用绝对路径。
- 若打开网址出现下载框提示下载，这是虚拟主机无法解析 `.php` 文件造成的，见于 `.html` 可以正常显示在浏览器中但 `.php` 文件会出现下载框提示下载。解决方法：查看 nginx.conf 文件中虚拟主机的 server 模块是否含有 `location /.php$` 的子模块，若没有，按上边的格式添加，重启 nginx 即可解决。

## 3. Refer Links

https://www.zybuluo.com/phper/note/89391

https://www.zybuluo.com/phper/note/90310

https://www.zybuluo.com/phper/note/133244

http://seanlook.com/2015/05/17/nginx-install-and-config/
