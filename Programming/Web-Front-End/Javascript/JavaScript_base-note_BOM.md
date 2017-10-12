- [JavaScript Note - BOM](#javascript-note---bom)
  - [window 对象](#window-%E5%AF%B9%E8%B1%A1)
  - [history 对象](#history-%E5%AF%B9%E8%B1%A1)
  - [Cookie](#cookie)
  - [Web Storage](#web-storage)
  - [同源政策](#%E5%90%8C%E6%BA%90%E6%94%BF%E7%AD%96)
  - [Ajax](#ajax)
    - [header](#header)
  - [CORS](#cors)
  - [Refer Links](#refer-links)

# JavaScript Note - BOM #

浏览器对象模型 BOM 提供给开发人员控制浏览器的接口，虽 BOM 作为 JavaScript 实现的一部分，但 BOM 至今仍无统一标准。

## window 对象 ##
所有浏览器都支持 window 对象，它表示浏览器窗口；

所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员；

全局变量是 window 对象的属性，全局函数是 window 对象的方法；
例：HTML DOM 的 document 也是 window 对象的属性之一
```javascript
window.document.getElementById("header");
```
等同于
```javascript
document.getElementById("header");
```

## history 对象 ##

## Cookie

## Web Storage

## 同源政策

## Ajax

### header
参见：   
https://xhr.spec.whatwg.org/#the-setrequestheader()-method   
https://fetch.spec.whatwg.org/#terminology-headers    
https://fetch.spec.whatwg.org/#forbidden-header-name     

使用 AJAX 发送 request 时，由于 JavaScript 最终依赖于浏览器运行，因此无法完全无限制的定制 request headers（受限于 W3C 标准）：   
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/12/e65bd2fa99c02a77e7836a21d661f88f.jpg)

> 
A forbidden header name is a header name that is a byte-case-insensitive match for one of:
`Accept-Charset`   
`Accept-Encoding`   
`Access-Control-Request-Headers`   
`Access-Control-Request-Method`   
`Connection`   
`Content-Length`   
`Cookie`     
`Cookie2`     
`Date`     
`DNT`     
`Expect`     
`Host`     
`Keep-Alive`     
`Origin`     
`Referer`     
`TE`     
`Trailer`     
`Transfer-Encoding`     
`Upgrade`     
`User-Agent`    
`Via`     
or a header name that starts with a byte-case-insensitive match for `Proxy-` or `Sec-` (including being a byte-case-insensitive match for just `Proxy-` or `Sec-`).

注：大部分浏览器遵循了该 W3C 标准，但早期的 IE 不遵循，可以设置如 UA 等 header；

例：使用 ajax 发送 request 时设置 UA：
```javascript
$.ajax({
  url: 'https://www.baidu.com',
  type: "GET",
  headers: {
    'User-Agent': '6666666'
  },
  error: function(jqXHR, textStatus, errorThrown) {
    //....
  },
  success: function(data, textStatus, jqXHR) {
    //....
  }
});
```
chrome 报错：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/12/8d89b40ec27526ee23573f5975ca6712.jpg)

## CORS

## Refer Links

W3C AJAX API：https://xhr.spec.whatwg.org/

http://javascript.ruanyifeng.com/
