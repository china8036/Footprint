- [Chrome 插件和应用开发](#chrome-%E6%8F%92%E4%BB%B6%E5%92%8C%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91)
  - [1. 概述](#1-%E6%A6%82%E8%BF%B0)
    - [1.1. Chrome 插件](#11-chrome-%E6%8F%92%E4%BB%B6)
    - [1.2. Chrome 应用](#12-chrome-%E5%BA%94%E7%94%A8)
  - [2. 组成](#2-%E7%BB%84%E6%88%90)
  - [3. manifest.json](#3-manifestjson)
  - [4. Browser Actions](#4-browser-actions)
    - [4.1. manifest.json](#41-manifestjson)
    - [4.2. UI 组成](#42-ui-%E7%BB%84%E6%88%90)
  - [5. Page Actions](#5-page-actions)
    - [5.1. manifest.json](#51-manifestjson)
    - [5.2. UI 组成](#52-ui-%E7%BB%84%E6%88%90)
    - [5.3. TIPs](#53-tips)
  - [6. 背景页](#6-%E8%83%8C%E6%99%AF%E9%A1%B5)
    - [6.1. manifest.json](#61-manifestjson)
  - [7. 选项页](#7-%E9%80%89%E9%A1%B9%E9%A1%B5)
  - [8. Cookie 操作](#8-cookie-%E6%93%8D%E4%BD%9C)
    - [8.1. 声明权限](#81-%E5%A3%B0%E6%98%8E%E6%9D%83%E9%99%90)
    - [8.2. Cookie API：chrome.cookies](#82-cookie-api%EF%BC%9Achromecookies)
      - [8.2.1. get](#821-get)
      - [8.2.2. set](#822-set)
      - [8.2.3. onChanged](#823-onchanged)
  - [9. 桌面通知](#9-%E6%A1%8C%E9%9D%A2%E9%80%9A%E7%9F%A5)
    - [9.1. 声明权限](#91-%E5%A3%B0%E6%98%8E%E6%9D%83%E9%99%90)
    - [9.2. API](#92-api)
    - [9.3. 与页面交互](#93-%E4%B8%8E%E9%A1%B5%E9%9D%A2%E4%BA%A4%E4%BA%92)
  - [10. content_scripts](#10-contentscripts)
  - [11. Refer Links](#11-refer-links)

# Chrome 插件和应用开发

## 1. 概述

### 1.1. Chrome 插件

Chrome 扩展文件以 `.crx` 为后缀名，在 Google Chrome 扩展官方网站下载扩展时，Chrome 会将 `.crx` 文件下载到 Chrome 的 Application Data 文件夹的 `User Data\Temp` 下，一般是 `C:\Documents and Settings\User\Local Settings\Application Data\Google\Chrome\User Data\Temp`，安装完成或者取消安装，该文件就会被删除。

`.crx` 实际上是一个压缩文件，使用解压文件打开这个文件就可以看到其中的文件目录。

### 1.2. Chrome 应用

Chrome 应用提供了与原生应用能力相同的体验，但是与网页一样安全。就像网上应用一样，Chrome 应用使用 HTML5、JavaScript 和 CSS 编写，但是 Chrome 应用从外观上与行为上都与原生应用类似，它们也具有类似于原生应用的能力，比网上应用可用的更强大。

Chrome 应用可以访问对传统网站不可用的 Chrome 浏览器 API 与服务。

## 2. 组成

一个插件一般共有 4 种文件：

1)	manifest.json：所有插件都要有这个文件，这是插件的配置文件，可看作插件的“入口”；

2)	*.png：小图标，推荐使用 `19*19` 的半透明 png 图片，更好的做法是同时提供一张 `38*38` 的半透明的 png 图片作为大图标；图标文件不要小于 `19px*19px`，但最好也不要超过这个尺寸，虽然大图它会自适应，但会使得应用文件变大。

3)	popup.html：popup 属于 Browser Actions，当点击图标时出现这个窗口，可以在里面放置任何 html 元素。但需要注意：对于 Chrome 扩展来说，很多对网页有意义的内容是无意义的（如`<title>`标签），所以我们可以只挑需要的写。

4)	*.js：html 页面所引用的 javascript 文件， 出于安全考虑，javascript 必须与 html 分开存放，inline-script 是禁止的；

## 3. manifest.json

格式：https://developer.chrome.com/extensions/manifest 

以下为必须项和推荐项，完整格式见官方 API：
```json
{
  // 以下字段必须提供
  "manifest_version": 2, // 固定的
  "name": "My Extension",// 插件名称
  "version": "versionString",// 插件版本，值最多可以是由三个圆点分为四段的版本号，每段只能是数字，每段数字不能大于 65535 且不能以 0 开头（可以是 0，但不可以是 0123），版本号段左侧为高位，每次更新扩展时，新的版本号必须比之前的版本号高

  // 以下字段建议提供
  "default_locale": "en",// 插件使用得语言
  "description": "A plain text description",// 插件的描述
  "icons": {// 插件使用的图标文件
      "16": "images/icon16.png",
      "48": "images/icon48.png",
      "128": "images/icon128.png"
  },

  // 以下 4 个字段多选一，或者都不提供
  "browser_action": {// 指定该扩展是一个插件，插件的图标放在 Chrome 的工具栏中
    "default_icon": {                     // optional
      "16": "images/icon16.png",           // optional
      "24": "images/icon24.png",           // optional
      "32": "images/icon32.png"            // optional
    },
    // "default_icon": "images/icon32.png",  // equivalent to "default_icon": { "32": "images/icon32.png" }
    "default_popup": "popup.html",// 单击图标弹出的页面
    "default_title": "Click here!" // 鼠标移到图标时显示的文字
  },

  "page_action": {// 指定该扩展是一个插件，插件的图标放在在地址内部右方
    "default_icon": {                     // optional
      "16": "images/icon16.png",           // optional
      "24": "images/icon24.png",           // optional
      "32": "images/icon32.png"            // optional
    },
    // "default_icon": "images/icon32.png",  // equivalent to "default_icon": { "32": "images/icon32.png" }
    
    "default_title": "Google Mail",      // optional; shown in tooltip
    "default_popup": "popup.html"        // optional
  },
  "theme": {// 指定该扩展是一个主题，主题和标准扩展的打包方式类似，但是主题中不能包含 JavaScript 或者 HTML 代码
    "images" : {
      "theme_frame" : "images/theme_frame_camo.png",
      "theme_frame_overlay" : "images/theme_frame_stripe.png",
      "theme_toolbar" : "images/theme_toolbar_camo.png",
      "theme_ntp_background" : "images/theme_ntp_background_norepeat.png",
      "theme_ntp_attribution" : "images/attribution.png"
    },
    "colors" : {
      "frame" : [71, 105, 91],
      "toolbar" : [207, 221, 192],
      "ntp_text" : [20, 40, 0],
      "ntp_link" : [36, 70, 0],
      "ntp_section" : [207, 221, 192],
      "button_background" : [255, 255, 255]
    },
    "tints" : {
      "buttons" : [0.33, 0.5, 0.47]
    },
    "properties" : {
      "ntp_background_alignment" : "bottom"
    }
  },
  "app": {// 指定该扩展是一个 Chrome 应用，可安装的 webapp，包括打包过的 app，需要这个字段来指定 app 需要使用的 url。最重要的是 app 的启动页面 ------ 当用户在点击 app 的图标后，浏览器将导航到的地方。
  },

  // 以下选项根据需要提供
  "permissions": [ // 定义扩展或 app 将使用的一组权限，详见 https://developer.chrome.com/extensions/declare_permissions
      "bookmarks", 
      "tabs", 
      "history",
      // 获取跨域请求允许，列出允许插件访问的 url
      "http://*/", // 这个插件被允许访问所有 http 链接
      "http://*.hacker.com/" // 这个插件被允许访 hacker.com 上的所有内容 
  ],
  
  "author": ...,// 作者信息

  "homepage_url":...,// 扩展的主页 url。扩展的管理界面里面将有一个链接指向这个 url。如果你通过了 Extensions Gallery 和 Chrome Web Store 来分发扩展，主页 缺省就是扩展的页面。

  "options_page": "options.html",// 指定选项页面

  "Content scripts":...,// 在 Web 页面内运行的 javascript 脚本。通过使用标准的 DOM，它们可以获取浏览器所访问页面的详细信息，并可以修改这些信息。

  "background": {// 指定插件的后台页面：这个页面不会显示，但它是扩展程序的后台服务，会一直保持运行，在你的扩展的整个生命周期都存在，同时，在同一时间只有一个实例处于活动状态。由于不显示，背景页不需要任何 HTML，仅仅需要 js 文件。
    "scripts": [
      "scripts/chromereload.js",
      "scripts/background.js"
    ],
    "page":"background.html",// 如果的确需要自己的背景页，可以使用 page 字段定义
    // Recommended
    "persistent": false // 如果没有 "persistent" 键，您将得到一个普通的后台网页。是否持久存在是事件页面与后台网页之间的根本区别
  },

  "content_scripts": [// 指定将哪些脚本何时注入到哪些页面中，当用户访问这些页面后，相应脚本即可自动运行
    {
      "matches": ...,// 定义了哪些页面会被注入脚本
      "exclude_matches":...,// 定义了哪些页面不会被注入脚本
      "css":...,// 要注入的样式表
      "js":...,// 要注入的 JavaScript
      "run_at":...,// 定义了何时进行注入
      "all_frames":...,// 定义脚本是否会注入到嵌入式框架中
      "include_globs":...,// 全局 URL 匹配
      "exclude_globs":...// 全局 URL 匹配
    },
    {

    },
    //...
  ],

  // more and more ... ... 
}
```

## 4. Browser Actions

browser actions 定义插件在 chrome 主工具条的地址栏右侧的行为。

### 4.1. manifest.json

在 extension manifest 中用下面的方式注册你的 browser action:

```json
{
  "name": "My extension",
  ...
  "browser_action": {
    "default_icon": "images/icon19.png", // optional 
    "default_title": "Google Mail",      // optional; shown in tooltip 
    "default_popup": "popup.html"        // optional 
  },
  ...
}
```

### 4.2. UI 组成

- 图标
  - Browser action 图标推荐使用宽高都为 19 像素，更大的图标会被缩小。
  - 图标的选择：
    - 使用一个静态图片或者使用 HTML5canvas element。 使用静态图片适用于简单的应用程序
    - 创建诸如平滑的动画之类更丰富的动态 UI（如 canvas element)。
  - 图标的修改：
    - 静态修改可以使用`browser_action`的 manifest 中 default_icon 字段
    - 动态修改调用 setIcon() 方法。
  - 尽量使用 alpha 通道并且柔滑你的图标边缘，因为很多用户使用 themes，你的图标应该在在各种背景下都表现不错。
- ToolTip
  
  修改`browser_action`的 manifest 中`default_title`字段，或者调用 setTitle() 方法。

- Badge
  - Browser actions 可以选择性的显示一个 badge— 在图标上显示一些文本。Badges 可以很简单的为 browser action 更新一些小的扩展状态提示信息。

  - 因为 badge 空间有限，所以只支持 4 个以下的字符。

  - 设置 badge 文字和颜色可以分别使用 setBadgeText()andsetBadgeBackgroundColor()。

- Popup
  - 如果 browser action 拥有一个 popup，popup 会在用户点击图标后出现。popup 可以包含任意你想要的 HTML 内容，并且会自适应大小。

  - 修改`browser_action`的 manifest 中`default_popup`字段来指定 HTML 文件， 或者调用 setPopup() 方法。

## 5. Page Actions

page actions 定义插件在 chrome 地址栏右侧的行为。

### 5.1. manifest.json

在 extension manifest 中用下面的方式注册你的 page action：
```json
{
  "name": "My extension",
  ...
  "page_action": {
    "default_icon": "icons/foo.png", // optional 
    "default_title": "Do action",    // optional; shown in tooltip 
    "default_popup": "popup.html"    // optional 
  },
  ...
}
```

### 5.2. UI 组成

同 browser actions 一样，page actions 可以有图标、提示信息、 弹出窗口。但没有 badge，也因此，作为辅助，page actions 可以有显示和消失两种状态。

使用方法 show() 和 hide() 可以显示和隐藏 page action。缺省情况下 page action 是隐藏的。当要显示时，需要指定图标所在的标签页，图标显示后会一直可见，直到该标签页关闭或开始显示不同的 URL （如：用户点击了一个连接）

### 5.3. TIPs

只对少数页面使用 page action，如果大多数页面都需要使用它，则使用 browser actions 代替。

## 6. 背景页

背景页是一个**运行在扩展进程中**的 HTML 页面。它**在你的扩展的整个生命周期都存在**，同时，**在同一时间只有一个实例处于活动状态**。

在一个有背景页的典型扩展中，用户界面（比如，浏览器行为或者页面行为和任何选项页）是由沉默视图实现的。当视图需要一些状态，它从背景页获取该状态。当背景页发现了状态改变，它会通知视图进行更新。

### 6.1. manifest.json

注册背景页：
```json
{
  "name": "My extension",
  ...
  "background": {
    "scripts": ["background.js"]
  },
  ...
}
```
一般，背景页不需要任何 HTML，仅仅需要 js 文件。浏览器的扩展系统会自动根据上面 scripts 字段指定的所有 js 文件自动生成背景页。

但如果你的确需要自己的背景页，可以使用 page 字段：
```json
{
  "name": "My extension",
  ...
  "background": {
    "page": "background.html"
  },
  ...
}
```

例：使用背景页来处理用户点击事件

background.js:
```javascript
// React when a browser action's icon is clicked.
chrome.browserAction.onClicked.addListener(function(tab) {
  
  var viewTabUrl = chrome.extension.getURL('image.html');
  var imageUrl = /* an image's URL */;
  
  // 返回属于你的扩展的每个活动页面的窗口对象列表
  var views = chrome.extension.getViews();

  for (var i = 0; i < views.length; i++) {
    var view = views[i];
    // If this view has the right URL and hasn't been used yet...
    if (view.location.href == viewTabUrl && !view.imageAlreadySet) {
      // ...call one of its functions and set a property.
      view.setImageUrl(imageUrl);
      view.imageAlreadySet = true;
      break; // we're done
    }
  }
});
```
image.html:
```html
<html>
  <script>
    function setImageUrl(url) {
      document.getElementById('target').src = url;
    }
  </script>
  <body>
    <p>
    Image here:
    </p>
    <img id="target" src="white.png" width="640" height="480">
  </body>
</html>
```

## 7. 选项页

为了让用户设定你的扩展功能，你可能需要提供一个选项页。如果你提供了选项页，在扩展管理页面 chrome://extensions 上会提供一个链接。点击选项链接就可以打开你的选项页。

例：

在 manifest.json 中配置选项页：
```json
{
  "name": "My extension",
  ...
  "options_page": "options.html",
  ...
}
```

编写选项页：
```html
<html>
  <head>
    <title>My Test Extension Options</title>
  </head>
  <script type="text/javascript">
    
    // Saves options to localStorage.
    function save_options() {
      var select = document.getElementById("color");
      var color = select.children[select.selectedIndex].value;
      localStorage["favorite_color"] = color;
      // Update status to let user know options were saved.
      var status = document.getElementById("status");
      status.innerHTML = "Options Saved.";
      setTimeout(function() {
        status.innerHTML = "";
      }, 750);
    }

    // Restores select box state to saved value from localStorage.
    function restore_options() {
      var favorite = localStorage["favorite_color"];
      if (!favorite) {
        return;
      }
      var select = document.getElementById("color");
      for (var i = 0; i < select.children.length; i++) {
        var child = select.children[i];
        if (child.value == favorite) {
          child.selected = "true";
          break;
        }
      }
    }
  </script>
  <body onload="restore_options()">
    Favorite Color:
    <select id="color">
      <option value="red">red</option>
      <option value="green">green</option>
      <option value="blue">blue</option>
      <option value="yellow">yellow</option>
    </select>
    <br>
    <button onclick="save_options()">Save</button>
  </body>
</html>
```

## 8. Cookie 操作

### 8.1. 声明权限

要使用 cookies API, 你必须在你的清单中声明"cookies"权限，以及任何你希望 cookie 可以访问的主机权限。例如：
```json
{
  "name": "My extension",
  ...
  "permissions": [
    "cookies",
    "*://*.google.com"
  ],
  ...
}
```

### 8.2. Cookie API：chrome.cookies

#### 8.2.1. get

> chrome.cookies.get(object details, function callback)

获取一个 cookie 的信息。如果对于给定的 URL 有多个 cookie 存在，将返回对应于最长路径的 cookie。对于路径长度相同的 cookies，将返回最早创建的 cookie。

#### 8.2.2. set

> chrome.cookies.set(object details)

用给定数据设置一个 cookie。如果相同的 cookie 存在，它们可能会被覆盖。

#### 8.2.3. onChanged

> chrome.cookies.onChanged.addListener(function(object changeInfo) {...});

当一个 cookie 被设置或者删除时候触发该事件。

## 9. 桌面通知

通知用户发生了一些重要的事情。桌面通知会显示在浏览器窗口之外。 下面的图片是通知显示时的效果，在不同的平台下，通知的显示效果会有些细微区别。

### 9.1. 声明权限

```json
{
  "name": "My extension",
  ...
  "permissions": [
    "notifications"
  ],
  ...
}
```
注意： 扩展声明的 notifications 权限总是允许创建通知。 这样申明之后就不再需要调用 webkitNotifications.checkPermission()。

### 9.2. API

例：创建一个简单的文字通知或 HTML 通知，然后显示通知
```javascript
// 创建一个简单的文字通知：
var notification = webkitNotifications.createNotification(
  '48.png',  // icon url - can be relative
  'Hello!',  // notification title
  'Lorem ipsum...'  // notification body text
);

// 或者创建一个 HTML 通知：
var notification = webkitNotifications.createHTMLNotification(
  'notification.html'  // html url - can be relative
);

// 显示通知
notification.show();
```

### 9.3. 与页面交互

扩展可以使用 getBackgroundPage() 和 getViews() 在通知与扩展页面中建立交互。 

例如：
```javascript
// 在通知中调用扩展页面方法。..
chrome.extension.getBackgroundPage().doThing();

// 从扩展页面调用通知的方法。..
chrome.extension.getViews({type:"notification"}).forEach(function(win) {
  win.doOtherThing();
});
```

## 10. content_scripts

通过 Manifest 中的 content_scripts 属性可以指定将哪些脚本何时注入到哪些页面中，当用户访问这些页面后，相应脚本即可自动运行，从而对页面 DOM 进行操作。

Manifest 的`content_scripts`属性值为数组类型，数组的每个元素可以包含 matches、`exclude_matches`、css、js、`run_at、all_frames`、`include_globs`和`exclude_globs`等属性。其中 matches 属性定义了哪些页面会被注入脚本，`exclude_matches`则定义了哪些页面不会被注入脚本，css 和 js 对应要注入的样式表和 JavaScript，`run_at`定义了何时进行注入，`all_frames`定义脚本是否会注入到嵌入式框架中，`include_globs`和`exclude_globs`则是全局 URL 匹配，最终脚本是否会被注入由 matches、`exclude_matches`、`include_globs`和`exclude_globs`的值共同决定。简单的说，如果 URL 匹配 mathces 值的同时也匹配`include_globs`的值，会被注入；如果 URL 匹配`exclude_matches`的值或者匹配 exclude_globs 的值，则不会被注入。

`content_scripts`中的脚本只是共享页面的 DOM1，而并不共享页面内嵌 JavaScript 的命名空间。也就是说，如果当前页面中的 JavaScript 有一个全局变量 a，`content_scripts`中注入的脚本也可以有一个全局变量 a，两者不会相互干扰。当然你也无法通过 content_scripts 访问到页面本身内嵌 JavaScript 的变量和函数。

## 11. Refer Links

官方文档：https://developer.chrome.com/extensions/getstarted.html 

Chrome JavaScript API：https://developer.chrome.com/extensions/api_index

中文文档：
- http://open.chrome.360.cn/extension_dev/overview.html
- https://crxdoc-zh.appspot.com 

入门教程：http://www.jianshu.com/p/cf5b3fba44ea 

《Chrome 扩展及应用开发（首发版）》：http://www.ituring.com.cn/book/miniarticle/110929

Chrome Web App 中文开发手册  https://crxdoc-zh.appspot.com/apps/ 

Google Plus 中文社群  https://plus.google.com/communities/105384952265487436177 