- [Java 网络编程：基本 API](#java-网络编程基本-api)
	- [1. InetAddress](#1-inetaddress)
		- [1.1. 常用 API](#11-常用-api)
		- [1.2. 类定义](#12-类定义)
		- [1.3. 静态变量](#13-静态变量)
		- [1.4. 静态内部类](#14-静态内部类)
		- [1.5. 静态代码块](#15-静态代码块)
		- [1.6. 方法实现](#16-方法实现)
			- [1.6.1. getByName 和 getAllByName](#161-getbyname-和-getallbyname)
			- [1.6.2. getByAddress](#162-getbyaddress)
	- [2. URI](#2-uri)
	- [3. URL](#3-url)
		- [3.1. 常用 API](#31-常用-api)
		- [3.2. 类定义](#32-类定义)
		- [3.3. 内部属性](#33-内部属性)
		- [3.4. 构造方法](#34-构造方法)
	- [4. URLConnection](#4-urlconnection)
		- [4.1. HttpURLConnection](#41-httpurlconnection)
		- [4.2. JarURLConnection](#42-jarurlconnection)
	- [5. URLPermission](#5-urlpermission)
	- [6. Refer Links](#6-refer-links)

# Java 网络编程：基本 API

Java 为网络支持提供了 `java.net` 包，该包提供了以编程方式进行网络操作的一系列相关接口。

## 1. InetAddress

> This Class represents an Internet Protocol (IP) address.

[InetAddress](https://docs.oracle.com/javase/9/docs/api/java/net/InetAddress.html) 类用于封装互联网协议 (IP) 地址，其内部隐藏了地址数字。该类本身并没有提供太多功能，但它是网络通信的基础。

在 InetAddress 类下还有 2 个子类：[Inet4Address](https://docs.oracle.com/javase/9/docs/api/java/net/Inet4Address.html) 和 [Inet6Address](https://docs.oracle.com/javase/9/docs/api/java/net/Inet6Address.html)，分别表示 IPv4 地址和 IPv6 地址。

### 1.1. 常用 API

InetAddress 类没有提供构造器，因此需要通过以下静态工厂方法来获取 InetAddress 实例：
- `static InetAddress getLocalHost​()`: 返回表示本机在网络中地址的 InetAddress 对象，如 192.168.56.1。
- `static InetAddress getLoopbackAddress​()`: 返回表示本地回环地址的 InetAddress 对象，即 127.0.0.1。
- `static InetAddress getByName​(String host)`: 返回表示指定主机名地址的 InetAddress 对象。若不能解析主机名，将引发一个 UnknownHostException 异常。
- `static InetAddress[] getAllByName​(String host)`: 返回表示由一个特殊名称分解的所有地址的 InetAddresses 类数组。若不能把名称分解成至少一个地址，它将引发一个 UnknownHostException 异常。
- `static InetAddress getByAddress​(byte[] addr)`: 返回指定 IP 地址的 InetAddress 对象。
- `static InetAddress getByAddress​(String host, byte[] addr)`: 返回指定主机名地址和 IP 地址的 InetAddress 对象。

eg:
```java
jshell> InetAddress.getLocalHost()
$4 ==> DESKTOP-B0AIJUG/192.168.56.1
jshell> InetAddress.getLoopbackAddress()
$3 ==> localhost/127.0.0.1
jshell> InetAddress.getByName("www.baidu.com")
$2 ==> www.baidu.com/14.215.177.38
jshell> InetAddress.getByAddress(new byte[] {(byte)192, (byte)168, 1, 3 }) // 超过 127 需要进行类型强制转换
$6 ==> /192.168.1.3
jshell> InetAddress.getByAddress("www.baidus.com", new byte[] {(byte)192, (byte)168, 1, 3 })
$7 ==> www.baidus.com/192.168.1.3
```

其它：
- `boolean	isReachable​(int timeout)`: 用于测试指定地址是否可达。该方法将使用 icmp echo request 进行探测，若防火墙阻塞了请求，且尝试在目标主机上建立 TCP 连接。
- `boolean	isReachable​(NetworkInterface netif, int ttl, int timeout)`: 
- `String	getHostName​()`: Gets the host name for this IP address.
- `String	getHostAddress​()`: Returns the IP address string in textual presentation.
- `String	getCanonicalHostName​()`: Gets the fully qualified domain name for this IP address.
- `byte[]	getAddress​()`: Returns the raw IP address of this InetAddress object.

### 1.2. 类定义

```java
public
class InetAddress implements java.io.Serializable {}
```

### 1.3. 静态变量

```java
static final int IPv4 = 1;
static final int IPv6 = 2; 
static transient boolean preferIPv6Address = false;
// 封装 IP Address 相关的关键信息
static InetAddressImpl  impl;
/* Used to store the name service provider */
private static List<NameService> nameServices = null;
```

其中，preferIPv6Address 指该 JVM 是否优先使用 IPv6 地址，它由一段静态代码段从 JVM 的参数"java.net.preferIPv6Addresses"中取得：
```java
static {
    preferIPv6Address = java.security.AccessController.doPrivileged(
        new GetBooleanAction("java.net.preferIPv6Addresses")).booleanValue();
    AccessController.doPrivileged(
        new java.security.PrivilegedAction<Void>() {
            public Void run() {
                System.loadLibrary("net");
                return null;
            }
        });
    init(); // native method, perform class load-time initialization
}
```

### 1.4. 静态内部类

```java
// 持有了该 InetAddress 类的一些关键信息
static class InetAddressHolder {
    InetAddressHolder() {}
    InetAddressHolder(String hostName, int address, int family) {
        this.originalHostName = hostName;
        this.hostName = hostName;
        this.address = address;
        this.family = family;
    }
    void init(String hostName, int family) {
        this.originalHostName = hostName;
        this.hostName = hostName;
        if (family != -1) {
            this.family = family;
        }
    }
    String originalHostName;    
    String hostName;
    // Holds a 32-bit IPv4 address.
    int address;
    /**
      * Specifies the address family type, for instance, '1' for IPv4
      * addresses, and '2' for IPv6 addresses.
      */
    int family;
}
```

### 1.5. 静态代码块

除了上边提到的加载 JVM 参数的静态代码块，InetAddress 类中还包含了一段静态代码：
```java
static {
    // 创建 impl 实例对象
    impl = InetAddressImplFactory.create();

    // 初始化 nameServices 链
    String provider = null;;
    String propPrefix = "sun.net.spi.nameservice.provider.";
    int n = 1;
    nameServices = new ArrayList<NameService>();
    provider = AccessController.doPrivileged(
            new GetPropertyAction(propPrefix + n));
    // 尝试去加载 sun.net.spi 中的 NameService Provider，如果你有自己实现了 NameService，或者用了第三方的 NameService，就会被加载进来
    while (provider != null) {
        NameService ns = createNSProvider(provider);
        if (ns != null)
            nameServices.add(ns);
        n++;
        provider = AccessController.doPrivileged(
                new GetPropertyAction(propPrefix + n));
    }

    // 若没有提供 nameService，则使用 default NameService Provider
    if (nameServices.size() == 0) {
        NameService ns = createNSProvider("default");
        nameServices.add(ns);
    }
}
```

其中：
```java
class InetAddressImplFactory {
    static InetAddressImpl create() {
        // 先判断系统是不是支持 IPv6，如果支持，它就会去加载一个 Inet6AddressImpl, 否则就会加载 Inet4AddressImpl
        return InetAddress.loadImpl(isIPv6Supported() ?
                                    "Inet6AddressImpl" : "Inet4AddressImpl");
    }
    static native boolean isIPv6Supported();
}

// 通过反射获取 InetAddressImpl 实例对象
static InetAddressImpl loadImpl(String implName) {
    Object impl = null;

    String prefix = AccessController.doPrivileged(
                  new GetPropertyAction("impl.prefix", ""));
    try {
        impl = Class.forName("java.net." + prefix + implName).newInstance();
    } // ... 省略部分代码

    if (impl == null) {
        try {
            impl = Class.forName(implName).newInstance();
        } catch (Exception e) {
            throw new Error("System property impl.prefix incorrect");
        }
    }

    return (InetAddressImpl) impl;
}
```

Inet4AddressImpl 类和 Inet6AddressImpl 类的定义同样位于 java.net 包下：
```java
class Inet4AddressImpl implements InetAddressImpl {
    public native String getLocalHostName() throws UnknownHostException;
    public native InetAddress[]
        lookupAllHostAddr(String hostname) throws UnknownHostException;
    public native String getHostByAddr(byte[] addr) throws UnknownHostException;
    private native boolean isReachable0(byte[] addr, int timeout, byte[] ifaddr, int ttl) throws IOException;

    public synchronized InetAddress anyLocalAddress() {
        if (anyLocalAddress == null) {
            anyLocalAddress = new Inet4Address(); // {0x00,0x00,0x00,0x00}
            anyLocalAddress.holder().hostName = "0.0.0.0";
        }
        return anyLocalAddress;
    }

    public synchronized InetAddress loopbackAddress() {
        if (loopbackAddress == null) {
            byte[] loopback = {0x7f,0x00,0x00,0x01};
            loopbackAddress = new Inet4Address("localhost", loopback);
        }
        return loopbackAddress;
    }

  public boolean isReachable(InetAddress addr, int timeout, NetworkInterface netif, int ttl) throws IOException {
      byte[] ifaddr = null;
      if (netif != null) {
          /*
           * Let's make sure we use an address of the proper family
           */
          java.util.Enumeration<InetAddress> it = netif.getInetAddresses();
          InetAddress inetaddr = null;
          while (!(inetaddr instanceof Inet4Address) &&
                 it.hasMoreElements())
              inetaddr = it.nextElement();
          if (inetaddr instanceof Inet4Address)
              ifaddr = inetaddr.getAddress();
      }
      return isReachable0(addr.getAddress(), timeout, ifaddr, ttl);
  }
    private InetAddress      anyLocalAddress;
    private InetAddress      loopbackAddress;
}
```
```java
class Inet6AddressImpl implements InetAddressImpl {
    public native String getLocalHostName() throws UnknownHostException;
    public native InetAddress[]
        lookupAllHostAddr(String hostname) throws UnknownHostException;
    public native String getHostByAddr(byte[] addr) throws UnknownHostException;
    private native boolean isReachable0(byte[] addr, int scope, int timeout, byte[] inf, int ttl, int if_scope) throws IOException;

    public boolean isReachable(InetAddress addr, int timeout, NetworkInterface netif, int ttl) throws IOException {
        byte[] ifaddr = null;
        int scope = -1;
        int netif_scope = -1;
        if (netif != null) {
            /*
             * Let's make sure we bind to an address of the proper family.
             * Which means same family as addr because at this point it could
             * be either an IPv6 address or an IPv4 address (case of a dual
             * stack system).
             */
            java.util.Enumeration<InetAddress> it = netif.getInetAddresses();
            InetAddress inetaddr = null;
            while (it.hasMoreElements()) {
                inetaddr = it.nextElement();
                if (inetaddr.getClass().isInstance(addr)) {
                    ifaddr = inetaddr.getAddress();
                    if (inetaddr instanceof Inet6Address) {
                        netif_scope = ((Inet6Address) inetaddr).getScopeId();
                    }
                    break;
                }
            }
            if (ifaddr == null) {
                // Interface doesn't support the address family of
                // the destination
                return false;
            }
        }
        if (addr instanceof Inet6Address)
            scope = ((Inet6Address) addr).getScopeId();
        return isReachable0(addr.getAddress(), scope, timeout, ifaddr, ttl, netif_scope);
    }

    public synchronized InetAddress anyLocalAddress() {
        if (anyLocalAddress == null) {
            if (InetAddress.preferIPv6Address) {
                anyLocalAddress = new Inet6Address();
                anyLocalAddress.holder().hostName = "::";
            } else {
                anyLocalAddress = (new Inet4AddressImpl()).anyLocalAddress();
            }
        }
        return anyLocalAddress;
    }

    public synchronized InetAddress loopbackAddress() {
        if (loopbackAddress == null) {
             if (InetAddress.preferIPv6Address) {
                 byte[] loopback =
                        {0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,
                         0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x01};
                 loopbackAddress = new Inet6Address("localhost", loopback);
             } else {
                loopbackAddress = (new Inet4AddressImpl()).loopbackAddress();
             }
        }
        return loopbackAddress;
    }

    private InetAddress      anyLocalAddress;
    private InetAddress      loopbackAddress;
}
```

如果我们想让我们的应用走谷歌的公共 DNS 服务器：8.8.8.8，但是我们又不想让用户去修改系统配置，可以在程序启动前进行 JRE 的参数配置：
```java
System.setProperty("sun.net.spi.nameservice.provider.1", "dns,sun");
System.setProperty("sun.net.spi.nameservice.nameservers", "8.8.8.8");
System.setProperty("sun.net.spi.nameservice.provider.2", "default");
```

### 1.6. 方法实现

#### 1.6.1. getByName 和 getAllByName

静态方法 getByName() 和 getAllByName() 能根据指定主机名获取其封装了 IP 地址的 InetAddress 对象，因此，这 2 个静态方法中包含了 DNS 查询的内部实现：
```java
public static InetAddress getByName(String host)
    throws UnknownHostException {
    return InetAddress.getAllByName(host)[0];
}

public static InetAddress[] getAllByName(String host)
    throws UnknownHostException {
    return getAllByName(host, null);
}
```
具体实现：
```java
private static InetAddress[] getAllByName(String host, InetAddress reqAddr)
    throws UnknownHostException {
    if (host == null || host.length() == 0) { // 如果传进来的 host 为 null 或者空字符串，就会返回 loopback
        InetAddress[] ret = new InetAddress[1];
        ret[0] = impl.loopbackAddress();
        return ret;
    }
    
    boolean ipv6Expected = false;
    if (host.charAt(0) == '[') {
        // This is supposed to be an IPv6 literal
        if (host.length() > 2 && host.charAt(host.length()-1) == ']') {
            host = host.substring(1, host.length() -1);
            ipv6Expected = true;
        } else {
            // This was supposed to be a IPv6 address, but it's not!
            throw new UnknownHostException(host + ": invalid IPv6 address");
        }
    }

    // 如果传进来 IP 地址，不再进行 DNS look up
    if (Character.digit(host.charAt(0), 16) != -1
        || (host.charAt(0) == ':')) {
        byte[] addr = null;
        int numericZone = -1;
        String ifname = null;
        // see if it is IPv4 address
        addr = IPAddressUtil.textToNumericFormatV4(host);
        if (addr == null) {
            // This is supposed to be an IPv6 literal
            // Check if a numeric or string zone id is present
            int pos;
            if ((pos=host.indexOf ("%")) != -1) {
                numericZone = checkNumericZone (host);
                if (numericZone == -1) { /* remainder of string must be an ifname */
                    ifname = host.substring (pos+1);
                }
            }
            if ((addr = IPAddressUtil.textToNumericFormatV6(host)) == null && host.contains(":")) {
                throw new UnknownHostException(host + ": invalid IPv6 address");
            }
        } else if (ipv6Expected) {
            // Means an IPv4 litteral between brackets!
            throw new UnknownHostException("["+host+"]");
        }
        InetAddress[] ret = new InetAddress[1];
        if(addr != null) {
            if (addr.length == Inet4Address.INADDRSZ) {
                ret[0] = new Inet4Address(null, addr);
            } else {
                if (ifname != null) {
                    ret[0] = new Inet6Address(null, addr, ifname);
                } else {
                    ret[0] = new Inet6Address(null, addr, numericZone);
                }
            }
            return ret;
        }
    } else if (ipv6Expected) {
        // We were expecting an IPv6 Litteral, but got something else
        throw new UnknownHostException("["+host+"]");
    }
    return getAllByName0(host, reqAddr, true);
}

private static InetAddress[] getAllByName0 (String host, InetAddress reqAddr, boolean check)
    throws UnknownHostException  {

    /* If it gets here it is presumed to be a hostname */
    /* Cache.get can return: null, unknownAddress, or InetAddress[] */

    /* make sure the connection to the host is allowed, before we
      * give out a hostname
      */
    if (check) {
        SecurityManager security = System.getSecurityManager();
        if (security != null) {
            security.checkConnect(host, -1);
        }
    }
    // 先从 cache 里面查找
    InetAddress[] addresses = getCachedAddresses(host);

    // 如果 Cache miss，那么就会调用配置的 nameServices 执行真正 DNS 查询
    if (addresses == null) {
        addresses = getAddressesFromNameService(host, reqAddr);
    }

    if (addresses == unknown_array)
        throw new UnknownHostException(host);

    return addresses.clone();
}

// 在缓存中进行 DNS 查询
private static InetAddress[] getCachedAddresses(String hostname) {
    hostname = hostname.toLowerCase();
    // search both positive & negative caches
    synchronized (addressCache) {
        // init cache, 把 anyLocalAddress(0.0.0.0) 放进 cache 里面
        cacheInitIfNeeded();
        CacheEntry entry = addressCache.get(hostname);
        if (entry == null) {
            // positive Cache 指的是 host 解析成功的，negative cache 值解析失败的，即解析成了 0.0.0.0
            entry = negativeCache.get(hostname);
        }

        if (entry != null) {
            return entry.addresses;
        }
    }
    // not found
    return null;
}
// 调用配置的 nameServices 执行真正的 DNS 查询
private static InetAddress[] getAddressesFromNameService(String host, InetAddress reqAddr)
    throws UnknownHostException
{
    InetAddress[] addresses = null;
    boolean success = false;
    UnknownHostException ex = null;
    // 先检查 host 是否在 lookupTable 中
    if ((addresses = checkLookupTable(host)) == null) {
        try {
            for (NameService nameService : nameServices) { // 通过 NameService 链依次解析
                try {
                    addresses = nameService.lookupAllHostAddr(host);
                    success = true;
                    break;
                } catch (UnknownHostException uhe) {
                    if (host.equalsIgnoreCase("localhost")) {
                        InetAddress[] local = new InetAddress[] { impl.loopbackAddress() };
                        addresses = local;
                        success = true;
                        break;
                    }
                    else {
                        addresses = unknown_array;
                        success = false;
                        ex = uhe;
                    }
                }
            }

            // More to do?
            if (reqAddr != null && addresses.length > 1 && !addresses[0].equals(reqAddr)) {
                // Find it?
                int i = 1;
                for (; i < addresses.length; i++) {
                    if (addresses[i].equals(reqAddr)) {
                        break;
                    }
                }
                // Rotate
                if (i < addresses.length) {
                    InetAddress tmp, tmp2 = reqAddr;
                    for (int j = 0; j < i; j++) {
                        tmp = addresses[j];
                        addresses[j] = tmp2;
                        tmp2 = tmp;
                    }
                    addresses[i] = tmp2;
                }
            }
            // Cache the address.
            cacheAddresses(host, addresses, success);

            if (!success && ex != null)
                throw ex;

        } finally {
            // Delete host from the lookupTable and notify
            // all threads waiting on the lookupTable monitor.
            updateLookupTable(host);
        }
    }

    return addresses;
}
```

#### 1.6.2. getByAddress

```java
public static InetAddress getByAddress(byte[] addr)
    throws UnknownHostException {
    return getByAddress(null, addr);
}
public static InetAddress getByAddress(String host, byte[] addr)
    throws UnknownHostException {
    if (host != null && host.length() > 0 && host.charAt(0) == '[') {
        if (host.charAt(host.length()-1) == ']') {
            host = host.substring(1, host.length() -1);
        }
    }
    if (addr != null) {
        if (addr.length == Inet4Address.INADDRSZ) {
            return new Inet4Address(host, addr);
        } else if (addr.length == Inet6Address.INADDRSZ) {
            byte[] newAddr
                = IPAddressUtil.convertFromIPv4MappedAddress(addr);
            if (newAddr != null) {
                // IPv4 是长度为 4 个字节的 byte 数组
                return new Inet4Address(host, newAddr);
            } else {
                // IPv6 是长度为 16 个字节的 byte 数组
                return new Inet6Address(host, addr);
            }
        }
    }
    throw new UnknownHostException("addr is of illegal length");
}
```

其中，Inet4Address 类与 Inet6Address 类的构造方法如下：
```java
Inet4Address(String hostName, byte addr[]) {
    holder().hostName = hostName;
    holder().family = IPv4;
    if (addr != null) {
        if (addr.length == INADDRSZ) {
            // 底层用的是位运算，将 4 个字节直接按位运算转成了 int 类型，因此即使 byte 数组中有元素超过了 127 也不会溢出
            int address  = addr[3] & 0xFF;
            address |= ((addr[2] << 8) & 0xFF00);
            address |= ((addr[1] << 16) & 0xFF0000);
            address |= ((addr[0] << 24) & 0xFF000000);
            holder().address = address;
        }
    }
    holder().originalHostName = hostName;
}
Inet6Address(String hostName, byte addr[]) {
    holder6 = new Inet6AddressHolder();
    try {
        initif (hostName, addr, null);
    } catch (UnknownHostException e) {} /* cant happen if ifname is null */
}
```

## 2. URI

java.net.[URI](https://docs.oracle.com/javase/9/docs/api/java/net/URI.html) 类对象表示 Uniform Resource Identifier 统一资源标识符，主要提供以下功能：
- 验证 URI 格式
  - `URI(String str)`: 构造函数，若格式不正确则抛出 URISyntaxException。
  - `URI create(String str)`: 若格式不正确则抛出 unchecked 的 IllegalArgumentException。
- 提取 URI 各组件
  - `getAuthority()`
  - `getFragment()`
  - `getHost()`
  - `getPath()`
  - `getPort()`
  - `getQuery()`
  - `getScheme()`
  - `getSchemeSpecificPart()`
  - `getUserInfo()`
- 标准化、解析化和相对化
  - `normalize()`: 返回符合标准的 URI 新对象。如`x/y/../z/./q`->`x/z/q`。
  - `resolve(String/URI uri)`: 进行反向解析，以入参作为相对 URI，以 resolve 方法所属对象作为基本 URI 来得到一个新的标准的 URI 对象。
  - `relativize(URI uri)`: 相对化操作，就是获取 URI 中的相对 URI。
- 将 URI 转成 URL
  - `URL toURL()`: 将 URI 转换为 URL。

注意：URI 类中不含任何定位、搜索和读写资源的操作，唯一的作用就是解析。若需要对资源进行读写等操作，应使用 URL 类。

## 3. URL

java.net.[URL](https://docs.oracle.com/javase/9/docs/api/java/net/URL.html) 类对象表示 Uniform Resource Locator 统一资源定位符，是指向网络资源的“指针”。

### 3.1. 常用 API

URL 类中提供了多个构造器用于创建 URL 对象：
- `URL(String protocol, String host, int port, String file)`
- `URL(String protocol, String host, String file)`
- `URL(String protocol, String host, int port, String file, URLStreamHandler handler)`
- `URL(String spec)` 
- `URL(URL context, String spec)`
- `URL(URL context, String spec, URLStreamHandler handler)`

URL 类中提供了多个方法用于访问该 URL 对于的资源：
- `String getFile()`: 获取该 URL 的资源名。
- `String getHost)`: 获取该 URL 的主机名。
- `String getPath()`: 获取该 URL 的路径部分。
- `int getPort()`: 获取该 URL 的端口号。
- `String getProtocol()`: 获取该 URL 的协议名称。
- `String getQuery()`: 获取该 URL 的查询字符串部分。
- `String getRef()`: 获取该 URL 的锚点（也称为"引用"）。
- `URLConnection openConnection()`: 获取与该 URL 的 URLConnection 对象，它代表了与 URL 所引用的远程对象的连接。
- `final InputStream openStream()`: 打开此连接，并获取与该 URL 资源的 InputStream。

### 3.2. 类定义

```java
public final class URL implements java.io.Serializable {}
```

### 3.3. 内部属性

```java
private String protocol;

/**
  * The host name to connect to.
  */
private String host;

/**
  * The protocol port to connect to.
  */
private int port = -1;

/**
  * The specified file name on that host. {@code file} is
  * defined as {@code path[?query]}
  */
private String file;

/**
  * The query part of this URL.
  */
private transient String query;

/**
  * The authority part of this URL.
  */
private String authority;

/**
  * The path part of this URL.
  */
private transient String path;

/**
  * The userinfo part of this URL.
  */
private transient String userInfo;

/**
  * # reference.
  */
private String ref;
```
容易看出，这些属性与 URL 格式的每一部分一一对应：
> scheme:[//[user[:password]@]host[:port]][/path](?query)[#fragment]

### 3.4. 构造方法

URL 类的多个构造方法最终会调用到以下这个 Constructor：
```java
public URL(URL context, String spec, URLStreamHandler handler)
    throws MalformedURLException
{
    String original = spec;
    int i, limit, c;
    int start = 0;
    String newProtocol = null;
    boolean aRef=false;
    boolean isRelative = false;

    // Check for permission to specify a handler
    if (handler != null) {
        SecurityManager sm = System.getSecurityManager();
        if (sm != null) {
            checkSpecifyHandler(sm);
        }
    }

    try {
        limit = spec.length();
        // 先把头尾的空格去掉，注意，它这里用的判断是<= ' '， 因为在 ASCII 码中小于空格 (32) 的都是不可见的字符
        while ((limit > 0) && (spec.charAt(limit - 1) <= ' ')) {
            limit--;        //eliminate trailing whitespace
        }
        while ((start < limit) && (spec.charAt(start) <= ' ')) {
            start++;        // eliminate leading whitespace
        }
        // 然后判断传入的字符串中是否以 url: 开头， 如果有，就去掉，所以，如果传入 URL 字符串是以 url: 开头的，JDK 还是能解析出来的
        if (spec.regionMatches(true, start, "url:", 0, 4)) {
            start += 4;
        }
        if (start < spec.length() && spec.charAt(start) == '#') {
            /* we're assuming this is a ref relative to the context URL.
              * This means protocols cannot start w/ '#', but we must parse
              * ref URL's like: "hello:there" w/ a ':' in them.
              */
            aRef=true;
        }
        // 判断是否是相对 URL
        for (i = start ; !aRef && (i < limit) &&
                  ((c = spec.charAt(i)) != '/') ; i++) {
            if (c == ':') {
                // 取第一个：前面的所有字符作为 protocol 字符串
                String s = spec.substring(start, i).toLowerCase();
                if (isValidProtocol(s)) { // 验证 protocol 字符串格式，验证逻辑比较粗糙
                    newProtocol = s;
                    start = i + 1;
                }
                break;
            }
        }
        // Only use our context if the protocols match.
        protocol = newProtocol;
        if ((context != null) && ((newProtocol == null) ||
                        newProtocol.equalsIgnoreCase(context.protocol))) {
            // inherit the protocol handler from the context
            // if not specified to the constructor
            if (handler == null) {
                handler = context.handler;
            }

            // If the context is a hierarchical URL scheme and the spec
            // contains a matching scheme then maintain backwards
            // compatibility and treat it as if the spec didn't contain
            // the scheme; see 5.2.3 of RFC2396
            if (context.path != null && context.path.startsWith("/"))
                newProtocol = null;

            if (newProtocol == null) {
                protocol = context.protocol;
                authority = context.authority;
                userInfo = context.userInfo;
                host = context.host;
                port = context.port;
                file = context.file;
                path = context.path;
                isRelative = true;
            }
        }

        if (protocol == null) {
            throw new MalformedURLException("no protocol: "+original);
        }

        // 根据 protocol 来加载相应的 URLStreamHandler，如果没有对应的 URLStreamHandler 则抛出 MalformedURLException 异常
        if (handler == null &&
            (handler = getURLStreamHandler(protocol)) == null) {
            throw new MalformedURLException("unknown protocol: "+protocol);
        }

        this.handler = handler;

        i = spec.indexOf('#', start);
        if (i >= 0) {
            ref = spec.substring(i + 1, limit);
            limit = i;
        }

        /*
          * Handle special case inheritance of query and fragment
          * implied by RFC2396 section 5.2.2.
          */
        if (isRelative && start == limit) {
            query = context.query;
            if (ref == null) {
                ref = context.ref;
            }
        }
        // 通过这个 Handler 来解析和构造这个 URL 对象
        handler.parseURL(this, spec, start, limit);

    } catch(MalformedURLException e) {
        throw e;
    } catch(Exception e) {
        MalformedURLException exception = new MalformedURLException(e.getMessage());
        exception.initCause(e);
        throw exception;
    }
}

static URLStreamHandler getURLStreamHandler(String protocol) {
    URLStreamHandler handler = handlers.get(protocol);
    if (handler == null) {
        boolean checkedWithFactory = false;
        // Use the factory (if any)
        if (factory != null) {
            handler = factory.createURLStreamHandler(protocol);
            checkedWithFactory = true;
        }
        // Try java protocol handler
        if (handler == null) {
            String packagePrefixList = null;

            packagePrefixList
                = java.security.AccessController.doPrivileged(
                new sun.security.action.GetPropertyAction(
                    protocolPathProp,""));
            if (packagePrefixList != "") {
                packagePrefixList += "|";
            }
            // REMIND: decide whether to allow the "null" class prefix or not.
            packagePrefixList += "sun.net.www.protocol";
            StringTokenizer packagePrefixIter =
                new StringTokenizer(packagePrefixList, "|");
            while (handler == null &&
                    packagePrefixIter.hasMoreTokens()) {

                String packagePrefix =
                  packagePrefixIter.nextToken().trim();
                try {
                    String clsName = packagePrefix + "." + protocol +
                      ".Handler";
                    Class<?> cls = null;
                    try {
                        // 根据 protocol，使用反射去相应的包里面去加载 Handler 类，并创建实例
                        cls = Class.forName(clsName);
                    } catch (ClassNotFoundException e) {
                        ClassLoader cl = ClassLoader.getSystemClassLoader();
                        if (cl != null) {
                            cls = cl.loadClass(clsName);
                        }
                    }
                    if (cls != null) {
                        handler  =
                          (URLStreamHandler)cls.newInstance();
                    }
                } catch (Exception e) {
                    // any number of exceptions can get thrown here
                }
            }
        }
        // ...
    }
    return handler;
}
```

从源码中可知，URL 类是依据 URLStreamHandler 来处理 URL 字符串的，若没有相应的协议处理器则抛 MalformedURLException。处理流程如下：
1. 查看处理器缓存 HashTable handlers，若存在缓存项则直接返回。
1. 若缓存中没有则查看是否有 URLStreamHandlerFactory 实例，若存在则调用其 createURLStreamHandler(String protocol)。默认情况下 URLStreamHandlerFactory 实例为 null。
1. 若 2 中返回 null，则通过系统属性 java.protocol.handler.pkgs 获取以|分隔的包名列表，然后逐一检查是否存在继承了 URLStreamHandler 的<package>.<protocol>.Handler 类，有则返回，无则继续遍历。
1. 若 3 中遍历失败，则检查是否存在继承了 URLStreamHandler 的<system default package>.<protocol>.Handler 的内置类。
1. 上述均失败则抛出 MalformedURLException。

JDK 内置提供了 http、https、ftp、file 和 jar 协议的 URLStreamHandler，而其他协议的处理器则需开发者自行继承 URLStreamHandler 来实现。

## 4. URLConnection

> The abstract class URLConnection is the superclass of all classes that represent a communications link between the application and a URL. Instances of this class can be used both to read from and to write to the resource referenced by the URL. 

[URLConnection](https://docs.oracle.com/javase/9/docs/api/java/net/URLConnection.html) 类的对象表示 Java 应用程序与 URL 资源之间的通信连接。程序可以通过 URLConnection 对象向该 URL 发送请求、读取 URL 资源。

一般情况下，创建一个 URL 连接对象并与该 URL 资源进行交互包含了以下几个步骤：
1. 创建 URL 对象，通过 URL 对象的 openConnection() 方法创建对应的 URLConnection 对象，此时 request 还未真正发送。
1. 调用 URLConnection 对象的方法进行连接参数、请求属性的设置。
    
    建立连接之前，可通过以下方法进行 request header 的设置：
    - setAllowUserInteraction
    - setDoInput: URL 连接可用于输入和 / 或输出。如果打算使用 URL 连接进行输入，则将 DoInput 标志设置为 true；如果不打算使用，则设置为 false。默认值为 true。
    - setDoOutput: URL 连接可用于输入和 / 或输出。如果打算使用 URL 连接进行输出，则将 DoOutput 标志设置为 true；如果不打算使用，则设置为 false。默认值为 false。
    - setIfModifiedSince
    - setUseCaches
    - setRequestProperty
    - addRequestProperty

1. 调用 URLConnection 对象的 connect() 方法发送请求，此次 request 才真正发送，建立了实际连接。
    - 如果是 HTTP GET 请求，则使用 connect() 方法发送请求即可。 
    - 如果是 HTTP POST 请求，则需要获取 URLConnection 对象的 OutputStream 来发送请求参数。

1. URL 远程资源变为可用状态，程序可以访问远程资源的头字段、或通过输入流读取远程资源的数据。
    
    远程资源可用后，可通过以下方法获取响应的 header fields 和 contents：
    - getContent
    - getHeaderField
    - getInputStream: 返回 URL 的输入流，用于读取资源。
    - getOutputStreamL: 返回 URL 的输出流，用于写入资源。
    远程资源可用后，可通过以下方法获取特定响应 header field 的值：
    - getContentEncoding
    - getContentLength
    - getContentType
    - getDate
    - getExpiration
    - getLastModified

NOTE: 如果既要使用输入流读取 URLConnection 响应的内容，又需要使用输出流发送请求参数，则一定要先使用输出流，再使用输入流。

eg:
```java
// 向指定 URL 发送 GET 方法的请求 
public static String sendGet(String url, String param) {  
String result = "";  
BufferedReader in = null;  
try {  
    String urlName = url + "?" + param;  
    URL realUrl = new URL(urlName);  
    // 打开和 URL 之间的连接  
    URLConnection conn = realUrl.openConnection();  
    // 设置通用的请求属性  
    conn.setRequestProperty("accept", "*/*");  
    conn.setRequestProperty("connection", "Keep-Alive");  
    conn.setRequestProperty("user-agent",  
            "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)");  
    // 建立实际的连接  
    conn.connect();  
    // 获取所有响应头字段  
    Map<String, List<String>> map = conn.getHeaderFields();  
    // 遍历所有的响应头字段  
    for (String key : map.keySet()) {  
        System.out.println(key + "--->" + map.get(key));  
    }  
    // 定义 BufferedReader 输入流来读取 URL 的响应  
    in = new BufferedReader(  
            new InputStreamReader(conn.getInputStream()));  
    String line;  
    while ((line = in.readLine()) != null) {  
        result += "/n" + line;  
    }  
} catch (Exception e) {  
    System.out.println("发送 GET 请求出现异常！" + e);  
    e.printStackTrace();  
} finally {  // 使用 finally 块来关闭输入流  
    try {  
        if (in != null) {  
            in.close();  
        }  
    } catch (IOException ex) {  
        ex.printStackTrace();  
    }  
}  
return result;  
}  
// 向指定 URL 发送 POST 方法的请求  
public static String sendPost(String url, String param) {  
PrintWriter out = null;  
BufferedReader in = null;  
String result = "";  
try {  
    URL realUrl = new URL(url);  
    // 打开和 URL 之间的连接  
    URLConnection conn = realUrl.openConnection();  
    // 设置通用的请求属性  
    conn.setRequestProperty("accept", "*/*");  
    conn.setRequestProperty("connection", "Keep-Alive");  
    conn.setRequestProperty("user-agent",  
            "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)");  
    // 发送 POST 请求必须设置如下两行  
    conn.setDoOutput(true);  
    conn.setDoInput(true);  
    // 获取 URLConnection 对象对应的输出流  
    out = new PrintWriter(conn.getOutputStream());  
    // 发送请求参数  
    out.print(param);  
    // flush 输出流的缓冲  
    out.flush();  
    // 定义 BufferedReader 输入流来读取 URL 的响应  
    in = new BufferedReader(  
            new InputStreamReader(conn.getInputStream()));  
    String line;  
    while ((line = in.readLine()) != null) {  
        result += "/n" + line;  
    }  
} catch (Exception e) {  
    System.out.println("发送 POST 请求出现异常！" + e);  
    e.printStackTrace();  
} finally {  // 使用 finally 块来关闭输出流、输入流  
    try {  
        if (out != null) {  
            out.close();  
        }  
        if (in != null) {  
            in.close();  
        }  
    } catch (IOException ex) {  
        ex.printStackTrace();  
    }  
}  
return result;  
}  
```

### 4.1. HttpURLConnection

[HttpURLConnection](https://docs.oracle.com/javase/9/docs/api/java/net/HttpURLConnection.html) 类是 URLConnection 类的扩展类，表示 Java 应用程序与 URL 资源之间的一个 HTTP 连接。

### 4.2. JarURLConnection

[JarURLConnection](https://docs.oracle.com/javase/9/docs/api/java/net/JarURLConnection.html) 类是 URLConnection 类的扩展类，表示 Java 应用程序与 URL 资源之间的一个 Jar 连接。

## 5. URLPermission

[URLPermission](https://docs.oracle.com/javase/9/docs/api/java/net/URLPermission.html) 是 Java8 新增的工具类，用于管理 URLConnection 的权限问题。

## 6. Refer Links

[深入 InetAddress 源码](http://ericcenblog.com/2016/09/23/zhong-du-java-ioyuan-ma-bi-ji-1/)

[Java DNS 查询内部实现](http://ju.outofmemory.cn/entry/111943)

[InetAddress.getByAddress() 源码解析](https://blog.csdn.net/ling376962380/article/details/72824880)

[Java URL 类源码剖析](http://ericcenblog.com/2017/05/15/java-urllei-yuan-ma-pou-xi/)