

# Python Requests NOTE

## 概念

Requests是一个封装了HTTP/1.1协议的Python库，可以看作是urllib库的更高级版本。

## 使用

### 请求

#### 发送请求

##### 请求方法

requests库提供了http所有的基本请求方式：
```python
r = requests.get("http://httpbin.org/get")
r = requests.post("http://httpbin.org/post")
r = requests.put("http://httpbin.org/put")
r = requests.delete("http://httpbin.org/delete")
r = requests.head("http://httpbin.org/get")
r = requests.options("http://httpbin.org/get")
```

这些方法向目标URL发送不同method的HTTP请求后，将响应response封装后返回。


##### 请求头部

使用requests库发送的请求的默认请求报文头部的User-Agent字段大概是这样：
```
User-Agent: python-requests/2.3.0 CPython/2.6.6 Windows/7
# 这样的User-Agent字段会直接被识别出不是使用浏览器访问
```

请求方法中提供了关键字参数headers，若想为请求添加 HTTP 头部，只要简单地传递一个 dict（字典） 给 headers 参数即可。
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


##### 发送cookie

请求方法中提供了关键字参数cookie：
```python
url = 'http://httpbin.org/cookies'
cookies = dict(cookies_are='working')
r = requests.get(url, cookies=cookies)
```


##### 重定向设置

默认情况下，除了 HEAD, Requests 会自动处理所有重定向。

可以使用响应对象的 history 方法来追踪重定向。

如果你使用的是GET、OPTIONS、POST、PUT、PATCH 或者 DELETE，那么你可以通过 allow_redirects 参数禁用重定向处理：
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


##### 超时设置

可以利用 timeout参数来配置最大请求时间（秒）：
```python
# 设置请求并返回响应的超时时间为10秒
requests.get('http://github.com', timeout=10)
# 设置请求超时时间为3秒，响应超时时间为7秒
requests.get('http://github.com', timeout=(3, 7))
```

注：timeout 仅对请求连接过程有效，与响应体的下载无关。


##### session对象

http://docs.python-requests.org/zh_CN/latest/user/advanced.html 

使用requests的请求方法每次请求其实都相当于发起了一个新的请求。也就是相当于我们每个请求都用了不同的浏览器单独打开的效果，它并不在一个会话当中。

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

requests库提供了session对象，可实现会话控制：
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

<!-- TODO: 单个session可以直接用session.cookies发送，但若要发送多个cookie，必须使用dict封装后才能发送？待验证？？？？ -->

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


##### SSL证书验证

Requests可以为HTTPS请求验证SSL证书，就像web浏览器一样。要想检查某个主机的SSL证书，你可以使用 verify 参数(默认情况下 verify 是 True)：
```python
r = requests.get('https://kyfw.12306.cn/otn/', verify=True)
# requests.exceptions.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:590)
```

##### 使用代理

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
使用本地socks5代理：
```python
# 在本地运行socks5的代理服务器后（如shadowsock）
proxies = {‘http’:’socks5://127.0.0.1:1080’, ‘https’: ’socks5://127.0.0.1:1080’}
url = ‘https://www.facebook.com’
# 利用代理访问被墙的网站
response = requests.get(url, timeout=10, proxies=proxies)
```


NOTE：若使用http代理，则发起请求时也要使用http，否则不会走代理通道；

##### 使用事件钩子

http://www.imooc.com/video/13091 


#### GET请求

##### 传递 URL 参数

- 方式一：
  
  手工构建 URL；

- 方式二：
  
  使用get()方法的params 关键字参数传递URL参数，例如：
  ```python
  # 使用 params 关键字参数，以一个字典来提供所有URL参数
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
  
  - 若通过方式二传参，会自动对参数进行URL编码，作用相当于urllib.urlencode；
  
  - get方法的参数是params，post方法的参数是data，注意区别；


#### POST请求

对于POST请求的参数，requests提供了关键字参数data，将post请求参数以一个字典的形式传入即可：

##### 表单请求

```python
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.post("http://httpbin.org/post", data=payload)
```

##### JSON 请求

```python
# 使用 json.dumps() 方法把表单数据序列化即可
url = 'http://httpbin.org/post'
payload = {'some': 'data'}
r = requests.post(url, data=json.dumps(payload))
```


##### 上传文件

请求方法中提供了关键字参数file：
```python
url = 'http://httpbin.org/post'
files = {'file': open('test.txt', 'rb')}
r = requests.post(url, files=files)
```
requests支持流式上传（这允许你发送大的数据流或文件而无需先把它们读入内存），要使用流式上传，仅需为你的请求体提供一个类文件对象即可：
```python
with open('massive-body') as f:
    requests.post('http://some.url/streamed', data=f)
```



### 响应

requests库的get()、post()等方法发送请求后，将收到的响应response封装后作为返回值。

#### 响应内容

- .text属性返回响应报文的响应体内容；

- 响应头部
  使用.header可以查看以一个 Python 字典形式展示的服务器响应头
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


- .encoding属性返回响应报文的编码格式；
  
  requests会基于请求的HTTP 头部对响应的编码作出有根据的推测，使用其推测的编码格式自动为encoding属性复制，但也可通过设置该属性从而手动指定编码格式；

- .status_code属性返回响应状态码；
  
  为方便引用，Requests还附带了一个内置的状态码查询对象：
  ```python
  r.status_code == requests.codes.ok
  # True
  ```

- .reason属性返回响应状态码描述；

- .history属性返回请求过程中的重定向；

- .url属性返回响应来源的url；

- .elapsed

#### json响应

Requests 中也有一个内置的 JSON 解码器：.json()方法；
```python
r = requests.get('https://github.com/timeline.json')
print(r.json())
```
如果 JSON 解码失败， r.json() 就会抛出一个异常。例如，相应内容是 401 (Unauthorized)，尝试访问 r.json 将会抛出 ValueError: No JSON object could be decoded 异常。

#### 原始套接字响应

在初始请求中设置 stream=True后，使用.raw属性可以获取来自服务器的原始套接字响应；
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
		# 每128写入一次
for chunk in response.iter_content(128):
			fd.write(chunk)
```


#### 二进制响应

对于非文本响应，可以通过.content属性以字节的方式访问请求响应体，Requests 会自动解码 gzip 和 deflate 传输编码的响应数据；
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

#### 响应cookie

如果一个响应中包含了cookie，那么我们可以利用 cookies 属性来拿到：
```python
url = 'http://example.com'
r = requests.get(url)
print(r.cookies)
print(r.cookies['example_cookie_name']) #若cookie不存在，会抛出异常
print(r.cookies.get(‘example_cookie_name’)) # 若cookie不存在，输出none
```




### 异常处理

在requests.exceptions中定义了HTTP请求过程中可能出现的大部分异常：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/6/e76e4a1ef9470ee3cefec780e6431f2b.jpg)

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


### 获取源代码

http://www.imooc.com/article/17119?block_id=tuijian_wz 	

1. 正常情况下， 网站服务器给我们直接返回html源码。 

2. html源码里面会指明我们还需要去请求的其他文件如css， js和image等 

3. 这些请求在浏览器获取到html之后浏览器会主动分析这些请求然后依次去请求， 

4. 然后浏览器会去执行js和css等文件， 这时候js文件实际上是可以直接操作html内容的， js可以修改我们的html源码。 

5. 我们直接通过requests.get方法或者urllib获取到的html源码实际上是浏览器处理之前的原始html






## Refer Links

Requests2.10.0中文文档：http://docs.python-requests.org/zh_CN/latest/user/quickstart.html 

Requests2.10.0 API：http://docs.python-requests.org/zh_CN/latest/api.html 

静觅 » Python爬虫利器一之Requests库的用法：http://cuiqingcai.com/2556.html 
