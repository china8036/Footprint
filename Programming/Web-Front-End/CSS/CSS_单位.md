- [CSS 单位](#css-%E5%8D%95%E4%BD%8D)
  - [1. Absolute Lengths Unit](#1-absolute-lengths-unit)
    - [1.1. px](#11-px)
    - [1.2. mm](#12-mm)
    - [1.3. pt](#13-pt)
    - [1.4. in](#14-in)
    - [1.5. cm](#15-cm)
  - [2. Relative Lengths Unit](#2-relative-lengths-unit)
    - [2.1. Font-relative lengths](#21-font-relative-lengths)
      - [2.1.1. rem & em](#211-rem-em)
        - [2.1.1.1. 如何转化为像素值](#2111-%E5%A6%82%E4%BD%95%E8%BD%AC%E5%8C%96%E4%B8%BA%E5%83%8F%E7%B4%A0%E5%80%BC)
        - [2.1.1.2. 为什么使用 rem 和 em](#2112-%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BD%BF%E7%94%A8-rem-%E5%92%8C-em)
      - [2.1.2. ex & ch](#212-ex-ch)
    - [2.2. Viewport-percentage lengths](#22-viewport-percentage-lengths)
      - [2.2.1. vh & vw](#221-vh-vw)
      - [2.2.2. vmin & vmax](#222-vmin-vmax)
      - [2.2.3. vi & vb](#223-vi-vb)
  - [3. 浏览器兼容情况](#3-%E6%B5%8F%E8%A7%88%E5%99%A8%E5%85%BC%E5%AE%B9%E6%83%85%E5%86%B5)
  - [4. Refer Links](#4-refer-links)

# CSS 单位

## 1. Absolute Lengths Unit

| Unit | Description                              |
| ---- | ---------------------------------------- |
| cm   | centimeters[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_cm) |
| mm   | millimeters[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_mm) |
| in   | inches (1in = 96px = 2.54cm)[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_in) |
| px * | pixels (1px = 1/96th of 1in)[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_px) |
| pt   | points (1pt = 1/72 of 1in)[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_pt) |
| pc   | picas (1pc = 12 pt)                      |

`* Pixels` (px) are relative to the viewing device. For low-dpi devices, 1px is one device pixel (dot) of the display. For printers and high resolution screens 1px implies multiple device pixels.

### 1.1. px

Depends on the viewing device. For screen displays, it traditionally represents one device pixel (dot). However, for printers and high-resolution screens, one CSS pixel implies multiple device pixels, so that the number of pixels per inch stays around 96.

### 1.2. mm

One millimeter.

### 1.3. pt

One point (1/72nd of an inch).

### 1.4. in

One inch (2.54 centimeters).

### 1.5. cm

One centimeter (10 millimeters).

## 2. Relative Lengths Unit

| Unit | Description                              |
| ---- | ---------------------------------------- |
| em   | Relative to the font-size of the element (2em means 2 times the size of the current font)[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_em) |
| ex   | Relative to the x-height of the current font (rarely used)[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_ex) |
| ch   | Relative to width of the "0" (zero)      |
| rem  | Relative to font-size of the root element |
| vw   | Relative to 1% of the width of the viewport*[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_vw) |
| vh   | Relative to 1% of the height of the viewport*[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_vh) |
| vmin | Relative to 1% of viewport's* smaller dimension[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_vmin) |
| vmax | Relative to 1% of viewport's* larger dimension[Try it](https://www.w3schools.com/cssref/tryit.asp?filename=trycss_unit_vmax) |
| %    |                                          |

Tip: The em and rem units are practical in creating perfectly scalable layout!

`* Viewport` = the browser window size. If the viewport is 50cm wide, 1vw = 0.5cm.

### 2.1. Font-relative lengths

Font-relative lengths specify the `<length>` value in terms of the size of a particular character or font attribute in the font currently in effect in an element or its parent.

Note: If the `<html>` and `<body>` elements are set as `overflow:auto`, space taken by scrollbars is not subtracted from the viewport, whereas it will be subtracted if set as `overflow:scroll`.

#### 2.1.1. rem & em

em 和 rem 都是灵活可扩展的单位，由浏览器转换为像素值，使用 em 和 rem 单位可以让我们的设计更加灵活，能够控制元素整体放大缩小，而不是固定大小。

##### 2.1.1.1. 如何转化为像素值

- rem 如何转化为像素值

  当使用 rem 单位，他们转化为像素大小取决于页根元素的字体大小，即 html 元素的字体大小。 根元素字体大小乘以你 rem 值。

  例如，根元素的字体大小 16px，10rem 将等同于 160px，即 10 x 16 = 160。

- em 如何转换为像素值

  当使用 em 单位时，像素值将是 em 值乘以使用 em 单位的元素的字体大小。

  例如，如果一个 div 有 18px 字体大小，10em 将等同于 180px，即 10 × 18 = 180。

  有一个比较普遍的误解，认为 em 单位是相对于父元素的字体大小。 事实上，根据 W3 标准 ，它们是相对于使用 em 单位的元素的字体大小。父元素的字体大小可以影响 em 值，但这种情况的发生，纯粹是因为继承。

##### 2.1.1.2. 为什么使用 rem 和 em

- 为什么使用 rem 单位

  Rem 单位提供最伟大的力量并不仅仅是他们提供一致尺寸而不是继承。相反，它给我们的一个途经去获取用户的偏好来影响网站中每一处使用 rem 的元素大小，不再是使用固定的 px 单位。

  为此，使用 rem 单位的主要目的应该是**确保无论用户如何调整自己的浏览器（如通过缩放改变页面的 font-size 大小），我们的布局都能调整到合适大小来达到最佳体验**。

  - 一切有可扩展性要求的组件尺寸，都应使用rem，包括heights，widths，padding，margin，border，font-size，shadows等属性。

    P.S. 在实际创建布局时，往往直接以px为单位更方便，但部署时应通过转换使用rem单位（可使用Stylus / Sass / Less来自动转换单位）。

  - 媒体查询应始终使用 rem 单位

  - 除非确定要使用 em，否则都应优先选择 rem 作为单位（包括 font-size 的值）, 因为 em 的继承难以管理

  - 一般使用 rem 单位的字体大小，em 单位只在特殊的情况下使用

- 为什么使用 em 单位

  em 单位取决于一个元素的 font-size 值而非 html 元素的字体大小。

  为此，em 单位的主要目的应该是**保持在一个特定的设计元素范围内的可伸缩性**。

  - 在设计一些组件（如按钮、菜单、标题）时，一般会对组件设置明确的字体大小，并且我们希望，当字体大小发生改变时，整个组件能保持原样式进行整体缩放。

    因此，对于这种情况，一般建议采用rem设置组件的font-size（保留可扩展性的同时避免继承的混淆），然后使用em来设置组件的padding、 margin、 width、 height和line-height等值，从而保证如果更改组件的字体大小，组件周围的间距将在剩余的空间按比例缩放。
    <!-- TODO: 实际尝试，全用rem&采用rem和em混合，哪种效果更优？ -->

    ![image](http://img.cdn.firejq.com/jpg/2017/11/10/480bb1d3e501ab0320c40e997c5fd577.jpg)

    ![image](http://img.cdn.firejq.com/jpg/2017/11/10/9f74df459e9372c4052f9debd5aae4f4.jpg)

  - 设置h1, h2, h3, h4, h5, h6 的下间距与他们当前的字体大小一致:
    ```css
    h1, h2, h3, h4, h5, h6 { margin-bottom: 1em; }
    ```



NOTE：

- 在多列布局中不要使用 em 或 rem，布局中的列宽通常应使用 %，从而可以流畅适应无法预知大小的视区。但对于单一列仍应使用rem来设置max-width：
  ```css
  .container {
    width: 100%;
    max-width: 75rem;
  }
  ```
  在保持列的可扩展性的同时可防止组件过宽而样式崩坏。

- 若一旦缩放就会破坏布局结构（这种情况很少见），就不要使用 em 或 rem，而使用固定长度单位，如px。


#### 2.1.2. ex & ch

ex和ch单位，与em和rem相似，依赖于当前字体和字体大小。然而，与em和rem不同的是，这两个单位只也依赖于font-family，因为它们被定为基于特殊字体的法案。


### 2.2. Viewport-percentage lengths

Viewport-percentage lengths define the `<length>` value relative to the size of the viewport, i.e., the visible portion of the document. Viewport lengths are invalid in `@page` declaration blocks.

#### 2.2.1. vh & vw

vh: Equal to 1% of the height of **the viewport's initial containing block**(window.innerHeight).

vw: Equal to 1% of the width of **the viewport's initial containing block**(window.innerWidth).

#### 2.2.2. vmin & vmax

vmin: Equal to the smaller of vw and vh.

vmax: Equal to the larger of vw and vh.

#### 2.2.3. vi & vb

vi: Equal to 1% of the size of the initial containing block, in the direction of the root element’s inline axis.

vb: Equal to 1% of the size of the initial containing block, in the direction of the root element’s block axis.

例如，如果浏览器设置为1100px宽、700px高，1vmin会是7px,1vmax为11px。然而，如果宽度设置为800px，高度设置为1080px，1vmin将会等于8px而1vmax将会是10.8px。

应用：

如果你需要一个总是在屏幕上可见的元素。使用高度和宽度设置为低于100的vmin值将可以实现这个效果：
```css
.box { height: 100vmin; width: 100vmin; }
```

如果你需要一个总是覆盖可视窗口的正方形(一直接触屏幕的四条边),使用相同的规则只是把单位换成vmax：
```css
.box { height: 100vmax; width: 100vmax; }
```

## 3. 浏览器兼容情况

![image](http://img.cdn.firejq.com/jpg/2017/11/9/d4ec2124c21c2d088cd3b279ce0da3b4.jpg)

## 4. Refer Links

https://www.w3schools.com/cssref/css_units.asp

https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Values_and_units

https://developer.mozilla.org/en-US/docs/Web/CSS/length

https://webdesign.tutsplus.com/tutorials/comprehensive-guide-when-to-use-em-vs-rem--cms-23984

https://www.zhihu.com/question/46279241

https://www.w3cplus.com/css/7-css-units-you-might-not-know-about.html

