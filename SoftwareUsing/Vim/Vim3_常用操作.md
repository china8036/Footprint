- [Vim 常用操作](#vim-常用操作)
  - [1. 浏览目录](#1-浏览目录)
  - [2. 打开文件](#2-打开文件)
  - [3. 刷新文件](#3-刷新文件)
  - [4. 游标移动](#4-游标移动)
  - [5. 屏幕移动](#5-屏幕移动)
  - [6. 插入文本](#6-插入文本)
  - [7. 保存文档](#7-保存文档)
  - [8. % 操作](#8--操作)
  - [9. 退出 vim](#9-退出-vim)
    - [9.1. 命令行模式下退出](#91-命令行模式下退出)
    - [9.2. 普通模式下退出](#92-普通模式下退出)
  - [10. 删除文本](#10-删除文本)
  - [11. 撤销操作](#11-撤销操作)
  - [12. 复制粘贴](#12-复制粘贴)
  - [13. 搜索文本](#13-搜索文本)
  - [14. 替换文本](#14-替换文本)
    - [14.1. 移动后替换](#141-移动后替换)
    - [14.2. 搜索后替换](#142-搜索后替换)
  - [15. 字符操作](#15-字符操作)
  - [16. 执行 shell](#16-执行-shell)
  - [17. 使用 HELP](#17-使用-help)
  - [18. 多窗口 (Window) 操作](#18-多窗口-window-操作)
  - [19. 多标签 (Tab) 操作](#19-多标签-tab-操作)
  - [20. 保存会话](#20-保存会话)
  - [21. 缓冲区操作](#21-缓冲区操作)
  - [22. 选择操作](#22-选择操作)
  - [23. 宏 (Macro) 操作](#23-宏-macro-操作)
  - [24. 映射 (Map) 操作](#24-映射-map-操作)
  - [25. 主题操作](#25-主题操作)
  - [26. 自动命令操作](#26-自动命令操作)
    - [26.1. autocmd 命令](#261-autocmd-命令)
    - [26.2. vim 事件 (events)](#262-vim-事件-events)
    - [26.3. pattern](#263-pattern)
  - [27. 自定义命令操作](#27-自定义命令操作)
  - [28. 自定义函数操作](#28-自定义函数操作)
  - [29. Quickfix 操作](#29-quickfix-操作)
  - [30. Refer Links](#30-refer-links)

# Vim 常用操作

## 1. 浏览目录

使用 `:Exp[lore] 目录路径` 或 `:e 目录路径` 即可打开目录（默认为当前路径），在目录浏览状态下可进行以下操作：
- 选择文件可直接打开
- `–` 到上级目录
- `D` 删除文件（大写）
- `R` 改文件名（大写）
- `s` 对文件排序（小写）
- `x` 执行文件
- `v` 创建新的水平分屏并在新的窗口中进入子目录

如果你要改变当前浏览的目录，或是查看当前浏览的目录，你可以使用和 shell 一样的命令：
- `:cd <dir>` 改变当前目录
- `:pwd` 查看当前目录
- `Ctrl` + `o` 返回文件编辑

打开文件后若想返回刚刚浏览的目录，使用`:Rex[plore]`。

## 2. 打开文件

使用 vim 命令打开指定文件：`vim practice_1.txt`。

进入命令行模式后输入 `:e 文件路径` 也可以打开相应文件。

## 3. 刷新文件

若在编辑过程中，文件内容发生了更新，需要进行文件重新载入的刷新操作：
- 使用 `:e` 重新载入文件
- 使用 `:e!` 放弃当前已编辑的内容，重新载入文件

## 4. 游标移动

在普通模式下使用方向键或者 `h,j,k,l` 键可以移动游标。
- h	左
- l	右
- j	下
- k	上

![image](http://img.cdn.firejq.com/jpg/2018/10/26/7aa3667c17db58c9150ffa3713859276.jpg)

- `w`	移动到下一个单词开头
- `W`	移动到下一个单词开头，忽略非字母字符
- `b`	移动到上一个单词开头
- `B`	移动到上一个单词，忽略非字母字符
- `e` 移动到下一个单词末尾
- `E` 移动到下一个单词末尾，忽略非字母字符
- `ge` 移动到上一个单词末尾
- `0`/`home` 移动到行首
- `$`/`end` 移动到行尾
- `^` 移动到本行的第一个非 blank 字符
- `g_` 移动到本行最后一个非 blank 字符

- `Ctrl` + `g` 显示总行数和进度
- `gg` 跳转到文档头部
- `G` 跳转到文档末尾
- 跳转到指定行`
  - `number + gg`
  - `number + G`
  - `:number`
- `g;` 跳转到最后一个发生修改的行
- `t [char]` 移动到指定字符的前一个字符
- `Ctrl` + `f`/`F` 光标向下移动一页
- `Ctrl` + `b`/`B` 光标向上移动一页
- `Ctrl` + `u` 光标向上移动半页
- `Ctrl` + `d` 光标向下移动半页

P.S.
- 在命令前输入一个数字 n 可重复执行命令 n 次。

- 为什么 vim 会使用这么奇葩的方向键 h,j,k,l 及那么别扭反人类的 Esc 作为退出插入模式的键？Vim 作者 Bill Joy 最初使用的机器是 [ADM-3A](https://en.wikipedia.org/wiki/ADM-3A) 终端机，其键盘布局如下：

  ![image](http://img.cdn.firejq.com/jpg/2018/10/25/a431bfbace91896c69c6b0feed3b2999.jpg)

## 5. 屏幕移动

- Ctrl + e 屏幕向下移动一行
- Ctrl + y 屏幕向上移动一行
- zt 屏幕移动到当前行置顶的位置
- zz 屏幕移动到当前行居中的位置
- zb 屏幕移动到当前行置底的位置

## 6. 插入文本

- 在插入模式下，直接输入字符即可插入文本
- 在命令模式下，使用 `r 文件名`(read) 可将指定文件内容插入到当前游标处
- 在命令模式下，使用 `r !command`(read) 可将指定 command 在 shell 中执行的输出结果文本插入到当前游标处，如 `r !pwd`

## 7. 保存文档

从普通模式输入：进入命令行模式，输入 w 回车，保存文档。

输入 `:w 文件名` 可以将文档另存为其他文件名或存到其它路径下。

在 VISUAL 模式下，选择指定文本区块后输入 `:w 文件名` 可将被选择的文本区块另存为新的文档。

## 8. % 操作

Vim 有 48 个寄存器，其中 % 只读寄存器中保存着当前文件路径。

- 打印当前文件名 `:echo @%`
- 打印当前文件绝对路径 `:echo expand('%:p')`
- 备份当前文件 main.cpp 到 main.cpp.bak `:w % %.bak`
- 插入当前文件名 `"%p`
- 拷贝当前文件名到剪切板，当然你可以把它做成快捷键 `:let @*=expand("%:t")`

## 9. 退出 vim

### 9.1. 命令行模式下退出

在命令行模式中，有以下退出方式：
- `:q!`	强制退出，不保存
- `:q` 退出
- `:wq!` 强制保存并退出
- `:w file_path` 另存为
- `:saveas file_path` 另存为
- `:x` 保存并退出
- `:wq` 保存并退出

### 9.2. 普通模式下退出

普通模式：
- ZZ 相当于 `:wq` 或 `:x`
- ZQ 相当于 `:q!`
- Ctrl + w + q 相当于 `:q`

## 10. 删除文本

**在 vim 中，所有使用 d 命令进行删除的操作，事实上都是剪切操作**。

在普通模式下：
- `x`/`d + right`	删除游标所在的字符
- `X`/`Backspace`/`Delete`/`d + left`	删除游标所在前一个字符
- `[n]dd`	从当前光标往下删除 n 个整行（包括当前行）
- `d + up` 删除当前光标往上的 2 行
- `d + down` 删除当前光标往下的 2 行
- `d[n]w` 删除光标后的 n 个单词（不适用中文），包括单词后的空白也会一起删除，操作结束后光标落在下一个单词的开头
- `d[n]e` 删除光标后的 n 个单词（不适用中文），不会删除单词后的空白，操作结束后光标落在被删除单词的下一个字符
- `d[n]b` 删除光标前的 n 个单词（不适用中文）
- `d^` 删除至行首第一个非空白字符处
- `d0` 删除至行首
- `d$` / `D` 删除至行尾
- `dG` 删除当前行到文档结尾处的所有内容
- `dgg`	删除当前行到文档首部的所有内容
- `d/<pattern>` 删除**当前行内**从当前游标到指定模式串前的所有内容（不包括该模式串）
- `dt<word>` 删除**当前行内**从当前游标到指定字符前的所有内容（不包括该字符）
- `di'` 删除单引号内的内容
- `di"` 删除双引号内的内容
- `di(` 删除小括号内的内容
- `di[` 删除中括号内的内容
- `di{` 删除大括号内的内容
- `di<` 删除尖括号内的内容
- `dit` 删除 html 标签内容
- `da'` 删除单引号内的内容以及单引号本身
- `da"` 删除双引号内的内容以及双引号本身
- `da(` 删除小括号内的内容以及小括号本身
- `da[` 删除中括号内的内容以及中括号本身
- `da{` 删除大括号内的内容以及大括号本身
- `da<` 删除尖括号内的内容以及尖括号本身
- `dat` 删除 html 标签内容以及 html 标签本身
- 在普通模式下，使用 `v` 进行 VISUAL 模式，批量选择文本后使用`x`或`d`可删除被选择的文本

- `J` 删除下一行行首的换行符，即将游标所在行与下一行结合成同一行
- `K` 删除上一行行末的换行符，即将游标所在行与上一行结合成同一行

- `s` 删除光标后的一个字符并进入 insert 模式
- `C` 删除光标到行末的所有字符并进入到 insert 模式
- `S` / `cc` 删除光标所在的一整行并进入到 insert 模式
- `cw` / `ce` 删除光标后一个单词并进入 insert 模式
- `ci'` 删除单引号内的内容并进入 insert 模式
- `ci"` 删除双引号内的内容并进入 insert 模式
- `ci(` 删除小括号内的内容并进入 insert 模式
- `ci[` 删除中括号内的内容并进入 insert 模式
- `ci{` 删除大括号内的内容并进入 insert 模式
- `ci<` 删除尖括号内的内容并进入 insert 模式
- `cit` 删除 html 标签内容，并进入 insert 模式
- `ca'` 删除单引号内的内容以及单引号本身，并进入 insert 模式
- `ca"` 删除双引号内的内容以及双引号本身，并进入 insert 模式
- `ca(` 删除小括号内的内容以及小括号本身，并进入 insert 模式
- `ca[` 删除中括号内的内容以及中括号本身，并进入 insert 模式
- `ca{` 删除大括号内的内容以及大括号本身，并进入 insert 模式
- `ca<` 删除尖括号内的内容以及尖括号本身，并进入 insert 模式
- `cat` 删除 HTML 标签内容以及 HTML 标签本身，并进入 insert 模式

在命令模式下：
- `:a,b d` 删除第 a 行到第 b 行之间的所有内容，a/b 默认值为当前行的行数
- `:a d` 删除第 a 行
- `:g/rex/d` 删除所有与正则表达式匹配的内容

## 11. 撤销操作

在普通模式下：
- 使用 `u` 撤销最后一个操作
- 使用 `U` 撤销一整行的操作
- 使用 `Ctrl` + `r` 撤销上一个撤销操作

## 12. 复制粘贴

- `y`(yank/copy) 复制选中文本
- `yw`(yank/copy) 复制一个单词
- `yy`(yank/copy) 复制一整行
- `p`(put/paste) 粘贴到光标后
- `P`(put/paste) 粘贴到光标前

Vim 中有 12 个粘贴板（寄存器），使用 `:h reg` 可以查看相关文档，使用 `:reg` 可以看到当前所有粘贴板（寄存器）内的内容：

![image](http://img.cdn.firejq.com/jpg/2018/7/2/0e40c91b4939caff03430c67320c0049.jpg)

其中：
- `"` 粘贴板：默认粘贴板，直接按 y 就复制到这个粘贴板中，直接按 p 就粘贴这个粘贴板中的内容。
- `+` 粘贴板：系统粘贴板，使用 `"+y` 将内容复制到该粘贴板后可以使用 `Ctrl + v` 将其粘贴到其他文档（如 firefox、gedit）中，同理，要把在其他地方用 `Ctrl＋C` 或右键复制的内容复制到 vim 中，需要在正常模式下按 `"+p`。
- 数字粘贴板：
- 字母粘贴板：

操作：
- 要将 vim 的内容复制到某个粘贴板，需要退出编辑模式，进入正常模式后，选择要复制的内容，然后按 `"Ny`（注意带引号）完成复制，其中 N 为粘贴板号。
- 要将 vim 某个粘贴板里的内容粘贴进来，需要退出编辑模式，在正常模式按 `"Np`，其中 N 为粘贴板号。

## 13. 搜索文本

在普通模式下：
- 使用 `/` 进入向下搜索模式，键入关键词后回车即可
- 使用 `?` 进入向上搜索模式，键入关键词后回车即可
- 使用 `n` 找到下一个匹配
- 使用 `N` 找到上一个匹配
- 使用 `Ctrl` + `o` 返回上一个位置
- 使用 `Ctrl` + `i` 回到下一个位置
- 将游标移动到大 / 中 / 小括号处，使用 `%` 找到与之配对的另一个括号
- 使用 `*` 或 `#` **快速搜索当前游标所在的单词**
- 使用 `f<word>` 在当前行向后找到第一个 `word` 字符出现的位置并移动到该位置
- 使用 `F<word>` 在当前行向前找到第一个 `word` 字符出现的位置并移动到该位置

## 14. 替换文本

### 14.1. 移动后替换

- 替换单个字符

  在普通模式下，将光标移动至要替换的字符处，使用 `r` 进入替换状态，再按下目标字符即可完成替换，完成替换后自动结束替换状态。

- 替换多个字符

  在普通模式下，将光标移动至要替换的字符处，使用 `R` 进入 REPLACE 模式，键入目标字符可替换原字符，使用 `backspace` 还原替换，使用 `esc` 退出替换模式。

### 14.2. 搜索后替换

输入 `:` 进行命令模式：
- 使用 `s/old/new` 替换当前游标所在行的 old 为 new，只替换第一个匹配的 old
- 使用 `s/old/new/g` 替换当前游标所在行的 old 为 new，替换当前行所有匹配的 old
- 使用 `a,bs/old/new/g` 替换从行 a 到行 b 的 old 为 new，替换区间内所有匹配的 old
- 使用 `%s/old/new/g` 替换整个文件中的 old 为 new
- 使用 `%s/old/new/gc` 逐个替换整个文件中的 old 为 new，每次替换都使用交互模式进行确认

e.g.
- 使用 `%s/^/#/g` 在所有行的行首加上 `#`（注释）

## 15. 字符操作

- ~ 将游标所在的字符进行大小写转换
- U 将选中字符变大写
- u 将选中字符变小写
- gU 将游标所在的字符变大写
- gU 将游标所在的字符变小写
- gUU 将一整行变大写
- guu 将一整行变小写
- ga 查看游标所在的字符的 ASCII 码
- g8 查看游标所在的字符的 UTF-8 编码
- gf 打开游标处所指路径的文件
- 缩进
  - `> + enter` / `>>` 向右缩进一个单位
  - `< + enter` / `<<` 向左缩进一个单位
  - `:a,b >` 将第 a 行到第 b 行的文本向右缩进一个单位（`>>`缩进 2 个单位）
  - `:a,b <` 将第 a 行到第 b 行的文本向左缩进一个单位（`>>`缩进 2 个单位）

- `Ctrl` + `a` 当前数字自增一次
- `Ctrl` + `x` 当前数字自减一次

## 16. 执行 shell

Type  `:!`	followed by an external command to execute that command. All  `:`  commands must be finished by hitting <ENTER>.

eg:
```
:!pwd
```

## 17. 使用 HELP

> Vim has a comprehensive on-line help system.

使用以下方式获取 HELP：
- F1 打开 help
- `:help` 打开 help，`:help subject` 打开指定主题的 help，如 `:help w` / `:help insert-index`

## 18. 多窗口 (Window) 操作

- shell 中使用 `vim -On a.cpp b.cpp c.cpp` 使用垂直分屏打开多个文件进行浏览，n 表示分成 n 个屏
- shell 中使用 `vim -on a.cpp b.cpp c.cpp` 使用水平分屏打开多个文件进行浏览，n 表示分成 n 个屏

- `:split` / `:sp` / `Ctrl` + `w` + `s` 创建水平分屏，新窗口内容与原窗口相同
- `:vsplit` / `:vsp` / `Ctrl` + `w` + `v` 创建垂直分屏，新窗口内容与原窗口相同
- `:He`(Hexplore) 把当前窗口上下分屏，并在下面进行目录浏览
- `:He!` 把当前窗口上下分屏，并在上面进行目录浏览
- `:Ve`(Vexplore) 把当前窗口左右分屏，并在左边进行目录浏览
- `:Ve!` 把当前窗口左右分屏，并在右边进行目录浏览
- `:set scb`(scrollbind) 到需要同步移动的两个屏中都输入改名了，从而让两个分屏中的文件同步移动

- `Ctrl` + `w` + `_` ( 或 <Ctrl-w>| ) : 最大化尺寸 (<C-w>| 垂直分屏)
- `Ctrl` + `w` + `+` 增加当前窗口的尺寸
- `Ctrl` + `w` + `-` 减少当前窗口的尺寸
- `Ctrl` + `w` + `=` 让所有窗口的尺寸一致
- `:res[ize] +n`: 批量增加当前的垂直窗口的尺寸
- `:res[ize] -n`: 批量减少当前的垂直窗口的尺寸
- `:vertical res[ize] +n`: 批量增加当前的水平窗口的尺寸
- `:vertical res[ize] -n`: 批量减少当前的水平窗口的尺寸
- `Ctrl` + `w` + `<direction>` 在多个窗口中进行切换，direction 可以是 `hjkl` 或 `←↓↑→` 中的一个
- `:q` / `Ctrl` + `w` + `q` 关闭当前窗口

## 19. 多标签 (Tab) 操作

- shell 中使用 `vim -p a.cpp b.cpp c.cpp` 使用多个 Tab 打开多个文件进行浏览

- `:bufdo tab split` 把 buffer 中的文件全转成 Tab
- `:tabs` 查看所有打开的 Tab 以及 Tab 内的分屏情况
- `:tabnew` 创建新的空白 Tab 页
- `:Te`(TabExplorer) 创建新的 Tab 页，并进行目录浏览
- `gt` 到下一个 Tab
- `gT` 到前一个 Tab
- `{i}gt` 到指定 Tab
- `:tabclose` 关闭当前 Tab
- `:tabclose{i}` 关闭指定 Tab
- `:tabonly` 仅保留当前 Tab，关闭其它所有 Tab
- `:qa` 关闭所有 Tab 并退出
- `:wqa` 保存所有 Tab 后关闭并退出

## 20. 保存会话

- 如果你用 Tab 或 Window 打开了许多文件，还设置了各种滚屏同步，或是行号……，那么，你可以用下面的命令来保存会话：
  ```bash
  :mksession mysession.vim
  ```
  如果文件重复，vim 默认会报错，如果你想强行写入的话，你可以在 mksession 后加 `!`。

- 使用以下命令打开会话
  ```bash
  vim -S mysession.vim
  ```

## 21. 缓冲区操作

当在 vim 中打开其他文件时，当前文件会被“关闭”，而实际上并没有被关闭，这些文件都放在缓冲区中，针对缓冲区文件，可进行以下操作：
- 使用 `:files` / `:buffers` / `:ls` 查看缓冲区。查看缓冲区文件时，文件相关标记的含义如下：
  - `–` （非活动的缓冲区）
  - `a` （当前被激活缓冲区）
  - `h` （隐藏的缓冲区）
  - `%` （当前的缓冲区）
  - `#` （交换缓冲区）
  - `=` （只读缓冲区）
  - `+` （已经更改的缓冲区）
- 使用 `:buffer n` 打开序号为 n 的缓冲区文件。
- 使用以下命令快速切换缓冲区文件：
  - `:bnext` 缩写 :`bn`
  - `:bprevious` 缩写 :`bp`
  - `:blast` 缩写 :`bl`
  - `:bfirst` 缩写 :`bf`
  - `:ball` 缩写：`ba`

## 22. 选择操作

- `viw` 选中当前单词
- `vaw` 选中当前单词，包括周边
- `vis` 选中当前句子
- `vas` 选中当前句子，包括周边
- `vip` 选中当前段落
- `vap` 选中当前段落，包括周边
- `vi(` 选中小括号内的内容
- `vi[` 选中中括号内的内容
- `vi{` 选中大括号内的内容
- `vit` 选中 HTML 标签内的内容
- `va(` 选中小括号内的内容，包括周边
- `va[` 选中中括号内的内容，包括周边
- `va{` 选中大括号内的内容，包括周边
- `vat` 选中 HTML 标签内的内容，包括周边

## 23. 宏 (Macro) 操作

- `qa` 开始录制宏到寄存器 a 中
- `qA` 在寄存器 a 追加宏操作
- `q` 结束宏录制操作
- `@a` 执行寄存器 a 的宏操作
- `n@a` 执行 n 次寄存器 a 的宏操作
- `@@` 执行上一次宏操作
- `:let @a=` 编辑寄存器 a 的宏操作

例：在一个只有一行且这一行只有“1”的文本中：
1. qa 开始录制
1. Yp 复制行。
1. <C-a> 增加 1.
1. q 停止录制。
1. @a → 在 1 下面写下 2
1. @@ → 在 2 正面写下 3
1. 现在做 100@@ 会创建新的 100 行，并把数据增加到 103.

## 24. 映射 (Map) 操作

[vimrc doc: map](http://vimcdoc.sourceforge.net/doc/map.html)

Map 是 Vim 强大的一个重要原因，可以自定义各种快捷键，用起来自然得心应手。通过 `:h map-commands` 可以查看相关文档。

- `:map [key] YOUR_COMMAND`

  eg:
  - `:map <F2> a<C-R>=strftime("%c")<CR><Esc>`: 定义 F2 为快捷键，在光标之后追加当前的日期和时间。
  - `:map <S-F2> <Esc>i<ul><CR><Space><Space><li></li><CR></ul><Esc>kcit`: 定义 `Shift + F2` 为快捷键，在光标后追加 HTML 标签，并将光标移动到标签体的位置。

- `:mapclear`

- `:unmap`

对于以上基本的映射命令，可能有以下前缀：
- `nore`: 表示非递归。
- `n`: 表示在普通模式下生效。
- `v`: 表示在可视模式下生效。
- `i`: 表示在插入模式下生效。
- `c`: 表示在命令行模式下生效。

eg:
- `imap <silent> <C-D><C-D> <C-R>=strftime("%e %b %Y")<CR>`: 在 Insert 模式下输入两次 CTRL-D 将促使 Vim 调用它的内置 strftime() 函数并插入当前的日期。**可以使用相同的通用模式，让插入模式或缩写词执行任何可编写脚本的操作**。只需要将相应的 Vimscript 表达式或函数调用放到第一个 `<C-R>=`（告诉 Vim 插入后面内容的计算结果）和最后一个 `<CR>`（告诉 Vim 实际地执行前面的表达式）之间。
- `imap <silent> <C-D><C-D> <C-R>=getcwd()<CR>`: 插入当前路径。
- `imap <silent> <C-C> <C-R>=string(eval(input("Calculate: ")))<CR>`: 嵌入一个简单的计算器，在文本插入期间输入 `CTRL-C` 便可调用该计算器。

对于更加复杂的执行逻辑，通常最好将代码重构为一个用户定义的函数，键映射后直接调用该函数即可。

## 25. 主题操作

通过 `:colorscheme theme_name` 可以载入指定的颜色主题。

## 26. 自动命令操作

[vimrc doc: autocmd](http://vimcdoc.sourceforge.net/doc/autocmd.html)

在文件读写，缓冲区或窗口进出，甚至 Vim 退出等时刻，都可以指定要自动执行的命令。

### 26.1. autocmd 命令

若 vim 编译时加入了 `+autocmd` 特性，则可通过 `:autocmd` / `:au` 命令可以在 vim 中定义自动命令操作：
- 定义自动命令

  - `:au[tocmd] [group] {event} {pat} [nested] {cmd}`: 把 {cmd} 加到 Vim 在匹配 {pat} 模式的文件执行 {event} 事件时自动执行的命令列表。Vim 总把 {cmd} 加到已有的自动命令之后，这样保证自动命令的执行顺序与其定义的顺序相同。如果没有给定 {group} 参数，Vim 使用当前组 （由 `:augroup` 定义）。

    可以通过 `:set verbose=9` 使 Vim 在执行自动命令时回显，便于调试和测试。

    例：在前端开发中使 1 个 tab 为 2 个 spaces
    ```
    autocmd BufNewFile,BufRead *.html,*.htm,*.css,*.js set expandtab tabstop=2 shiftwidth=2
    ```
    例：使用本地缓冲区的缩写和自动命令来创建一个简单的“snippet”系统：
    ```
    :autocmd FileType python     :iabbrev <buffer> iff if:<left>
    :autocmd FileType javascript :iabbrev <buffer> iff if ()<left>
    ```

  - `:au[tocmd]! [group] {event} {pat} [nested] {cmd}`: 覆盖原有和 {event} 事件和 {pat} 模式相关联的自动命令。

- 删除自动命令

  - `:au[tocmd]! [group] {event} {pat}`:  删除所有和 {event} 事件和 {pat} 模式相关联的自动命令。
  - `:au[tocmd]! [group] * {pat}`: 删除所有和 {pat} 模式相关联的自动命令。
  - `:au[tocmd]! [group] {event}`: 删除所有和 {event} 事件相关联的自动命令。
  - `:au[tocmd]! [group]`: 删除所有的自动命令。没有给出组时，不要轻易用本命令，会对插件、语法高亮等产生破坏。

- 列出自动命令

  - `:au[tocmd] [group] {event} {pat}`: 显示所有和 {event} 事件和 {pat} 模式相关联的自动命令。
  - `:au[tocmd] [group] * {pat}`: 列出所有和 {pat} 模式相关联的自动命令。
  - `:au[tocmd] [group] {event}`: 列出所有和 {event} 事件相关联的自动命令。
  - `:au[tocmd] [group]`: 列出所有的自动命令。

### 26.2. vim 事件 (events)

[使用脚本编写 Vim 编辑器，第 5 部分 事件驱动的脚本编写和自动化](http://www.ibm.com/developerworks/cn/linux/l-vim-script-5/index.html?ca=drs-)

Vim 提供了 78 种编辑事件的通知，分为八大类：
- 对话开始和清理事件
- 文件读取事件
- 文件编辑事件
- 缓冲区修改事件
- 选项设置事件
- 窗口相关事件
- 用户交互事件
- 异步通知事件

在 `autocmd` 的 {event} 参数中，可以指定要注册自动命令的 vim 事件，完整的 vim 事件列表可以通过 `:h autocommand-events` 查看。

NOTE
- 多个事件用逗号分隔（不能有空格）。
- 事件名不区分大小写，可以使用 "BUFread" 或者 "bufread" 来取代 "BufRead"

一个简单 Vim 编辑对话中的事件序列：
```
> vim
    BufWinEnter     (create a default window)
    BufEnter        (create a default buffer)
    VimEnter        (start the Vim session):edit example.txt
    BufNew          (create a new buffer to contain demo.txt)
    BufAdd          (add that new buffer to the session’s buffer list)
    BufLeave        (exit the default buffer)
    BufWinLeave     (exit the default window)
    BufUnload       (remove the default buffer from the buffer list)
    BufDelete       (deallocate the default buffer)
    BufReadCmd      (read the contexts of demo.txt into the new buffer)
    BufEnter        (activate the new buffer)
    BufWinEnter     (activate the new buffer's window)i
    InsertEnter     (swap into Insert mode)
Hello
    CursorMovedI    (insert a character)
    CursorMovedI    (insert a character)<ESC>
    InsertLeave     (swap back to Normal mode):wq
    BufWriteCmd     (save the buffer contents back to disk)
    BufWinLeave     (exit the buffer's window)
    BufUnload       (remove the buffer from the buffer list)
    VimLeavePre     (get ready to quit Vim)
    VimLeave        (quit Vim)
```

### 26.3. pattern

{pat} 参数可以是逗号分隔的列表，相当于对每个模式分别给出该命令。

eg:
```
:autocmd BufRead *.txt,*.info set et
```
等价于：
```
:autocmd BufRead *.txt set et
:autocmd BufRead *.info set et
```

文件模式 {pat} 用以下两种方式之一匹配文件名：
- 如果模式里没有 `/`，只匹配文件名的尾部（不包括它之前的目录路径）。
- 如果模式里有 `/`，既匹配短本件名（你输入的），也匹配完整文件名 （扩展为完整路径并进行完符号链接的解析以后)。

## 27. 自定义命令操作

通过 `:command` 命令可以在 vim 中自定义命令：

- `:command <your_command> <exec_code>` 自定义命令。命令名称必须以大写字母开头。
- `:command! <your_command> <exec_code>` 强制自定义命令，若该命令已有定义则直接覆盖。
- `:command -xargs=n <your_command> <exec_code>` 自定义命令，同时设置该命令有 n 个参数，n 的取值如下：
  - `-nargs=0`: 不接受任何参数（默认）
  - `-nagrs=1`: 只接受一个参数
  - `-nargs=*`: 可接收任意个数参数
  - `-nargs=?`: 可接受 1 个或者 0 个参数
  - `-nargs=+`: 至少提供一个参数
- `:command -complete=custom,<fucn_name> <your_command> <exec_code>` 自定义命令，同时设置该命令的自动补全函数。

eg:
1. 定义 `:command TT pyx print("123")`
1. 执行 `:TT`，显示 `123`


## 28. 自定义函数操作

通过 `:function` 命令可以在 vim 中自定义函数：
- `:fucntion func_name`:

## 29. Quickfix 操作

TODO: https://coolshell.cn/articles/11312.html Quickfix 部分

## 30. Refer Links

[Vim 快速入门](https://www.shiyanlou.com/courses/2/labs/16/document)

[A vim Tutorial and Primer](https://danielmiessler.com/study/vim/)

[简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)

[vimtutor](http://www2.geog.ucl.ac.uk/~plewis/teaching/unix/vimtutor)

[无所不能的 vim-vim 到底能做什么](http://www.vimer.cn/archives/2026.html)

[10 Time-Saving Tips for UNIX Vim Beginners](https://code.tutsplus.com/tutorials/10-time-saving-tips-for-unix-vim-beginners--cms-22386)

[VIM 解决中文编码问题](http://www.vimer.cn/archives/87.html)

[无插件 VIM 编程技巧](https://coolshell.cn/articles/11312.html)

[VIM 的分屏功能](https://coolshell.cn/articles/1679.html)

[vim 屏幕滚动学习](https://blog.csdn.net/wsxtag/article/details/6171884)

[在 Vim 中优雅地查找和替换](https://harttle.land/2016/08/08/vim-search-in-file.html)

[在 Vim 中进行文件目录操作](https://harttle.land/2016/10/14/vim-file-and-directory.html)

[Vim 是从何而来：智慧积累的伟大力量](https://mp.weixin.qq.com/s?__biz=MjM5NzU0MzU0Nw==&mid=2651379681&idx=1&sn=0c7d146e329c0950dd1d6ac448e8f376&chksm=bd241cf58a5395e3f5c784d6af2cc47246c473f29e2fc87cdb3a46f918ecfe452e38c5c4bcba&mpshare=1&scene=1&srcid=0825kbOSWUzxWZcNOoQ7tB8M#rd)

[vim 常用命令](http://blog.jayself.com/2014/07/28/vim_command/)

[Vim 快捷键](http://blog.jex.tw/blog/2013/05/15/vim/)

[IMOOC: 优雅玩转 Vim](https://www.imooc.com/learn/1049)

TODO:

[所需即所获：像 IDE 一样使用 vim](https://github.com/yangyangwithgnu/use_vim_as_ide)

[VIM 下的跳转练习](http://www.cnblogs.com/moiyer/archive/2010/04/01/1952681.html)
