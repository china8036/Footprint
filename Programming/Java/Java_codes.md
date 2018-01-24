- [Java 实用代码段](#java-%E5%AE%9E%E7%94%A8%E4%BB%A3%E7%A0%81%E6%AE%B5)
  - [获取客户端真实 ip](#%E8%8E%B7%E5%8F%96%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%9C%9F%E5%AE%9Eip)

# Java 实用代码段

## 获取客户端真实 ip

在 java 中我们通常可以这样获取客户端 ip 地址：
```java
request.getRemoteAddr()
```
但是这个方法有个弊端，就是如果对方使用了反向代理，那么这个方法获取到的永远都是反向代理服务器的 ip，而并非用户的真实 ip。这样也能达到禁止访问的目的，但是对于已经发生的恶意入侵，我们却无法定位到真实的用户主机。

获取客户端真实 ip：
```java
public static String getClientIP(HttpServletRequest request) {
        String ip = request.getHeader("X-Forwarded-For");
        if (StringUtils.isNotEmpty(ip) && !"unKnown".equalsIgnoreCase(ip)) {
            // 多次反向代理后会有多个 ip 值，第一个 ip 才是真实 ip
            int index = ip.indexOf(",");
            if (index != -1) {
                return ip.substring(0, index);
            } else {
                return ip;
            }
        }
        ip = request.getHeader("X-Real-IP");
        if (StringUtils.isNotEmpty(ip) && !"unKnown".equalsIgnoreCase(ip)) {
            return ip;
        }
        return request.getRemoteAddr();
}
```