- [Vim 插件：常用插件](#vim-插件常用插件)
  - [1. 插件管理器 Vundle](#1-插件管理器-vundle)
  - [2. 自动补全 YouCompleteMe](#2-自动补全-youcompleteme)
  - [3. Nerdtree](#3-nerdtree)
  - [4. ctrlp](#4-ctrlp)
  - [5. Refer Links](#5-refer-links)

# Vim 插件：常用插件

## 1. 插件管理器 Vundle

[Vundle](https://github.com/VundleVim/Vundle.vim)

1. 下载 Vundle `git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`

1. 配置 Vundle
    ```
    set nocompatible              " be iMproved, required
    filetype off                  " required

    " set the runtime path to include Vundle and initialize
    set rtp+=~/.vim/bundle/Vundle.vim
    call vundle#begin()
    " alternatively, pass a path where Vundle should install plugins
    "call vundle#begin('~/some/path/here')

    " let Vundle manage Vundle, required
    Plugin 'VundleVim/Vundle.vim'

    " The following are examples of different formats supported.
    " Keep Plugin commands between vundle#begin/end.
    " plugin on GitHub repo
    Plugin 'tpope/vim-fugitive'
    " plugin from http://vim-scripts.org/vim/scripts.html
    " Plugin 'L9'
    " Git plugin not hosted on GitHub
    Plugin 'git://git.wincent.com/command-t.git'
    " git repos on your local machine (e.g. when working on your own plugin)
    Plugin 'file:///home/gmarik/path/to/plugin'
    " The sparkup vim script is in a subdirectory of this repo called vim.
    " Pass the path to set the runtimepath properly.
    Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
    " Install L9 and avoid a Naming conflict if you've already installed a
    " different version somewhere else.
    " Plugin 'ascenator/L9', {'name': 'newL9'}

    " All of your Plugins must be added before the following line
    call vundle#end()            " required
    filetype plugin indent on    " required
    " To ignore plugin indent changes, instead use:
    "filetype plugin on
    "
    " Brief help
    " :PluginList       - lists configured plugins
    " :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
    " :PluginSearch foo - searches for foo; append `!` to refresh local cache
    " :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
    "
    " see :h vundle for more details or wiki for FAQ
    " Put your non-Plugin stuff after this line
    ```
1. 执行安装 `vim +PluginInstall +qall`

常用命令：
- `:PluginInstall` 安装插件
- `:PluginUpdate` 更新插件
- `:Pluginclean` 清除不用的插件

## 2. 自动补全 YouCompleteMe

[YouCompleteMe](https://github.com/Valloric/YouCompleteMe) 是一款使用 python 开发的 Vim 插件。

[YouCompleteMe 中容易忽略的配置](https://zhuanlan.zhihu.com/p/33046090)

1. 通过 Vundle 下载安装
    1. 在 `.vimrc` 中添加：`Plugin 'Valloric/YouCompleteMe'`.
    1. 安装依赖：`sudo apt install build-essential cmake python3-dev`
    1. 编译安装 YCM：`cd ~/.vim/bundle/YouCompleteMe && sudo python3 install.py --clang-completer`

    The following additional language support options are available:
    - C# support: install Mono and add --cs-completer when calling install.py.
    - Go support: install Go and add --go-completer when calling install.py.
    - JavaScript and TypeScript support: install Node.js and npm then install the TypeScript SDK with npm install -g typescript.
    - Rust support: install Rust and add --rust-completer when calling install.py.
    - Java support: install JDK8 (version 8 required) and add --java-completer when calling install.py.
    To simply compile with everything enabled, there's a --all flag. So, to install with all language features, ensure xbuild, go, tsserver, node, npm, rustc, and cargo tools are installed and in your PATH.

1. 配置 YCM

    - [C-family Semantic Completion](https://github.com/Valloric/YouCompleteMe/#c-family-semantic-completion)

      配置 `.ycm_extra_conf.py` 文件：
      1. 复制 [ycmd's own .ycm_extra_conf.py](https://raw.githubusercontent.com/Valloric/ycmd/66030cd94299114ae316796f3cad181cac8a007c/.ycm_extra_conf.py) 到 `.vim/bundle/` 目录下。根据系统 C++ 头文件索引的位置修改其中的 `flags`，如：添加 `'-isystem', '/usr/include/c++/4.8.2'`.
      1. 在 vimrc 中配置 YCM
          ```
          let g:ycm_global_ycm_extra_conf = '~/.vim/bundle/.ycm_extra_conf.py'
          let g:ycm_confirm_extra_conf = 0
          ```
NOTE
- You can use `Ctrl+Space` to trigger the completion suggestions anywhere, even without a string prefix. This is useful to see which top-level functions are available for use.

## 3. Nerdtree

https://github.com/scrooloose/nerdtree

https://github.com/scrooloose/nerdtree/blob/master/doc/NERDTree.txt

https://segmentfault.com/a/1190000015143474

```
?: 快速帮助文档
o: 打开一个目录或者打开文件，创建的是 buffer，也可以用来打开书签
go: 打开一个文件，但是光标仍然留在 NERDTree，创建的是 buffer
t: 打开一个文件，创建的是 Tab，对书签同样生效
T: 打开一个文件，但是光标仍然留在 NERDTree，创建的是 Tab，对书签同样生效
i: 水平分割创建文件的窗口，创建的是 buffer
gi: 水平分割创建文件的窗口，但是光标仍然留在 NERDTree
s: 垂直分割创建文件的窗口，创建的是 buffer
gs: 和 gi，go 类似
x: 收起当前打开的目录
X: 收起所有打开的目录
e: 以文件管理的方式打开选中的目录
D: 删除书签
P: 大写，跳转到当前根路径
p: 小写，跳转到光标所在的上一级路径
K: 跳转到第一个子路径
J: 跳转到最后一个子路径
<C-j>和<C-k>: 在同级目录和文件间移动，忽略子目录和子文件
C: 将根路径设置为光标所在的目录
u: 设置上级目录为根路径
U: 设置上级目录为跟路径，但是维持原来目录打开的状态
r: 刷新光标所在的目录
R: 刷新当前根路径
I: 显示或者不显示隐藏文件
f: 打开和关闭文件过滤器
q: 关闭 NERDTree
A: 全屏显示 NERDTree，或者关闭全屏
```

## 4. ctrlp

https://github.com/kien/ctrlp.vim

## 5. Refer Links
