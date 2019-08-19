- [SublimeText Note](#sublimetext-note)
	- [1. 安装 PackageControl](#1-安装-packagecontrol)
	- [2. 安装插件](#2-安装插件)
	- [3. 配置文件](#3-配置文件)
	- [4. 快捷键](#4-快捷键)

# SublimeText Note

## 1. 安装 PackageControl

https://packagecontrol.io/installation

使用 ctrl+`调出 console，执行代码：
```python
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```
即可完成 PackageControl 的安装。

安装完成后，使用 ctrl+shfit+p，输入`install`，选择包管理工具，即可开始管理安装插件：

![image](http://img.cdn.firejq.com/jpg/2017/10/28/a46a273a72d3cd88ca192804ba01357a.jpg)

## 2. 安装插件

- CodeIntel：自动补全 + 成员 / 方法提示；

- Bracket Highlighter：括号匹配及高亮；

- SublimeLinter：代码 pep8 标准规范格式检查；

- ConvertToUTF8：编码转换工具；

- Trimmer：自动删除这些不必要的空格和空行；

- MarkdownEditing：Markdown 利器；

- CSScomb：CSS 属性排序，CSScomb 可以按照一定的 CSS 属性排序规则，将杂乱无章的 CSS 属性进行重新排序。选中要排
序的 CSS 代码，按 Ctrl+Shift+C，即可对 CSS 属性重新排序。因为这个插件使用 PHP 写的，要使他工作需要在环境变
量中添加 PHP 的路径，具体请看 github 上的说明；

- JsFormat：JavaScript 格式化，按快捷键 Ctrl+Alt+F 即可格式化当前的 js 文件；

- Alignment：等号对齐，按 Ctrl+Alt+A，可以将凌乱的代码以等号为准左右对齐；

- HTML-CSS-JS Prettify：最好用的代码格式化工具，需要先安装 nodejs；
通过鼠标右键 HTML/CSS/JS Prettify > Set Plugin Options 保证插件路径与 Node.js 安装路径一致；
选择需要格式化的 html/css/js 代码，右键 HTML/CSS/JS Prettify > prettify code；

- nginx：支持 nginx.conf 语法高亮，sublime 默认不支持；

- Emmet：前端开发神器，输入标签简写形式，然后按 Tab 键即可自动完成标签，如 div#nav>div.item$*4；

- markdown 实时预览环境搭建：
  - MarkdownEditing：语法高亮；
  - Auto saved：针对一个文档实现空闲 x 秒后自动保存
    安装完毕后，Preference->Package Settings->Auto-save->打开 Settings-Defualt 和 Settings-User，将
    Default 的内容复制粘贴到 User 里面，然后修改配置：
    ```
    // AutoSave's default settings.

    {
      "auto_save_on_modified": true,
      "auto_save_delay_in_seconds": 0.15,
      "auto_save_all_files": true,
      "auto_save_current_file": true,
      "auto_save_backup": false,
      "auto_save_backup_suffix": "autosave"
    }
    ```
    经过实测，0.15 是一个比较能接受的值，不会对磁盘造成频繁读写的影响，延迟也不大。 最后就是打开本文档的自动
    保存功能了： 按下 ctrl+shift+P 打开快速菜单，键入`auto` ，选择到 all file 按下回车，看到左下角状态栏显示 auto
    saved on 即可。
  - chrome 浏览器安装插件 MarkDown Preview Plus：使得 chrome 打开的所有 md 文件都支持实时预览
chrome 中扩展程序设置：
    ![image](http://img.cdn.firejq.com/jpg/2017/10/28/73295d6f389fa01103433142428148d0.jpg)
    设置选项：
    ![image](http://img.cdn.firejq.com/jpg/2017/10/28/fc471eaeadbcbf67fd95beac8ee25265.jpg)

## 3. 配置文件

```
{
	"draw_minimap_border": true,
	"always_show_minimap_viewport": true,
	"caret_style": "phase",
	"color_scheme": "Packages/User/SublimeLinter/Monokai (SL).tmTheme",
	"default_encoding": "UTF-8",
	"dpi_scale": 1.0,
	"fade_fold_buttons": true,
	"fold_buttons": true,
	"font_face": "Consolas",
	"font_size": 16.0,
	"highlight_line": true,
	"highlight_modified_tabs": true,
	"ignored_packages":
	[
		"Vintage"
	],
	"line_padding_bottom": 1,
	"line_padding_top": 1,
	"margin": 2,
	"overlay_scroll_bars": "enable",
	"scroll_past_end": true,
	"show_encoding": true,
	"smart_indent": false,
	"spell_check": false,
	"tab_size": 4,
	"tanslate_tabs_to_spaces": false,
	"trim_automatic_white_space": true,
	"update_check": false,
	"word_wrap": true,
	"save_on_focus_lost": true,
}

```

## 4. 快捷键

```
Ctrl+Shift+P：打开命令面板
Ctrl+P：搜索项目中的文件
Ctrl+W：关闭当前打开文件
Ctrl+Shift+W：关闭所有打开文件
Ctrl+D：选择单词，重复可增加选择下一个相同的单词
Ctrl+L：选择行，重复可依次增加选择下一行
Ctrl+Shift+L：选择多行
Ctrl+X：删除当前行
Ctrl+F：查找内容
Ctrl+Shift+F：查找并替换
Ctrl+H：替换
Ctrl+R：前往 method
Ctrl+N：新建窗口
Ctrl+F2：设置 / 删除标记
Ctrl+/：注释当前行
Ctrl+Shift+/：当前位置插入注释
Ctrl+Alt+/：块注释，并 Focus 到首行，写注释说明用的
Ctrl+Shift+A：选择当前标签前后，修改标签用的
F11：全屏
Shift+F11：全屏免打扰模式，只编辑当前文件
Alt+F3：选择所有相同的词
Alt+.：闭合标签
Alt+Shift+ 数字：分屏显示
Alt+ 数字：切换打开第 N 个文件
Shift+ 右键拖动：光标多不，用来更改或插入列内容
Ctrl+ 依次左键点击或选取，可需要编辑的多个位置
Ctrl+Shift+ 上下键：替换行
```