- [前端常见问题收录](#%E5%89%8D%E7%AB%AF%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E6%94%B6%E5%BD%95)
  - [`<input type = "submit">` 提交方式和用 js 的 `form.submit()` 有什么区别](#input-type-submit-%E6%8F%90%E4%BA%A4%E6%96%B9%E5%BC%8F%E5%92%8C%E7%94%A8-js-%E7%9A%84-formsubmit-%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)
  - [CSS选择器如何选中上一个相邻元素？为什么没有这种选择器？](#css%E9%80%89%E6%8B%A9%E5%99%A8%E5%A6%82%E4%BD%95%E9%80%89%E4%B8%AD%E4%B8%8A%E4%B8%80%E4%B8%AA%E7%9B%B8%E9%82%BB%E5%85%83%E7%B4%A0%EF%BC%9F%E4%B8%BA%E4%BB%80%E4%B9%88%E6%B2%A1%E6%9C%89%E8%BF%99%E7%A7%8D%E9%80%89%E6%8B%A9%E5%99%A8%EF%BC%9F)
  - [`height:100%`时无法触发scroll事件](#height100%E6%97%B6%E6%97%A0%E6%B3%95%E8%A7%A6%E5%8F%91scroll%E4%BA%8B%E4%BB%B6)

# 前端常见问题收录


## `<input type = "submit">` 提交方式和用 js 的 `form.submit()` 有什么区别

<!-- TODO: 做实验验证

http://blog.sina.com.cn/s/blog_693d183d0100uolj.html

https://www.cnblogs.com/siqi/archive/2012/11/30/2796671.html

https://www.zhihu.com/question/21316196

-->




## CSS选择器如何选中上一个相邻元素？为什么没有这种选择器？

https://www.zhihu.com/question/38235620

https://www.zhihu.com/question/21508830/answer/18465691

浏览器最初设计的时候就考虑了渐进显示，也就是整个文档加载了多少就显示多少内容，而不用等整个下载完。渐进显示在CSS上的原理就是一个节点所适用的样式只取决于它和它之前的节点（父节点、它之前的兄弟节点）的性质。

而所设想的selector（如选中上一个相邻元素的选择器）则恰好相反。也就是当浏览器解析到一个新节点时，可能改变之前节点所适用的样式——因而要求在解析一个新节点后，得回头重新计算之前节点所匹配的样式，此即所谓“回溯”。在最坏的情况下所导致大量的重新计算和reflow，可以相当于重新render整个网页。


## `height:100%`时无法触发scroll事件


