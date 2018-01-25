- [Sass NOTE](#sass-note)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [Sass 定义](#sass-%E5%AE%9A%E4%B9%89)
    - [Feature](#feature)
    - [Sass 和 Scss](#sass-%E5%92%8C-scss)
  - [安装](#%E5%AE%89%E8%A3%85)
  - [使用 CLI](#%E4%BD%BF%E7%94%A8-cli)
  - [语法](#%E8%AF%AD%E6%B3%95)
    - [注释 (Comments)](#%E6%B3%A8%E9%87%8A-comments)
    - [SassScript](#sassscript)
      - [变量 (Variables)](#%E5%8F%98%E9%87%8F-variables)
      - [数据类型](#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)
      - [运算](#%E8%BF%90%E7%AE%97)
      - [函数](#%E5%87%BD%E6%95%B0)
    - [嵌套 (Nested Rules)](#%E5%B5%8C%E5%A5%97-nested-rules)
    - [@ 规则和指令](#%E8%A7%84%E5%88%99%E5%92%8C%E6%8C%87%E4%BB%A4)
      - [@import](#import)
      - [@extend：继承](#extend%EF%BC%9A%E7%BB%A7%E6%89%BF)
      - [@media](#media)
    - [流程控制 Control Directives](#%E6%B5%81%E7%A8%8B%E6%8E%A7%E5%88%B6-control-directives)
      - [@if](#if)
      - [@for](#for)
      - [@each](#each)
      - [@while](#while)
  - [Compass](#compass)
    - [概述](#%E6%A6%82%E8%BF%B0)
    - [安装 compass](#%E5%AE%89%E8%A3%85-compass)
    - [使用 compass cli](#%E4%BD%BF%E7%94%A8-compass-cli)
      - [项目初始化](#%E9%A1%B9%E7%9B%AE%E5%88%9D%E5%A7%8B%E5%8C%96)
      - [编译](#%E7%BC%96%E8%AF%91)
      - [实时编译](#%E5%AE%9E%E6%97%B6%E7%BC%96%E8%AF%91)
    - [配置：config.rb](#%E9%85%8D%E7%BD%AE%EF%BC%9Aconfigrb)
      - [output_style](#outputstyle)
    - [模块](#%E6%A8%A1%E5%9D%97)
      - [reset 模块](#reset-%E6%A8%A1%E5%9D%97)
      - [CSS3 模块](#css3-%E6%A8%A1%E5%9D%97)
      - [layout 模块](#layout-%E6%A8%A1%E5%9D%97)
      - [typography 模块](#typography-%E6%A8%A1%E5%9D%97)
      - [Helper 模块](#helper-%E6%A8%A1%E5%9D%97)
      - [utilities 模块](#utilities-%E6%A8%A1%E5%9D%97)
  - [Refer Links](#refer-links)

# Sass NOTE

## 概述

### Sass 定义

Sass(Syntactically Awesome StyleSheets) 是对 CSS 的扩展，让 CSS 语言更强大、优雅。 它允许你使用变量、嵌套规则、 mixins、导入等众多功能， 并且完全兼容 CSS 语法。 Sass 有助于保持大型样式表结构良好， 同时也让你能够快速开始小型项目， 特别是在搭配 [Compass](http://compass-style.org/) 样式库一同使用时。

### Feature

- 完全兼容 CSS3
- 在 CSS 语言基础上添加了扩展功能，比如变量、嵌套 (nesting)、混合 (mixin)
- 对颜色和其它值进行操作的{Sass::Script::Functions 函数}
- 函数库控制指令之类的高级功能
- 良好的格式，可对输出格式进行定制
- 支持 Firebug

### Sass 和 Scss

参见 [SCSS 与 Sass 异同](http://sass.bootcss.com/docs/scss-for-sass-users/)

Sass 有两种语法：Sass 和 Scss。SCSS 是 Sass 3 引入新的语法，其语法完全兼容 CSS3，并且继承了 Sass 的强大功能。也就是说，任何标准的 CSS3 样式表都是具有相同语义的有效的 SCSS 文件。另外，SCSS 还能识别大部分 CSS hacks（一些 CSS 小技巧）和特定于浏览器的语法，例如：古老的 IE filter 语法。

- 具体应该使用 sass 还是 scss，取决于开发团队是对 ruby 较为熟悉（选用 sass） 还是对 css 较为熟悉（选用 scss），甚至可以在同一个项目中混用 sass 和 scss，但不可在同一个文件中混用。

- 相互转换

  安装 sass 后，可使用 sass-convert 对 scss 和 sass 进行相互转换：
  
  将 scss 转换为 sass：
  ```
  sass-convert main.scss main.sass
  ```
  将 sass 转换为 scss：
  ```
  sass-convert main.sass main.scss
  ```

## 安装

参考 [sass 安装](http://sass-lang.com/install)

1. Sass 引擎采用 ruby 编写，因此需要先[安装 ruby](https://www.ruby-lang.org/en/documentation/installation/) 环境：
    - Windows 下安装：
        
      下载 [RubyInstallers](https://rubyinstaller.org/downloads/)，安装后验证：

      ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/6/6f9ac61fcaf30f6f50da9cf97fc26e01.jpg)

2. 安装 sass

    ```shell
    gem install sass
    ```
    ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/6/9d96d26f7144ae52fc2e736721cc10a8.jpg)

3. 更新 sass

    ```shell
    gem update
    ```

## 使用 CLI

- 编译 scss 文件
  ```shell
  sass input.scss output.css
  ```

- 监视文件的改动并更新 CSS
  ```shell
  sass --watch input.scss:output.css
  ```
  监视整个目录：
  ```shell
  sass --watch app/sass:public/stylesheets
  ```

## 语法

### 注释 (Comments)

sass 中支持使用 // 和 /* */ 进行注释，但 // 注释不会编译到 css 文件中。

一般会在 sass 的宿主文件中通过块注释，说明每个被引入的文件的作用：
```scss
/**
  * CONTENTS
  *
  * SETTINGS
  * variables……………………变量集中存储文件
  * mixin………………………………mixin 集中存储文件
  * 
  * TOOLS
  *
  * COMPONENTS
  * reset………………………………compass 内置浏览器样式重置文件
  *
  * BUSINESS
  *
  * BASE
  * screen.scss………………当前站点的主页样式文件
  */
@import "variables", "compass/reset";
//……
```

### SassScript

In addition to the plain CSS property syntax, Sass supports a small set of extensions called SassScript. SassScript allows properties to use variables, arithmetic, and extra functions. SassScript can be used in any property value.

SassScript can also be used to generate selectors and property names, which is useful when writing mixins. This is done via interpolation.

#### 变量 (Variables)

- 声明
  ```scss
  $headFontSize: 16px;
  ```

- 使用
  ```scss
  p {
    font-size: $headFontSize;
  }
  ```

NOTE：一般会将变量集中写在一个文件_variables.scss（用单下划线开头命名表示该文件仅作为局部使用)，再在宿主文件 screen.scss 中引入并使用：
```scss
@import "variables";
```

#### 数据类型

SassScript 支持六种主要的数据类型：

- 数字（例如 1.2、13、10px）

- 文本字符串，无论是否有引号（例如 "foo"、'bar'、baz）

- 颜色（例如 blue、#04a3f9、rgba(255, 0, 0, 0.5)）

- 布尔值（例如 true、false）

- 空值（例如 null）

- 值列表，用空格或逗号分隔（例如 1.5em 1em 0 2em、Helvetica, Arial, sans-serif）

SassScript 还支持所有其他 CSS 属性值类型， 例如 Unicode 范围和 !important 声明。 然而，它不会对这些类型做特殊处理。 它们只会被当做不带引号的字符串看待。

#### 运算

所有数据类型都支持等式运算 (== and !=)。 另外，每种数据类型也有其支持的特殊运算符。

- 数字运算

  SassScript 支持数字的标准运算（加 +、减 -、乘 *、除 / 和取模 %），并且，如果需要的话，也可以在不同单位间做转换：
  ```scss
  p {
    width: 1in + 8pt;
  }
  ```
  数字也支持关系运算（<、>、<=、>=）， 等式运算（==、!=）被所有数据类型支持。

  CSS 允许 / 出现在属性值里，作为分隔数字的一种方法。 既然 SassScript 是 CSS 属性语法的扩展， 他就必须支持这种语法，同时也允许 / 用在除法运算上。 也就是说，默认情况下，在 SassScript 里用 / 分隔的两个数字， 都会在 CSS 中原封不动的输出。然而，在以下三种情况中，/ 会被解释为除法运算。 这就覆盖了绝大多数真正使用除法运算的情况。 这些情况是：
  - 如果数值或它的任意部分是存储在一个变量中或是函数的返回值。
  - 如果数值被圆括号包围。
  - 如果数值是另一个数学表达式的一部分。

- 字符串运算

  `+` 运算符可以用来连接字符串：
  ```scss
  p {
    cursor: e + -resize;
  }
  ```
  注意，如果有引号的字符串被添加了一个没有引号的字符串 （也就是，带引号的字符串在 + 符号左侧）， 结果会是一个有引号的字符串。 同样的，如果一个没有引号的字符串被添加了一个有引号的字符串 （没有引号的字符串在 + 符号左侧）， 结果将是一个没有引号的字符串。

- 布尔运算
  
  SassScript 支持布尔值做 and、or 和 not 运算。

- 圆括号
  
  圆括号可以用来改变运算顺序。

#### 函数

- function

  定义：
  ```scss
  @function double($n) {
    @return $n * 2;
  }
  ```
  使用：
  ```scss
  #sidebar {
    width: double(5px);
  }
  ```

- mixin

  定义：
  ```scss
  @mixin col {// 不带参数
    width: 50%;
  }
  @mixin col($width) {// 带参数
    width: $width;
  }
  @mixin col($width: 50%) {// 带默认值的参数
    width: $width;
  }
  ```
  使用：
  ```scss
  p {// 不带参数
    @include col();
  }
  p {// 带参数
    @include col(50%);
  }
  p {// 带默认值的参数
    @include col();
  }
  ```

### 嵌套 (Nested Rules)

- 选择器嵌套
  ```scss
  p {
    a {
      //...
    }
  }
  ```
  - 引用父选择符： &
    ```scss
    p {
      &:hover {
        //...
      }
    }
    ```

- 属性嵌套
  ```scss
  p {
    font: {
      family: Arail;
      size: 16px;
    }
  }
  ```

### @ 规则和指令

#### @import

css3 中支持使用 @import 来引入外部文件，但其存在以下缺点：
- 必须放置在 css 文件的头部，否则不生效
- 加载外部文件时，会造成浏览器阻塞，大大增加了页面的加载时间

因此，sass 中提供了改进的 @import（即 control directives），在编译阶段完成文件的引入且可在文件的任意位置使用，解决了原生 @import 存在的问题，默认情况下，在 sass/scss 文件中会使用改进的 @import，当满足以下条件时才使用原生 @import：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/6/106e874436b65f0336abf77f19c3054f.jpg)

基于 sass 的 @import 有以下既定规则：
- 默认寻路路径根目录是当前文件所在目录，但可通过 load_paths 选项配置其它寻路路径，如缺省的`@import "compass/reset"`就是使用了该选项。
- 没有文件后缀名时，会尝试添加 `.scss/.sass` 后缀。
- 同一目录下，局部文件和非局部文件不能重名。

#### @extend：继承

```scss
.error {
  color: red;
}
.serious-error {
  border: 1px solid red;
  @extend .error;
}
```

- Placeholder Selectors：可以使用 % 声明仅用于继承的选择器样式，这种选择器不会被编译到 css 文件中。
- 不可继承诸如`.A .B`的链式选择器。

#### @media

在 sass 中，@media 可以直接在选择器中书写，编译后 sass 会自动将 media query 的部分提升至最高层：
```scss
.sidebar {
  width: 300px;
  @media screen and (orientation: landscape) {
    width: 500px;
  }
}
```
会被编译为：
```scss
.sidebar {
  width: 300px;
}

@media screen and (orientation: landscape) {
  .sidebar {
    width: 500px; 
  } 
}
```

### 流程控制 Control Directives

#### @if

#### @for

#### @each

#### @while

## Compass

### 概述

Sass 本身只是一个编译器，Compass 在它的基础上，封装了一系列有用的模块和模板，补充 Sass 的功能。它们之间的关系，有点像 Javascript 和 jQuery、Ruby 和 Rails、python 和 Django 的关系。

### 安装 compass

安装 compass 的同时会安装对应版本的 sass。

```shell
gem install compass
```
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/12/6/14c0265d74bdc3e133a05cfe4b51e7b9.jpg)

### 使用 compass cli

http://compass-style.org/help/tutorials/production-css/

#### 项目初始化

```shell
mkdir compass-demo
compass create compass-demo
``` 
项目目录中的 config.rb 文件为项目的配置文件，子目录 sass 存放 Sass 源文件，子目录 stylesheets 存放编译后的 css 文件。

#### 编译

```shell
compass compile
```
该命令在项目根目录下运行，会将 sass 子目录中的 scss 文件，编译成 css 文件，保存在 stylesheets 子目录中。

Compass 只编译发生变动的文件，如果你要重新编译未变动的文件，需要使用 --force 参数：
```shell
compass compile --force
```

#### 实时编译

只要 scss 文件发生变化，就会被自动编译成 css 文件：
```shell
cd compass-demo
compass watch
```

### 配置：config.rb

#### output_style

默认状态下，编译出来的 css 文件带有大量的注释。但是，生产环境需要压缩后的 css 文件，可配置编译后将 css 文件进行压缩：
```ruby
output_style = :compressed
```
可以通过指定 environment 的值（:production 或者：development），智能判断编译模式：
```ruby
environment = :development
output_style = (environment == :production) ? :compressed : :expanded
```

### 模块

Compass 采用模块结构，不同模块提供不同的功能。内置核心模块：
- reset
- css3
- layout
- typography
- helper
- utilities
除了内置模块，开发者可以使用自定义的第三方模块。

#### reset 模块

加载：
```scss
@import "compass/reset";
```
编译后，会生成相应的 css reset 代码。

#### CSS3 模块

该模块提供多种 CSS3 命令。

例：
- 圆角

  ```scss
  @import "compass/css3";
  .round {
    @include border-radius(5px);
  }
  ```
  编译后：
  ```css
  .rounded {
    -moz-border-radius: 5px;
    -webkit-border-radius: 5px;
    -o-border-radius: 5px;
    -ms-border-radius: 5px;
    -khtml-border-radius: 5px;
    border-radius: 5px;
  }
  ```
  如果只需要左上角为圆角，写法为：
  ```scss
  @include border-corner-radius(top, left, 5px);
  ```

- 透明

  ```scss
  @import "compass/css3";
  .opacity {
    @include opacity(0.5); 
  }
  ```
  编译后：
  ```css
  .opacity {
    filter: progid:DXImageTransform.Microsoft.Alpha(Opacity=0.5);
    opacity: 0.5;
  }
  ```

#### layout 模块

layout 模块提供常见的布局。

例：
- sticky-footer

  ```scss
  @import "compass/layout";
  #footer {
    @include sticky-footer(54px);
  }
  ```

#### typography 模块

typography 模块提供版式功能。

例：
- 指定链接颜色

  使用 mixin：`link-colors($normal, $hover, $active, $visited, $focus);`

  ```scss
  @import "compass/typography";
  a {
    link-colors($normal, $hover, $active, $visited, $focus);
  }
  ```

#### Helper 模块

Helper 模块提供了一系列函数（函数与 mixin 的主要区别是，不需要使用 @include 命令，可以直接调用)。

例：
- image-width() 和 image-height() 返回图片的宽和高

- inline-image() 可以将图片转为 data 协议的数据
  ```scss
  @import "compass";
  .icon {
    background-image: inline-image("icon.png");
  }
  ```
  编译后：
  ```css
  .icon {
    background-image: url('data:image/png;base64,iBROR...QmCC');
  }
  ```

#### utilities 模块

utilities 模块某些不属于其他模块的功能。

例：
- 清除浮动

  ```scss
  @import "compass/utilities/";
  .clearfix {
    @include clearfix;
  }
  ```

## Refer Links

[sass 官方文档](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)

[sass 中文官网](https://www.sass.hk/)

[sass 中文文档](http://sass.bootcss.com/docs/sass-reference/)

[compass 官方文档](http://compass-style.org/help/)

[Sass 和 Compass 必备技能之 Sass](https://www.imooc.com/learn/364)

[Sass 和 Compass 必备技能之 Compass](https://www.imooc.com/learn/371)
