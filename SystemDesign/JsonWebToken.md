- [JSON Web Tokens](#json-web-tokens)
  - [1. 基本概念](#1-%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
  - [2. JWT 构造](#2-jwt-%E6%9E%84%E9%80%A0)
  - [3. 工作流程](#3-%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B)
  - [4. Java 实现](#4-java-%E5%AE%9E%E7%8E%B0)
  - [5. Refer Links](#5-refer-links)

# JSON Web Tokens

## 1. 基本概念

Json Web Token，缩写为 JWT，是基于 [RFC 7519](https://tools.ietf.org/html/rfc7519) 标准定义的一种可以安全传输的 小巧 和 自包含 的 JSON 对象。由于数据是使用数字签名的，所以是可信任的和安全的。JWT 可以使用 HMAC 算法对 secret 进行加密或者使用 RSA 的公钥私钥对来进行签名。

## 2. JWT 构造

下边是一个 JWT 生成后的样子：
```
eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ3YW5nIiwiY3JlYXRlZCI6MTQ4OTA3OTk4MTM5MywiZXhwIjoxNDg5Njg0NzgxfQ.RC-BYCe_UZ2URtWddUpWXIp4NMsoeq2O6UF-8tVplqXY1-CI9u1-a-9DAAJGfNWkHE81mpnR3gXzfrBAB3WUAg
```

可以看到这个 token 分成了三部分，每部分用 `.` 分隔，每段都是用 Base64 编码的。如果我们用一个 Base64 的解码器，可以看到
- 第一部分 `eyJhbGciOiJIUzUxMiJ9` 被解析成了：
  ```json
  {
      "alg":"HS512"
  }
  ```
  这是告诉我们 HMAC 采用 HS512 算法对 JWT 进行的签名。
- 第二部分 `eyJzdWIiOiJ3YW5nIiwiY3JlYXRlZCI6MTQ4OTA3OTk4MTM5MywiZXhwIjoxNDg5Njg0NzgxfQ` 被解码之后是：
  ```json
  {
      "sub":"wang",
      "created":1489079981393,
      "exp":1489684781
  }
  ```
  这段告诉我们这个 Token 中含有的数据声明（Claim），这个例子里面有三个声明：sub, created 和 exp。在我们这个例子中，分别代表着用户名、创建时间和过期时间，当然你可以把任意数据声明在这里，但由于数据声明（Claim）是公开的，千万不要把密码等敏感字段放进去，否则就等于是公开给别人了。

- 第三段 `RC-BYCe_UZ2URtWddUpWXIp4NMsoeq2O6UF-8tVplqXY1-CI9u1-a-9DAAJGfNWkHE81mpnR3gXzfrBAB3WUAg` 同样使用 Base64 解码之后：
  ```json
  D X    DmYTeȧLUZcPZ0$gZAY_7wY@
  ```
  最后一段其实是签名，这个签名必须知道秘钥才能计算。这个也是 JWT 的安全保障。服务端收到 token 后，利用对称密钥对这一段代码进行解密验证，从而保证了 token 的安全性。

因此，JWT 是由三段组成的，按官方的叫法分别是 header（头）、payload（负载）和 signature（签名）：
```
header.payload.signature
```
- header 

  头中的数据通常包含两部分：一个是我们刚刚看到的 alg，这个词是 algorithm 的缩写，就是指明算法。另一个可以添加的字段是 token 的类型（按 RFC 7519 实现的 token 机制不只 JWT 一种)，但如果我们采用的是 JWT 的话，指定这个就多余了。
  ```json
  {
    "alg": "HS512",
    "typ": "JWT"
  }
  ```

- payload

  payload 中可以放置三类数据：系统保留的、公共的和私有的：
  - 系统保留的声明（Reserved claims）：这类声明不是必须的，但是是建议使用的，包括：iss （签发者), exp （过期时间),sub （主题), aud （目标受众) 等。这里我们发现都用的缩写的三个字符，这是由于 JWT 的目标就是尽可能小巧。
  - 公共声明：这类声明需要在 IANA JSON Web Token Registry 中定义或者提供一个 URI，因为要避免重名等冲突。
  - 私有声明：这个就是你根据业务需要自己定义的数据了。

- signature

  采用 header 中声明的算法，接受三个参数：base64 编码的 header、base64 编码的 payload 和秘钥（secret）进行运算。签名这一部分如果你愿意的话，可以采用 RSASHA256 的方式进行公钥、私钥对的方式进行，如果安全性要求的高的话。
  ```
  HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload),
    secret)
  ```

## 3. 工作流程

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/3/18/dfc6b4daecf619a55e7ae69cd24e2973.jpg)

## 4. Java 实现

为了简化我们的工作，这里使用一个比较成熟的 JWT 类库 [jjwt](https://github.com/jwtk/jjwt)，用于 Java 和 Android 的 JWT token 的生成和验证。

JWT 生成：
```java
String generateToken() {
    Map<String, Object> claims = new HashMap<>();
    claims.put(CLAIM_KEY_USERNAME, username());

    return Jwts.builder()
            .setClaims(claims)
            .setExpiration(generateExpirationDate())
            .signWith(SignatureAlgorithm.HS512, secret) // 采用什么算法是可以自己选择的，不一定非要采用 HS512
            .compact();
}
```

JWT 解析：
```java
Claims getClaimsFromToken(String token) {
    Claims claims;
    try {
        claims = Jwts.parser()
                .setSigningKey(secret)
                .parseClaimsJws(token)
                .getBody();
    } catch (Exception e) {
        claims = null;
    }
    return claims;
}
```

## 5. Refer Links

[JWT.IO allows you to decode, verify and generate JWT.](https://jwt.io/)

[使用 JWT 和 Spring Security 保护 REST API](https://juejin.im/post/58c29e0b1b69e6006bce02f4#heading-2)