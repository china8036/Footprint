- [Java 网络编程：代理](#java)
  - [1. Proxy](#1-proxy)
  - [2. ProxySelector](#2-proxyselector)
  - [3. Refer Links](#3-refer-links)

# Java 网络编程：代理

从 JDK5 开始，Java 在 java.net 包中提供了用于代理服务器的操作接口：Proxy 和 ProxySelector 类。

## 1. Proxy

[Proxy](https://docs.oracle.com/javase/9/docs/api/java/net/Proxy.html) 类代表一个代理服务器，可以在创建 Socket 连接或打开 URLConnection 连接时指定 Proxy。

Proxy 类提供了一个构造器：
- `Proxy​(Proxy.Type type, SocketAddress sa)`: 用于创建一个表示代理服务器的 Proxy 对象。其中：
  - type 参数表示该代理服务器的代理类型：
    - Proxy.Type.DIRECT: 表示直接连接，不使用代理。
    - Proxy.Type.HTTP: 表示支持高级协议代理，如 HTTP、FTP 等。
    - Proxy.Type.SOCKS: 表示 SOCKS（v4 或 v5）代理。
  - sa 参数表示指定代理服务器的 IP 地址和端口。

创建 Proxy 对象后，程序就可以在使用 URLConnection 打开连接或创建 Socket 对象时，传入该 Proxy 对象，作为此次连接所使用的代理服务器：
- `URLConnection openConnection(Proxy proxy)`
- `Socket(Proxy proxy)`

## 2. ProxySelector

[ProxySelector](https://docs.oracle.com/javase/9/docs/api/java/net/ProxySelector.html) 类代表一个代理选择器，它提供了对代理服务器更加灵活的控制，可以对 HTTP、HTTPS、SOCKS 等协议分别进行代理设置，且支持设置直连的主机和地址。

ProxySelector 本身是一个抽象类，程序无法创建它的实例，因此，开发者应继承 ProxySelector，并实现以下方法，来创建一个自定义的代理选择器：
- `abstract List<Proxy>	select​(URI uri)`: 根据业务需要返回代理服务器列表，如果该方法返回的集合中只包含了一个 Proxy，则该 Proxy 会作为默认的代理服务器。
- `abstract void	connectFailed​(URI uri, SocketAddress sa, IOException ioe)`: 连接代理服务器失败时会回调该方法。

实现了自定义的代理选择器后，可调用 ProxySelector 的静态方法注册该代理选择器：
- `static void	setDefault​(ProxySelector ps)`: Sets (or unsets) the system-wide proxy selector.

实际上，JDK 为 ProxySelector 提供了一个默认实现类：sun.net.api.Default​ProxySelector，并默认已注册。但由于该类属于非公开 API，因此应通过 ProxySelector 的静态方法来获取 Default​ProxySelector 的实例对象：
- `static ProxySelector	getDefault​()`: Gets the system-wide proxy selector.
Default​ProxySelector 的实现策略如下：
- `select`: Default​ProxySelector 会根据系统属性来决定使用哪个代理服务器，即检测系统属性与 URL 之间的匹配，再决定使用相应的属性值作为代理服务器。常用的代理服务器的属性名如下：
  - http.proxyHost: 设置 HTTP 访问所使用的代理服务器的主机地址。该属性名的前缀可以更改为 https、ftp 等。
  - http.proxyPort: 设置 HTTP 访问所使用的代理服务器的主机端口。该属性名的前缀可以更改为 https、ftp 等。
  - http.nonProxyHosts: 设置 HTTP 访问中不需要使用代理服务器的主机，支持使用 `*` 通配符，支持指定多个地址（多个地址之间使用 `|` 分隔）。
- `connectFailed​`: 如果连接失败，Default​ProxySelector 会尝试不使用代理服务器，而是直接连接远程资源。

## 3. Refer Links
