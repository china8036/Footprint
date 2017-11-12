- [CSS 选择器](#css-%E9%80%89%E6%8B%A9%E5%99%A8)
  - [选择器种类](#%E9%80%89%E6%8B%A9%E5%99%A8%E7%A7%8D%E7%B1%BB)
    - [基本选择器](#%E5%9F%BA%E6%9C%AC%E9%80%89%E6%8B%A9%E5%99%A8)
    - [组合选择器](#%E7%BB%84%E5%90%88%E9%80%89%E6%8B%A9%E5%99%A8)
    - [属性选择器](#%E5%B1%9E%E6%80%A7%E9%80%89%E6%8B%A9%E5%99%A8)
    - [伪类选择器](#%E4%BC%AA%E7%B1%BB%E9%80%89%E6%8B%A9%E5%99%A8)
    - [伪元素选择器](#%E4%BC%AA%E5%85%83%E7%B4%A0%E9%80%89%E6%8B%A9%E5%99%A8)
  - [CSS 选择器优先级](#css-%E9%80%89%E6%8B%A9%E5%99%A8%E4%BC%98%E5%85%88%E7%BA%A7)
  - [Refer Links](#refer-links)

  
# CSS 选择器

## 选择器种类

### 基本选择器

| 选择器         | 含义                                  |
|-------------|-------------------------------------|
| *       | 通用元素选择器，匹配任何元素                      |
| p       | 标签选择器，匹配所有使用 E 标签的元素                |
| .info   | class 选择器，匹配所有 class 属性中包含 info 的元素 |
| #footer | id 选择器，匹配所有 id 属性等于 footer 的元素      |

实例：
```css
* { margin:0; padding:0; }

p { font-size:2em; }

.info { background:#ff0; }

p.info { background:#ff0; }

p.info.error { color:#900; font-weight:bold; }

#info { background:#ff0; }

p#info { background:#ff0; }
```

### 组合选择器

| 选择器   | 含义                                        |
|-----------|-------------------------------------------|
| E, F   | 多元素选择器，同时匹配所有 E 元素或 F 元素，E 和 F 之间用逗号分隔    |
| E F   | 后代元素选择器，匹配所有属于 E 元素任意后代（包括所有子孙节点）的 F 元素，E 和 F 之间用空格分隔 |
| E > F | 子元素选择器，匹配所有 E 元素的子元素 F （只包括直接孩子）                   |
| E + F | 毗邻元素选择器，匹配所有紧随 E 元素之后的同级元素 F              |
| E ~ F | 匹配任何在 E 元素之后的同级 F 元素(不用紧随) |

实例：
```css
div p { color:#f00; }

#nav li { display:inline; }

#nav a { font-weight:bold; }

div > strong { color:#f00; }

p + p { color:#f00; }

html > body table + ul {margin-top:20px;}

p ~ ul { background:#ff0; }
```

### 属性选择器


| 选择器         | 含义      |
|---------------|-----------|
| E[att]      | 匹配所有具有 att 属性的 E 元素，不考虑它的值。（注意：E 在此处可以省略，比如"[cheacked]"。以下同。） |                                                                                                    |
| E[att=val]  | 匹配所有 att 属性等于"val"的 E 元素                                      |                                                                                                    |
| E[att~=val] | 匹配所有 att 属性具有多个空格分隔的值、其中一个值等于"val"的 E 元素                      |                                                                                                    |
| E[att\|=val]  | 匹配所有 att 属性具有多个连字号分隔（hyphen-separated）的值，其中一个值以"val"开头的 E 元素，主要用于 lang 属性，比如"en"、"en-us"、"en-gb"等等 |
| E[att^="val"] | 属性 att 的值以"val"开头的元素   |
| E[att$="val"] | 属性 att 的值以"val"结尾的元素   |
| E[att*="val"] | 属性 att 的值包含"val"字符串的元素 |

例：
```css
p[title] { color:#f00; }

div[class=error] { color:#f00; }

td[headers~=col1] { color:#f00; }

p[lang|=en] { color:#f00; }

blockquote[class=quote](cite) { color:#f00; }

div[id^="nav"] { background:#ff0; }
```



### 伪类选择器

| 选择器           | 含义                           |
|---------------|---------------------------------|
| E:first-child | 匹配父元素的第一个子元素          |
| E:link        | 匹配所有未被点击的链接            |
| E:visited     | 匹配所有已被点击的链接            |
| E:active      | 匹配鼠标已经其上按下、还没有释放的 E 元素 |
| E:hover       | 匹配鼠标悬停其上的 E 元素         |
| E:focus       | 匹配获得当前焦点的 E 元素         |
| E:lang(c)     | 匹配 lang 属性等于 c 的 E 元素  |
| E:enabled    | 匹配表单中激活的元素                            |
| E:disabled   | 匹配表单中禁用的元素                            |
| E:checked    | 匹配表单中被选中的 radio（单选框）或 checkbox（复选框）元素 |
| E::selection | 匹配用户当前选中的元素                           |
| E:root                | 匹配文档的根元素，对于 HTML 文档，就是 HTML 元素                                                             |
| E:nth-child(n)        | 匹配其父元素的第 n 个子元素，第一个编号为 1                                                                   |
| E:nth-last-child(n)   | 匹配其父元素的倒数第 n 个子元素，第一个编号为 1                                                                 |
| E:nth-of-type(n)      | 与：nth-child() 作用类似，但是仅匹配使用同种标签的元素                                                          |
| E:nth-last-of-type(n) | 与：nth-last-child() 作用类似，但是仅匹配使用同种标签的元素                                                     |
| E:last-child          | 匹配父元素的最后一个子元素，等同于：nth-last-child(1)                                                        |
| E:first-of-type       | 匹配父元素下使用同种标签的第一个子元素，等同于：nth-of-type(1)                                                     |
| E:last-of-type        | 匹配父元素下使用同种标签的最后一个子元素，等同于：nth-last-of-type(1)                                               |
| E:only-child          | 匹配父元素下仅有的一个子元素，等同于：first-child:last-child 或  :nth-child(1):nth-last-child(1)               |
| E:only-of-type        | 匹配父元素下使用同种标签的唯一一个子元素，等同于：first-of-type:last-of-type 或  :nth-of-type(1):nth-last-of-type(1) |
| E:empty               | 匹配一个不包含任何子元素的元素，注意，文本节点也被看作子元素 |
| E:not(s) | 匹配不符合当前选择器的任何元素 |
| E:target | 匹配文档中特定"id"点击后的效果 |

实例：
```css
p:first-child { font-style:italic; }

input[type=text]:focus { color:#000; background:#ffe; }

input[type=text]:focus:hover { background:#fff; }

q:lang(sv) { quotes: "\201D" "\201D""\2019" "\2019"; }

a:link:after { content: " (" attr(href) ")"; }

input[type="text"]:disabled { background:#ddd;}

p:nth-child(3) { color:#f00; }

p:nth-child(odd) { color:#f00; }

p:nth-child(even) { color:#f00; }

p:nth-child(3n+0) { color:#f00; }

p:nth-child(3n) { color:#f00; }

tr:nth-child(2n+11) { background:#ff0; }

tr:nth-last-child(2) { background:#ff0; }

p:last-child { background:#ff0; }

p:only-child { background:#ff0; }

p:empty { background:#ff0; }

:not(p) { border:1px solid #ccc; }
```


- 当我们写一个导航，每个导航间都有空隙（除了最后一个），我们需要写这样的选择器：
  ```css
  .nav a {
      margin-right: 10px;
  }
  .nav a:last-child {
      margin-right: 0;
  }
  ```
  但可通过 :not 简化：
  ```css
  .nav a:not(last-child) { /* 除了最后一个 */
      margin-right: 10px;
  }
  ```


### 伪元素选择器

伪元素选择器在CSS 2.1中定义：

| 选择器            | 含义              |
|----------------|-----------------|
| E:first-line   | 匹配 E 元素的第一行     |
| E:first-letter | 匹配 E 元素的第一个字母   |
| E:before       | 在 E 元素之前插入生成的内容 |
| E:after        | 在 E 元素之后插入生成的内容 |

实例：
```css
p:first-line { font-weight:bold; color;#600; }

.preamble:first-letter { font-size:1.5em;font-weight:bold; }

.cbb:before { content:""; display:block;height:17px; width:18px; background:url(top.png) no-repeat 0 0; margin:0 0 0-18px; }

```


##  CSS 选择器优先级

ID > 类|伪类|属性选择 > 标签选择|伪对象 > 通配符



## Refer Links 

http://www.ruanyifeng.com/blog/2009/03/css_selectors.html

[456 Berea Street](http://www.456bereastreet.com/archive/200509/css_21_selectors_part_1/)