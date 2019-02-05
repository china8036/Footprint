- [VS code](#vs-code)
  - [1. Plugin](#1-plugin)
  - [2. Key](#2-key)
  - [3. macOS mojave 字体发虚](#3-macos-mojave-字体发虚)
  - [4. Refer Links](#4-refer-links)

# VS code

## 1. Plugin

- Settings Sync: 同步 vscode 设置
  - 使用教程：http://www.bijishequ.com/detail/513176
  - 在 GitHub 中 new 一个 personal access token：https://github.com/settings/tokens/new；
  勾选 gist 后生成 personal access token
  ![image](http://img.cdn.firejq.com/jpg/2017/9/28/960d0ae37fa3affbb2a4793c2120fdd9.jpg)
  复制生成的 token，在 VS code 中按下 alt + shift + u，输入 token 后 enter 即完成 settings 的上传 / 更新；
  需要同步时，在 ctrl + shift + p 中输入 `sync`，选择 download 即可；

- TODO HIGHLIGHT
- vscode-icons
- markdownlint:markdown 语法提示器
- Bracket Pair Colorizer ：对括号对进行着色
- filesize : 会在左下角显示文件大小
- Path Intellisense: 路径提示器
- HTML Snippets：HTML5 代码提示
- HTML CSS Support：让 html 标签上写 class 智能提示当前项目所支持的样式
- jQuery Code Snippets：jQuery 代码提示
- Atuo Rename Tag：修改 html 标签时自动帮你完成尾部闭合标签的同步修改
- writing4cn：markdown 的中文格式化方案，如执行 `pangu`，自动在中英文字符间增加一个半角空白等
- markdown-preview-enhanced：预览增强，见 https://shd101wyy.github.io/markdown-preview-enhanced/#/zh-cn/
- markdown preview github styling
- markdown all in one

## 2. Key

![image](http://img.cdn.firejq.com/jpg/2017/9/28/5adbfc65263aebefb11b54ca3e40c5df.jpg)

<!-- TODO VS code -->
- 实现保存的时候自动执行某些任务？如 pangu
- 快捷键查询
	- 删除 / 选择整行
	- 打开设置
	- 设置打开预览、关闭预览的快捷键
	- 设置在浏览器打开 markdown 预览的快捷键
	- 设置执行 pangu 的快捷键
- TODO list 插件是怎么使用的？
- 怎么在 VS code 中快速在当前目录打开 git-bash？
- 怎么进行多行选择、任意区域选择？

## 3. macOS mojave 字体发虚

在终端执行：
```
defaults write -g CGFontRenderingFontSmoothingDisabled -bool NO
```

重启 VSCode 后即可解决。

## 4. Refer Links

http://www.cnblogs.com/rengised/p/6985031.html

http://www.nshen.net/article/2015-11-20/vscode/

https://jeasonstudio.gitbooks.io/vscode-cn-doc/content/