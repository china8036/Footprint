- [Chrome应用开发](#chrome%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91)
  - [基本框架](#%E5%9F%BA%E6%9C%AC%E6%A1%86%E6%9E%B6)
  - [Refer Links](#refer-links)

# Chrome应用开发

Chrome 应用提供了与原生应用能力相同的体验，但是与网页一样安全。就像网上应用一样，Chrome 应用使用 HTML5、JavaScript 和 CSS 编写，但是 Chrome 应用从外观上与行为上都与原生应用类似，它们也具有类似于原生应用的能力，比网上应用可用的更强大。

Chrome 应用可以访问对传统网站不可用的 Chrome 浏览器 API 与服务。

## 基本框架

manifest.json示例： 
```
{
    "manifest_version": 2,
    "name": "Add-To-PAC",
    "version": "0.9.0",
    "description": "Add the current domain to the local PAC file.",

    "app":
    {
        "background":
        {
            "scripts": [
                // "scripts/chromereload.js",
                "scripts/background.js"
            ]
        }
    },

    "icons":
    {
        "16": "icon-16.png",
        "128": "icon-128.png"
    },
    "permissions": [
        "http://*/",
        {
            "fileSystem": ["write", "directory"]
        }
    ],

    "author": "firejq@outlook.com"

}
```

## Refer Links
Chrome Web App中文开发手册  https://crxdoc-zh.appspot.com/apps/ 

Google Plus中文社群  https://plus.google.com/communities/105384952265487436177 
