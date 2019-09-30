- [Vim 配置](#vim-配置)
  - [1. 临时配置](#1-临时配置)
  - [2. vimrc 配置](#2-vimrc-配置)
  - [3. 常见配置](#3-常见配置)
  - [4. 其它配置](#4-其它配置)
    - [4.1. 粘贴时格式混乱](#41-粘贴时格式混乱)
    - [4.2. 键位更改](#42-键位更改)
  - [5. Refer Links](#5-refer-links)

# Vim 配置

## 1. 临时配置

在命令模式下，可直接通过命令来临时设置 vim，重启 vim 后失效。

`set [no]field[=value]`: 属性设置，set 可以同时设置多个选项，多个选项会用空格分隔。
- `set ic`(ignore case) 和 `set noic`(no ignore case) 设置搜索时是否忽略大小写
- `set hls`(hlsearch) 设置搜索时匹配的文本高亮，使用 `set nohls` 取消高亮
- `set is`(incsearch) 设置搜索时 partial matches，使用 `set nois` 取消 partial matches

- 可使用 `:set field?` 来判断属性 filed 是否已被设置，如 `:set number?` 返回 `number` 或 `nonumber`。

## 2. vimrc 配置

> Vim has many more features than Vi, but most of them are disabled by default. To start using more features you have to create a "vimrc" file.

vimrc 即 vim run command，因此 vim 在加载 vimrc 文件时每一行都作为一个命令来执行。

vimrc 文件分为系统级别和用户级别，分别对整个系统和个别用户生效。可通过 `:version` 来查看配置文件位置，eg:
```
   system vimrc file: "$VIM/vimrc"
     user vimrc file: "$HOME/.vimrc"
 2nd user vimrc file: "~/.vim/vimrc"
```

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

## 3. 常见配置

- Tab 设置

  在 vim 中通常有以下 4 种设置 tab 的方式：
  - Always keep 'tabstop' at 8, set 'softtabstop' and 'shiftwidth' to 4 (or 3 or whatever you prefer) and use 'noexpandtab'.  Then Vim will use a mix of tabs and spaces, but typing <Tab> and <BS> will behave like a tab appears every 4 (or 3) characters.

  - Set 'tabstop' and 'shiftwidth' to whatever you prefer and use 'expandtab'.  This way you will always insert spaces.  The formatting will never be messed up when 'tabstop' is changed.

  - Set 'tabstop' and 'shiftwidth' to whatever you prefer and use a |modeline| to set these values when editing the file again.  Only works when using Vim to edit the file.
  - Always set 'tabstop' and 'shiftwidth' to the same value, and 'noexpandtab'.  This should then work (for initial indents only) for any tabstop setting that people use.  It might be nice to have tabs after the first non-blank inserted as spaces if you do this though.  Otherwise aligned comments will be wrong when 'tabstop' is changed.

常用配置列表：
```
" 不要使用 vi 的键盘模式，而是使用 vim 的键盘模式
set nocompatible

" 语法高亮
set syntax=on

" 去掉输入错误的提示声音，相当于 noeb
set noerrorbells

" 在右下角显示按键命令
set showcmd

" 在处理未保存或只读文件的时候，弹出确认
set confirm

" 显示行号，相当于 set nu
set number

" 历史记录数，默认值为 500
set history=1000

" 禁止生成临时文件
set nobackup
set noswapfile

" 自动缩进，复制当前行的缩进方式到下一行
set autoindent
" 使用 C 样式的缩进
" set cindent
" 智能自动缩进，可识别编程语言中的花括号并在花括号中自动缩进
set smartindent

" 使用空格代替 tab
set expandtab
" 在行和段开始处使用制表符，相当于 sta
set smarttab
" Number of spaces that a <Tab> in the file counts for，default 8，相当于 ts
set tabstop=4
" Number of spaces that a <Tab> counts，defalut 0，相当于 sts
set softtabstop=4
set shiftwidth=4

" 在前端开发中使 1 个 tab 为 2 个 spaces
autocmd BufNewFile,BufRead *.html,*.htm,*.css,*.js set expandtab tabstop=2 shiftwidth=2

" 搜索时忽略大小写，相当于 ic
set ignorecase
" 搜索时匹配的文本高亮，相当于 hls
set hlsearch
" 搜索时边输入边匹配（否则需要回车后才匹配），相当于 is
set incsearch

" 行内替换
set gdefault

" 编码设置
set encoding=utf-8  "encoding vim 内部编码
"set fileencoding=utf-8  "file encoding vim 解析文档使用的编码
set fileencodings=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936  "file encoding vim 解析文档使用的编码的顺序列表
" 防止菜单乱码
if(g:iswindows==1)
    source $VIMRUNTIME/delmenu.vim
    source $VIMRUNTIME/menu.vim
    language messages zh_CN.utf-8
endif
" 让 vim 默认以双字节处理特殊字符
if v:lang =~? '^\(zh\)\|\(ja\)\|\(ko\)'
    set ambiwidth=double
endif

" 让 vim 不要自动设置字节序标记，因为并不是所有编辑器都可以识别字节序标记的
set nobomb

" 语言设置
set langmenu=zh_CN.UTF-8
set helplang=cn

" 设置状态行显示，相当于 ls
" 0: never 1: only if there are at least two windows 2: always
set laststatus=2                                    " 总是显示状态栏
" 状态行显示的内容，相当于 stl
set statusline=[%F%m%r%h%w]                         " 显示文件路径
set statusline+=\                                   " 空格分隔
set statusline+=[FORMAT=%{&ff}]                     " 显示文件格式
set statusline+=\                                   " 空格分隔
set statusline+=[TYPE=%Y]                           " 显示文件类型
set statusline+=\                                   " 空格分隔
set statusline+=[POS=%l,%v](%p%%)                   " 显示当前位置
set statusline+=\                                   " 空格分隔
set statusline+=%{strftime(\"%y/%m/%d\ %H:%M\")}    " 显示当前时间
" 命令行（在状态行下）的高度，默认为 1，这里是 2
" set cmdheight=2

" 在编辑过程中，在右下角显示光标位置的状态行
set ruler

" Influences the working of <BS>, <Del>, CTRL-W and CTRL-U in Insert mode
"   `indent`  allow backspacing over autoindent
"   `eol`     allow backspacing over line breaks (join lines)
"   `start`   allow backspacing over the start of insert; CTRL-W and CTRL-U stop once at the start of insert.
set backspace=indent,eol,start

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
" 'mouse' 选项的字符决定 Vim 在什么场合下会使用鼠标：
" n 普通模式；v 可视模式；i 插入模式；c 命令行模式；h 在帮助文件里，以上所有的模式；a 以上所有的模式；r 跳过 |hit-enter| 提示；A 在可视模式下自动选择；
set mouse=a
set selection=exclusive
set selectmode=mouse,key

" 通过使用 :commands 命令，告诉我们文件的哪一行被改变过
set report=0

" 启动的时候不显示援助索马里儿童的提示
set shortmess=atI

" 在被分割的窗口间显示空白，便于阅读
set fillchars=vert:\ ,stl:\ ,stlnc:\

" 高亮显示匹配的括号
set showmatch
" 匹配括号高亮的时间（单位是十分之一秒）
set matchtime=5
" 高亮显示当前列，相当于 cuc
set cursorcolumn
" 高亮显示当前行，相当于 cul
set cursorline

" 光标移动到 buffer 的顶部和底部时保持 10 行距离
set scrolloff=10

"文件自动检测外部更改
set autoread

" Tell vim what background you are using [dark|light]
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

" C 的编译和运行
func! CompileRunGcc()
exec "w"
exec "!gcc % -o %<"
exec "! ./%<"
endfunc
map <C-F5> :call CompileRunGcc()<CR>

" C++ 的编译和运行
func! CompileRunGpp()
exec "w"
exec "!g++ % -o %<"
exec "! ./%<"
endfunc
map <C-F6> :call CompileRunGpp()<CR>

" 能够漂亮地显示 .NFO 文件
set encoding=utf-8
function! SetFileEncodings(encodings)
    let b:myfileencodingsbak=&fileencodings
    let &fileencodings=a:encodings
endfunction
function! RestoreFileEncodings()
    let &fileencodings=b:myfileencodingsbak
    unlet b:myfileencodingsbak
endfunction
autocmd BufReadPre *.nfo call SetFileEncodings('cp437')|set ambiwidth=single
autocmd BufReadPost *.nfo call RestoreFileEncodings()

" 高亮显示普通 txt 文件（需要 txt.vim 脚本）
autocmd BufRead,BufNewFile *  setfiletype txt

" 用空格键来开关折叠
set foldenable
set foldmethod=manual
nnoremap <space> @=((foldclosed(line('.')) < 0) ? 'zc' : 'zo')<CR>

" 触发 paste 模式
set pastetoggle=<F12>
```

## 4. 其它配置

### 4.1. 粘贴时格式混乱

[Turning off auto indent when pasting text into vim](https://stackoverflow.com/questions/2514445/turning-off-auto-indent-when-pasting-text-into-vim)

- 解决方案 1: use `:r! cat` and then paste ( `shift + insert` ) the content, and `CTRL+D`.
- 解决方案 2: use `:set paste` / `:set nopaste` / `set pastetoggle=<F3>`

### 4.2. 键位更改

将 CapsLock 替换为 Esc
- Windows
  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
  "Scancode Map"=hex:00,00,00,00,00,00,00,00,02,00,00,00,01,00,3a,00,00,00,00,00
  ```
  保存以上脚本为 reg 文件，直接运行后重启即可。

- Linux

- Mac

  通过 Karabiner-Elements 进行键位映射修改

## 5. Refer Links

[Vimscript 状态条](https://www.ctolib.com/docs-learn-vim-c-n2smlozt.html)

[可能是 Windows 下最漂亮的 Gvim 配置了](https://keelii.com/2016/06/13/awsome-window-vimrc/)
