- [Spring MVC Note](#spring-mvc-note)
    - [@RequestBody](#requestbody)
    - [@ResponseBody](#responsebody)
        - [响应 JSON 数据](#%E5%93%8D%E5%BA%94-json-%E6%95%B0%E6%8D%AE)
        - [响应 XML 数据](#%E5%93%8D%E5%BA%94-xml-%E6%95%B0%E6%8D%AE)
        - [Accpect 与 produces](#accpect-%E4%B8%8E-produces)
        - [ContentType 与 consumes](#contenttype-%E4%B8%8E-consumes)
        - [响应媒体类型](#%E5%93%8D%E5%BA%94%E5%AA%92%E4%BD%93%E7%B1%BB%E5%9E%8B)
    - [HttpMessageConverter](#httpmessageconverter)
        - [自定义 HttpMessageConverter](#%E8%87%AA%E5%AE%9A%E4%B9%89-httpmessageconverter)
    - [@RequestMapping](#requestmapping)
        - [支持的方法参数类型](#%E6%94%AF%E6%8C%81%E7%9A%84%E6%96%B9%E6%B3%95%E5%8F%82%E6%95%B0%E7%B1%BB%E5%9E%8B)
            - [HttpServlet](#httpservlet)
            - [WebRequest](#webrequest)
            - [InputStream、OutputStream、Reader、Writer](#inputstream%E3%80%81outputstream%E3%80%81reader%E3%80%81writer)
            - [@PathVariable、@RequestParam、@CookieValue、@RequestHeader](#pathvariable%E3%80%81requestparam%E3%80%81cookievalue%E3%80%81requestheader)
            - [@ModelAttribute](#modelattribute)
            - [Model 和 ModelMap](#model-%E5%92%8C-modelmap)
            - [实体类](#%E5%AE%9E%E4%BD%93%E7%B1%BB)
            - [MultipartFile](#multipartfile)
            - [Errors 和 BindingResult](#errors-%E5%92%8C-bindingresult)
        - [支持的返回类型](#%E6%94%AF%E6%8C%81%E7%9A%84%E8%BF%94%E5%9B%9E%E7%B1%BB%E5%9E%8B)
    - [@SessionAttributes](#sessionattributes)
    - [处理 PUT/DELETE/PATCH 请求](#%E5%A4%84%E7%90%86-putdeletepatch-%E8%AF%B7%E6%B1%82)
    - [Refer Links](#refer-links)

# Spring MVC Note 

## @RequestBody 

@RequestBody 将 HTTP 请求正文转换为适合的 HttpMessageConverter 对象。



- 作用： 

    - 该注解用于读取Request请求的body部分数据，使用系统默认配置的HttpMessageConverter进行解析，然后把相应的数据绑定到要返回的对象上；

    - 再把HttpMessageConverter返回的对象数据绑定到 controller中方法的参数上。

- 使用：
    - GET、POST方式提时， 根据request header Content-Type的值来判断:
        - application/x-www-form-urlencoded， 可选（即非必须，因为这种情况的数据@RequestParam, @ModelAttribute也可以处理，当然@RequestBody也能处理）；
        - multipart/form-data, 不能处理（即使用@RequestBody不能处理这种格式的数据）；
        - 其他格式， 必须（其他格式包括application/json, application/xml等。这些格式的数据，必须使用@RequestBody来处理）(POST 模式下，使用 @RequestBody 绑定请求对象，Spring 会帮你进行协议转换，将 Json、Xml 协议转换成你需要的对象。)；

    - PUT方式提交时， 根据request header Content-Type的值来判断:
        - application/x-www-form-urlencoded， 必须；
        - multipart/form-data, 不能处理；
        - 其他格式， 必须；

- 说明：
    
    request的body部分的数据编码格式由header部分的Content-Type指定；

## @ResponseBody

@ResponseBody 将内容或对象作为 HTTP 响应正文返回，并调用适合 HttpMessageConverter 的 Adapter 转换对象，写入输出流。

GET 模式下，可以使用 @ResponseBody 注解，Controller 返回的 Java 对象可以自动被转换成对应的 XML 或者 JSON 数据。同时使用 @PathVariable 绑定输入参数，非常适合 Restful 风格，且隐藏了参数与路径的关系，可以提升网站的安全性，静态化页面，降低恶意攻击风险。

### 响应 JSON 数据

例：

如果想要从 spring 获得一个 json 形式返回值，操作起来是非常容易的。首先定义一个实体类：
```java
public class Book {
    private Integer id;
    private String bookName;
}
```
接着定义一个后端端点：
```java
@RestController
public class BookController {
    @GetMapping(value = "/book/{bookId}")
    public Book getBook(@PathVariable("bookId") Integer bookId) {
        return new Book(bookId, "book" + bookId);
    }
}
```
在 RestController 中，相当于给所有的 xxxMapping 端点都添加了 @ResponseBody 注解，不返回视图，只返回数据。使用 http 工具访问这个后端端点 localhost:8080/book/2，便可以得到如下的响应：
```json
{
    "id": 2,
    "bookName": "book2"
}
```

除此之外，可用过 produces 参数显示指定返回 json 格式：
```java
@RestController
public class BookController {
    @GetMapping(value = "/book/{bookId}", produces = {"application/json;charset=UTF-8"})
    public Book getBook(@PathVariable("bookId") Integer bookId) {
        return new Book(bookId, "book" + bookId);
    }
}
```
则控制器会直接返回 POJO，POJO 自动转化为 json 格式。

### 响应 XML 数据

**JSON 可直接自动转换，而 XML 需要在 entity 中标注 @XmlRootElement 和 @XmlElement 等**。

如果我们需要将 Book 对象以 XML 的形式返回，则需要给 Book 对象添加 @XmlRootElement 注解，让 spring 内部能够解析 XML 对象。
```java
@XmlRootElement
public class Book {
    private Integer id;
    private String bookName;
}
```
在我们未对 web 层的 BookController 做任何改动之前，尝试访问 localhost:8080/book/2 时，会发现得到的结果仍然是前面的 JSON 对象。这是因为 Book 对象如今既可以被解析为 XML，也可以被解析为 JSON，想让 RestController 返回 XML 解析结果，有以下实现方法：

### Accpect 与 produces

- http 客户端使用 **Accpect** 指定接收的返回结果类型

  http 协议中，可以给请求头添加 Accept 属性
  ```
  GET /book/2 HTTP/1.1
  Host: localhost:8080
  Accept: application/xml
  ```
  响应内容如下：
  ```xml
  <?xml version="1.0" encoding="UTF-8" standalone="yes"?>
  <book>
      <bookName>book2</bookName>
      <id>2</id>
  </book>
  ```

  - 解析：

    Accpect 包含在 http 协议的请求头中，其本身代表着客户端发起请求时，期望返回的响应结果的媒体类型。如果服务端可能返回多个媒体类型，则可以通过 Accpect 指定具体的类型。

- 在 RestController 后端端点中使用 **produces** 指定返回类型

  ```java
  @RestController
  public class BookController {
      @GetMapping(value = "/book/{bookId}", produces = {"application/xml"})
      public Book getBook(@PathVariable("bookId") Integer bookId) {
          return new Book(bookId, "book" + bookId);
      }
  }
  ```
  此时即使将请求中的 Accept: application/xml 去除，依旧可以返回上述的 XML 结果。

  通常情况下，我们的服务端返回的形式一般是固定的，即限定了是 JSON，XML 中的一种，不建议依赖于客户端添加 Accept 的信息，而是在服务端限定 produces 类型。

  - produces

    produces 是 Spring 为我们提供的注解参数，代表着服务端能够支持返回的媒体类型，我们注意到 produces 后跟随的是一个数组类型，也就意味着服务端支持多种媒体类型的响应。
  
  实例：通过 produce 解决 @Response 返回 json 时出现中文乱码的问题：
  
  在 RequestMapping 的注解参数中加上 produces = "text/html;charset=UTF-8"即可，此处 produces 的参数值就是 HTTP Response 的 Content-Type 字段值。

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/c52b495ae5f8fc7e581f8369d3899208.jpg)

  <!-- TODO 那 HttpServletRequest 和 HttpServletResponse 的 setEncoding 方法是设置哪里的？-- 测试结果：设置了 UTF-8 之后 Content-Type 不随之改变。 -->

在接口交互时，最良好的对接方式，当然是客户端指定 Accpect，同时服务端也指定 produces，这样可以避免模棱两可的请求响应，避免出现意想不到的对接结果。

### ContentType 与 consumes

和 Accpect&produces 相反，这两个参数是与用于限制请求的。

- ContentType 包含在 http 协议的请求头中，其本身代表着客户端发起请求时，告知服务端自己的请求媒体类型是什么。

- consumes 是 Spring 为我们提供的注解参数，代表着服务端能够支持处理的请求媒体类型，同样是一个数组，意味着服务端支持多种媒体类型的请求。

一般而言，consumes 与 produces 对请求响应媒体类型起到的限制作用，即：窄化。

### 响应媒体类型

常见的 HTTP 响应媒体类型：

| 媒体类型                               | 含义         |
| ---------------------------------- | ---------- |
| text/html                          | HTML 格式     |
| text/plain                         | 纯文本格式      |
| text/xml, application/xml          | XML 数据格式    |
| application/json                   | JSON 数据格式   |
| image/gif                          | gif 图片格式    |
| image/png                          | png 图片格式    |
| application/octet-stream           | 二进制流数据     |
| application/ x-www-form-urlencoded | form 表单数据   |
| multipart/form-data                | 含文件的 form 表单 |

在 JAVA 中，提供了 MediaType 这样的抽象，来与 http 的媒体类型进行对应。‘/’之前的名词，如 text，application 被称为类型（type），‘/’之后被称为子类型 (subType)。

## HttpMessageConverter

HttpMessageConverter 是一个顶级接口，该接口完成了 spring 中众多实体类等复杂类型的数据转换以及与媒体类型的对应。

```java
public interface HttpMessageConverter<T> {
    boolean canRead(Class<?> var1, MediaType var2);
    boolean canWrite(Class<?> var1, MediaType var2);
    List<MediaType> getSupportedMediaTypes();
    T read(Class<? extends T> var1, HttpInputMessage var2) throws IOException, HttpMessageNotReadableException;
    void write(T var1, MediaType var2, HttpOutputMessage var3) throws IOException, HttpMessageNotWritableException;
}
```

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/c92c4d1be49ca0d60b021006efbda623.jpg)

对于添加了 @RequestBody 和 @ResponseBody 注解的后端端点，都会经历由 HttpMessageConverter 进行的数据转换的过程。而在 Spring 启动之初，就已经有一些默认的转换器被注册了。通过在 RequestResponseBodyMethodProcessor 中打断点，我们可以获取到一个 converters 列表：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/37b8d8680f2e5bab95e513ca34d3c78e.jpg)

### 自定义 HttpMessageConverter

参考：https://lexburner.github.io/2017/08/30/%E8%A7%A3%E6%9E%90Spring%E4%B8%AD%E7%9A%84ResponseBody%E5%92%8CRequestBody/

## @RequestMapping

@RequestMapping 可用于标记处理器方法支持的**方法参数**和**返回类型**。

### 支持的方法参数类型

#### HttpServlet

HttpServlet 对象，主要包括 HttpServletRequest 、HttpServletResponse 和 HttpSession 对象。 这些参数 Spring 在调用处理器方法的时候会自动给它们赋值，所以当在处理器方法中需要使用到这些对象的时候，可以直接在方法上给定一个方法参数的申明，然后在方法体里面直接用就可以了。但是有一点需要注意的是在使用 HttpSession 对象的时候，如果此时 HttpSession 对象还没有建立起来的话就会有问题。


#### WebRequest

Spring 自己的 WebRequest 对象。 使用该对象可以访问到存放在 HttpServletRequest 和 HttpSession 中的属性值。

#### InputStream、OutputStream、Reader、Writer

InputStream 、OutputStream 、Reader 和 Writer 。 InputStream 和 Reader 是针对 HttpServletRequest 而言的，可以从里面取数据；OutputStream 和 Writer 是针对 HttpServletResponse 而言的，可以往里面写数据。

#### @PathVariable、@RequestParam、@CookieValue、@RequestHeader

使用 @PathVariable 、@RequestParam 、@CookieValue 和 @RequestHeader 标记的参数。

#### @ModelAttribute

使用 @ModelAttribute 标记的参数。

例：Activity是一个Java Bean
```java
	@PostMapping()
	public ResponseEntity<?> activityCreate(@ModelAttribute Activity activity) {
		try {
			saveUploadedFiles(activity.getActivityPoster());
		} catch (IOException e) {
			return new ResponseEntity<>(HttpStatus.BAD_REQUEST);
		}

		return new ResponseEntity<>("Successfully uploaded!", HttpStatus.OK);
	}
```

####  Model 和 ModelMap 

java.util.Map 、Spring 封装的 Model 和 ModelMap 。 这些都可以用来封装模型数据，用来给视图做展示。


#### 实体类

实体类。 可以用来接收上传的参数。

#### MultipartFile

Spring 封装的 MultipartFile 。 用来接收上传文件的。

#### Errors 和 BindingResult

Spring 封装的 Errors 和 BindingResult 对象。 这两个对象参数必须紧接在需要验证的实体对象参数之后，它里面包含了实体对象的验证结果。

### 支持的返回类型
参见：http://blog.csdn.net/mafan121/article/details/45060135

（SpringMVC 支持的返回值类型有：ModelAndView,Model,ModelMap,Map,View,void,Sting）

- 一个包含模型和视图的 ModelAndView 对象。 

- 一个模型对象，这主要包括 Spring 封装好的 Model 和 ModelMap ，以及 java.util.Map ，当没有视图返回的时候视图名称将由 RequestToViewNameTranslator 来决定。

- 一个 View 对象。这个时候如果在渲染视图的过程中模型的话就可以给处理器方法定义一个模型参数，然后在方法体里面往模型中添加值。

- 一个 String 字符串。这往往代表的是一个视图名称。这个时候如果需要在渲染视图的过程中需要模型的话就可以给处理器方法一个模型参数，然后在方法体里面往模型中添加值就可以了。

- 返回值是 void 。这种情况一般是我们直接把返回结果写到 HttpServletResponse 中了，如果没有写的话，那么 Spring 将会利用 RequestToViewNameTranslator 来返回一个对应的视图名称。如果视图中需要模型的话，处理方法与返回字符串的情况相同。

- 如果处理器方法被注解 @ResponseBody 标记的话，那么处理器方法的任何返回类型都会通过 HttpMessageConverters 转换之后写到 HttpServletResponse 中，而不会像上面的那些情况一样当做视图或者模型来处理。

- 除以上几种情况之外的其他任何返回类型都会被当做模型中的一个属性来处理，而返回的视图还是由 RequestToViewNameTranslator 来决定，添加到模型中的属性名称可以在该方法上用 `@ModelAttribute(“attributeName”)` 来定义，否则将使用返回类型的类名称的首字母小写形式来表示。使用 `@ModelAttribute` 标记的方法会在 `@RequestMapping` 标记的方法执行之前执行。

- [`ResponseEntity<T>`对象](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html)

TODO：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/10/30/e4466d1a68c785fd36106ce04b925392.jpg)

SpringMVC 未指定跳转页面时，有 @ResponseBody 注解则会根据请求的路径最后 / 后的字段去掉后缀作为跳转的文件名；

## @SessionAttributes

@SessionAttributes 一般是标记在 Controller 类上的，可以通过名称、类型或者名称加类型的形式来指定哪些属性是需要存放在 session 中的。






## 处理 PUT/DELETE/PATCH 请求

浏览器的 form 表单只支持 GET 和 POST 请求，而 DELETE 和 PUT 请求并不支持。为了解决这个问题，Spring MVC 提供了一个 HiddenHttpMethodFilter，可以将带有_method 参数的 POST 请求转换为 PUT 或 DELETE 请求。使用 Spring Boot 时，它已经被默认配置生效了。

例：
```html
<!-- 发送 put 请求 -->
<form action="/blogs/1" method="post">
    <input type="hidden" name="_method" value="put" >
    <input type="submit" value="更新">
</form>
    
<!-- 发送 delete 请求 -->
<form action="/blogs/1" method="post">
    <input type="hidden" name="_method" value="delete" >
    <input type="submit" value="删除">
</form>
```

这样的 POST 请求，到达 Spring 后，会经过 HiddenHttpMethodFilter 的处理，然后就会转化为 PUT 和 DELETE 请求被 Spring MVC 处理，匹配 @PutMapping 和 @DeleteMapping。

需要注意的是，_method 字段只是用于提示 Spring MVC 框架我们发送的请求应该作为什么方法来处理，所以将其设为 hidden，让用户不可见。

## Refer Links

解析 Spring 中的 ResponseBody 和 RequestBody：  https://lexburner.github.io/2017/08/30/%E8%A7%A3%E6%9E%90Spring%E4%B8%AD%E7%9A%84ResponseBody%E5%92%8CRequestBody/
