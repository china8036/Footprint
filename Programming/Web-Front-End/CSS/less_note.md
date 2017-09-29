- [Less Note](#less-note)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [使用](#%E4%BD%BF%E7%94%A8)
        - [引入](#%E5%BC%95%E5%85%A5)
        - [混入](#%E6%B7%B7%E5%85%A5)
        - [嵌套](#%E5%B5%8C%E5%A5%97)
        - [变量与作用域](#%E5%8F%98%E9%87%8F%E4%B8%8E%E4%BD%9C%E7%94%A8%E5%9F%9F)
        - [参数混入](#%E5%8F%82%E6%95%B0%E6%B7%B7%E5%85%A5)
    - [Refer Links](#refer-links)

# Less Note

## 概述

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展，可构建动态的 CSS；

作为 CSS 的一种扩展，Less 完全兼容 CSS 语法；

Less 可以运行在 Node 或浏览器端；

在浏览器上跑 less.js 非常方便开发，但是不推荐用于生产环境。在生产环境中，性能和可靠性非常重要，我们建议最好使用 node.js 或其它第三方工具进行预编译。

## 使用

### 引入

- 在 HTML 文件的 Head 中导入 less 文件
  ```html
  <link type="text/less" rel="stylesheet/less" href="test.less" />
  ```

- 在 HTML 文件的 Head 中导入 less.js：
  ```html
  <script src="less.js" type="text/javascript"></script>
  ```
  cdn address：https://cdn.bootcss.com/less.js/{version}/less.min.js   
  GitHub address：https://github.com/less/less.js/blob/master/dist/less.min.js   

注意：     
- 务必确保在 less.js 之前加载你的样式表，即 less.js 最后引入；
- 引入 XX.less 文件时，rel 属性要设置为“stylesheet/less”；
- 如果加载多个 .less 样式表文件，每个文件都会被单独编译。因此，一个文件中所定义的任何变量、mixin 或命名空间都无法在其它文件中访问；

### 混入

混入，指的是在 CSS 代码当中再书写 CSS 代码（也就是样式中的样式），对于一些会在样式表中重复使用的样式规则，使用样式的嵌套方式进行书写。

例：
```less
.text-overflow {
    display: block;
    /*内联对象需加*/
    word-break: keep-all;
    /* 不换行 */
    white-space: nowrap;
    /* 不换行 */
    overflow: hidden;
    /* 内容超出宽度时隐藏超出部分的内容 */
    text-overflow: ellipsis;
    /* 当对象内文本溢出时显示省略标记 (...) ；需与 overflow:hidden; 一起使用。*/
}

.arclist dd h2 {
    height: 36px;
    margin-bottom: 10px;
    font-size: 30px;
    line-height: 36px;
    color: #39f;
    .text-overflow;
}
```

对于网页当中，必然存在着很多的相同样式，比如所有的 dl 列表中，dt 中的 a 标签以及 img 标签的控制几乎是类似的，那么对于这种不适合提取出来为单独类名的相同样式，我们就可以在 less 当中利用混入的功能进行书写，从而减少在编写时的代码量，提升编写代码的速度。

### 嵌套

嵌套，在我们 HTML 代码中一直都在使用，如 div 中的 p 元素等，对于我们样式的书写，需要使用 div p {} 进行样式的指定，这也就是我们的后代选择器。不过写过项目的人都知道，为了防止样式的覆盖，每种选择器都是一大长串，写起来很麻烦。

less 利用嵌套的书写方式，让我们在写后代选择器的时候有了新的方法。

原来后代选择器的书写方式：
```css
.arclist dl {
    padding: 10px 20px;
    border-bottom: 1px dotted #ccc;
}
.arclist dl:after {
    .common-clearfloat;
}
.arclist dt {
    float: left;
    width: 120px;
    height: 120px;
}
```
通过嵌套的方式书写样式的方法：
```less
.arclist {
    dl {
        padding: 10px 20px;
        border-bottom: 1px dotted #ccc;
        &:after {
            .common-clearfloat;
        }
        dt {    
            float: left;
            width: 120px;
            height: 120px;
        }
    }
}
```

### 变量与作用域

- 变量定义
  ```less
  @variable: value;
  ```
- 变量引用
  ```less
  .head {
    border-color: @variable;
  }
  ```
- 变量计算    
  less 中的 CSS 变量是可以进行运算的，而且不需要进行额外的处理（即不需要去掉 px 或者转换为字符串等），直接使用最基本的 CSS 样式书写和运算即可；
  ```less
  @fontSize: 16px;
  .common-div {
      width: 200px;
      height: 200px;
      line-height: 200px;
      text-align: center;
      font-weight: bold;
      border: 1px solid #000;
  }

  .con1 {
      .common-div;
      font-size: @fontSize;
  }

  .con2 {
      .common-div;
      font-size: @fontSize + 4px;
  }

  .con3 {
      .common-div;
      font-size: @fontSize - 4px;
  }
  ```

- 变量作用域
  基本原理等同于 JS 的作用域，由自身开始，不断向其父级进行变量的查找，直到查找到符合条件的变量为止；


### 参数混入

```
.border-radius( @radius: 10px) {
    -webkit-border-radius: @radius;
    -moz-border-radius: @radius;
    border-radius: @radius;
}
```


## Refer Links

http://lesscss.org/features/

https://github.com/less/less.js

http://less.bootcss.com/ 