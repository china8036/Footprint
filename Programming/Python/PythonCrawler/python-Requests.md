- [Python Requests NOTE](#python-requests-note)
  - [1. 概念](#1-%E6%A6%82%E5%BF%B5)
  - [2. 使用](#2-%E4%BD%BF%E7%94%A8)
    - [2.1. 请求](#21-%E8%AF%B7%E6%B1%82)
      - [2.1.1. 发送请求](#211-%E5%8F%91%E9%80%81%E8%AF%B7%E6%B1%82)
        - [2.1.1.1. 请求方法](#2111-%E8%AF%B7%E6%B1%82%E6%96%B9%E6%B3%95)
        - [2.1.1.2. 请求头部](#2112-%E8%AF%B7%E6%B1%82%E5%A4%B4%E9%83%A8)
        - [2.1.1.3. 发送 cookie](#2113-%E5%8F%91%E9%80%81-cookie)
        - [2.1.1.4. 重定向设置](#2114-%E9%87%8D%E5%AE%9A%E5%90%91%E8%AE%BE%E7%BD%AE)
        - [2.1.1.5. 超时设置](#2115-%E8%B6%85%E6%97%B6%E8%AE%BE%E7%BD%AE)
        - [2.1.1.6. session 对象](#2116-session-%E5%AF%B9%E8%B1%A1)
        - [2.1.1.7. SSL 证书验证](#2117-ssl-%E8%AF%81%E4%B9%A6%E9%AA%8C%E8%AF%81)
        - [2.1.1.8. 使用代理](#2118-%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86)
        - [2.1.1.9. 使用事件钩子](#2119-%E4%BD%BF%E7%94%A8%E4%BA%8B%E4%BB%B6%E9%92%A9%E5%AD%90)
      - [2.1.2. GET 请求](#212-get-%E8%AF%B7%E6%B1%82)
        - [2.1.2.1. 传递 URL 参数](#2121-%E4%BC%A0%E9%80%92-url-%E5%8F%82%E6%95%B0)
      - [2.1.3. POST 请求](#213-post-%E8%AF%B7%E6%B1%82)
        - [2.1.3.1. 表单请求](#2131-%E8%A1%A8%E5%8D%95%E8%AF%B7%E6%B1%82)
        - [2.1.3.2. JSON 请求](#2132-json-%E8%AF%B7%E6%B1%82)
        - [2.1.3.3. 上传文件](#2133-%E4%B8%8A%E4%BC%A0%E6%96%87%E4%BB%B6)
    - [2.2. 响应](#22-%E5%93%8D%E5%BA%94)
      - [2.2.1. 响应内容](#221-%E5%93%8D%E5%BA%94%E5%86%85%E5%AE%B9)
      - [2.2.2. json 响应](#222-json-%E5%93%8D%E5%BA%94)
      - [2.2.3. 原始套接字响应](#223-%E5%8E%9F%E5%A7%8B%E5%A5%97%E6%8E%A5%E5%AD%97%E5%93%8D%E5%BA%94)
      - [2.2.4. 二进制响应](#224-%E4%BA%8C%E8%BF%9B%E5%88%B6%E5%93%8D%E5%BA%94)
      - [2.2.5. 响应 cookie](#225-%E5%93%8D%E5%BA%94-cookie)
    - [2.3. 异常处理](#23-%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86)
    - [2.4. 获取源代码](#24-%E8%8E%B7%E5%8F%96%E6%BA%90%E4%BB%A3%E7%A0%81)
  - [3. Refer Links](#3-refer-links)

# Python Requests NOTE

## 1. 概念

Requests 是一个封装了 HTTP/1.1 协议的 Python 库，可以看作是 urllib 库的更高级版本。

## 2. 使用

### 2.1. 请求

#### 2.1.1. 发送请求

##### 2.1.1.1. 请求方法

requests 库提供了 http 所有的基本请求方式：
```python
r = requests.get("http://httpbin.org/get")
r = requests.post("http://httpbin.org/post")
r = requests.put("http://httpbin.org/put")
r = requests.delete("http://httpbin.org/delete")
r = requests.head("http://httpbin.org/get")
r = requests.options("http://httpbin.org/get")
```

这些方法向目标 URL 发送不同 method 的 HTTP 请求后，将响应 response 封装后返回。

##### 2.1.1.2. 请求头部

使用 requests 库发送的请求的默认请求报文头部的 User-Agent 字段大概是这样：
```
User-Agent: python-requests/2.3.0 CPython/2.6.6 Windows/7
# 这样的 User-Agent 字段会直接被识别出不是使用浏览器访问
```

请求方法中提供了关键字参数 headers，若想为请求添加 HTTP 头部，只要简单地传递一个 dict（字典） 给 headers 参数即可。
```
payload = {'key1': 'value1', 'key2': 'value2'}
headers = {'content-type': 'application/json'}
r = requests.get("http://httpbin.org/get", params=payload, headers=headers)
```

NOTE:

- 定制 header 的优先级低于某些特定的信息源，例如：

  - 如果在 .netrc 中设置了用户认证信息，使用 headers= 设置的授权就不会生效。而如果设置了 auth= 参数，``.netrc`` 的设置就无效了。

  - 如果被重定向到别的主机，授权 header 就会被删除。

  - 代理授权 header 会被 URL 中提供的代理身份覆盖掉。

  - 在我们能判断内容长度的情况下，header 的 Content-Length 会被改写。

- 所有的 header 值必须是 string、bytestring 或者 unicode。尽管传递 unicode header 也是允许的，但不建议这样做。

##### 2.1.1.3. 发送 cookie

请求方法中提供了关键字参数 cookie：
```python
url = 'http://httpbin.org/cookies'
cookies = dict(cookies_are='working')
r = requests.get(url, cookies=cookies)
```

##### 2.1.1.4. 重定向设置

默认情况下，除了 HEAD, Requests 会自动处理所有重定向。

可以使用响应对象的 history 方法来追踪重定向。

如果你使用的是 GET、OPTIONS、POST、PUT、PATCH 或者 DELETE，那么你可以通过 allow_redirects 参数禁用重定向处理：
```python
>>> r = requests.get('http://github.com', allow_redirects=False)
>>> r.status_code
301
>>> r.history
[]
```

如果你使用了 HEAD，你也可以启用重定向：
```python
>>> r = requests.head('http://github.com', allow_redirects=True)
>>> r.url
'https://github.com/'
>>> r.history
[<Response [301]>]
```

##### 2.1.1.5. 超时设置

可以利用 timeout 参数来配置最大请求时间（秒）：
```python
# 设置请求并返回响应的超时时间为 10 秒
requests.get('http://github.com', timeout=10)
# 设置请求超时时间为 3 秒，响应超时时间为 7 秒
requests.get('http://github.com', timeout=(3, 7))
```

注：timeout 仅对请求连接过程有效，与响应体的下载无关。

##### 2.1.1.6. session 对象

http://docs.python-requests.org/zh_CN/latest/user/advanced.html

使用 requests 的请求方法每次请求其实都相当于发起了一个新的请求。也就是相当于我们每个请求都用了不同的浏览器单独打开的效果，它并不在一个会话当中。

证明：
```python
requests.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
r = requests.get("http://httpbin.org/cookies")
print(r.text)
```
结果是：
```
{
  "cookies": {}
}
```

requests 库提供了 session 对象，可实现会话控制：
```python
s = requests.Session()
s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
r = s.get("http://httpbin.org/cookies")
print(r.text)
```
结果是：
```
{
  "cookies": {
    "sessioncookie": "123456789"
  }
}
```
会话也可用来为请求方法提供缺省数据。这是通过为会话对象的属性提供数据来实现的：
```python
s = requests.Session()
s.auth = ('user', 'pass')
s.headers.update({'x-test': 'true'})

# both 'x-test' and 'x-test2' are sent
s.get('http://httpbin.org/headers', headers={'x-test2': 'true'})
```
任何你传递给请求方法的字典都会与已设置会话层数据合并。方法层的参数覆盖会话的参数。

不过需要注意，就算使用了会话，方法级别的参数也不会被跨请求保持。下面的例子只会和第一个请求发送 cookie ，而非第二个：
```python
s = requests.Session()

r = s.get('http://httpbin.org/cookies', cookies={'from-my': 'browser'})
print(r.text)
# '{"cookies": {"from-my": "browser"}}'

r = s.get('http://httpbin.org/cookies')
print(r.text)
# '{"cookies": {}}'
```
如果你要手动为会话添加 cookie，就是用 Cookie utility 函数 来操纵 Session.cookies。

<!-- TODO: 单个 session 可以直接用 session.cookies 发送，但若要发送多个 cookie，必须使用 dict 封装后才能发送？待验证？？？？ -->

例：模拟登陆华工图书馆并保持登陆
```python
import requests

userid = 'xxx'#用户名
passwd = 'xxx'#密码

login_url = 'http://202.38.232.10/opac/servlet/opac.go'
list_url = 'http://202.38.232.10/opac/servlet/opac.go?cmdACT=loan.list'
data = {'cmdACT': 'mylibrary.login',
        'libcode': '', 'method': 'myinfo',
        'userid': userid, 'passwd': passwd}
userAgent = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/60.0.3072.0 Safari/537.36'

s = requests.session()
s.headers['User-Agent'] = userAgent
s.post(url=login_url, data=data)
r = s.get(list_url)
```

##### 2.1.1.7. SSL 证书验证

Requests 可以为 HTTPS 请求验证 SSL 证书，就像 web 浏览器一样。要想检查某个主机的 SSL 证书，你可以使用 verify 参数（默认情况下 verify 是 True)：
```python
r = requests.get('https://kyfw.12306.cn/otn/', verify=True)
# requests.exceptions.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:590)
```

##### 2.1.1.8. 使用代理

如果需要使用代理，你可以通过为任意请求方法提供 proxies 参数来配置单个请求
```python
proxies = {
  "http": "http://41.118.132.69:4433"
}
r = requests.post("http://httpbin.org/post", proxies=proxies)
```
也可以通过环境变量 `HTTP_PROXY` 和 `HTTPS_PROXY` 来配置代理
```python
export HTTP_PROXY="http://10.10.1.10:3128"
export HTTPS_PROXY="http://10.10.1.10:1080"
```
使用本地 socks5 代理：
```python
# 在本地运行 socks5 的代理服务器后（如 shadowsock）
proxies = {‘http’:’socks5://127.0.0.1:1080’, ‘https’: ’socks5://127.0.0.1:1080’}
url = ‘https://www.facebook.com’
# 利用代理访问被墙的网站
response = requests.get(url, timeout=10, proxies=proxies)
```

NOTE：若使用 http 代理，则发起请求时也要使用 http，否则不会走代理通道；

##### 2.1.1.9. 使用事件钩子

http://www.imooc.com/video/13091

#### 2.1.2. GET 请求

##### 2.1.2.1. 传递 URL 参数

- 方式一：

  手工构建 URL；

- 方式二：

  使用 get() 方法的 params 关键字参数传递 URL 参数，例如：
  ```python
  # 使用 params 关键字参数，以一个字典来提供所有 URL 参数
  # 字典里值为 None 的键都不会被添加到 URL 的查询字符串里
  payload = {'key1': 'value1', 'key2': 'value2', ‘key3’: None}
  r = requests.get("http://httpbin.org/get", params=payload)
  print(r.url)
  # http://httpbin.org/get?key2=value2&key1=value1

  # 将一个列表作为值传入
  payload = {'key1': 'value1', 'key2': ['value2', 'value3']}
  r = requests.get('http://httpbin.org/get', params=payload)
  print(r.url)
  # http://httpbin.org/get?key1=value1&key2=value2&key2=value3
  ```
  NOTE：

  - 若通过方式二传参，会自动对参数进行 URL 编码，作用相当于 urllib.urlencode；

  - get 方法的参数是 params，post 方法的参数是 data，注意区别；

#### 2.1.3. POST 请求

对于 POST 请求的参数，requests 提供了关键字参数 data，将 post 请求参数以一个字典的形式传入即可：

##### 2.1.3.1. 表单请求

```python
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.post("http://httpbin.org/post", data=payload)
```

##### 2.1.3.2. JSON 请求

```python
# 使用 json.dumps() 方法把表单数据序列化即可
url = 'http://httpbin.org/post'
payload = {'some': 'data'}
r = requests.post(url, data=json.dumps(payload))
```

##### 2.1.3.3. 上传文件

请求方法中提供了关键字参数 file：
```python
url = 'http://httpbin.org/post'
files = {'file': open('test.txt', 'rb')}
r = requests.post(url, files=files)
```
requests 支持流式上传（这允许你发送大的数据流或文件而无需先把它们读入内存），要使用流式上传，仅需为你的请求体提供一个类文件对象即可：
```python
with open('massive-body') as f:
    requests.post('http://some.url/streamed', data=f)
```

### 2.2. 响应

requests 库的 get()、post() 等方法发送请求后，将收到的响应 response 封装后作为返回值。

#### 2.2.1. 响应内容

- .text 属性返回响应报文的响应体内容；

- 响应头部
  使用。header 可以查看以一个 Python 字典形式展示的服务器响应头
  ```
  r.headers
  # 结果
  {
      'content-encoding': 'gzip',
      'transfer-encoding': 'chunked',
      'connection': 'close',
      'server': 'nginx/1.0.4',
      'x-runtime': '148ms',
      'etag': '"e1ca502697e5c9317743dc078f67693f"',
      'content-type': 'application/json'
  }
  ```
  根据 RFC 2616， HTTP 头部是大小写不敏感的。因此，我们可以使用任意大写形式来访问这些响应头字段：
  ```python
  >>> r.headers['Content-Type']
  'application/json'

  >>> r.headers.get('content-type')
  'application/json'
  ```

- .encoding 属性返回响应报文的编码格式；

  requests 会基于请求的 HTTP 头部对响应的编码作出有根据的推测，使用其推测的编码格式自动为 encoding 属性复制，但也可通过设置该属性从而手动指定编码格式；

- .status_code 属性返回响应状态码；

  为方便引用，Requests 还附带了一个内置的状态码查询对象：
  ```python
  r.status_code == requests.codes.ok
  # True
  ```

- .reason 属性返回响应状态码描述；

- .history 属性返回请求过程中的重定向；

- .url 属性返回响应来源的 url；

- .elapsed

#### 2.2.2. json 响应

Requests 中也有一个内置的 JSON 解码器：.json() 方法；
```python
r = requests.get('https://github.com/timeline.json')
print(r.json())
```

如果 JSON 解码失败， r.json() 就会抛出一个异常。例如，相应内容是 401 (Unauthorized)，尝试访问 r.json 将会抛出 ValueError: No JSON object could be decoded 异常。

#### 2.2.3. 原始套接字响应

在初始请求中设置 stream=True 后，使用。raw 属性可以获取来自服务器的原始套接字响应；
```python
r = requests.get('https://github.com/timeline.json', stream=True)
r.raw
# <requests.packages.urllib3.response.HTTPResponse object at 0x101194810>
r.raw.read(10)
# '\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x03'
```
一般情况下，应该以下面的模式将文本流保存到文件：
```python
with open(filename, 'wb') as fd:
    for chunk in r.iter_content(chunk_size):
        fd.write(chunk)
```

例：下载图片到本地
```python
# 方式一：没有关闭流对象
url = ‘xxxxx’
response = requests.get(url, headers=headers, stream=True)
with open(‘xxx.jpg’, ‘wb’) as fd:
	for chunk in response.iter_content(128):
		fd.write(chunk)

# 方式二：改进版，利用上下文自动关闭流对象
url = ‘xxxxx’
response = requests.get(url, headers=headers, stream=True)
from contextlib import closing
with closing(requests.get(url, headers=headers, stream=True)) as response:
	# 打开文件
	with open(‘xxx.jpg’, ‘wb’) as fd:
		# 每 128 写入一次
for chunk in response.iter_content(128):
			fd.write(chunk)
```

#### 2.2.4. 二进制响应

对于非文本响应，可以通过。content 属性以字节的方式访问请求响应体，Requests 会自动解码 gzip 和 deflate 传输编码的响应数据；
```python
# 以请求返回的二进制数据创建一张图片
from PIL import Image
from io import BytesIO
i = Image.open(BytesIO(r.content))
```

例：下载图片并保存到本地
```python
img_url = ‘xxxx’
response = request.get(img_url)
f = open(‘xxx.jpg’, ‘ab’)
f.write(response.content)
f.close()
```

#### 2.2.5. 响应 cookie

如果一个响应中包含了 cookie，那么我们可以利用 cookies 属性来拿到：
```python
url = 'http://example.com'
r = requests.get(url)
print(r.cookies)
print(r.cookies['example_cookie_name']) #若 cookie 不存在，会抛出异常
print(r.cookies.get(‘example_cookie_name’)) # 若 cookie 不存在，输出 none
```

### 2.3. 异常处理

在 requests.exceptions 中定义了 HTTP 请求过程中可能出现的大部分异常：

![image](http://img.cdn.firejq.com/jpg/2017/11/6/e76e4a1ef9470ee3cefec780e6431f2b.jpg)

使用示例：
```python
import requests
from requests import exceptions
try:
	response = requests.get(url, timeout=10)
	# 设置在异常信息中显示响应状态码
	response.raise_for_status()
except exceptions.Timeout as e:
	print(e.message)
except exceptions.HTTPError as e:
	print(e.message)
else:
	print(‘success’)
	print(response.status_code)
```

### 2.4. 获取源代码

http://www.imooc.com/article/17119?block_id=tuijian_wz

1. 正常情况下， 网站服务器给我们直接返回 html 源码。

2. html 源码里面会指明我们还需要去请求的其他文件如 css， js 和 image 等

3. 这些请求在浏览器获取到 html 之后浏览器会主动分析这些请求然后依次去请求，

4. 然后浏览器会去执行 js 和 css 等文件， 这时候 js 文件实际上是可以直接操作 html 内容的， js 可以修改我们的 html 源码。

5. 我们直接通过 requests.get 方法或者 urllib 获取到的 html 源码实际上是浏览器处理之前的原始 html

## 3. Refer Links

Requests2.10.0 中文文档：http://docs.python-requests.org/zh_CN/latest/user/quickstart.html

Requests2.10.0 API：http://docs.python-requests.org/zh_CN/latest/api.html

静觅 » Python 爬虫利器一之 Requests 库的用法：http://cuiqingcai.com/2556.html
