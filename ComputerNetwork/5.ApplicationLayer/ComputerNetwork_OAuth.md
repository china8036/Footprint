- [OAuth 协议](#oauth-协议)
  - [1. 基本概念](#1-基本概念)
  - [2. 基本原理](#2-基本原理)
  - [3. Refer Links](#3-refer-links)

# OAuth 协议

## 1. 基本概念

OAuth(Open Authorization) 开放授权，是一个开放标准，允许用户让第三方应用访问该用户在某一网站上存储的私密的资源（如照片，视频，联系人列表），而无需将用户名和密码提供给第三方应用。

OAuth 允许用户提供一个令牌，而不是用户名和密码来访问他们存放在特定服务提供者的数据。每一个令牌授权一个特定的网站（例如，视频编辑网站) 在特定的时段（例如，接下来的 2 小时内）内访问特定的资源（例如仅仅是某一相册中的视频）。这样，OAuth 让用户可以授权第三方网站访问他们存储在另外服务提供者的某些特定信息，而非所有内容。

OAuth 是 OpenID 的一个补充，但是完全不同的服务。

版本：
- OAuth1.0 存在安全漏洞，现已被弃用。
- OAuth 2.0 是 OAuth 协议的下一版本，但不向下兼容 OAuth 1.0，OAuth2.0 安全便捷，被广泛使用。

## 2. 基本原理

![image](http://img.cdn.firejq.com/jpg/2018/10/5/3ff445a9cccc2b264ed903a1bd92925e.jpg)

![image](http://img.cdn.firejq.com/jpg/2018/10/5/da546f80e73b99b9c7c5685ecbf06fa9.jpg)

（redirect_uri 为登录成功后跳转到的页面）

![image](http://img.cdn.firejq.com/jpg/2018/10/5/f08f4172d5f5a1d634f3389b48d2414a.jpg)

![image](http://img.cdn.firejq.com/jpg/2018/10/5/19092155d31cca4af3c654a33e81cb6b.jpg)

AccessToken：用户通过第三方应用访问 OAuth 接口的请求令牌；

RefreshToken：

<!-- TODO: -->

## 3. Refer Links

[阮一峰：理解 OAuth2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)