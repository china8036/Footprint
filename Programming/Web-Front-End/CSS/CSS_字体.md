- [CSS 字体](#css-%E5%AD%97%E4%BD%93)
  - [1. font-family](#1-font-family)
  - [2. 字体分类](#2-%E5%AD%97%E4%BD%93%E5%88%86%E7%B1%BB)
  - [3. 系统预装字体](#3-%E7%B3%BB%E7%BB%9F%E9%A2%84%E8%A3%85%E5%AD%97%E4%BD%93)
  - [4. 兼容情况](#4-%E5%85%BC%E5%AE%B9%E6%83%85%E5%86%B5)
  - [5. 中文字体](#5-%E4%B8%AD%E6%96%87%E5%AD%97%E4%BD%93)
  - [6. 主流网站设置的字体](#6-%E4%B8%BB%E6%B5%81%E7%BD%91%E7%AB%99%E8%AE%BE%E7%BD%AE%E7%9A%84%E5%AD%97%E4%BD%93)

# CSS 字体

## 1. font-family

CSS 的 font-family 命令，指定了网页元素所使用的字体。

例：
```css
font-family: Georgia, "Times New Roman", 
             "Microsoft YaHei", "微软雅黑", 
             STXihei, "华文细黑", 
             serif;
```
选择规则：

（1）优先使用排在前面的字体。

（2）如果找不到该种字体，或者该种字体不包括所要渲染的文字，则使用下一种字体。

（3）如果所列出的字体，都无法满足需要，则让操作系统自行决定使用哪种字体。

- 绝大部分中文字体里包含英文字母（基本上都很丑），而英文字体是不包含中文字符的。因此 font-family 应该优先指定英文字体，然后再指定中文字体。否则，中文字体所包含的英文字母，会取代英文字体，而这往往很丑的

- 为了保证兼容性，中文字体的中文名称和英文名称，应该都写入 font-family。比如，"微软雅黑"的英文名称是 Microsoft YaHei

- 如果字体名称中间有空格，则要用双引号把字体名称包起来。如 "Microsoft Yahei"

## 2. 字体分类

- 衬线字体（Serif Fonts）
  
  eg：Georgia, Times New Roman, Times, serif；

  衬线字体指的是笔画的末端带有衬线的字体，例如“宋体”，“仿宋体”，“楷体”。这类字体，开始和结束的地方有装饰，更为醒目，适合印刷字体，也可以用作文章的标题如图。

- 非衬线字体（Sans-Serif Fonts）
  
  eg：Arial, Verdana, Geneva, Helvetica, sans-serif；

  非衬线字体这些字体是没有上下短线的，清晰度好，往往用于正文，更适合在 web 中显示。例如“微软雅黑”，“黑体”，都是无线衬字体。

- 等宽字体（Monospace Fonts）

  eg：Courier New, Courier, monospace；

  Serif 和 Sans-Serif 字体都是成比例的，Monospace 字体不是成比例的，每个字宽度都是一致的。但中文不存在这样的情况，所以等宽字体应该是针对英文来说的。

- 草书字体（Cursive）

  这些字体试图模仿人的手写体，例如 Zapf Chancery。

- Fantasy

  这类字体没有明确特征，无法归入上诉四种系列的字体归入这类字体。

## 3. 系统预装字体

- Windows 操作系统
  ```
  黑体：SimHei
  宋体：SimSun
  新宋体：NSimSun
  仿宋：FangSong
  楷体：KaiTi
  仿宋 GB2312：FangSongGB2312
  楷体 GB2312：KaiTiGB2312
  微软雅黑：Microsoft YaHei （Windows 7 开始提供）
  ```

- OS X 操作系统
  ```
  冬青黑体：Hiragino Sans GB （SNOW LEOPARD 开始提供）
  华文细黑：STHeiti Light （又名 STXihei）
  华文黑体：STHeiti
  华文楷体：STKaiti
  华文宋体：STSong
  华文仿宋：STFangsong
  ```

## 4. 兼容情况

移动端：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/9/2b914871a29c3cc6be5adc9a9194511d.jpg)

PC 端：
![image](http://otaivnlxc.bkt.clouddn.com/jpg/2017/11/9/fe4a7fe02fbbc7e490fac89b52c2f86c.jpg)

## 5. 中文字体

- 宋体（SimSun）

  宋体是最常见的中文字体，如果没有指定字体，操作系统往往选择它来渲染。

  ```css
  font-family: Arial, Helvetica, tahoma, verdana, 宋体，SimSun, 华文细黑，STXihei, sans-serif;
  ```

- 微软雅黑（Microsoft YaHei）

  微软雅黑的美观度和清晰度都较好，可以作为网页的首选字体。它在 Mac 平台的对应字体是华文细黑（STXihei）。

  Windows 7 之后的版本才安装了这个字体。如果没有，可以选择黑体（Simhei）替代。不过，黑体比较粗，不应用于字号较小的文字。

  ```css
  font-family: Tahoma, Arial, Helvetica, "Microsoft YaHei New", "Microsoft Yahei", "微软雅黑", 宋体，SimSun, STXihei, "华文细黑", sans-serif;
  ```

- 仿宋（FangSong）
  
  这种字体是衬线体，比宋体的装饰性更强。如果字号太小，会影响清晰度，所以只有在字号大于 14px 的情况下，才可以考虑这种字体。

  ```css
  font-family: Georgia, "Times New Roman", "FangSong", "仿宋", STFangSong, "华文仿宋", serif;
  ```

- 楷体（KaiTi）
  
  楷体也是衬线体，装饰性与仿宋体接近，但是宽度更大，笔画更清楚一些。这种字体也应该在大于 14px 的情况下才使用。

  ```css
  font-family: Georgia, "Times New Roman", "KaiTi", "楷体", STKaiti, "华文楷体", serif;
  ```

## 6. 主流网站设置的字体

- Type Is Beautiful （有关文字设计和视觉文化的网站)
  ```css
  "Classic Grotesque W01",Arial,"Hiragino Sans GB",
  "STHeiti","Microsoft YaHei","WenQuanYi Micro Hei",SimSun,sans-serif
  ```

- 百度
  ```css
  arial,"Hiragino Sans GB","Microsoft Yahei","微软雅黑","宋体", 宋体，
  Tahoma,Arial,Helvetica,STHeiti
  ```

- 淘宝
  ```css
  tahoma,arial,"Hiragino Sans GB", 宋体，sans-serif
  ```

- QQ(http://www.qq.com/)
  ```css
  "宋体","Arial Narrow",HELVETICA
  ```

- 豆瓣
  ```css
  Helvetica,Arial,sans-serif
  ```

- Github
  ```css
  Helvetica,arial,freesans,clean,sans-serif,"Segoe UI Emoji","Segoe UI Symbol"
  ```

- 知乎
  ```css
  font-family: Helvetica Neue,Helvetica,PingFang SC,Hiragino Sans GB,Microsoft YaHei,Arial,sans-serif
  ```