- [CSS 伪元素与伪类](#css-%E4%BC%AA%E5%85%83%E7%B4%A0%E4%B8%8E%E4%BC%AA%E7%B1%BB)
  - [1. 伪元素](#1-%E4%BC%AA%E5%85%83%E7%B4%A0)
    - [1.1. :first-line](#11-first-line)
    - [1.2. :first-letter](#12-first-letter)
    - [1.3. :before 与：after](#13-before-%E4%B8%8E%EF%BC%9Aafter)
      - [1.3.1. content 属性](#131-content-%E5%B1%9E%E6%80%A7)
      - [1.3.2. 实用技巧](#132-%E5%AE%9E%E7%94%A8%E6%8A%80%E5%B7%A7)
  - [2. 伪类](#2-%E4%BC%AA%E7%B1%BB)
    - [2.1. 锚伪类](#21-%E9%94%9A%E4%BC%AA%E7%B1%BB)
    - [2.2. :first-child](#22-first-child)
    - [2.3. :lang](#23-lang)
    - [2.4. 表单相关伪类](#24-%E8%A1%A8%E5%8D%95%E7%9B%B8%E5%85%B3%E4%BC%AA%E7%B1%BB)
  - [3. 伪类与伪元素的比较](#3-%E4%BC%AA%E7%B1%BB%E4%B8%8E%E4%BC%AA%E5%85%83%E7%B4%A0%E7%9A%84%E6%AF%94%E8%BE%83)
  - [4. Refer Links](#4-refer-links)

# CSS 伪元素与伪类

> CSS introduces the concepts of pseudo-elements and pseudo-classes  to permit formatting based on information that lies outside the document tree.

伪类和伪元素用于格式化不在文档树中的部分，比如，一句话中的第一个字母，或者是列表中的第一个元素。

## 1. 伪元素

伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如说，我们可以通过：before 来在一个元素前增加一些文本，并为这些文本添加样式。虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。你可以用伪元素制造视觉上的效果，但是不会增加 JS 查 DOM 的负担，它对 JS 是透明的，开发者无法用 js 获取到这个伪元素，或者增、删、改一个伪元素。

语法：
> selector:pseudo-element {property:value;}

伪元素总结图：        
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/0051da13a17ca02b9aeab3bcb087d4d6.jpg)

NOTE：

- img/input 等单标签是没有 before/after 伪元素的，因为它们本身不可以有子元素，如果你给 img 添加一个 before，那么会被浏览器忽略。

- 伪元素默认是行内元素

- 伪元素不存在于 DOM 文档中，无法被鼠标选中，但可以通过 JavaScript 和 CSS 操作 

### 1.1. :first-line

"first-line" 伪元素用于向文本的首行设置特殊样式。

例：
```css
p:first-line {
  color: #ff0000;
  font-variant: small-caps;
}
```

NOTE："first-line" 伪元素只能用于块级元素。

### 1.2. :first-letter

"first-letter" 伪元素用于向文本的首字母设置特殊样式：

例：
```css
p:first-letter {
  color:#ff0000;
  font-size:xx-large;
}
```

NOTE:"first-letter" 伪元素只能用于块级元素。

### 1.3. :before 与：after

":before" 伪元素可以在元素的内容前面插入新内容，新内容会作为已选中元素的第一个子元素。

":after" 伪元素可以在元素的内容之后插入新内容，新内容会作为已选中元素的最后一个子元素。

例：在每个 `<h1>` 元素前面插入一幅图片
```css
h1:before {
  content:url(logo.gif);
}
```

#### 1.3.1. content 属性

这两个伪元素有一个特有的属性 content，用于在 CSS 渲染中向元素逻辑上的头部或尾部添加内容。注意这些添加不会改变文档内容，不会出现在 DOM 中，不可复制，仅仅是在 CSS 渲染层加入。

取值：

- [String] – 使用引号包括一段字符串，将会向元素内容中添加字符串。
  
  例：
  ```css
  a:after { content: "↗"; }
  ```

- attr() – 调用当前元素的属性，可以方便的比如将图片的 Alt 提示文字或者链接的 Href 地址显示出来。
  
  例：
  ```css
  a:after { content:"(" attr(href) ")"; }
  ```

- url() / uri() – 用于引用媒体文件。
  
  例：
  ```css
  h1::before { content: url(logo.png); }
  ```

- counter() –  调用计数器，可以不使用列表元素实现序号功能。具体请参见 counter-increment 和 counter-reset 属性的用法。
  
  例：
  ```css
  h2:before { 
    counter-increment: chapter; 
    content: "Chapter " counter(chapter) ". " 
  }
  ```

#### 1.3.2. 实用技巧

[我不知道你知不知道我知道的伪元素小技巧](https://juejin.im/post/5a0029a45188254dd935cc40?utm_medium=fe&utm_source=weixinqun)

- 清除浮动

  不少人的解决办法是添加一个空的 div 应用 `clear:both;` 属性。若使用伪元素，无需增加没有意义的元素即可实现相同效果：
  ```css
  /* .clear-fix { *overflow: hidden; *zoom: 1; } */
  .clear-fix:after {
    display: table;/*行内元素是 inline 的，所以要改变它的 display，很多人都忽略了 before/after 是一个行内元素。*/
    content: "";
    width: 0;
    clear: both;
  }
  ```

- 给 blockquote 引用段添加巨大的引号作为背景

  可以用 :before 来代替 background 了，即可以给背景留下空间，还可以直接使用文字而非图片：
  ```css
  blockquote::before {
    content: open-quote;
    position: absolute;
    z-index: -1;
    color: #DDD;
    font-size: 120px;
    font-family: serif;
    font-weight: bolder;
  }
  ```

- 画分割线

  目标效果：![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/febecf951304aa6346700b8bcf99f7b5.jpg)

  ```html
  <p class="or">or</p>
  ```

  ```css
  .or{
      text-align: center;
  }
  .or:after, 
  .or:before{
      content: "";
      position: absolute; /* 注意把一个元素 absolute 定位后会强制把它强制 display:block，即使你再 dislay:table-cell 之类的也不管用。 */
      top: 12px;
      height: 1px;
      background-color: #ccc;
      width: 200px
  }
  .or:after{
      right: 0;
  }
  .or:before{
      left: 0;
  }
  ```
  
  NOTE：**页面中的视觉辅助性元素都推荐使用伪元素画**。



## 2. 伪类

伪类用于当已有元素处于的某个状态时，为其添加对应的样式，这个状态是根据用户行为而动态变化的。比如说，当用户悬停在指定的元素时，我们可以通过：hover 来描述这个元素的状态。虽然它和普通的 css 类相似，可以为已有的元素添加样式，但是它只有处于 DOM 树无法描述的状态下才能为元素添加样式，所以将其称为伪类。

语法：
> selector : pseudo-class {property: value}

NOTE：伪类名称对大小写不敏感。

伪类总结图：    
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/5/7c5020a1963abf8587b83e53821a9948.jpg)

### 2.1. 锚伪类

在浏览器中，链接的不同状态都可以不同的方式显示，这些状态包括：活动状态，已被访问状态，未被访问状态，和鼠标悬停状态。

```css
a:link {color: #FF0000}		/* 未访问的链接 */
a:visited {color: #00FF00}	/* 已访问的链接 */
a:hover {color: #FF00FF}	/* 鼠标移动到链接上 */
a:active {color: #0000FF}	/* 选定的链接 */
```

NOTE:

- a:hover 必须被置于 a:link 和 a:visited 之后，才是有效的

- a:active 必须被置于 a:hover 之后，才是有效的

### 2.2. :first-child

使用 :first-child 伪类可用于选择作为任何元素的第一个子元素的指定元素。

例：
  ```html
  <div>
    <p>These are the necessary steps:</p>
    <ul>
      <li>Intert Key</li>
      <li>Turn key <strong>clockwise</strong></li>
      <li>Push accelerator</li>
    </ul>
    <p>Do <em>not</em> push the brake at the same time as the accelerator.</p>
  </div>
  ```

  ```csss
  p:first-child {font-weight: bold;}
  li:first-child {text-transform:uppercase;}
  ```
  第一个规则将作为某元素第一个子元素的所有 p 元素设置为粗体。第二个规则将作为某个元素（在 HTML 中，这肯定是 ol 或 ul 元素）第一个子元素的所有 li 元素变成大写。

  最常见的错误是认为 p:first-child 之类的选择器会选择 p 元素的第一个子元素。

### 2.3. :lang

:lang 伪类使你有能力为不同的语言定义特殊的规则。

例：为属性值为 no 的 q 元素定义引号的类型：
```html
<html>
  <head>
    <style type="text/css">
      q:lang(no) {
        quotes: "~" "~"
      }
    </style>
  </head>
  <body>
    <p>文字<q lang="no">段落中的引用的文字</q>文字</p>
  </body>
</html>
```

### 2.4. 表单相关伪类

[美化表单的CSS高级技巧](https://www.w3cplus.com/css/advanced-css-form-styling.html)

- :placeholder-shown:检测用户当前是否可见placeholder，可见时元素被匹配

  可以轻松地作为一个渐进的增强工作，但目前[浏览器支持情况较差](https://caniuse.com/#feat=css-placeholder-shown)

  例：
  
  ![image](https://www.w3cplus.com/sites/default/files/blogs/2017/1711/placeholder-shown.gif)

  ```css
  .form-group { 
    position: relative; 
    padding-top: 1.5rem; 
  } 
  label { 
    position: absolute; 
    top: 0; 
    font-size: var(--font-size-small); 
    opacity: 1; 
    transform: translateY(0); 
    transition: all 0.2s ease-out; 
  } 
  input:placeholder-shown + label { 
    opacity: 0; 
    transform: translateY(1rem); 
  }
  ```

- :required：如果规定了require的input没有输入任何内容，提交表单时，该伪类的元素会被匹配

- :optional

- :disabled

- :read-only

- :valid

- :invalid

- :in-range/:out-of-range

- :checked


## 3. 伪类与伪元素的比较

> CSS 伪元素用于将特殊的效果添加到某些选择器。
> 
> CSS 伪类用于向某些选择器添加特殊的效果。

**伪类的效果可以通过添加一个实际的类来达到，而伪元素的效果则需要通过添加一个实际的元素才能达到**，这也是为什么他们一个称为伪类，一个称为伪元素的原因。

实际上，css3 为了区分两者，已经**明确规定了伪类用一个冒号来表示，而伪元素则用两个冒号来表示**。
```css
:Pseudo-classes
::Pseudo-elements
```
但因为兼容性的问题，所以现在大部分还是统一的单冒号。

## 4. Refer Links

http://www.w3school.com.cn/css/css_pseudo_elements.asp

http://www.w3school.com.cn/css/css_pseudo_classes.asp

https://segmentfault.com/a/1190000000484493

http://www.alloyteam.com/2016/05/summary-of-pseudo-classes-and-pseudo-elements/

http://yincheng.site/using-before-after

http://blog.dimpurr.com/css-before-after/

https://www.w3cplus.com/css/advanced-css-form-styling.html