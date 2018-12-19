- [UEditor 使用笔记](#ueditor-%E4%BD%BF%E7%94%A8%E7%AC%94%E8%AE%B0)
    - [1. 使用](#1-%E4%BD%BF%E7%94%A8)
    - [2. 输入过滤](#2-%E8%BE%93%E5%85%A5%E8%BF%87%E6%BB%A4)
    - [3. 解决方法一：在 ueditor.config.js 中的 xss 过滤器中添加白名单](#3-%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%E4%B8%80%EF%BC%9A%E5%9C%A8-ueditorconfigjs-%E4%B8%AD%E7%9A%84-xss-%E8%BF%87%E6%BB%A4%E5%99%A8%E4%B8%AD%E6%B7%BB%E5%8A%A0%E7%99%BD%E5%90%8D%E5%8D%95)
    - [4. 解决方法二：关闭部分过滤处理](#4-%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%E4%BA%8C%EF%BC%9A%E5%85%B3%E9%97%AD%E9%83%A8%E5%88%86%E8%BF%87%E6%BB%A4%E5%A4%84%E7%90%86)
    - [5. 解决方法三：完全关闭过滤处理](#5-%E8%A7%A3%E5%86%B3%E6%96%B9%E6%B3%95%E4%B8%89%EF%BC%9A%E5%AE%8C%E5%85%A8%E5%85%B3%E9%97%AD%E8%BF%87%E6%BB%A4%E5%A4%84%E7%90%86)

# UEditor 使用笔记

## 1. 使用

官方文档：http://fex.baidu.com/ueditor/

解压下载的包，在解压后的目录创建 demo.html 文件：
```html
<!DOCTYPE HTML>
<html lang="en-US">
<head>
    <meta charset="UTF-8">
    <title>ueditor demo</title>
</head>

<body>
    <!-- 加载编辑器的容器 -->
    <script id="container" name="content" type="text/plain">
        这里写你的初始化内容
    </script>
    <!-- 配置文件 -->
    <script type="text/javascript" src="ueditor.config.js"></script>
    <!-- 编辑器源码文件 -->
    <script type="text/javascript" src="ueditor.all.js"></script>
    <!-- 实例化编辑器 -->
    <script type="text/javascript">
        var ue = UE.getEditor('container');
    </script>
</body>
</html>
```

结果：静态代码（服务器端）（Ctrl+U 的视图）
![image](http://img.cdn.firejq.com/jpg/2017/10/25/6f012b5f5ef230789200fce6596e67ef.jpg)

```html
<script id="editor" type="text/plain" style="width:550px;height:500px;"></script>
<!-- 这句话加载出了 ueditor -->
<!-- 其中的 style 即可指定生成 ueditor 的尺寸 -->
```

动态代码（即加载了 js 之后的结果）（客户端浏览器）（F12 视图）
![image](http://img.cdn.firejq.com/jpg/2017/10/25/2207dc1160c7a6d3c6d6927d8521f50c.jpg)

## 2. 输入过滤

百度的 Ueditor 编辑器出于安全性考虑（防止 XSS)，针对进入编辑器的富文本内容提供了节点级别的过滤，虽然代码安全了，但是在编辑器中输入的 html 代码大量样式都会被过滤。

## 3. 解决方法一：在 ueditor.config.js 中的 xss 过滤器中添加白名单

例：https://www.cnblogs.com/theroad/p/5761743.html

在编辑器中插入 section 和 img 标签，样式都被清除，解决方法：在 ueditor.config.js 中添加 section 和 img 标签的属性白名单
![image](http://img.cdn.firejq.com/jpg/2017/10/25/c69d7f82f5bba73826e20227cda5c6d8.jpg)

![image](http://img.cdn.firejq.com/jpg/2017/10/25/5c6e6f68af6f19e1c787165e734c31e9.jpg)

需要说明的是，此处的配置仅针对非纯文本粘贴模式时有效，如用户开启了纯文本模式，则需要手动修改 paste.js 中对应的黑白名单配置。由于该模式使用较少，所以未对外提供配置项。

- 或者直接关闭 xss 过滤器

## 4. 解决方法二：关闭部分过滤处理

http://blog.csdn.net/wdw984/article/details/22375199?utm_source=tuicool&utm_medium=referral

在 ueditor.all.js 文件内搜索 allowDivTransToP, 找到如下的代码：

第一步将 allowDivTransToP 设置为 false；（因为默认的设置是将 div 自动转换为 p，这样写好的样式就找不到相应的 div 了）

第二步将 addInputRule 函数中的 switch 代码段中的 case style ，script 选择给删除或者注释；（避免出现编辑器将 style 或 script 自动的转换成别的标签）

![image](http://img.cdn.firejq.com/jpg/2017/10/25/7f1d36b05feb466d99590e02be589a7a.jpg)

例 2：往 ueditor 中复制表格不显示边框解决：https://www.wanweiwang.cn/FAQ/view/544.html

## 5. 解决方法三：完全关闭过滤处理

http://zzc1684.iteye.com/blog/2325762

在 ueditor.all.js 中搜索 UE.plugins['defaultfilter']，在函数第一行直接 return;，就屏蔽了整个过滤逻辑。

![image](http://img.cdn.firejq.com/jpg/2017/10/25/88260f2885c6d25cc4165926dcb6d5cc.jpg)
