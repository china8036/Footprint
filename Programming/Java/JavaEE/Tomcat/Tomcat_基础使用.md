- [Tomcat Base Note](#tomcat-base-note)
  - [1. Tomcat 概述](#1-tomcat-%E6%A6%82%E8%BF%B0)
  - [2. Tomcat & Apache](#2-tomcat-apache)
  - [3. tomcat & JSP & Servlet](#3-tomcat-jsp-servlet)
  - [4. Tomcat 容器等级](#4-tomcat-%E5%AE%B9%E5%99%A8%E7%AD%89%E7%BA%A7)
  - [5. tomcat 安装](#5-tomcat-%E5%AE%89%E8%A3%85)
    - [5.1. windows 安装 Tomcat](#51-windows-%E5%AE%89%E8%A3%85-tomcat)
    - [5.2. Ubuntu安装 Tomcat](#52-ubuntu%E5%AE%89%E8%A3%85-tomcat)
    - [5.3. 常见问题](#53-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
      - [5.3.1. JDK 环境变量的设置问题导致 tomcat 无法启动](#531-jdk-%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F%E7%9A%84%E8%AE%BE%E7%BD%AE%E9%97%AE%E9%A2%98%E5%AF%BC%E8%87%B4-tomcat-%E6%97%A0%E6%B3%95%E5%90%AF%E5%8A%A8)
        - [5.3.1.1. 在 windows 下配置 Tomcat](#5311-%E5%9C%A8-windows-%E4%B8%8B%E9%85%8D%E7%BD%AE-tomcat)
        - [5.3.1.2. ubuntu](#5312-ubuntu)
      - [5.3.2. 腾讯云 ubuntu 中 tomcat 启动缓慢](#532-%E8%85%BE%E8%AE%AF%E4%BA%91-ubuntu-%E4%B8%AD-tomcat-%E5%90%AF%E5%8A%A8%E7%BC%93%E6%85%A2)
  - [6. conf/server.xml 配置](#6-confserverxml-%E9%85%8D%E7%BD%AE)
    - [6.1. 修改服务器监听端口](#61-%E4%BF%AE%E6%94%B9%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%9B%91%E5%90%AC%E7%AB%AF%E5%8F%A3)
    - [6.2. 修改 webapp 路径](#62-%E4%BF%AE%E6%94%B9-webapp-%E8%B7%AF%E5%BE%84)
  - [7. 使用非 root 用户启动 tomcat](#7-%E4%BD%BF%E7%94%A8%E9%9D%9E-root-%E7%94%A8%E6%88%B7%E5%90%AF%E5%8A%A8-tomcat)
  - [8. Eclipse 中整合 Tomcat 常见问题](#8-eclipse-%E4%B8%AD%E6%95%B4%E5%90%88-tomcat-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
    - [8.1. Eclipse 中 tomcat 启动后](#81-eclipse-%E4%B8%AD-tomcat-%E5%90%AF%E5%8A%A8%E5%90%8E)
    - [8.2. 使用 Eclipse 部署 web 项目与手工部署的区别](#82-%E4%BD%BF%E7%94%A8-eclipse-%E9%83%A8%E7%BD%B2-web-%E9%A1%B9%E7%9B%AE%E4%B8%8E%E6%89%8B%E5%B7%A5%E9%83%A8%E7%BD%B2%E7%9A%84%E5%8C%BA%E5%88%AB)
    - [8.3. 手动删除 webapps 下的的项目后，tomcat 无法启动](#83-%E6%89%8B%E5%8A%A8%E5%88%A0%E9%99%A4-webapps-%E4%B8%8B%E7%9A%84%E7%9A%84%E9%A1%B9%E7%9B%AE%E5%90%8E%EF%BC%8Ctomcat-%E6%97%A0%E6%B3%95%E5%90%AF%E5%8A%A8)
  - [9. Tomcat 目录结构](#9-tomcat-%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84)
  - [10. Tomcat 中各端口作用](#10-tomcat-%E4%B8%AD%E5%90%84%E7%AB%AF%E5%8F%A3%E4%BD%9C%E7%94%A8)
  - [11. Tomcat 安全配置和性能优化](#11-tomcat-%E5%AE%89%E5%85%A8%E9%85%8D%E7%BD%AE%E5%92%8C%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96)
  - [12. Tomcat 配置 Https](#12-tomcat-%E9%85%8D%E7%BD%AE-https)
    - [12.1. 使用本地证书](#121-%E4%BD%BF%E7%94%A8%E6%9C%AC%E5%9C%B0%E8%AF%81%E4%B9%A6)
    - [12.2. 使用授权证书](#122-%E4%BD%BF%E7%94%A8%E6%8E%88%E6%9D%83%E8%AF%81%E4%B9%A6)

# Tomcat Base Note

## 1. Tomcat 概述

> The Apache Tomcat® software is an open source implementation of the Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies. The Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket specifications are developed under the Java Community Process.

Tomcat 是由 Apache 软件基金会下属的 Jakarta 项目开发的一个 Servlet 容器，按照 Sun Microsystems 提供的技术规范，实现了对 Servlet 和 JavaServer Page（JSP）的支持，并提供了作为 Web 服务器的一些特有功能，如 Tomcat 管理和控制平台、安全域管理和 Tomcat 阀等。由于 Tomcat 本身也内含了一个 HTTP 服务器，它也可以被视作一个单独的 Web 服务器。

为解析 JSP，Tomcat 提供了一个 Jasper 编译器用以将 JSP 编译成对应的 Servlet，从而在 Servlet 容器中运行。

## 2. Tomcat & Apache

http://developer.51cto.com/art/201007/210894.htm

Apache 原是指 Apache 软件公司，所谓 Apache Tomcat 是指 Apache 软件公司下的开源软件项目 Tomcat；但习惯上将 Apache 公司下的服务器软件项目 apache web server 直接称为 apache，因此下文中的 apache 一律指 apache web server。

Apache 是专门用了提供 HTTP 服务的，以及相关配置的（例如虚拟主机、URL 转发等等），而 Tomcat 是 Apache 组织在符合 Java EE 的 JSP、Servlet 标准下开发的一个 JSP/Servlet 应用服务器。

相同点：
- 两者都是 Apache 组织开发的；
- 两者都有 HTTP 服务的功能；
- 两者都是免费的；

不同点：
- Tomcat 是应用（Java）服务器，它只是一个 Servlet(JSP 也翻译成 Servlet) 容器，可以认为是 Apache 的扩展，但是可以独立于 Apache 运行（Tomcat 也可独立处理静态的 HTTP 请求）；

- Apache 侧重于 HTTP Server，即解析静态 HTML 网页；Tomcat 侧重于 Servlet 引擎，如果以 Standalone 方式运行，功能上与 Apache 等效， 支持 JSP，但对静态网页不太理想；

- Apache 搭配 PHP，只需安装 PHP 插件即可建立交互联系，而 Apache 搭配 Java，需要 Tomcat 作为应用程序服务器 / 容器（Apache 与 Tomcat 交互联系，Java 编写的 servlet 运行在 Tomcat 中）。

两者搭配：
- Apache 支持静态网页（Html），Tomcat 支持静态 + 动态（如。php, .jsp, .asp 页面），要在 Apache 环境下运行 JSP 的话就需要一个解释器来执行 JSP 网页，而这个 JSP 解释器就是 Tomcat, 若使用 Apache+Tomcat 组合解析 JSP 页面的请求时，Apache 只是作为一个转发，对 JSP 的处理是由 Tomcat 来处理的。

- 一般会将来自客户端浏览器的 http 请求交给 web server（如 apache/nginx），若是静态请求，直接由其返回静态页面作为响应；若是动态交互请求，则 web server 需要将请求转交给服务器端的 CGI 程序（如 php，python，perl 等）或 servlet 程序（如 jsp），程序处理完之后再将结果以静态页面的形式返回给 web server，从而由 web server 返回给客户端响应。

搭配使用的好处：
- 如果客户端请求的是静态页面，则只需要 Apache 服务器响应请求；如果客户端请求动态页面，则是 Tomcat 服务器响应请求。因为 JSP 是服务器端解释代码的，这样整合就可以减少 Tomcat 用于处理客户端请求的服务开销。

## 3. tomcat & JSP & Servlet

- servlet 是处理 http request 的应用程序；

- tomcat 是 servlet 的运行环境 / 容器；

- jsp 可以理解成 servlet+html；

## 4. Tomcat 容器等级

Tomcat 容器分为四个等级：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/f11d78821e667a6a5385877641b29c11.jpg) 

其中，Servlet 容器管理 Context 容器，一个 Context 对应一个 Web 工程。

## 5. tomcat 安装

### 5.1. windows 安装 Tomcat

1.	安装 JDK；
1.	安装 tomcat9：
    
    官网下载 tomcat 的 zip 包和源码包：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/16c0a3f83efc6310a17bf95c85bd5a8c.jpg)

    解压后即可完成安装；
    
1. 启动：由 tomcat 目录下 /bin/startuop.bat 和 /bin/shutdown.bat 负责服务器的启动和关闭；

1. 测试：访问 localhost:8080，出现下图表示 tomcat 启动成功：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/34d519576be07441c70ff84a6ef847b2.jpg)

### 5.2. Ubuntu安装 Tomcat

http://zyjustin9.iteye.com/blog/2177291

http://blog.topspeedsnail.com/archives/4551

1. 安装 JDK：
1. 安装 tomcat9：

    到 tomcat 官网下载 tar.gz 压缩文件，解压到 /opt/tomcat9/；

    修改默认端口为 80：`vim /opt/tomcat9/conf/server.xml`；

    将启动命令添加到 init.d 中：
    ```bash
    vim /etc/init.d/tomcat9；
    ```
    ```shell
    #!/bin/sh
    #tomcat auto-start

    case $1 in
    start)
    sh /opt/tomcat9/bin/startup.sh
    ;;
    stop)
    sh /opt/tomcat9/bin/shutdown.sh
    ;;
    restart)
    sh /opt/tomcat9/bin/shutdown.sh
    sh /opt/tomcat9/bin/startup.sh
    ;;
    *)
    echo 'Usage:tomcat7 start|stop|restart'
    ;;
    esac
    exit 0
    ```
    测试：（若使用 iptables，需要开放 80 端口)
    ```bash
    /etc/init.d/tomcat9 start；
    ```
    在浏览器访问服务器地址；

### 5.3. 常见问题

#### 5.3.1. JDK 环境变量的设置问题导致 tomcat 无法启动

##### 5.3.1.1. 在 windows 下配置 Tomcat

P.S. 若在安装 jdk 时已经配置好了 jdk 的环境变量：
- JAVA_HOME = D:\JAVA\jdk1.6.0
- JRE_HOME = D:\JAVA\jdk1.6.0\jre
- path = ********;%JAVA_HOME%\bin
则以下操作不必进行；

在 bin 目录下编辑 start.bat 和 shutdown.bat 文件，分别在最前边加入：
```
set JAVA_HOME=C:\java\jdk1.8.0_102
set JRE_HOME=C:\java\jdk1.8.0_102\jre
```
之后运行 start.bat 即可启动 tomcat 服务器，运行 shutdown.bat 即可关闭 tomcat 服务器，默认采用 8080 端口（http://localhost:8080 访问）（可在 /conf/server.xml 中修改默认端口）。

##### 5.3.1.2. ubuntu

```bash
vim bin/catalina.sh
```
追加以下 2 行
```shell
export JAVA_HOME=/home/weblogic/jdk1.7.0_72
export JRE_HOME=/home/weblogic/jdk1.7.0_72/jre
```

```bash
vim setclasspath.sh 
```
同样追加以下 2 行
```shell
export JAVA_HOME=/home/weblogic/jdk1.7.0_72
export JRE_HOME=/home/weblogic/jdk1.7.0_72/jre
```

#### 5.3.2. 腾讯云 ubuntu 中 tomcat 启动缓慢

Tomcat 从 7 版本开始，非常依赖于 SecureRandom 这个类去生成随机的串用作 seesion ids 等，而 SecureRandom 依赖于熵（entropy）的输入。如果缺少熵，那么就会造成随机数的生产非常缓慢，导致 tomcat 启动变慢。所以解决方案就是指定一个非空的熵（entropy）给 SeureRandom。

P.S. tomcat 没有完全启动之前，运行 shutdown.sh 还会报错。

解决：
- 方案一：
  
  `/bin/catalina.sh` 中追加：
  ```shell  
  JAVA_OPTS="$JAVA_OPTS -Djava.security.egd=file:/dev/./urandom"
  ```

- 方案二：（**亲测有效**）
  ```bash
  apt install haveged
  ```

## 6. conf/server.xml 配置

### 6.1. 修改服务器监听端口

在 /conf/server.xml 中：

例：将 8080 端口的代码注释，改为 80 端口

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/4330183493b1f243d9cc854c610a8c14.jpg)

保存后重新启动即可。

### 6.2. 修改 webapp 路径

conf/server.xml 中，修改 Host 标签的 appBase 属性：
```xml
<Host name="localhost"  appBase="xxxx" unpackWARs="true" autoDeploy="true">
```

## 7. 使用非 root 用户启动 tomcat

使用 startup.sh 启动 tomcat 虽然已经可以正常启动 Tomcat 服务器，但是是以系统 root 账户来启动的，这是一种很不安全的行为。

Java 程序与 C 程序不同。nginx,httpd 使用 root 用户启动守护 80 端口，子进程 / 线程会通过 setuid(),setgid() 两个函数切换到普通用户。即父进程所有者是 root 用户，子进程与多线程所有者是一个非 root 用户，这个用户没有 shell，无法通过 ssh 与控制台登陆系统。但对于 Java 而言，由于 Java 的 JVM 是与系统无关的，是建立在 OS 之上的，因此**你使用什么用户启动 Tomcat，那么 Tomcat 就会继承该所有者的权限**。

**Linux 系统小于 1024 的端口只有 root 可以使用，这也是为什么 Tomcat 默认端口是 8080。如果你想使用 80 端口只能使用 root 启动 Tomcat。这有带来了很多安全问题。**

官方推荐的解决方法是**使用 root 权限执行 Tomcat 自带的脚本 daemon.sh，该脚本使用 jsvc 以普通用户权限去启动 Tomcat**，原理是 root 用户 fork 非 root 用户，同时可以监听 80 端口。

1.	编译获取 jsvc

    进入 Tomcat 安装目录的 bin 文件夹找到 commons-daemon-native.tar.gz
    ```bash
    tar -zvxf commons-daemon-native.tar.gz
    cd commons-daemon-1.0.15-native-src/unix/
    ./configure
    make
    ```
    执行成功后会在当前目录生成一个 jsvc 的目录，将其复制到 tomcat 的 bin 目录下：
    ```bash
    cp jsvc ../../
    ```

2.	设置非 ROOT 启动

    为 Tomcat 添加一个指定的非 ROOT 用户

    ```bash
    useradd tomcat -M -d / -s /usr/sbin/nologin
    # -M 不创建用户主目录  -d 指定用户主目录  -s 指定用户 shell
    # /sbin/nologin , 意味着该用户不能登录，同时也没有给它指定密码，因此这个用户只能用于启动 tomcat，没有 Shell 权限，即便服务器被注入也无法运行 linux 命令
    ```

    设置 tomcat 安装目录下所有文件的所有者和所属组为 tomcat（由于 tomcat 是由 root 解压的，所有文件所属权为 root，如果不进行移权其他用户将无法启动 tomcat 服务）：
    ```bash
    chown -hR tomcat:tomcat /usr/tomcat9
    ```

    修改 bin/daemon.sh：

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/95d3e7ca09565a476bf15d1d7db3c43c.jpg)

3.	
    - 启动服务器：
      ```bash
      ./daemon.sh start
      ```
    - 关闭服务器：
      ```bash
      ./daemon.sh stop
      ```

4.	在 /etc/init.d/ 中建立 daemon.sh 软连接，方便操作（若在之前的配置中已在该目录下创建了 tomcat9 文件，则先把源文件删除）
    ```bash
    ln -s /opt/tomcat9/bin/daemon.sh /etc/init.d/tomcat9
    ```
    启动服务器
    ```bash
    /etc/init.d/tomcat9 start
    ```
    关闭服务器：
    ```bash
    /etc/init.d/tomcat9 stop
    ```
    查看 tomcat 版本：
    ```bash
    /etc/init.d/tomcat9 version
    ```

5.	开机自启动
    ```bash
    vim /etc/rc.local
    ```
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/368ff105fcd8a89ccc3d34af8a95bc30.jpg)

## 8. Eclipse 中整合 Tomcat 常见问题

### 8.1. Eclipse 中 tomcat 启动后

http://blog.csdn.net/wqjsir/article/details/7169838/

症状：

tomcat 在 eclipse 里面能正常启动，而在浏览器中访问 http://localhost:8080/ 不能访问，且报 404 错误。同时其他项目页面也不能访问。

关闭 eclipse 里面的 tomcat，在 tomcat 安装目录下双击 startup.bat 手动启动 tomcat 服务器。访问 htt://localhost:8080/ 能正常访问 tomcat 管理页面。

在 Eclipse 的 workspace 中能找到 web 项目的源代码，但在 tomcat 的 webapps 中找不到；

症状原因：

eclipse 将 tomcat 的项目发布目录（tomcat 目录中的 webapp）重定向了，所以你会发现在 tomcat 安装目录下的 webapp 目录里面找不到你的项目文件。

解决办法：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/e60f318bc7ac78e1c3f1db807fb29c09.jpg)

在 eclipse 中的 server 页面，双击 tomcat 服务，会看到如图所示的配置页面：
可以看到红圈中选择的是 Use workspace metadata(does not modify Tomcat installion)
如果该 tomcat 中部署了项目的话，这红圈中的选项会灰掉不能修改，要修改必须得先把 tomcat 中的部署的服务都移除。
如图：
 
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/00cbfee6e27c147b2bf8bf126ef59ce5.jpg)

通过右键单击 tomcat 服务器选择 Add and Remove，在弹出的对话框中移除已部署的项目。移除完确定后，将看到上面的选项面板部分可编辑了（可能需要 clean 一下）；

选择 Use tomcat installation(Task control of Tomcat installation) 即选择 tomcat 的安装目录来作为项目的发布目录；

然后，下来四行，看到"Deploy Path"了没？这是项目部署路径，即 web 项目要发布到哪里，它后面的值默认是"wtpwebapps", 把它改成"webapps", 也就是 tomcat 中发布项目所在的文件夹名字；

修改后关掉该页面，保存配置。这样就将项目部署到了 tomcat 安装目录下的 webapp；

重启 tomcat 服务器，访问 http://localhost:8080 则能正常访问了，自己部署的项目也能正常访问了；

这样，即在 Eclipse 的 workspace 中能找到 web 项目的源码，也能在 tomcat 的 webapp 路径下找到 web 项目源码（两者区别在于：workspace 中包含了 src 即 java 源代码，而 webapp 下的是 java 源码编译后的 class 字节码文件，放于 WEB-INF/classes 中）。

### 8.2. 使用 Eclipse 部署 web 项目与手工部署的区别

- 使用 IDE：
  
  若只部署到 workspace 中，即使用选项
 
  此时 Eclipse 将 tomcat 服务器的 webapp 目录重定向到了 workspace 中该 web 项目路径下的 webcontent/webRoot 目录，即：tomcat 服务器只能访问到 Eclipse 的 workspace 中的资源文件，访问不到 tomcat 根目录下的资源文件；
  
  此情况下，IDE 将 src 中的 java 源文件编译成 class 文件之后，将 class 文件放到 webRoot/WEB-INF/classes 中，这一过程由 ide 自动完成；
  
  若同时部署到 workspace 中和 tomcat/webapp 中，即使用选项；
    
  此时 tomcat 能正常访问到 tomcat/webapp 下的 web 项目，Eclipse 在 workspace 和 webapp 下同时存放了该 web 项目，不同的是，在 workspace 中既有 src 源文件又有项目根目录，而在 tomcat/webapp 下只有项目根目录，src 中的源文件以编译后的 class 字节码文件形式存放在 WEB-IMF/classes 中；

- 手工部署：
  
  类比 Eclipse 的部署方式，手工部署一般有三种方式：
  
  - 将 java 源文件编译成 class 文件存放在 WEB-IMF/classes 中；
  
  - 将 java 源文件打包成 jar 包存放在 WEB-INF/lib 中；
  
  - 在系统的 CLASSPATH 变量中加入 class 文件的地址，这种方法不利于管理和移植，一般不采用；

### 8.3. 手动删除 webapps 下的的项目后，tomcat 无法启动

解决方法：进入 TOMCAT_HOME\conf\Catalina\localhost 下有一个"项目目录。xml"的配置文件，直接删除，重启 MyEclipse，一切 OK。

原因：手动删除项目后，该项目的配置文件并没有被删除，存在于 conf\Catalina\localhost 下，TOMCAT 启动时根据配置文件加载项目由于对应的项目物理上已经不存在了，因此会抛出一大堆异常。同**卸载应用程序时直接手动删除应用程序目录一样的道理**。

在 MyEclipse 下部署的 web 项目到 tomcat 下“不会”修改 conf/server.xml 文件。虽然目前还不明白原因。但是 eclipse 部署时候就“会”修改，server.xml 会在文件末尾部分的<Host>...</Host>中多出来一个<Content..../>标签。我的内容如下：

```xml
<Context docBase="StrutsDevTemplate" 

path="/StrutsDevTemplate"

reloadable="true" 

source="org.eclipse.jst.jee.server:StrutsDevTemplate"/>
```

我在 eclipse 下建立了两个，结果就多出来两个。在删除了 webapps 和 work 下相应的文件，再把 server.xml 中相应的<Content>删除了之后，重启 Tomcat 就不会再报错了。

为了让 tomcat 只运行 conf/server.xml 中指定的 web 应用，可以有以下几种办法：
- 实现一：
  1. 将要部署的 WEB 应用放在 webapps 以外的路径，并在 server.xml 相应的 context 中的 docBase 指定。
    
  1. 删除 webapps 中的所有文件夹，以及 conf/catalina/localhost 下所有 xml 文件。
        注：webapps 是 server.xml 中的 Host 元素的 appBase 属性的值。
- 实现二：
  
  1. 修改 server.xml 中 Host 元素的属性，添加或修改：
      ```xml
      deployXML="false" deployOnStartup="false" autoDeploy="false"
      ```
  1. 含义：
      ``` 
      deployXML="false": 不部署 conf/catalina/localhost 下的 xml 相应的 WEB 应用     
      deployOnStartup="false" : tomcat 启动时，不部署 webapps 下的所有 web 应用     
      autoDeploy="false": 避免 tomcat 在扫描改动时，再次把 webapps 下的 web 应用给部署进来
      ```

## 9. Tomcat 目录结构

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/5800f686e375b9981d1d9ca90c73359e.jpg)

## 10. Tomcat 中各端口作用

查看 conf/server.xml：
```xml
<Server port="8005" shutdown="SHUTDOWN"> 
<Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  redirectPort="8443" URIEncoding="UTF-8"/>        
<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />   
```

解读：

Tomcat 服务器通过 Connector 连接器组件与客户程序建立连接，Connector 组件负责接收客户的请求，以及把 Tomcat 服务器的响应结果发送给客户。默认情况下，Tomcat 在 server.xml 中配置了两种连接器：
- 第一个连接器监听 8080 端口，负责建立 HTTP 连接。在通过浏览器访问 Tomcat 服务器的 Web 应用时，使用的就是这个连接器；
- 第二个连接器监听 8009 端口，负责和其他的 HTTP 服务器建立连接。在把 Tomcat 与其他 HTTP 服务器（如 Nginx、Apache）集成时，就需要用到这个连接器；

Tomcat 默认端口：
- 8005：远程停用服务端口；
- 8080：HTTP 端口；
- 8443：HTTPS 端口；
- 8009：AJP 端口  APACHE 能过 AJP 协议访问 TOMCAT 的 8009 端口；

## 11. Tomcat 安全配置和性能优化

[Tomcat 安全配置与性能优化](https://netkiller.github.io/journal/tomcat.html)

- 删除 /usr/local/tomcat/webapps 下的所有预设目录；
  
  还有涉及管理页面的 2 个配置文件 host-manager.xml 和 manager.xml 也需要一并删掉。这两个文件存放在 Tomcat 安装目录下的 conf/Catalina/localhost 目录下。

- 删除 jspx 文件的解析
  
  默认的 Tomcat 是可以解析 jspx 文件的，如果项目中不需要支持 jspx 文件，可以删除对 jspx 文件的解析功能，这个可能会带来安全风险；
  
  conf/web.xml:
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/0d95a1616098d3487a10341d1438b167.jpg)

- 统一错误页面，禁止显示服务器的错误信息
  
  conf/web.xml：
  ```xml
  <error-page>
      <error-code>404</error-code>
      <location>/404.jsp</location>
  </error-page>
  ```

- JVM 优化：使用 Server JRE 替代 JDK
  
  服务器上不安装 JDK，只安装 JRE，而且是 Server JRE；
  
  理由：
  - 在 Oracle 官网上可以下载两种类型的 JRE，一种是针对服务器使用优化的 Server JRE，一种是针对客户端使用优化的 JRE；
  - 服务器上根本不需要编译器，代码应该在 Release 服务器上完成编译打包工作；
  - 一旦服务器被控制，可以防止在其服务器上编译其他恶意代码并植入到你的程序中；

- maxThreads 连接数限制
  
  maxThreads 是 Tomcat 所能接受最大连接数。一般设置不要超过 8000 以上，如果你的网站访问量非常大可能使用运行多个 Tomcat 实例的方法，即在一个服务器上启动多个 tomcat 然后做负载均衡处理。

  conf/server.xml:
  ```xml
  <Connector port="8080" address="localhost"
    maxThreads="2048" maxHttpHeaderSize="8192"
    emptySessionPath="true" protocol="HTTP/1.1"
    enableLookups="false" redirectPort="8181" acceptCount="100"
    connectionTimeout="20000" disableUploadTimeout="true" />
  ```
  参数：
  - maxThreads：客户请求最大线程数
  - minSpareThreads：初始化时创建的 socket 线程数
  - maxSpareThreads：连接器的最大空闲 socket 线程数

- 不使用 Tomcat 虚拟主机
  
  不要使用 Tomcat 的虚拟主机，每个站点一个实例。若要在服务器上部署多个网站，则启动多个 tomcat 实例。

  这也是 PHP 运维在这里常犯的错误，PHP 的做法是一个 Web 下面放置多个虚拟主机，而不是每个主机启动一个 web 服务器。Tomcat 是多线程，共享内存，任何一个虚拟主机中的应用出现崩溃，会影响到所有应用程序。采用多个实例方式虽然开销比较大，但保证了应用程序隔离与安全。

- 启用压缩传输
  
  通常使用 gzip 压缩。
  
  conf/server.xml：
  ```xml
  <Connector port="8080" protocol="HTTP/1.1"
                connectionTimeout="20000"
                redirectPort="8443"
                compression="on"
                compressionMinSize="2048"
                noCompressionUserAgents="gozilla, traviata"
                compressableMimeType="text/html,text/xml,text/javascript,text/css,text/plain,,application/octet-stream"/>
  ```
  参数：
  - compression：打开压缩功能   
  - compressionMinSize：启用压缩的输出内容大小，这里面默认为 2KB
  - compressableMimeType：压缩类型	
  
  注意：**压缩会增加 Tomcat 负担，最好采用 Nginx + Tomcat 或者 Apache + Tomcat 方式，压缩交由 Nginx/Apache 去做**。

- 关闭 8005 端口
  
  Tomcat 提供了 8005 端口实现远程关闭服务器，`telnet localhost 8005` 然后输入 `SHUTDOWN` 就可以关闭 Tomcat，对于生产环境中的服务器是不安全的，因此，应关闭该端口的功能：
  
  conf/server/xml：将 8005 改为 -1（不存在 -1 端口）
  
  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/067d00720c8e7bd226a0862f69fbde32.jpg)

- 管理 8009 端口

  8009 端口为 AJP 使用，AJP 是为 Tomcat 与 HTTP 服务器之间通信而定制的协议，能提供较高的通信速度和效率。如果 tomcat 前端放的是 apache 的时候，会使用到 AJP 这个连接器。若网站前端不用 apache，则无需用到此连接器，因此可以注销掉该连接器。

  conf/server.xml：将以下代码注释
  ```xml
  <!--
      <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
  -->
  ```

- 隐藏 HTTP 头部的服务器信息
  
  conf/server.xml：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/dfb0aefa925b36d4a5d28879a5db6e8d.jpg)
  
  效果：

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/d0736e7e3068603e56538bc75d905562.jpg)

- 关闭 war 自动部署
  
  防止被植入木马等恶意程序，应关闭 war 包的自动部署和加载；
  
  conf/server.xml：
  ```xml
  <Host name="localhost"  appBase="webapps" unpackWARs="false" autoDeploy="false ">
  ```

- 修改 session 的 cookie 变量
  
  修改 Cookie 变量 JSESSIONID， 这个 cookie 是用于维持 Session 关系。可以修改为 PHPSESSID，用以伪装后台语言。
  
  conf/server.xml：
  ```xml
  <Context path="" docBase="path/to/your" reloadable="false" sessionCookiePath="/" sessionCookieName="PHPSESSID">	
  ```

## 12. Tomcat 配置 Https

官方教程：https://tomcat.apache.org/tomcat-9.0-doc/ssl-howto.html 

要使用 https，首先需要 SSL 证书，SSL 证书可以通过两个渠道获得：
- 公开可信认证机构：例如 CA, 但是申请一般是收费的，一般几百到几千一年。
- 自己生成的本地证书：虽然安全性不是那么高，但胜在成本低。
### 12.1. 使用本地证书

1. 使用 jdk 生成证书：
   
    ```bash
    keytool -genkey -v -alias tomcatKey -keyalg RSA -validity 3650
    ```
    注意：
    - 在 linux 中会在当前路径下生成证书，windows 中会在~路径下生成证书；
    - 要想指定一个不同的位置或文件名，可以在上述的 keytool 命令上添加 -keystore 参数，后跟到达 keystore 文件的完整路径名。
    <!-- - 口令统一设定为 firejq； -->

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/0db1b7e771d080e536619f305e32ec36.jpg)

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/e14ce27b1bdd91f52896edceb6554995.jpg)

1. 配置 server.xml
    
    在 server.xml 中加入：
    ```xml
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
                  maxThreads="150" SSLEnabled="true" scheme="https" secure="true" keystoreFile="证书路径" keystorePass="口令" clientAuth="false" sslProtocol="TLS"/>
    ```
    参数说明：
    - port: https 的端口，默认 8443
    - clientAuth: 设置是否双向验证，默认为 false，设置为 true 代表双向验证 keystoreFile
    - keystoreFile: keystore 证书的路径
    - keystorePass: 生成 keystore 时的口令

    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/1/24/021dd2e4b7ed1841a4c6489f87a11573.jpg)

    如果要隐藏端口号，就要把 Tomcat 的 HTTPS 端口设为 443。

1. 测试：访问 https://ip:8443

1. 配置所有 HTTP 请求都使用 HTTPS 协议
  
    在 web.xml 文件中，增加配置如下：
    ```xml
    <login-config>  
        <!-- Authorization setting for SSL -->  
        <auth-method>CLIENT-CERT</auth-method>  
        <realm-name>Client Cert Users-only Area</realm-name>  
    </login-config>  
    <security-constraint>
      <!-- Authorization setting for SSL -->
        <web-resource-collection>
            <web-resource-name>SSL</web-resource-name>
            <url-pattern>/*</url-pattern>
        </web-resource-collection>
        <user-data-constraint>
            <transport-guarantee>CONFIDENTIAL</transport-guarantee>
        </user-data-constraint>
    </security-constraint>
    ```
    将 URL 映射设为 /* ，这样你的整个应用都要求是 HTTPS 访问，而 transport-guarantee 标签设置为 CONFIDENTIAL 以便使应用支持 SSL。
    如果你希望关闭 SSL ，只需要将 CONFIDENTIAL 改为 NONE 即可。

### 12.2. 使用授权证书

免费 HTTPS 证书 Let's Encrypt 安装教程：https://foofish.net/https-free-for-lets-encrypt.html 

Tomcat 部署 Let’s Encrypt 免费 SSL 证书 && 自动续期：https://my.oschina.net/chaon/blog/717902 

授权的 SSL 证书需要向国际公认的证书证书认证机构（简称 CA，Certificate Authority）申请。CA 机构颁发的证书有 3 种类型：
- 域名型 SSL 证书（DV SSL）：信任等级普通，只需验证网站的真实性便可颁发证书保护网站；  
- 企业型 SSL 证书（OV SSL）：信任等级强，须要验证企业的身份，审核严格，安全性更高；
- 增强型 SSL 证书（EV SSL）：信任等级最高，一般用于银行证券等金融机构，审核严格，安全性最高，同时可以激活绿色网址栏。

如果是个人博客、中小企业网站和一些传统行业的形象展示类网站，一般来说只需要申请一个域名型 SSL 证书（DV SSL）就足够了，一方面这类网站确实没有值得加密的信息，另一方面 DV SSL 证书申请流程简洁，费用低，甚至有一些可以免费申请。

目前证书有以下几种常用文件格式：JKS(.keystore)，微软 (.pfx)，PEM(.key + .crt)。其中，tomcat 使用 JKS 格式，nginx 使用 PEM 格式。
1. 准备好 JKS 格式的证书
1. 配置 conf/server.xml 文件：
    ```xml
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
              maxThreads="150" SSLEnabled="true">
        <SSLHostConfig>
            <Certificate certificateKeystoreFile="/conf/ims.cn1.jks"
                        certificateKeyAlias="1"
                  certificateKeystorePassword="密码"
                        type="RSA" />
        </SSLHostConfig>
    </Connector>
    ```
    参数说明：
    - certificateKeystorePassword 为 jks 密码；
    - certificateKeyAlias 为 jks 别名，没有特殊情况的别名就是 1。
