- [CSS 最佳实践](#css-%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
  - [编码规范](#%E7%BC%96%E7%A0%81%E8%A7%84%E8%8C%83)
  - [高可维护性的 CSS](#%E9%AB%98%E5%8F%AF%E7%BB%B4%E6%8A%A4%E6%80%A7%E7%9A%84-css)
    - [尺寸单位的选择](#%E5%B0%BA%E5%AF%B8%E5%8D%95%E4%BD%8D%E7%9A%84%E9%80%89%E6%8B%A9)
  - [高性能的 CSS](#%E9%AB%98%E6%80%A7%E8%83%BD%E7%9A%84-css)
    - [高效使用 CSS 选择器](#%E9%AB%98%E6%95%88%E4%BD%BF%E7%94%A8-css-%E9%80%89%E6%8B%A9%E5%99%A8)
    - [CSS 的图片处理](#css-%E7%9A%84%E5%9B%BE%E7%89%87%E5%A4%84%E7%90%86)
    - [减少 CSS 代码量](#%E5%87%8F%E5%B0%91-css-%E4%BB%A3%E7%A0%81%E9%87%8F)
    - [其它](#%E5%85%B6%E5%AE%83)
  - [Refer Links](#refer-links)

# CSS 最佳实践

## 编码规范

- 使用两个空格的 soft tabs；

- 使用组合选择器时，保持每个独立的选择器单独占用一行；

- 为了代码的易读性，在每个声明的左括号前增加一个空格，声明块的右括号应该另起一行；

- 每条声明冒号 “:” 之后应该插入一个空格，“，” 逗号之后应该插入一个空格；

- 每条声明应该只占用一行来保证错误报告更加准确；

- 所有声明应该以分号结尾。虽然最后一条声明后的分号是可选的，但是如果没有他，你的代码会更容易出错；

- 不要在颜色值 rgb(), rgba(), hsl(), hsla(), 和 rect() 中增加空格；

- 不要在属性取值或者颜色参数前面添加不必要的 0 （比如，使用 .5 替代 0.5 和 -.5px 替代 0.5px)；

- 所有的十六进制值都应该使用小写字母，例如 #fff。因为小写字母有更多样的外形，在浏览文档时，他们能够更轻松的被区分开来；

- 尽可能使用短的十六进制数值，例如使用 #fff 替代 #ffffff；

- 为选择器中的属性取值添加引号，例如 input[type="text"]。 他们只在某些情况下可有可无，所以都使用引号可以增加一致性；

- 不要为 0 指明单位，比如使用 `margin: 0`; 而不是 `margin: 0px;`；   
  例：
  ```css
  /* Bad CSS */
  .selector, .selector-secondary, .selector[type=text] {
    padding:15px;
    margin:0px 0px 15px;
    background-color:rgba(0, 0, 0, 0.5);
    box-shadow:0 1px 2px #CCC,inset 0 1px 0 #FFFFFF
  }

  /* Good CSS */
  .selector,
  .selector-secondary,
  .selector[type="text"] {
    padding: 15px;
    margin-bottom: 15px;
    background-color: rgba(0,0,0,.5);
    box-shadow: 0px 1px 2px #ccc, inset 0 1px 0 #fff;
  }
  ```

- 相关的属性声明应该以下面的顺序分组处理：   
		 Positioning   
		 Box model 盒模型   
		 Typographic 排版   
		 Visual 外观      

  Positioning 处在第一位，因为他可以使一个元素脱离正常文本流，并且覆盖盒模型相关的样式。盒模型紧跟其后，因为他决定了一个组件的大小和位置；
  其他属性只在组件内部起作用或者不会对前面两种情况的结果产生影响，所以他们排在后面；
  例：
  ```css
  .declaration-order {
    /* Positioning */
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 100;

    /* Box-model */
    display: block;
    float: right;
    width: 100px;
    height: 100px;

    /* Typography */
    font: normal 13px "Helvetica Neue", sans-serif;
    line-height: 1.5;
    color: #333;
    text-align: center;

    /* Visual */
    background-color: #f5f5f5;
    border: 1px solid #e5e5e5;
    border-radius: 3px;

    /* Misc */
    opacity: 1;
  }
  ```
- 尽量将媒体查询的位置靠近他们相关的规则。不要将他们一起放到一个独立的样式文件中，或者丢在文档的最底部；
- 与 <link> 相比，@import 更慢，需要额外的页面请求，并且可能引发其他的意想不到的问题。应该避免使用他们，可使用多个 <link> 元素或 CSS 预处理器将样式编译到一个文件中；

- 当使用厂商前缀属性时，通过缩进使取值垂直对齐以便多行编辑，如：    
  例：
  ```css
  /* Prefixed properties */
  .selector {
    -webkit-box-shadow: 0 1px 2px rgba(0,0,0,.15);
            box-shadow: 0 1px 2px rgba(0,0,0,.15);
  }
  ```

-  在一个声明块中只包含一条声明的情况下，为了易读性和快速编辑可以考虑移除其中的换行；所有包含多条声明的声明块应该分为多行；    
  例：
    ```css
    /* Single declarations on one line */
    .span1 { width: 60px; }
    .span2 { width: 140px; }
    .span3 { width: 220px; }
    ```

- 坚持限制属性取值简写的使用，属性简写需要你必须显式设置所有取值；   
  例：
  ```css
    /* Bad example */
    .element {
      margin: 0 0 10px;
      background: red;
      background: url("image.jpg");
      border-radius: 3px 3px 0 0;
    }
    
    /* Good example */
    .element {
      margin-bottom: 10px;
      background-color: red;
      background-image: url("image.jpg");
      border-top-left-radius: 3px;
      border-top-right-radius: 3px;
    }
  ```

- 好的代码注释传达上下文和目标。不要简单地重申组件或者 class 名称；

- Class 命名：
	- 保持 Class 命名为全小写，可以使用短划线（不要使用下划线和 camelCase 命名）。短划线应该作为相关类的自然间断。（例如，.btn 和 .btn-danger)；
	- 避免过度使用简写。.btn 可以很好地描述 button，但是 .s 不能代表任何元素；
	- Class 的命名应该尽量短，也要尽量明确；
	- 命名时使用最近的父节点或者父 class 作为前缀；
	- 使用 .js-* classes 来表示行为（相对于样式)，但是不要在 CSS 中定义这些 classes；   
    例：
      ```css
        /* Bad example */
        .t { ... }
        .red { ... }
        .header { ... }
        
        /* Good example */
        .tweet { ... }
        .important { ... }
        .tweet-header { ... }
      ```

- 代码组织
	- 以组件为单位组织代码。
	- 制定一个一致的注释层级结构。
	- 使用一致的空白来分割代码块，这样做在查看大的文档时更有优势。
	- 当使用多个 CSS 文件时，通过组件而不是页面来区分他们。页面会被重新排列组合，而组件是可以移动的。



## 高可维护性的 CSS

### 尺寸单位的选择

W3C 官方文档中将尺寸单位分为相对长度单位和绝对长度单位，其中：
- 绝对长度单位定义的长度是固定的，使用的是物理度量单位（cm、mm、in、px、pt、pc 等）；

- 相对长度单位在定义长度时是相对与其它长度的，它又分为字体相对单位和视窗相对单位，字体相对单位（em、ex、ch、rem），视窗相对单位（vw、vh、vmin、vmax）

使用最广泛的长度单位是 em、px 和 %，其中 em 的计算相对于自身元素的字体尺寸，而 % 的计算相对于父级元素的尺寸；

如何设置元素的尺寸和字体大小？
- 尽量使用相对尺寸
  使用相对尺寸，不是是页面整体布局是自适应的，页面整体的尺寸可以是固定尺寸也可以是自适应尺寸，取决于页面的设计；   
  尽量使用相对尺寸，指的是局部元素的尺寸要尽量使用相对尺寸，即局部自适应，这样当整体模块尺寸更改时旧无需更改内部子模块的尺寸了；   
  使用相对尺寸时，若期望尺寸随着字体改变而改变，应使用 em 为单位，若期望尺寸随着父级元素尺寸的改变而改变，应使用 % 作为单位；实际情况下，一般设置行高会使用 em，而设置 height 或 width 会使用 %；   

- 只有当元素尺寸可预知时才使用绝对尺寸
  如：设计商要求使用绝对宽度、网页整体宽度固定、侧边栏宽度固定、页头页尾的高度固定等；

- 使用 rem 设置字体大小
  CSS3 中引入了 rem 单位，rem 是相对于根元素的字体大小来计算实际字体大小的，避免了当元素层级过多时，em 单位计算维护成本高的问题；

## 高性能的 CSS

### 高效使用 CSS 选择器

在 CSS 中，选择器按**从右到左**的顺序进行匹配，与我们的视觉习惯正好相反；结合 CSS 选择器的匹配顺序，我们定义选择器时应该尽量让第一次匹配时的数量达到最少，且让整体的匹配查找次数最少；

- 避免使用通配符

- 避免使用标签选择器和单个属性选择器作为关键选择器

- 不要在 ID 选择器前使用标签名

注意：从整体上看，CSS 代码中性能的可提升空间很小，因此，CSS 样式还是需要以可维护性为主，提升性能为辅，在保证 CSS 样式的可维护性前提下来编写高性能的 CSS 选择器；

### CSS 的图片处理

- 不要设置图片尺寸，图片缩放会导致 CPU 额外的计算，占用渲染实践，且过大尺寸的图片在传输时会浪费更多的带宽，**尽量在图片制作时安装实际需求的尺寸进行设计和制作**；

- 使用 CSS 雪碧图

### 减少 CSS 代码量

- 定义简洁的 CSS 规则

- 合并相关的 CSS 规则
  如 margin、padding、border、text、list-style 等样式规则；

- 定义简洁的属性值
  如：颜色值#FF33EE 可简化为#F3E，尺寸`0.8rem`可简化为`.8rem`，尺寸为 0 时可省略单位；

- 合并相同的定义

- 删除无效的定义

### 其它

- 避免使用 `@import`

- 避免使用 IE 独有的样式：图片滤镜和 CSS 表达式


## Refer Links
https://zoomzhao.github.io/code-guide/

《Web 前端开发最佳实践》 党建 著