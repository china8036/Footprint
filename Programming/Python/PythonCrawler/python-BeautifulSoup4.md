- [Python BeautifulSoup 4 NOTE](#python-beautifulsoup-4-note)
  - [介绍](#%E4%BB%8B%E7%BB%8D)
    - [解析器](#%E8%A7%A3%E6%9E%90%E5%99%A8)
  - [使用](#%E4%BD%BF%E7%94%A8)
    - [构造](#%E6%9E%84%E9%80%A0)
    - [格式化输出](#%E6%A0%BC%E5%BC%8F%E5%8C%96%E8%BE%93%E5%87%BA)
    - [四大对象](#%E5%9B%9B%E5%A4%A7%E5%AF%B9%E8%B1%A1)
      - [Tag](#tag)
        - [对象属性](#%E5%AF%B9%E8%B1%A1%E5%B1%9E%E6%80%A7)
        - [对象方法](#%E5%AF%B9%E8%B1%A1%E6%96%B9%E6%B3%95)
      - [NavigableString](#navigablestring)
      - [BeautifulSoup](#beautifulsoup)
      - [Comment](#comment)
      - [其它](#%E5%85%B6%E5%AE%83)
    - [DOM Tree 遍历](#dom-tree-%E9%81%8D%E5%8E%86)
    - [DOM Tree 搜索](#dom-tree-%E6%90%9C%E7%B4%A2)
    - [CSS 选择器](#css-%E9%80%89%E6%8B%A9%E5%99%A8)
  - [Refer Links](#refer-links)

# Python BeautifulSoup 4 NOTE

## 介绍

Beautiful Soup 是一个可以从 HTML 或 XML 文件中提取数据的 Python 库。它能够通过你喜欢的转换器实现惯用的文档导航，查找，修改文档的方式。

### 解析器

| 解析器           | 使用方法                                     | 优势                                   | 劣势                                   |
| ------------- | ---------------------------------------- | ------------------------------------ | ------------------------------------ |
| Python 标准库     | BeautifulSoup(markup, "html.parser")     | Python 的内置标准库；  执行速度适中；  文档容错能力强；     | Python 2.7.3 or 3.2.2) 前 的版本中文档容错能力差； |
| lxml HTML 解析器 | BeautifulSoup(markup, "lxml")            | 速度快；  文档容错能力强；                       | 需要安装 C 语言库；                            |
| lxml XML 解析器  | BeautifulSoup(markup, ["lxml-xml"])     BeautifulSoup(markup, "xml") | 速度快；  唯一支持 XML 的解析器；                   | 需要安装 C 语言库；                            |
| html5lib      | BeautifulSoup(markup, "html5lib")        | 最好的容错性；  以浏览器的方式解析文档；  生成 HTML5 格式的文档； | 速度慢；  不依赖外部扩展；                       |

NOTE：

1.	推荐使用 lxml 作为解析器，因为效率更高

2.	如果一段 HTML 或 XML 文档格式不正确的话，那么在不同的解析器中返回的结果可能是不一样的

## 使用

### 构造

```python
# 传入一个文件句柄
soup = BeautifulSoup(open("index.html"))
# 传入一段字符串
soup = BeautifulSoup("<html>data</html>")
# BeautifulSoup 在解析之前默认会先把文本转换成 unicode 编码，可以用 from_encoding 指定编码
BeautifulSoup("<a></b>", "html.parser")
# Beautiful Soup 会选择最合适的解析器来解析文档，如果手动指定解析器则选择指定的解析器来解析文档
BeautifulSoup(“<a></a>”, "xml")

```

### 格式化输出

```python
soup.prettify()
# 指定输出编码
soup.prettify("gbk")
```

### 四大对象

Beautiful Soup 将复杂 HTML 文档转换成一个复杂的树形结构，每个节点都是 Python 对象，所有对象可以归纳为 4 种：Tag , NavigableString , BeautifulSoup , Comment；

#### Tag

Tag 即为 HTML 中的一个个标签，Tag 对象与 XML 或 HTML 原生文档中的 tag 命名相同：
```python
soup = BeautifulSoup(' <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>')
tag = soup.a
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>
soup.head
# <head><title>The Dormouse's story</title></head>
# 会将标签体内的所有子标签一并包含
# 注意：查找的是在所有内容中的第一个符合要求的标签
```

##### 对象属性

- name
  
  每个 tag 都有自己的名字，通过 tag.name 来获取；

  如果改变了 tag 的 name, 那将影响所有通过当前 Beautiful Soup 对象生成的 HTML 文档；
  ```python
  soup.name
  #[document] 
  # soup 对象本身比较特殊，它的 name 即为 [document]，对于其他内部标签，输出的值便为标签本身的名称
  soup.head.name
  #head
  ```

- attributes

  一个 html 标签会有多个属性，如<a href=”xxxxx” class=’blodest’/>中的 href、class，在 tag 对象中，通过字典 attrs 来存储这些属性 (attributes)。例如：tag[‘class’]、tag[‘href’]；

  - 获取 tag 的所有属性（返回一个字典）
    ```python
    soup.p.attrs
    #{'class': ['title'], 'name': 'dromouse'}
    ```

  -	获取 tag 的单个属性
    ```python
    # 使用 []
    soup.p['class']
    #['title']
    # 使用 get 方法
    soup.p.get('class')
    #['title']
    ```
  - tag 的属性可以被添加，删除或修改。例如：
    ```python
    tag['class'] = 'verybold'
    tag['id'] = 1
    tag
    # <blockquote class="verybold" id="1">Extremely bold</blockquote>

    del tag['class']
    del tag['id']
    tag
    # <blockquote>Extremely bold</blockquote>

    tag['class']
    # KeyError: 'class'
    print(tag.get('class'))
    # None
    ```
  -	html 中存在可以包含多个值的属性，如 accept-charset , headers 等，在 Beautiful Soup 中多值属性的返回类型是 list。
    
    例如：
    ```python
    css_soup = BeautifulSoup('<p class="body strikeout"></p>')
    css_soup.p['class']
    # ["body", "strikeout"]

    css_soup = BeautifulSoup('<p class="body"></p>')
    css_soup.p['class']
    # ["body"]
    ```
    如果某个属性看起来好像有多个值，但在任何版本的 HTML 定义中都没有被定义为多值属性，那么 Beautiful Soup 会将这个属性作为字符串返回
    ```python
    id_soup = BeautifulSoup('<p id="my id"></p>')
    id_soup.p['id']
    # 'my id'
    ```
    如果转换的文档是 XML 格式，那么 tag 中不包含多值属性如果转换的文档是 XML 格式，那么 tag 中不包含任何多值属性。

- contents

  tag 的 .content 属性可以将 tag 的直接子节点以列表的方式输出；
  ```python
  soup.head.contents 
  #[<title>The Dormouse's story</title>]
  ```

- children

  tag 的 .children 属性可以将 tag 的直接子节点以 list 生成器的方式输出；
  ```python
  soup.head.children
  #<listiterator object at 0x7f71457f5710>
  for child in  soup.body.children:
  print child
  ```

- descendants

  .contents 和 .children 属性仅包含 tag 的直接子节点，.descendants 属性可以对所有 tag 的子孙节点进行递归循环，和 children 类似，我们也需要遍历获取其中的内容：
  ```python
  for child in soup.descendants:
  print child
  ```

- string、strings、stripped_strings
  
  - string

  如果一个标签里面没有标签了，那么 .string 就会返回标签里面的内容。如果标签里面只有唯一的一个标签了，那么 .string 也会返回最里面的内容；
  ```python
  soup.head.string
  #The Dormouse's story
  soup.title.string
  #The Dormouse's story
  ```
  如果 tag 包含了多个子节点，tag 就无法确定，string 方法应该调用哪个子节点的内容，.string 的输出结果是 None；
  ```python
  soup.html.string
  # None
  ```
  - strings
    
  获取多个内容，不过需要遍历获取；

  - stripped_strings

  输出的字符串中可能包含了很多空格或空行，使用 .stripped_strings 可以去除多余空白内容；

- parent：获取节点的直接父节点；

  parents：获取节点的所有父节点；

- `next_sibling、previous_sibling`：获取节点的下一个、上一个兄弟节点；

  注意：实际文档中的 tag 的 `.next_sibling` 和 `.previous_sibling` 属性通常是字符串或空白，因为空白或者换行也可以被视作一个节点，所以得到的结果可能是空白或者换行

  - `next_siblings、previous_siblings`：获取全部兄弟节点；

- `next_element、previous_element`

  与 `.next_sibling`、 `.previous_sibling` 不同，它并不是针对于兄弟节点，而是在所有节点，不分层次；
  - `next_elements`  `.previous_elements`
    
    通过 `.next_elements` 和 `.previous_elements` 的迭代器就可以向前或向后访问文档的解析内容，就好像文档正在被解析一样；

  
##### 对象方法

- find_all( name , attrs , recursive , text , **kwargs )

  find_all() 方法搜索当前 tag 的所有 tag 子节点（包括子节点的子节点）, 并判断是否符合过滤器的条件。
  参数说明：

  -	name：

    查找所有名字为 name 的 tag；

    字符串对象会被自动忽略掉，name 参数可以是字符串、正则表达式、列表、True、方法；

    - 传字符串：

    最简单的过滤器是字符串。在搜索方法中传入一个字符串参数，Beautiful Soup 会查找与字符串完整匹配的内容；
    ```python
    # 查找文档中所有的<b>标签
    soup.find_all('b')
    # [<b>The Dormouse's story</b>]
    ```
    
    - 传正则表达式：
    ```python
    如果传入正则表达式作为参数，Beautiful Soup 会通过正则表达式的 match() 来匹配内容。
    # 找出所有以 b 开头的标签，这表示<body>和<b>标签都应该被找到
    import re
    for tag in soup.find_all(re.compile("^b")):
        print(tag.name)
    # body
    # b
    ```

    - 传列表
    
    如果传入列表参数，Beautiful Soup 会将与列表中任一元素匹配的内容返回： 
    ```python
    soup.find_all(["a", "b"])
    # [<b>The Dormouse's story</b>,
    #  <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
    ```

    - 传 True
    
    True 可以匹配任何值，下面代码查找到所有的 tag, 但是不会返回字符串节点
    ```python
    for tag in soup.find_all(True):
        print(tag.name)
    # html
    # head
    # title
    # body
    # p
    # b
    # p
    # a
    ```

    - 传方法
    
    如果没有合适过滤器，那么还可以定义一个方法，方法只接受一个元素参数，如果这个方法返回 True 表示当前元素匹配并且被找到，如果不是则反回 False
    ```python
    # 下面方法校验了当前元素，如果包含 class 属性却不包含 id 属性，那么将返回 True:
    def has_class_but_no_id(tag):
        return tag.has_attr('class') and not tag.has_attr('id')
    # 将这个方法作为参数传入 find_all() 方法，将得到所有<p>标签：
    soup.find_all(has_class_but_no_id)
    # [<p class="title"><b>The Dormouse's story</b></p>,
    #  <p class="story">Once upon a time there were...</p>,
    #  <p class="story">...</p>]
    ```

  - attrs
    
    查找所有属性包含 attrs 的 tag；
    
    如果一个关键字参数的关键字不是函数原型中的参数名，搜索时会把该参数当作指定名字 tag 的属性来搜索，如果包含一个名字为 id 的参数，Beautiful Soup 会搜索每个 tag 的”id”属性：
    ```python
    soup.find_all(id='link2')
    # [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
    # 使用多个指定名字的参数可以同时过滤 tag 的多个属性
    soup.find_all(href=re.compile("elsie"), id='link1')
    # [<a class="sister" href="http://example.com/elsie" id="link1">three</a>]
    在这里我们想用 class 过滤，不过 class 是 python 的关键词，这怎么办？加个下划线就可以
    soup.find_all("a", class_="sister")
    # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
    #  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
    ```
    有些 tag 属性在搜索不能使用，比如 HTML5 中的 data-* 属性
    ```python
    data_soup = BeautifulSoup('<div data-foo="value">foo!</div>')
    data_soup.find_all(data-foo="value")
    # SyntaxError: keyword can't be an expression
    ```
    但是可以通过 find_all() 方法的 attrs 参数定义一个字典参数来搜索包含特殊属性的 tag
    ```python
    data_soup.find_all(attrs={"data-foo": "value"})
    # [<div data-foo="value">foo!</div>]
    ```
  
  - text
  
    查找标签体文本为 text 的 tag；
    
    与 name 参数一样，text 参数接受 字符串 , 正则表达式 , 列表，True；
    ```python
    soup.find_all(text="Elsie")
    # [u'Elsie']

    soup.find_all(text=["Tillie", "Elsie", "Lacie"])
    # [u'Elsie', u'Lacie', u'Tillie']

    soup.find_all(text=re.compile("Dormouse"))
    [u"The Dormouse's story", u"The Dormouse's story"]
    ```

  - limit
  
    find_all() 方法返回全部的搜索结构，如果文档树很大那么搜索会很慢。如果我们不需要全部结果，可以使用 limit 参数限制返回结果的数量。效果与 SQL 中的 limit 关键字类似，当搜索到的结果数量达到 limit 的限制时，就停止搜索返回结果；
    ```python
    soup.find_all("a", limit=2)
    # [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
    #  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
    ```

  - recursive
  
    调用 tag 的 find_all() 方法时，Beautiful Soup 默认会检索当前 tag 的所有子孙节点，如果只想搜索 tag 的直接子节点，可以使用参数 recursive=False；
    ```python
    soup.html.find_all("title")
    # [<title>The Dormouse's story</title>]
    soup.html.find_all("title", recursive=False)
    # []
    ```

- find( name , attrs , recursive , text , **kwargs )
它与 find_all() 方法唯一的区别是 find_all() 方法的返回结果是值包含一个元素的列表，而 find() 方法直接返回结果；

- find_parents()  find_parent()
find_all() 和 find() 只搜索当前节点的所有子节点，孙子节点等。find_parents() 和 find_parent() 用来搜索当前节点的父辈节点，搜索方法与普通 tag 的搜索方法相同，搜索文档搜索文档包含的内容；

- find_next_siblings()  find_next_sibling()
这 2 个方法通过 .next_siblings 属性对当 tag 的所有后面解析的兄弟 tag 节点进行迭代，find_next_siblings() 方法返回所有符合条件的后面的兄弟节点，find_next_sibling() 只返回符合条件的后面的第一个 tag 节点；

- find_previous_siblings()  find_previous_sibling()
这 2 个方法通过 .previous_siblings 属性对当前 tag 的前面解析的兄弟 tag 节点进行迭代，find_previous_siblings() 方法返回所有符合条件的前面的兄弟节点，find_previous_sibling() 方法返回第一个符合条件的前面的兄弟节点；

- find_all_next()  find_next()
这 2 个方法通过 .next_elements 属性对当前 tag 的之后的 tag 和字符串进行迭代，find_all_next() 方法返回所有符合条件的节点，find_next() 方法返回第一个符合条件的节点；

- find_all_previous() 和 find_previous()
这 2 个方法通过 .previous_elements 属性对当前节点前面的 tag 和字符串进行迭代，find_all_previous() 方法返回所有符合条件的节点，find_previous() 方法返回第一个符合条件的节点；

- has_attr(xxx)：判断该标签是否定义了 xxx 属性，返回布尔值；

#### NavigableString

Beautiful Soup 用 NavigableString 类来包装 tag 中的字符串

```python
# <a>I am the string</a>
# 标签包含的字符串
tag.string
# 通过 unicode() 方法可以将其转换为 unicode 字符串
unicode_string = unicode(tag.string)
# tag 中包含的字符串不能编辑，但是可以被替换成其它的字符串
tag.string.replace_with("No longer bold")
```

NOTE：

1.	NavigableString 对象不支持 .contents 或 .string 属性或 find() 方法；

2.	如果想在 Beautiful Soup 之外使用 NavigableString 对象，需要调用 unicode() 方法，将该对象转换成普通的 Unicode 字符串，否则就算 Beautiful Soup 已方法已经执行结束，该对象的输出也会带有对象的引用地址。这样会浪费内存；

#### BeautifulSoup

BeautifulSoup 对象表示的是一个文档的全部内容。**大部分时候，可以把它当作 Tag 对象，它支持 遍历文档树 和 搜索文档树 中描述的大部分的方法**

BeautifulSoup 对象包含了一个值为 “[document]” 的特殊属性 .name；

```python
soup.name 
# [document]
soup.attrs 
#{} 空字典
```

#### Comment

Comment 对象是一个特殊类型的 NavigableString 对象，包装了 html/xml 中的文档注释部分；

```python
soup = BeautifulSoup("<b><!--Hey, buddy. Want to buy a used parser?--></b>")
comment = soup.b.string
# Hey, buddy. Want to buy a used parser?
# 输出内容不包含注释符，因此如果不好好处理，可能会对我们的文本处理造成意想不到的麻烦
type(comment)
# <class 'bs4.element.Comment'>
# 最好先判断类型是否为 Comment 类型，然后再进行其他操作
```

#### 其它

### DOM Tree 遍历

参见 Tag 对象的属性和方法；

### DOM Tree 搜索

参见 Tag 对象的属性和方法；

### CSS 选择器

在写 CSS 时，**标签名不加任何修饰，类名前加。，id 名前加 #**，在这里也可以利用类似的方法来筛选元素，用到的方法是 soup.select()，返回类型是 list，可以遍历形式输出，然后用 get_text() 方法来获取它的内容；

- 通过标签名查找
  ```python
  soup.select('title') 
  #[<title>The Dormouse's story</title>]
  ```

- 通过类名查找
  ```python
  soup.select('.sister')
  #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
  ```

- 通过 id 名查找
  ```python
  soup.select('#link1')
  #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
  ```

- 组合查找：多个条件用空格分开
  ```python
  soup.select('p #link1')
  #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>] 直接子标签查找
  ```

- 直接子标签查找
  ```python
  soup.select("head > title")
  #[<title>The Dormouse's story</title>]
  ```

- 属性查找

  查找时还可以加入属性元素，属性需要用中括号括起来，注意属性和标签属于同一节点，所以中间不能加空格，否则会无法匹配到。
  ```python
  soup.select('a[class="sister"]')
  # [<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, 
  # <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, 
  # <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

  soup.select('p a[href="http://example.com/elsie"]')
  #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
  ```

## Refer Links

Beautiful Soup 4.4.0 文档：https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/# 

静觅 » Python 爬虫利器二之 Beautiful Soup 的用法 http://cuiqingcai.com/1319.html 
