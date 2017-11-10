- [CSS](#css)
  - [vertical-align & line-height](#vertical-align-line-height)


# CSS 各种知识点 NOTE


## vertical-align & line-height

http://www.zhangxinxu.com/wordpress/2015/08/css-deep-understand-vertical-align-and-line-height/

https://zhuanlan.zhihu.com/p/25808995

https://zhuanlan.zhihu.com/p/25808995



- vertical-align 属性设置元素的垂直对齐方式。

  该属性定义行内元素的基线相对于该元素所在行的基线的垂直对齐。允许指定负长度值和百分比值。这会使元素降低而不是升高。在表单元格中，这个属性会设置单元格框中的单元格内容的对齐方式。

  - 取值

    | 值           | 描述                                     |
    | ----------- | -------------------------------------- |
    | baseline    | 默认。基线对齐：元素放置在父元素的基线上                       |
    | sub         | 垂直对齐文本的下标                             |
    | super       | 垂直对齐文本的上标                              |
    | top         | 把元素的顶端与行中最高元素的顶端对齐                     |
    | text-top    | 把元素的顶端与父元素字体的顶端对齐                      |
    | middle      | 把此元素放置在父元素的中部                         |
    | bottom      | 把元素的顶端与行中最低的元素的顶端对齐                   |
    | text-bottom | 把元素的底端与父元素字体的底端对齐                    |
    | length      |                                        |
    | %           | 使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。 |
    | inherit     | 规定应该从父元素继承 vertical-align 属性的值。        |

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/10/d8f016b8b78de4de138b2179d6925e9e.jpg)

- line-height 属性设置行间的距离（行高）。

  该属性会影响行框的布局。在应用到一个块级元素时，它定义了该元素中基线之间的最小距离而不是最大距离。

  line-height 与 font-size 的计算值之差（在 CSS 中成为“行间距”）分为两半，分别加到一个文本行内容的顶部和底部。可以包含这些内容的最小框就是行框。

  原始数字值指定了一个缩放因子，后代元素会继承这个缩放因子而不是计算值。

  - 取值  

    | 值        | 描述                           |
    | -------- | ---------------------------- |
    | normal   | 默认。设置合理的行间距。                 |
    | *number* | 设置数字，此数字会与当前的字体尺寸相乘来设置行间距。   |
    | *length* | 设置固定的行间距。                    |
    | *%*      | 基于当前字体尺寸的百分比行间距。             |
    | inherit  | 规定应该从父元素继承 line-height 属性的值。 |

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/10/7cb991bdfc4bbdc0eec71fa16177dffe.jpg)

  ![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/10/4ad4ab8395f114b25ce83fe6ba9e8c6b.jpg)



对于所有 display 值以 inline- 开头的元素（如 img / input / svg），其高度是基于 height、margin 和 border 属性的和；如果你将其 height 设置为 auto 的话，那么其高度的取值就是 line-height。


## padding 和 margin

padding 和 margin 属性使用`%`作为单位时，长度基准是父元素的**宽度**的百分比。