- [Bash](#bash)
  - [1. Hotkey](#1-hotkey)
  - [2. Refer Links](#2-refer-links)

# Bash

## 1. Hotkey

Bash 默认为 emacs 编辑模式，因此默认使用的是emacs风格的hotkey。可通过 `set -o vi/emacs` 切换为 vi 风格的hotkey。

- 光标移动
  - Ctrl + a	移动光标到行首
  - Ctrl + e	移动光标到行尾
  - Ctrl + xx	当前位置与行首之间光标切换
- 剪切粘贴
  - Ctrl + k	删除从光标到行尾
  - Ctrl + u	删除从光标到行首
  - Ctrl + w	从光标向前删除一个单词
  - Ctrl + d	删除光标下一个字母
  - Ctrl + h	删除光标前一个字母
  - Ctrl + y	粘贴上一次删除的文本
- 大小写转换
  - Alt + c	大写当前字母，并移动光标到单词尾
  - Alt + u	大写从当光标到单词尾
  - Alt + l	小写从当光标到单词尾
- 历史命令
  - Ctrl + r	向后搜索历史命令
  - Ctrl + g	退出搜索
  - Ctrl + p	历史中上一个命令
  - Ctrl + n	历史中下一个命令
  - Alt + .	上一个命令的最后一个单词
- 终端指令
  - Ctrl + l	清屏
  - Ctrl + s	停止输出（在 zsh 中为向前搜索历史命令）
  - Ctrl + q	继续输出
  - Ctrl + c	终止当前命令
  - Ctrl + z	挂起当前命令
  - Ctrl + d	结束输入（产生一个 EOF）

## 2. Refer Links

[Wikipedia: Shell Keyboard shortcuts](https://en.wikipedia.org/wiki/GNU_Readline#Emacs_keyboard_shortcuts)

[熟悉 Bash 快捷键来提高效率](https://harttle.land/2015/11/09/bash-shortcuts.html)

[让你提升命令行效率的 Bash 快捷键](https://linuxtoy.org/archives/bash-shortcuts.html)