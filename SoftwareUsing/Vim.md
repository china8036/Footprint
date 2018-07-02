- [Vim](#vim)
  - [1. 基本概念](#1-基本概念)
  - [2. 模式](#2-模式)
    - [2.1. 基本模式](#21-基本模式)
      - [2.1.1. 普通模式 (Normal mode)](#211-普通模式-normal-mode)
      - [2.1.2. 插入模式 (Insert mode)](#212-插入模式-insert-mode)
      - [2.1.3. 命令行模式 (Command line mode)](#213-命令行模式-command-line-mode)
      - [2.1.4. 可视模式 (Visual mode)](#214-可视模式-visual-mode)
      - [2.1.5. 选择模式 (Select mode)](#215-选择模式-select-mode)
      - [2.1.6. Ex 模式 (Ex mode)](#216-ex-模式-ex-mode)
    - [2.2. 派生模式](#22-派生模式)
  - [3. 常用操作](#3-常用操作)
    - [3.1. 浏览目录](#31-浏览目录)
    - [3.2. 打开文件](#32-打开文件)
    - [3.3. 游标移动](#33-游标移动)
    - [3.4. 插入文本](#34-插入文本)
    - [3.5. 保存文档](#35-保存文档)
    - [3.6. 退出 vim](#36-退出-vim)
      - [3.6.1. 命令行模式下退出](#361-命令行模式下退出)
      - [3.6.2. 普通模式下退出](#362-普通模式下退出)
    - [3.7. 删除文本](#37-删除文本)
    - [3.8. 撤销操作](#38-撤销操作)
    - [3.9. 复制粘贴](#39-复制粘贴)
    - [3.11. 搜索文本](#311-搜索文本)
    - [3.12. 替换文本](#312-替换文本)
      - [3.12.1. 移动后替换](#3121-移动后替换)
      - [3.12.2. 搜索后替换](#3122-搜索后替换)
    - [3.13. 字符操作](#313-字符操作)
    - [3.14. 执行 shell](#314-执行-shell)
    - [3.15. 多窗口切换](#315-多窗口切换)
    - [3.16. 使用 HELP](#316-使用-help)
  - [4. 配置 vim](#4-配置-vim)
    - [4.1. 临时配置](#41-临时配置)
    - [4.2. vimrc 配置](#42-vimrc-配置)
  - [5. 宏录制](#5-宏录制)
  - [6. 分屏操作](#6-分屏操作)
  - [7. Tab 页操作](#7-tab-页操作)
  - [8. 保存会话](#8-保存会话)
  - [9. 缓冲区操作](#9-缓冲区操作)
  - [10. Quickfix 操作](#10-quickfix-操作)
  - [11. Refer Links](#11-refer-links)

# Vim

## 1. 基本概念

[kana the wizard](http://whileimautomaton.net/2008/11/vimm3/operator) says there are five (5) levels to vim mastery:
- Level 0: Not knowing about vim
- Level 1: knows vim basics
- Level 2: knows visual mode
- Level 3: knows various motions
- Level 4: not needing visual mode

## 2. 模式

Vim 具有 6 种基本模式和 5 种派生模式，我们这里只简单介绍下 6 种基本模式，其中常用到的是普通模式、插入模式和命令行模式。

### 2.1. 基本模式

#### 2.1.1. 普通模式 (Normal mode)

vim 启动后默认进入普通模式，处于插入模式或命令行模式时只需要按 Esc 或者 Ctrl+[（这在 vim 课程环境中不管用) 即可进入普通模式。

在普通模式中，用的编辑器命令，比如移动光标，删除文本等等。这也是 Vim 启动后的默认模式。这正好和许多新用户期待的操作方式相反（大多数编辑器默认模式为插入模式）。

Vim 强大的编辑能来自于其普通模式命令。普通模式命令往往需要一个操作符结尾。例如普通模式命令 dd 删除当前行，但是第一个"d"的后面可以跟另外的移动命令来代替第二个 d，比如用移动到下一行的"j"键就可以删除当前行和下一行。另外还可以指定命令重复次数，2dd（重复 dd 两次），和 dj 的效果是一样的。用户学习了各种各样的文本间移动／跳转的命令和其他的普通模式的编辑命令，并且能够灵活组合使用的话，能够比那些没有模式的编辑器更加高效地进行文本编辑。

#### 2.1.2. 插入模式 (Insert mode)

在普通模式下使用下面的键将进入插入模式，并可以从相应的位置开始输入
- i	在当前光标处进行编辑
- I	在行首插入
- A	在行末插入
- a	在光标后插入编辑
- o	在当前行后插入一个新行
- O	在当前行前插入一个新行
- cw	替换从光标所在位置后到一个单词结尾的字符

在这个模式中，大多数按键都会向文本缓冲中插入文本。

在插入模式中，可以按 ESC 键回到普通模式。

在插入模式中，输入一个词的开头后，可以使用 `ctrl` + `p` 或 `ctrl` + `n` 自动补齐。 

#### 2.1.3. 命令行模式 (Command line mode)

普通模式中按 `:` 即可进入命令行模式。

在命令行模式中可以输入会被解释成并执行的文本。例如执行命令（: 键），搜索（/ 和？键）或者过滤命令（! 键）。在命令执行之后，Vim 返回到命令行模式之前的模式，通常是普通模式。

在命令行模式下，有以下常用的快捷技巧：
- 使用 `tab` 键可自动补全命令
- 使用 `ctrl` + `d` 可显示所有可补全的命令
- `.` （小数点) 可以重复上一次的命令
- `N<command>` 重复某个命令 N 次
- `<start position><command><end position>` 将游标的移动和命令操作结合，如：
  - `0y$` 先到行头，然后拷贝到本行最后一个字符，相当于 `yy`
  - `ye` 从当前位置拷贝到本单词的最后一个字符
  - `y2/foo` 拷贝 2 个 “foo” 之间的字符串

#### 2.1.4. 可视模式 (Visual mode)

普通模式下按 `v` 即可进入可视模式。

这个模式与普通模式比较相似，但是移动游标可批量选择文本，并且被选择的文本区域会呈现高亮。高亮区域可以是字符、行或者是一块文本。当执行一个非移动命令时，命令会被执行到这块高亮的区域上。Vim 的"文本对象"也能和移动命令一样用在这个模式中。

在当前模式下，除了移动光标逐个字符进行选择外，还可以使用区域选择：`v<object>`
- object 可能是： w 一个单词， W 一个以空格为分隔的单词， s 一个句字， p 一个段落。也可以是一个特别的字符："、 '、 )、 }、 ]。

选了某一区域后，可进行如下操作：
- J 把所有的行连接起来（变成一行）
- < 或 > 左右缩进
- = 自动给缩进
- 列操作
  - `ctrl` + `v` 进入列选择模式
  - `A` 进入插入模式
  - 输入字符串
  - `ESC`

#### 2.1.5. 选择模式 (Select mode)

这个模式和无模式编辑器的行为比较相似（Windows 标准文本控件的方式）。这个模式中，可以用鼠标或者光标键高亮选择文本，不过输入任何字符的话，Vim 会用这个字符替换选择的高亮文本块，并且自动进入插入模式。

#### 2.1.6. Ex 模式 (Ex mode)

这和命令行模式比较相似，在使用 `:visual` 命令离开 Ex 模式前，可以一次执行多条命令。

### 2.2. 派生模式

## 3. 常用操作

### 3.1. 浏览目录

进入命令行模式后输入 `:Explore 目录路径` 即可打开目录（默认为当前路径），在目录浏览状态下可进行以下操作：
- 选择文件可直接打开
- `–` 到上级目录
- `D` 删除文件（大写）
- `R` 改文件名（大写）
- `s` 对文件排序（小写）
- `x` 执行文件

如果你要改变当前浏览的目录，或是查看当前浏览的目录，你可以使用和 shell 一样的命令：
- :cd <dir> 改变当前目录
- :pwd 查看当前目录

### 3.2. 打开文件

使用 vim 命令打开指定文件：`vim practice_1.txt`。

进入命令行模式后输入 `:e 文件路径` 也可以打开相应文件。

### 3.3. 游标移动

在普通模式下使用方向键或者 `h,j,k,l` 键可以移动游标。
- h	左
- l	右（小写 L）
- j	下
- k	上
- w	移动到下一个单词开头
- e 移动到下一个单词末尾
- b	移动到上一个单词
- 0/home 移动到行首
- $/end 移动到行尾
- ^ 移动到本行的第一个非 blank 字符
- g_ 移动到本行最后一个非 blank 字符
- Ctrl g 显示当前行号和进度
- gg 跳转到文档头部
- G 跳转到文档末尾
- 行号 + G 跳转到指定行
- g; 跳转到最后一个发生修改的行
- t<char> 移动到指定字符的前一个字符

P.S.
- 在命令前输入一个数字 n 可重复执行命令 n 次。
- 为什么 vim 会使用这么奇葩的方向键 h,j,k,l 及那么别扭反人类的 Esc 作为退出插入模式的键，可以了解 [ADM-3A](https://en.wikipedia.org/wiki/ADM-3A)。

### 3.4. 插入文本

- 在插入模式下，直接输入字符即可插入文本
- 在命令模式下，使用 `r 文件名`(read) 可将指定文件内容插入到当前游标处
- 在命令模式下，使用 `r !command`(read) 可将指定 command 在 shell 中执行的输出结果文本插入到当前游标处，如 `r !pwd`

### 3.5. 保存文档

从普通模式输入：进入命令行模式，输入 w 回车，保存文档。

输入 `:w 文件名` 可以将文档另存为其他文件名或存到其它路径下。

在 VISUAL 模式下，选择指定文本区块后输入 `:w 文件名` 可将被选择的文本区块另存为新的文档。

### 3.6. 退出 vim

#### 3.6.1. 命令行模式下退出

在命令行模式中，有以下退出方式：
- :q!	强制退出，不保存
- :q	退出
- :wq!	强制保存并退出
- :w 文件路径	另存为
- :saveas 文件路径	另存为
- :x	保存并退出
- :wq	保存并退出

#### 3.6.2. 普通模式下退出

普通模式下输入 Shift+zz 即可保存退出 vim。

### 3.7. 删除文本

在普通模式下：
- x	删除游标所在的字符
- X	删除游标所在前一个字符
- Delete	同 x
- dd	删除整行
- dw	删除一个单词（不适用中文）
- d2w 删除 2 个单词（不适用中文）
- dt<word> 删除从当前游标到指定字符前的所有内容
- d$ 或 D	删除至行尾
- d^	删除至行首
- dG	删除到文档结尾处
- d1G	删至文档首部
- ce 删除一个单词并进入 Insert mode
- 在普通模式下，使用 `v` 进行 VISUAL 模式，批量选择文本后使用`x`或`d`可删除被选择的文本

除此之外，你还可以在命令之前加上数字，表示一次删除多行，如：`2dd` 表示一次删除 2 行。

### 3.8. 撤销操作

在普通模式下：
- 使用 `u` 撤销最后一个操作
- 使用 `U` 撤销一整行的操作
- 使用 `Ctrl` + `r` 撤销上一个撤销操作

### 3.9. 复制粘贴

Type	`y`(yank/copy)  to copy the text.

Type	`yw`(yank/copy)  to copy one word.

Type	`yy`(yank/copy)  to copy one line.

Type	`p`(put/paste)  to put the last deletion after the cursor.

事实上，Vim 中有 12 个粘贴板，使用 `:reg` 可以看到所有粘贴板内的内容：

![image](http://otaivnlxc.bkt.clouddn.com/jpg/2018/7/2/0e40c91b4939caff03430c67320c0049.jpg)

其中：
- `"` 粘贴板是默认粘贴板，直接按 y 就复制到这个粘贴板中，直接按 p 就粘贴这个粘贴板中的内容。
- `+` 粘贴板是系统粘贴板，使用 `"+y` 将内容复制到该粘贴板后可以使用 `Ctrl＋V` 将其粘贴到其他文档（如 firefox、gedit）中，同理，要把在其他地方用 Ctrl＋C 或右键复制的内容复制到 vim 中，需要在正常模式下按 `"+p`。
- 要将 vim 的内容复制到某个粘贴板，需要退出编辑模式，进入正常模式后，选择要复制的内容，然后按 `"Ny`（注意带引号）完成复制，其中 N 为粘贴板号。
- 要将 vim 某个粘贴板里的内容粘贴进来，需要退出编辑模式，在正常模式按 `"Np`，其中 N 为粘贴板号。

### 3.11. 搜索文本

在普通模式下：
- 使用 `/` 进入向下搜索模式，键入关键词后回车即可
- 使用 `?` 进入向上搜索模式，键入关键词后回车即可
- 使用 `n` 找到下一个匹配
- 使用 `N` 找到上一个匹配
- 使用 `ctrl` + `o` 返回上一个位置
- 使用 `ctrl` + `i` 回到下一个位置
- 将游标移动到大 / 中 / 小括号处，使用 `%` 找到与之配对的另一个括号
- 使用 `*` 或 `#` 快速搜索当前游标所在的单词

### 3.12. 替换文本

#### 3.12.1. 移动后替换

- 替换单个字符
  
  在普通模式下，将光标移动至要替换的字符处，使用 `r` 进入替换状态，再按下目标字符即可完成替换，完成替换后自动结束替换状态。

- 替换多个字符

  在普通模式下，将光标移动至要替换的字符处，使用 `R` 进入 REPLACE 模式，键入目标字符可替换原字符，使用 `backspace` 还原替换，使用 `esc` 退出替换模式

#### 3.12.2. 搜索后替换

输入 `:` 进行命令模式：
- 使用 `s/old/new` 替换当前游标所在行的 old 为 new，只替换第一个匹配的 old
- 使用 `s/old/new/g` 替换当前游标所在行的 old 为 new，替换当前行所有匹配的 old
- 使用 `a,bs/old/new/g` 替换从行 a 到行 b 的 old 为 new，替换区间内所有匹配的 old
- 使用 `%s/old/new/g` 替换整个文件中的 old 为 new
- 使用 `%s/old/new/gc` 逐个替换整个文件中的 old 为 new，每次替换都使用交互模式进行确认

### 3.13. 字符操作

- U 将选中字符变大写
- u 将选中字符变小写
- gU 将游标所在的字符变大写
- gU 将游标所在的字符变小写
- gUU 将一整行变大写
- guu 将一整行变小写
- ga 查看游标所在的字符的 ASCII 码
- g8 查看游标所在的字符的 UTF-8 编码
- gf 打开游标处所指路径的文件
- 

### 3.14. 执行 shell

Type  `:!`	followed by an external command to execute that command. All  `:`  commands must be finished by hitting <ENTER>.

eg:
```
:!pwd
```

### 3.15. 多窗口切换

- 使用 `ctrl` + `w` 在多个窗口中进行切换
- 使用 `:q` 关闭当前窗口

### 3.16. 使用 HELP

> Vim has a comprehensive on-line help system.

使用以下方式获取 HELP：
- F1 打开 help
- `:help` 打开 help，`:help subject` 打开指定主题的 help，如 `:help w` / `:help insert-index`

## 4. 配置 vim

### 4.1. 临时配置

在命令模式下，使用 `set xx` 设置 vim 属性：
- `set ic`(ignore case) 和 `set noic`(no ignore case) 设置搜索时是否忽略大小写
- `set hls`(hlsearch) 设置搜索时匹配的文本高亮，使用 `set nohls` 取消高亮
- `set is`(incsearch) 设置搜索时 partial matches，使用 `set nois` 取消 partial matches

### 4.2. vimrc 配置

> Vim has many more features than Vi, but most of them are disabled by default. To start using more features you have to create a "vimrc" file.
```
1. Start editing the "vimrc" file, this depends on your system:
:edit ~/.vimrc			for Unix
:edit $VIM/_vimrc		for MS-Windows

2. Now read the example "vimrc" file text:

:read $VIMRUNTIME/vimrc_example.vim

3. Write the file with:

:write

The next time you start Vim it will use syntax highlighting.
You can add all your preferred settings to this "vimrc" file.
```

常见配置说明：
```
" 不要使用 vi 的键盘模式，而是使用 vim 的键盘模式  
set nocompatible  
  
" 语法高亮  
set syntax=on  
  
" 去掉输入错误的提示声音  
set noeb  
  
" 在处理未保存或只读文件的时候，弹出确认  
set confirm  
  
" 自动缩进  
set autoindent  
set cindent  
  
" Tab 键的宽度  
set tabstop=4  
  
" 统一缩进为 4  
set softtabstop=4  
set shiftwidth=4  
  
" 不要用空格代替制表符  
set noexpandtab  
  
" 在行和段开始处使用制表符  
set smarttab  
  
" 显示行号  
set number  
  
" 历史记录数  
set history=1000  
  
"禁止生成临时文件  
set nobackup  
set noswapfile  
  
"搜索忽略大小写  
set ignorecase  
  
"搜索逐字符高亮  
set hlsearch  
set incsearch  
  
"行内替换  
set gdefault  
  
"编码设置  
set enc=utf-8  "encoding vim 内部编码
"set fenc=utf-8  "file encoding vim 解析文档使用的编码
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936  "file encoding vim 解析文档使用的编码的顺序列表

"防止菜单乱码
if(g:iswindows==1)
    source $VIMRUNTIME/delmenu.vim
    source $VIMRUNTIME/menu.vim
    language messages zh_CN.utf-8
endif

"让 vim 默认以双字节处理特殊字符
if v:lang =~? '^\(zh\)\|\(ja\)\|\(ko\)'
    set ambiwidth=double
endif

"让 vim 不要自动设置字节序标记，因为并不是所有编辑器都可以识别字节序标记的
set nobomb

"语言设置  
set langmenu=zh_CN.UTF-8  
set helplang=cn  
  
" 我的状态行显示的内容（包括文件类型和解码）  
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v](%p%%)\ %{strftime(\"%d/%m/%y\ -\ %H:%M\")}  
"set statusline=[%F]%y%r%m%*%=[Line:%l/%L,Column:%c](%p%%)  
  
" 总是显示状态行  
set laststatus=2  
  
" 在编辑过程中，在右下角显示光标位置的状态行  
set ruler             
  
" 命令行（在状态行下）的高度，默认为 1，这里是 2  
set cmdheight=2  
  
" 侦测文件类型  
filetype on  
  
" 载入文件类型插件  
filetype plugin on  
  
" 为特定文件类型载入相关缩进文件  
filetype indent on  
  
" 保存全局变量  
set viminfo+=!  
  
" 带有如下符号的单词不要被换行分割  
set iskeyword+=_,$,@,%,#,-  
  
" 字符间插入的像素行数目  
set linespace=0  
  
" 增强模式中的命令行自动完成操作  
set wildmenu  
  
" 使回格键（backspace）正常处理 indent, eol, start 等  
set backspace=2  
  
" 允许 backspace 和光标键跨越行边界  
set whichwrap+=<,>,h,l  
  
" 可以在 buffer 的任何地方使用鼠标（类似 office 中在工作区双击鼠标定位）  
set mouse=a  
set selection=exclusive  
set selectmode=mouse,key  
  
" 通过使用：commands 命令，告诉我们文件的哪一行被改变过  
set report=0  
  
" 启动的时候不显示那个援助索马里儿童的提示  
set shortmess=atI  
  
" 在被分割的窗口间显示空白，便于阅读  
set fillchars=vert:\ ,stl:\ ,stlnc:\  
  
" 高亮显示匹配的括号  
set showmatch  
  
" 匹配括号高亮的时间（单位是十分之一秒）  
set matchtime=5  
  
" 光标移动到 buffer 的顶部和底部时保持 3 行距离  
set scrolloff=3  

"文件自动检测外部更改
set autoread

"用浅色高亮当前行
autocmd InsertEnter * se cul

" 为 C 程序提供自动缩进  
set smartindent  

"背景色
set background=dark

" 只在下列文件类型被侦测到的时候显示行号，普通文本文件不显示  
if has("autocmd")  
   autocmd FileType xml,html,c,cs,java,perl,shell,bash,cpp,python,vim,php,ruby set number  
   autocmd FileType xml,html vmap <C-o> <ESC>'<i<!--<ESC>o<ESC>'>o-->  
   autocmd FileType java,c,cpp,cs vmap <C-o> <ESC>'<o/*<ESC>'>o*/  
   autocmd FileType html,text,php,vim,c,java,xml,bash,shell,perl,python setlocal textwidth=100  
   autocmd Filetype html,xml,xsl source $VIMRUNTIME/plugin/closetag.vim  
   autocmd BufReadPost *  
      \ if line("'\"") > 0 && line("'\"") <= line("$") |  
      \   exe "normal g`\"" |  
      \ endif  
endif " has("autocmd")  
  
" F5 编译和运行 C 程序，F6 编译和运行 C++ 程序  
" 请注意，下述代码在 windows 下使用会报错  
" 需要去掉。/ 这两个字符  
  
" C 的编译和运行  
map <F5> :call CompileRunGcc()<CR>  
func! CompileRunGcc()  
exec "w"  
exec "!gcc % -o %<"  
exec "! ./%<"  
endfunc  
  
" C++ 的编译和运行  
map <F6> :call CompileRunGpp()<CR>  
func! CompileRunGpp()  
exec "w"  
exec "!g++ % -o %<"  
exec "! ./%<"  
endfunc  
  
" 能够漂亮地显示。NFO 文件  
set encoding=utf-8  
function! SetFileEncodings(encodings)  
    let b:myfileencodingsbak=&fileencodings  
    let &fileencodings=a:encodings  
endfunction  
function! RestoreFileEncodings()  
    let &fileencodings=b:myfileencodingsbak  
    unlet b:myfileencodingsbak  
endfunction  
  
au BufReadPre *.nfo call SetFileEncodings('cp437')|set ambiwidth=single  
au BufReadPost *.nfo call RestoreFileEncodings()  
  
" 高亮显示普通 txt 文件（需要 txt.vim 脚本）  
au BufRead,BufNewFile *  setfiletype txt  
  
" 用空格键来开关折叠  
set foldenable  
set foldmethod=manual  
nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR>  
  
" minibufexpl 插件的一般设置  
let g:miniBufExplMapWindowNavVim = 1  
let g:miniBufExplMapWindowNavArrows = 1  
let g:miniBufExplMapCTabSwitchBufs = 1  
let g:miniBufExplModSelTarget = 1    
```

## 5. 宏录制

qa 操作序列 q, @a, @@
- qa 把你的操作记录在寄存器 a。
- 于是 @a 会 replay 被录制的宏。
- @@ 是一个快捷键用来 replay 最新录制的宏。

例：
在一个只有一行且这一行只有“1”的文本中，键入如下命令：
1. qa 开始录制
1. Yp 复制行。
1. <C-a> 增加 1.
1. q 停止录制。
1. @a → 在 1 下面写下 2
1. @@ → 在 2 正面写下 3
1. 现在做 100@@ 会创建新的 100 行，并把数据增加到 103.

## 6. 分屏操作

- shell 中使用 `vim -On a.cpp b.cpp c.cpp` 使用垂直分屏打开多个文件进行浏览，n 表示分成 n 个屏
- shell 中使用 `vim -on a.cpp b.cpp c.cpp` 使用水平分屏打开多个文件进行浏览，n 表示分成 n 个屏
- `:split` 创建水平分屏 ( `:vsplit` 创建垂直分屏 )
- `<Ctrl-w><direction>` direction 可以是 hjkl 或是 ←↓↑→ 中的一个，其用来切换分屏
- `<Ctrl-w>_` ( 或 <Ctrl-w>| ) : 最大化尺寸 (<C-w>| 垂直分屏)
- `<Ctrl-w>+` ( 或 <Ctrl-w>- ) : 增加尺寸
- `:He`(Hexplore) 把当前窗口上下分屏，并在下面进行目录浏览
- `:He!` 把当前窗口上下分屏，并在上面进行目录浏览
- `:Ve`(Vexplore) 把当前窗口左右分屏，并在左边进行目录浏览
- `:Ve!` 把当前窗口左右分屏，并在右边进行目录浏览
- `:set scb`(scrollbind) 到需要同步移动的两个屏中都输入改名了，从而让两个分屏中的文件同步移动

## 7. Tab 页操作

- shell 中使用 `vim -p a.cpp b.cpp c.cpp` 使用多个 Tab 打开多个文件进行浏览
- `:bufdo tab split` 把 buffer 中的文件全转成 Tab
- `:Te`(TabExplorer) 创建新的 Tab 页，并进行目录浏览
- `gt` 到下一个 Tab
- `gT` 到前一个 Tab
- `{i}gt` 到指定 Tab
- `:tabs` 查看所有打开的 Tab 以及 Tab 内的分屏情况
- `:tabclose{i}` 关闭指定 Tab
- `:qa` 关闭所有 Tab 并退出
- `:wqa` 保存所有 Tab 后关闭并退出

## 8. 保存会话

- 如果你用 Tab 或 Window 打开了好些文件的文件，还设置了各种滚屏同步，或是行号……，那么，你可以用下面的命令来保存会话：
  ```bash
  :mksession mysession.vim
  ```
  如果文件重复，vim 默认会报错，如果你想强行写入的话，你可以在 mksession 后加 `!`。

- 使用以下命令打开会话
  ```bash
  vim -S mysession.vim
  ```

## 9. 缓冲区操作

当在 vim 中打开其他文件时，当前文件会被“关闭”，而实际上并没有被关闭，这些文件都放在缓冲区中，针对缓冲区文件，可进行以下操作：
- 使用 `:ls` 查看缓冲区。查看缓冲区文件时，文件相关标记的含义如下：
  - `–` （非活动的缓冲区）
  - `a` （当前被激活缓冲区）
  - `h` （隐藏的缓冲区）
  - `%` （当前的缓冲区）
  - `#` （交换缓冲区）
  - `=` （只读缓冲区）
  - `+` （已经更改的缓冲区）
- 使用 `:buffer n` 打开序号为 n 的缓冲区文件。
- 使用以下命令快速切换缓冲区文件：
  - `:bnext` 缩写 :bn
  - `:bprevious` 缩写 :bp
  - `:blast` 缩写 :bl
  - `:bfirst` 缩写 :bf

## 10. Quickfix 操作

TODO: https://coolshell.cn/articles/11312.html Quickfix 部分

## 11. Refer Links

[Vim 快速入门](https://www.shiyanlou.com/courses/2/labs/16/document)

[A vim Tutorial and Primer](https://danielmiessler.com/study/vim/)

[简明 VIM 练级攻略](https://coolshell.cn/articles/5426.html)

[vimtutor](http://www2.geog.ucl.ac.uk/~plewis/teaching/unix/vimtutor)

[无所不能的 vim-vim 到底能做什么](http://www.vimer.cn/archives/2026.html)

[10 Time-Saving Tips for UNIX Vim Beginners](https://code.tutsplus.com/tutorials/10-time-saving-tips-for-unix-vim-beginners--cms-22386)

[VIM 解决中文编码问题](http://www.vimer.cn/archives/87.html)

[无插件 VIM 编程技巧](https://coolshell.cn/articles/11312.html)

[VIM 的分屏功能](https://coolshell.cn/articles/1679.html)