- [Vim 插件：python 插件开发](#vim-插件python-插件开发)
  - [1. 使 vim 支持 python](#1-使-vim-支持-python)
  - [2. vim module](#2-vim-module)
    - [2.1. vim 对象](#21-vim-对象)
    - [2.2. vim 函数](#22-vim-函数)
  - [3. 插件开发方式](#3-插件开发方式)
    - [3.1. 内嵌式](#31-内嵌式)
    - [3.2. 独立式](#32-独立式)
  - [4. Refer Links](#4-refer-links)

# Vim 插件：python 插件开发

Vim 为增强扩展能力，提供了很多外部语言接口（如 Python，ruby，lua，Perl 等），因此可以很方便的使用其它脚本语言来编写 vim 插件。

## 1. 使 vim 支持 python

1. 编译 vim，使 vim 支持 Python
  
    在编译之前，configure 的时候加上 `--enable-pythoninterp` 和 `--enable-python3interp` 选项，使之分别支持 Python2 和 Python3。编译好之后，可以通过 `vim --version | grep +python` 来查看是否已经支持 Python，结果中应该包含 `+python` 和 `+python3`，当然也可以编译成只支持 Python2 或 Python3。

1. 检查 vim 是否已支持 python
    - `:echo has("python")` 或 `:echo has("python3")`
    - `:pyx print("hello world")`

## 2. vim module

怎么用 Python 来访问 vim 的信息以及操作 vim 呢？vim 的 Python 接口提供了一个 vim 模块。vim 模块是 Python 和 vim 沟通的桥梁，通过它，Python 可以访问 vim 的一切信息以及操作 vim，就像使用 vimL 一样。

通过 `:h python-vim` 可查看 vim 模块的官方文档。

通过在 python 中 `import vim` 来使用 vim module。

### 2.1. vim 对象

vim 模块提供了一些有用的对象：

- Tabpage 对象 (`:h python-tabpage`): 一个 Tabpage 对象对应 vim 的一个 Tabpage。

- Window 对象 (`:h python-window`): 一个 Window 对象对应 vim 的一个 Window。

- Buffer 对象 (`:h python-buffer`): 一个 Buffer 对象对应 vim 的一个 buffer，Buffer 对象提供了一些属性和方法，可以很方便操作 buffer。 
  
  eg: 假定 b 是当前的 buffer:
  ```vim
  :py print b.name            # write the buffer file name
  :py b[0] = "hello!!!"       # replace the top line
  :py b[:] = None             # delete the whole buffer
  :py del b[:]                # delete the whole buffer
  :py b[0:0] = [ "a line" ]   # add a line at the top
  :py del b[2]                # delete a line (the third)
  :py b.append("bottom")      # add a line at the bottom
  :py n = len(b)              # number of lines
  :py (row,col) = b.mark('a') # named mark
  :py r = b.range(1,5)        # a sub-range of the buffer
  :py b.vars["foo"] = "bar"   # assign b:foo variable
  :py b.options["ff"] = "dos" # set fileformat
  :py del b.options["ar"]     # same as :set autoread<
  ```
- vim.current 对象 (`:h python-current`): vim.current 对象提供了一些属性，可以方便的访问“当前”的 vim 对象。
  - `vim.current.line`: String 类型，The current line (RW).	
  - `vim.current.buffer`: Buffer 类型，The current buffer (RW).	
  - `vim.current.window`: Window 类型，The current window (RW).	
  - `vim.current.tabpage`: TabPage 类型，The current tab page (RW).	
  - `vim.current.range`: Range 类型，The current line range (RO).	

### 2.2. vim 函数

vim 模块提供了两个非常有用的函数接口：

- `vim.eval(str)`: 求 vim 表达式 str 的值，返回结果类型为：
  - `string`: 如果 vim 表达式的值的类型是 string 或 number。
  - `list`: 如果 vim 表达式的值的类型是一个 vim list (`:h list`)。
  - `dictionary`: 如果 vim 表达式的值的类型是一个 vim dictionary (`:h dict`)。

  eg:
  ```
  :py sw = vim.eval("&shiftwidth")
  :py print vim.eval("expand('%:p')")
  :py print vim.eval("@a")
  ```

- `vim.command(str)`: 执行 vim 中的命令 str(ex-mode)，返回值为 None。

  eg:
  ```
  :py vim.command("%s/\s\+$//g")
  :py vim.command("set shiftwidth=4")
  :py vim.command("normal! dd")
  ```

## 3. 插件开发方式

### 3.1. 内嵌式

```vim
py[thon] << {endmarker}
{script}
{endmarker}
1
2
3
```

`{script}`中的内容为 Python 代码，`{endmarker}`是一个标记符号，可以是任何字符串，不过{endmarker}前面不能有任何的空白字符，也就是要顶格写。 

例如，写一个函数，打印出当前 buffer 所有的行 (Demo.vim):
```
function! Demo()
py << EOF
import vim
for line in vim.current.buffer:
    print line
EOF
endfunction
call Demo()
1
2
3
4
5
6
7
8
```
运行：`source %` 查看结果。

### 3.2. 独立式
 
把 Python 代码写到`*.py`中，vimL 只用来定义全局变量、map、command 等，用 `pyf[ile] {file}` 来运行 python 文件中的程序。

插件 LeaderF 就是采用这种方式。个人更喜欢这种方式，可以把全部精力集中在写 Python 代码上。

## 4. Refer Links

[如何用 vim 的插件开发？有什么实际中的技巧？](https://www.zhihu.com/question/25819155)

[如何使用 Python 编写 vim 插件](https://blog.csdn.net/archofortune/article/details/78653853)

[使用 python 写 vim 插件](https://selfboot.cn/2014/11/03/vim_plugin_with_python/)

[为 Vim 编辑器开发定制插件](https://www.ibm.com/developerworks/cn/aix/library/au-vimplugin/index.html)