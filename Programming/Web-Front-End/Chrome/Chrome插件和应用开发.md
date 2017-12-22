- [Chrome 插件和应用开发](#chrome-%E6%8F%92%E4%BB%B6%E5%92%8C%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [Chrome 插件](#chrome-%E6%8F%92%E4%BB%B6)
    - [Chrome 应用](#chrome-%E5%BA%94%E7%94%A8)
  - [文件组成](#%E6%96%87%E4%BB%B6%E7%BB%84%E6%88%90)
  - [manifest.json](#manifestjson)
  - [Refer Links](#refer-links)

# Chrome 插件和应用开发

## 概述

### Chrome 插件

Chrome 扩展文件以 `.crx` 为后缀名，在 Google Chrome 扩展官方网站下载扩展时，Chrome 会将 `.crx` 文件下载到 Chrome 的 Application Data 文件夹的 `User Data\Temp` 下，一般是 `C:\Documents and Settings\User\Local Settings\Application Data\Google\Chrome\User Data\Temp`，安装完成或者取消安装，该文件就会被删除。

`.crx` 实际上是一个压缩文件，使用解压文件打开这个文件就可以看到其中的文件目录。


### Chrome 应用

Chrome 应用提供了与原生应用能力相同的体验，但是与网页一样安全。就像网上应用一样，Chrome 应用使用 HTML5、JavaScript 和 CSS 编写，但是 Chrome 应用从外观上与行为上都与原生应用类似，它们也具有类似于原生应用的能力，比网上应用可用的更强大。

Chrome 应用可以访问对传统网站不可用的 Chrome 浏览器 API 与服务。

## 文件组成

一个插件一般共有 4 种文件：

1)	manifest.json：所有插件都要有这个文件，这是插件的配置文件，可看作插件的“入口”；

2)	*.png：小图标，推荐使用 `19*19` 的半透明 png 图片，更好的做法是同时提供一张 `38*38` 的半透明的 png 图片作为大图标；图标文件不要小于 `19px*19px`，但最好也不要超过这个尺寸，虽然大图它会自适应，但会使得应用文件变大。

3)	popup.html：popup 属于 Browser Actions，当点击图标时出现这个窗口，可以在里面放置任何 html 元素。但需要注意：对于 Chrome 扩展来说，很多对网页有意义的内容是无意义的（如`<title>`标签），所以我们可以只挑需要的写。

4)	*.js：html 页面所引用的 javascript 文件， 出于安全考虑，javascript 必须与 html 分开存放，inline-script 是禁止的；

## manifest.json

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

  // 以下4个字段多选一，或者都不提供
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
  "theme": {// 指定该扩展是一个主题，主题和标准扩展的打包方式类似, 但是主题中不能包含JavaScript或者HTML代码
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
  "app": {// 指定该扩展是一个 Chrome 应用，可安装的webapp，包括打包过的app，需要这个字段来指定app需要使用的url。最重要的是app的启动页面------当用户在点击app的图标后，浏览器将导航到的地方。
  },


  // 以下选项根据需要提供
  "permissions": [ // 定义扩展或app将使用的一组权限，详见 https://developer.chrome.com/extensions/declare_permissions
      "bookmarks", 
      "tabs", 
      "history",
      // 允许插件访问的 url
      "http://*/", // 这个插件被允许访问所有 http 链接
      "http://*.hacker.com/" // 这个插件被允许访 hacker.com 上的所有内容 
  ],
  
  "author": ...,// 作者信息

  "homepage_url":...,// 扩展的主页 url。扩展的管理界面里面将有一个链接指向这个url。如果你通过了Extensions Gallery和Chrome Web Store来分发扩展，主页 缺省就是扩展的页面。

  "options_page": "options.html",// 选项页面

  "Content scripts":...,//在Web页面内运行的javascript脚本。通过使用标准的DOM，它们可以获取浏览器所访问页面的详细信息，并可以修改这些信息。

  "background": {//指定插件的后台页面：这个页面不会显示，但它是扩展程序的后台服务，会一直保持运行，在你的扩展的整个生命周期都存在，同时，在同一时间只有一个实例处于活动状态。由于不显示，背景页不需要任何HTML，仅仅需要js文件。
    "scripts": [
      "scripts/chromereload.js",
      "scripts/background.js"
    ],
    "page":"background.html",// 如果的确需要自己的背景页，可以使用 page 字段定义
    // Recommended
    "persistent": false // 如果没有 "persistent" 键，您将得到一个普通的后台网页。是否持久存在是事件页面与后台网页之间的根本区别
  },

  // more and more ... ... 
}
```


## Refer Links

官方文档：https://developer.chrome.com/extensions/getstarted.html 

Chrome JavaScript API：https://developer.chrome.com/extensions/api_index

中文文档：
- http://open.chrome.360.cn/extension_dev/overview.html
- https://crxdoc-zh.appspot.com 

入门教程：http://www.jianshu.com/p/cf5b3fba44ea 

《Chrome 扩展及应用开发（首发版）》：http://www.ituring.com.cn/book/miniarticle/110929

Chrome Web App中文开发手册  https://crxdoc-zh.appspot.com/apps/ 

Google Plus中文社群  https://plus.google.com/communities/105384952265487436177 