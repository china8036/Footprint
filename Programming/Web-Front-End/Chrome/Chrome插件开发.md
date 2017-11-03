- [Chrome 插件开发](#chrome-%E6%8F%92%E4%BB%B6%E5%BC%80%E5%8F%91)
  - [组成](#%E7%BB%84%E6%88%90)
  - [基本框架](#%E5%9F%BA%E6%9C%AC%E6%A1%86%E6%9E%B6)
  - [Refer Links](#refer-links)

# Chrome 插件开发

Chrome 扩展文件以。crx 为后缀名，在 Google Chrome 扩展官方网站下载扩展时，Chrome 会将。crx 文件下载到 Chrome 的 Application Data 文件夹的 User Data\Temp 下，一般是 C:\Documents and Settings\User\Local Settings\Application Data\Google\Chrome\User Data\Temp，安装完成或者取消安装，该文件就会被删除。.crx 实际上是一个压缩文件，使用解压文件打开这个文件就可以看到其中的文件目录。

## 组成

每个插件一共有 4 种文件：

1)	manifest.json：所有插件都要有这个文件，这是插件的配置文件，可看作插件的“入口”；

2)	icon.png：小图标，推荐使用 19*19 的半透明 png 图片，更好的做法是同时提供一张 38*38 的半透明的 png 图片作为大图标；

3)	popup.html：popup 属于 Browser Actions，当点击图标时出现这个窗口，可以在里面放置任何 html 元素，它的宽度是自适应的；

4)	popup.js：html 页面所引用的 javascript 文件， 出于安全考虑，javascript 必须与 html 分开存放；

## 基本框架

格式：https://developer.chrome.com/extensions/manifest 

以下为必须项和推荐项，完整格式见官方 API	
```
{
  // Required
  "manifest_version": 2, // 固定的
  "name": "My Extension",// 插件名称
  "version": "versionString",// 插件使用的版本

  // Recommended
  "default_locale": "en",
  "description": "A plain text description",// 插件的描述
  "icons": {...},

  // Pick one (or none)
  "browser_action": {// 插件加载后生成图标
    "default_icon": "icon.png",// 图标的图片
    "default_popup": "popup.html",// 单击图标弹出的页面
    "default_title": "Click here!" // 鼠标移到图标显示的文字
  },
  "page_action": {...},

  "permissions": [ // 允许插件访问的 url
      "http://*/", 
      "bookmarks", 
      "tabs", 
      "history" 
  ],
  // Optional
  "author": ...,

    "options_page": "options.html",// 选项页面
    "background"://background script 即插件运行的环境
    {
        "scripts": [
            "scripts/chromereload.js",
            "scripts/background.js"
        ],
"page":"background.html",
       // Recommended
       "persistent": false// 如果没有 "persistent" 键，您将得到一个普通的后台网页。是否持久存在是事件页面与后台网页之间的根本区别

    },
}

```

其中：

图标文件不要小于 19px*19px，但最好也不要超过这个尺寸，虽然大图它会自适应，但会使得应用文件变大。然后完成和未完成状态的两个图标放到资源文件中，可以建立任意文件夹放进去，因为 CSS 文件把定义它们的路径。

- "permissions"：
  https://developer.chrome.com/extensions/declare_permissions 
  对插件的权限控制；
  假如一个插件的"permissions"里写有“http://*.hacker.com/”，那么这个插件就被允许访 hacker.com 上的所有内容；

- popup.html
  一般样式文件直接在 header 中即可，而 js 文件 必须 从外部引用，不能直接写在 html 中；

- Browser Actions（扩展图标）
  把 Browser Actions 翻译成扩展图标不是很准确，但它的功能就是把你的应用显示在 Chrome 工具栏上。

- Page Actions（地址栏图标）
  如果你熟悉一些 Chrome 插件的话，你一定会发现有些扩展的图标不是显示在地址栏的右边，而是显示在地址内部右方，这就是 Page Actions 地址栏图标。

- Background Pages 后台页面
  这个窗口不会显示，它是扩展程序的后台服务，它会一直保持运行。比如在一些需要数据保存程序中，如果当前用户关闭 popup，就需要 Background Pages 来进行相应的操作。

## Refer Links

官方文档 https://developer.chrome.com/extensions/getstarted.html 

中文文档 http://open.chrome.360.cn/extension_dev/overview.html  https://crxdoc-zh.appspot.com 

入门教程 http://www.jianshu.com/p/cf5b3fba44ea 
