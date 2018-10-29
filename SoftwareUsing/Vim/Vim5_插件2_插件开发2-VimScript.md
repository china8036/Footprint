- [Vim 插件：VimScript 插件开发](#vim-插件vimscript-插件开发)
  - [1. 运行 viml](#1-运行-viml)
  - [2. 语句](#2-语句)
  - [3. 注释](#3-注释)
  - [4. 变量](#4-变量)
    - [4.1. 定义](#41-定义)
    - [4.2. 类型](#42-类型)
      - [4.2.1. List](#421-list)
      - [4.2.2. Dictionary](#422-dictionary)
    - [4.3. 作用域](#43-作用域)
    - [4.4. 伪变量](#44-伪变量)
  - [5. 表达式](#5-表达式)
  - [6. 流程控制](#6-流程控制)
    - [6.1. 条件控制](#61-条件控制)
    - [6.2. 循环控制](#62-循环控制)
      - [6.2.1. for 循环](#621-for-循环)
      - [6.2.2. while 循环](#622-while-循环)
  - [7. 函数](#7-函数)
    - [7.1. 函数调用](#71-函数调用)
    - [7.2. 自定义函数](#72-自定义函数)
    - [7.3. 内置函数](#73-内置函数)
      - [7.3.1. 字符串操作](#731-字符串操作)
      - [7.3.2. List 操作](#732-list-操作)
      - [7.3.3. Dictionary 操作](#733-dictionary-操作)
      - [7.3.4. 缓冲区操作](#734-缓冲区操作)
      - [7.3.5. 光标操作](#735-光标操作)
      - [7.3.6. 变量操作](#736-变量操作)
  - [8. Refer Links](#8-refer-links)

# Vim 插件：VimScript 插件开发

Vim 的脚本语言被称为 Vim Script Language (VimL)，是典型的动态命令式语言，提供了大多数常见的语言特性：变量、表达式、控制结构、内置函数、用户定义函数、一级字符串、高级数据结构（列表和字典）、终端和文件 I/O、正则表达式模式匹配、异常和集成调试器。

可以通过 `:help vim-script-intro` 查看 Vim 自带的 Vimscript 文档。

## 1. 运行 viml

- 直接在命令模式下在冒号后面输入脚本命令。eg: `:call MyBackupFunc(expand('%'), { 'all':1, 'save':'recent'})`.

- 创建键盘映射来执行脚本命令。eg: `:nmap \b :call MyBackupFunc(expand('%'), { 'all': 1 })<CR>`.

- 将 viml 写入外部文件中（通常使用 `.vim` 作为扩展名），然后在 vim 中调用执行该文件。eg: `:source /full/path/to/the/scriptfile.vim`.

## 2. 语句

Vimscript 中的所有语句都以换行符结尾（和 shell 脚本或 Python 一样）。如果需要跨多个行运行一条语句，只需要使用反斜杠作为续行符。在表示续行时，反斜杠很少被放在行的末尾，而是放在延续行的开始处：
```
call SetName(
\             first_name,
\             middle_initial,
\             family_name
\           )
```

通过使用竖线做分隔符，可以将两个或更多个语句放在单个行中，也就是说，Vimscript 中的竖线相当于其他大多数编程语言中的分号：
```
echo "Starting..." | call Phase(1) | call Phase(2) | echo "Done"
```

## 3. 注释

Vimscript 注释以一个双引号开始并持续到行末。

NOTE：Vimscript 字符串也可以以双引号开头并**且始终优先于注释**。这意味着**不能把注释放到可能需要使用字符串的位置，因为注释将始终被解释为字符串**。

eg:
```
echo "> " "Print generic prompt
```
echo 命令要求一个或多个字符串，因此此行代码将生成一个错误，指出第二个字符串缺少一个右引号（Vim 是这样认为的）。然而，可以始终将注释放在语句的最前头，这样，通过在开始注释之前使用竖线显式地开始一条新语句，就可以解决上面的问题，如下所示：
```
echo "> " |"Print generic prompt
```

## 4. 变量

### 4.1. 定义

在 Vim 脚本里，可以使用关键字 let 来申明变量，最基本的方式为：
```
let name      = "Damian"
let height    = 165
let interests = [ 'Cinema', 'Literature', 'World Domination', 101 ]
let phone     = { 'cell':5551017346, 'home':5558038728, 'work':'?' }
```

### 4.2. 类型

Vim 脚本中支持以下几种数据类型：
- `Number`: 整数。
- `Float`: 浮点数。
- `String`: 字符串。字符串可以使用双引号或单引号作为分隔符进行指定，双引号字符串可用于特殊的 “转义序列”，而单引号会将包括在其中的所有内容视为文字字符。
- `List`: 列表。使用方括号分隔的有序值序列，并带有以 0 开头的隐式整数索引。例：`['Cinema', 'Literature', 'World Domination', 101]`。
- `Dictionary`: 字典。使用花括号分隔的一组无序值集合，带有显式的的字符串键。例如：`{'cell':5551017346, 'home':5558038728, 'work':'?'}`。
- `Funcref`: 

NOTE: 
- 列表或字典中的值不一定必须是相同的类型；可以混合使用字符串、数字，甚至是嵌套的列表或字典。
- 变量类型一旦分配后，就会保持不变并在运行时严格遵守：
  ```
  let interests = 'unknown' " Error: variable type mismatch
  ```

#### 4.2.1. List

在 Vimscript 中，列表是标量值的序列：字符串、数字、引用或它们之间的混合。

- 创建列表：
  ```
  let data = [1,2,3,4,5,6,"seven"]
  echo data[0]                 |" echoes: 1
  let data[1] = 42             |" [1,42,3,4,5,6,"seven"]
  let data[2] += 99            |" [1,42,102,4,5,6,"seven"]
  let data[6] .= ' samurai'    |" [1,42,102,4,5,6,"seven samurai"]
  let data[-1] .=  ' samurai'
  ```

- 像在许多其他动态语言中一样，Vimscript 列表不需要显式的内存管理：它们自动伸缩以适应需要储存的元素，并且在应用程序不需要这些元素时自动进行垃圾回收。

- 除了储存字符串或数字之外，列表还能储存其他列表。像在 C、C++ 或 Perl 中一样，如果一个列表包含其他列表，它就类似于多维数组：
  ```
  let pow = [
  \   [ 1, 0, 0, 0  ],
  \   [ 1, 1, 1, 1  ],
  \   [ 1, 2, 4, 8  ],
  \   [ 1, 3, 9, 27 ],
  \]
  " and later...
  echo pow[x](y)
  ```

- 当将一个变量赋值给任何列表时，您就为该列表分配了一个指针或引用。因此，从一个列表变量分配到另一个列表变量将导致它们同时指向或引用相同的底层列表：
  ```
  let old_suffixes = ['.c', '.h', '.py']
  let new_suffixes = old_suffixes
  let new_suffixes[2] = '.js'
  echo old_suffixes      |" echoes: ['.c', '.h', '.js']
  echo new_suffixes      |" echoes: ['.c', '.h', '.js']
  ```
  为了这种情况，可以调用内置的 `copy()` 浅复制函数（或 `deepcopy()` 深复制函数）来创建列表的副本：
  ```
  let old_suffixes = ['.c', '.h', '.py']
  let new_suffixes = copy(old_suffixes)
  let new_suffixes[2] = '.js'
  echo old_suffixes      |" echoes: ['.c', '.h', '.py']
  echo new_suffixes      |" echoes: ['.c', '.h', '.js']
  ```

#### 4.2.2. Dictionary

Vimscript 中的字典 在本质上和 AWK 关联数组、Perl 哈希表，或者 Python 字典都是一样。也就是说，这是一个无序容器，按字符串而不是整数来进行索引。

- 创建和访问字典
  ```
  let seen = {}   " Haven't seen anything yet
  
  let daytonum = { 'Sun':0, 'Mon':1, 'Tue':2, 'Wed':3, 'Thu':4, 'Fri':5, 'Sat':6 }
  let diagnosis = {
      \   'Perl'   : 'Tourettes',
      \   'Python' : 'OCD',
      \   'Lisp'   : 'Megalomania',
      \   'PHP'    : 'Idiot-Savant',
      \   'C++'    : 'Savant-Idiot',
      \   'C#'     : 'Sociopathy',
      \   'Java'   : 'Delusional',
      \}

  let lang = input("Patient's name? ")
  
  let Dx = diagnosis[lang]

  let Dx = diagnosis['Ruby']                          " E716: Key not present in Dictionary: Ruby
  let Dx = get(diagnosis, 'Ruby')                     " Returns: 0
  let Dx = get(diagnosis, 'Ruby', 'Schizophrenia')    " Returns: 'Schizophrenia'
  let Dx = diagnosis.Lisp                    " Same as: diagnosis['Lisp']
  diagnosis.Perl = 'Multiple Personality'    " Same as: diagnosis['Perl']
  ```

- 字典由引用（即指针）来表示，所以将字典赋给另一个变量就将两个变量设为相同的底层数据结构，前者相当于后者的别名。您可以首先通过复制或者深复制（deep-coping）原始内容来解决这个问题：
  ```
  let dict2 = dict1             " dict2 just another name for dict1
                                    
  let dict3 = copy(dict1)       " dict3 has a copy of dict1's top-level elements
  
  let dict4 = deepcopy(dict1)   " dict4 has a copy of dict1 (all the way down)
  ```

- 和列表一样，您可以用 `is` 操作符来比较身份，用 `==` 操作符来比较值：
  ```
  if dictA is dictB
      " They alias the same container, so must have the same keys and values
  elseif dictA == dictB
      " Same keys and values, but maybe in different containers
  else
      " Different keys and/or values, so must be different containers
  endif
  ```

- 将一个独立条目从字典中删除有两种方法：使用内置的 remove() 函数，或者使用 unlet 命令：
  ```
  let removed_value = remove(dict, "key")
  unlet dict["key"]
  ```
  从一个字典删除多个条目时，使用 filter() 会更简洁，更有效：
  ```
  " Remove any entry whose key starts with C...
  call filter(diagnosis, 'v:key[0] != "C"')
  
  " Remove any entry whose value doesn't contain 'Savant'...
  call filter(diagnosis, 'v:val =~ "Savant"')
  
  " Remove any entry whose value is the same as its key...
  call filter(diagnosis, 'v:key != v:val')
  ```

### 4.3. 作用域

通常 Vim 变量存在三种作用域：全局变量、局部变量、和脚本变量。一般情况下，Vim 会为该变量选择默认的作用域，若在函数内部，默认作用域是局部变量；而若在函数外部，默认作用域是全局变量。

除了默认方式，也可以通过使用变量前缀来显式指定变量的作用域：
- `g:varname`: 全局变量。
- `l:varname`: 局部变量，只可在函数内部使用。
- `s:varname`: 脚本变量，只可以在当前脚本函数内使用。
- `v:varname`: Vim 特殊变量。
- `b:varname`: 作用域限定在某一个缓冲区内。
- `w:varname`: 作用域限定在窗口内部。
- `t:varname`: 作用域限定在标签内部。

eg:
```
let g:helloworld = 1  " 这是一个全局变量， g: 前缀未省略
let helloworld = 1    " 这也是一个全局变量，在函数外部，默认的作用域是全局的

function! HelloWorld()
  let g:helloworld = 1    " 这是函数内部全局变量
  let helloworld = 1      " 这是一个函数内部的局部变量，在函数内部，默认的作用域为局部变量
endfunction
```

### 4.4. 伪变量

脚本可以使用伪变量 (pseudovariables) 访问 Vim 提供的其他类型的值容器：
- `&varname`:	访问一个 Vim 选项（如果指定的话，则为本地选项，否则为全局选项）。
  - `&l:varname`:	访问一个本地 Vim 选项。
  - `&g:varname`:	访问一个全局 Vim 选项。
- `@varname`:	访问一个 Vim Register。
- `$varname`:	访问一个环境变量。

eg: 例如，可以设置两个键映射（key-map）来增加或减小当前的表空间：
```
nmap <silent> ]] :let &tabstop += 1<CR>
nmap <silent> [[ :let &tabstop -= &tabstop > 1 ? 1 : 0<CR>
```

## 5. 表达式

Vimscript 中的表达式由大多数其他现代脚本语言中使用的相同的基本运算符组成，并且使用基本相同的语法。

```
赋值                     let var=expr
数值相加并赋值            let var+=expr
数值相减并赋值            let var-=expr
字符串连接并赋值          let var.=expr

三元运算符	              bool?expr-if-true:expr-if-false

逻辑                     OR	    bool||bool
逻辑                     AND	bool&&bool

数值或字符串相等          expr!=expr
数值或字符串不相等        expr>expr
数值或字符串大于          expr>=expr
数值或字符串大于等于      expr<expr
数值或字符串小于          expr<=expr
数值或字符串小于等于    expr==expr
字符串匹配      	    expr=~expr

数值相加                 num+num
数值相减                 num-num
字符串连接	             str.str

数值相乘                    num*num
数值相除                    num/num
数值系数	                num%num

转换为数值                NOT	+num
求负数                    -num
逻辑                      !bool

括号优先	              (expr)
```

NOTE
- VimScript 不支持一元运算符 `++` 和 `--`。
- 在 Vimscript 中，**比较函数始终执行数字比较，除非两个运算对象都是字符串**。特别是，如果一个运算对象是字符串，另一个是数字，那么字符串将被转换为数字，然后再对两个数字进行数值比较。

  eg:
  ```
  let ident = 'Vim'

  if ident == 0     "Always true (string 'Vim' converted to number 0)
  if ident == '0'   "Uses string equality if ident contains string, but numeric equality if ident contains number
  ```

- 字符串比较通常使用 Vim 的 ignorecase 选项的本地设置，但是任何字符串比较函数都可以被显式地标记为大小写敏感（通过附加一个 #）或大小写不敏感（通过附加一个 ?）。**强烈推荐对所有字符串比较使用 “显式设置大小写的” 运算对象**，因为这样可以确保无论用户选项设置如何变化，脚本都可以可靠地执行。

  eg:
  ```
  if name ==? 'Batman'         |"Equality always case insensitive
    echo "I'm Batman"
  elseif name <# 'ee cummings' |"Less-than always case sensitive
    echo "the sky was can dy lu minous"
  endif
  ```

- 在版本 7.2 之前，Vim 只支持整数运算。即使对于版本 7.2，如果其中一个运算对象被明确声明为浮点类型，那么 Vim 只支持浮点算术：
  ```
  let filecount = 234
  
  echo filecount/100   |" echoes 2
  echo filecount/100.0 |" echoes 2.34
  ```

## 6. 流程控制

NOTE: VimScript 目前不支持 switch case 语法。

### 6.1. 条件控制

if 语句是基本的条件控制语句，使用方法与 C 大致相同。

eg:
```
" 条件语句
" if else endif
let a = 2
let b = 1
if(a > b)
    echo "a 大于 b"
else
    echo "a 不大于 b"
endif
 
" if endif
let score=87.2
if(score >= 60)
    echo "及格"
endif
" if elseif elseif ... else endif
if(score > 90)
    echo "优秀"
elseif(score > 80)
    echo "良好"
elseif(score >= 60)
    echo "及格"
else
    echo "不及格"
endif
 
" 条件表达式最后结果必为 Number 类型
if("ture")  " 字符串自动转换为 Number, 此处转换后是 0
    echo "true"
else
    echo "false"
endif
```

NOTE
- C 中只能使用整数做条件，而 VimScript 中除了 Number 还可以使用 String 类型，因为 String 可以自动转换为 Number。
- 条件表达式不需要使用（）包裹，当然加上也没有错误，如 `if (3>2)` 与 `if 3>2` 是相同的。

### 6.2. 循环控制

无论是在 while 还是 for in 语句中，都可以使用 continue 和 break 来改变循环，这与 C 完全一样。

#### 6.2.1. for 循环

eg:
```
let sum = 0
for i in range(1,100)
    let sum += i
endfor
echo sum
```

eg: 
- 遍历一维列表
  ```
  for name in list
      echo name
  endfor
  ```

- 遍历二维列表
  ```
  for [name, rank, serial] in list_of_lists
      echo rank . ' ' . name . '(' . serial . ')'
  endfor
  ```
  相当于
  ```
  for nested_list in list_of_lists
      let name   = nested_list[0] 
      let rank   = nested_list[1] 
      let serial = nested_list[2] 
      
      echo rank . ' ' . name . '(' . serial . ')'
  endfor
  ```

#### 6.2.2. while 循环

eg:
```
let i=100
let sum = 0
while i>0
    let sum +=  i
    let i -= 1
endwhile
echo sum
```

## 7. 函数

### 7.1. 函数调用

- 使用关键字 `call` 调用函数，无法获取返回值。eg: `call search("Date: ", "W")`.

- 直接调用函数，可获取返回值。eg: 
  ```
  let line = getline(".")
  let repl = substitute(line, '\a', "*", "g")
  call setline(".", repl)
  ```

NOTE: 与 C 或 Perl 不同，Vimscript 并不允许在未使用返回值的情况下抛出函数的返回值。因此，如果打算使用函数作为过程或子例程并忽略它的返回值，那么必须使用 `call` 命令为调用添加前缀。

### 7.2. 自定义函数

Vimscript 中的函数使用 `function` 关键字定义，后跟函数名，然后是参数列表（这是强制的，即使该函数没有参数），**函数的参数在函数体内使用的时候前面需要加上 `a:`，这表示是一个函数参数**，否则运行时会报错，并提示 num1 是没有定义的变量。

函数体然后从下一行开始，一直连续下去，直到遇到一个匹配的 `endfunction` 关键字。

函数返回值使用 return 语句指定。可以根据需要指定任意数量的单独 return 语句。如果函数被用作一个过程，并且没有任何有用的返回值，那么可以不包含 return 语句。然而，Vimscript 函数始终 返回一个值，因此**如果没有指定任何 return，那么函数将自动返回 0**。

**函数名属于全局命名空间，在所有脚本中可用**。

**函数名的首字母必须大写**。Vim 为了避免用户自定义函数与内置函数命名冲突，强制要求用户自定义函数名的首字母必须是大写字母。

eg1:
```
function Min(num1, num2)
  if a:num1 < a:num2
    let smaller = a:num1
  else
    let smaller = a:num2
  endif
  return smaller
endfunction
 
echo Min(23,24)
```

eg2: 设置 `CTRL-B` 以调用函数来创建对当前文件的有限备份
```
function SaveBackup ()
    let b:backup_count = exists('b:backup_count') ? b:backup_count+1 : 1
    return writefile(getline(1,'$'), bufname('%') . '_' . b:backup_count)
endfunction
 
nmap <silent>  <C-B>  :call SaveBackup()<CR>
```

Vimscript 中的函数声明为运行时语句，因此如果一个脚本被加载两次，那么该脚本中的任何函数声明都将被执行两次，因此将重新创建相应的函数。因此 **Vimscript 提供了一个关键字修饰符 `function!`，允许在需要时指出某个函数声明可以被安全地重载**。

### 7.3. 内置函数

vim 7.3 版本提供了近 300 个内建函数供用户使用，要想轻松的操控 vim，必须熟练掌握常用的内置函数。由于内置函数是 C 编写的编译好的二进制代码，执行速度远远高于 VimScript 编写的函数，因此一般都建议不要重复实现与内置函数相同功能的函数。

可以通过 `:help Builtin Functions` 或 `:help function-list` 查看 Vim 内置函数的官方文档。

#### 7.3.1. 字符串操作

- `printf( {fmt}, {expr1} ...)`: 格式化，返回格式化后的字符串，而不是打印出来。与 C 版本的 printf 类似，只是这个函数不打印，而是返回格式化后的字符串。
- `tr( {src}, {fromstr}, {tostr} )`: 对 src 中出现的 formstr 中的字符，按照位置对应关系替换为 tostr 中的字符。
- `tolower( {expr} )`: 把大写字母转换为小写字母。
- `toupper( {expr} )`: 把小写字母转换为大写字母。
- 正则表达式支持
  - `match( {expr}, {pat}[, {start} [, {count}]] }`: 找到匹配位置索引值，并返回这个值。{expr}可以是两种数据类型，一是 String，此时返回开始匹配模式 d 第一个字符的索引；而是 List 类型，每个元素都是 String，此时返回的是匹配模式的那个字符串元素在 List 中的索引。如果没有找到匹配项，则返回 -1。
  - `matchend( {expr}, {pat}[, {start} [, {count}]] }`: 与 match 相同，只是返回的不是匹配的开始索引，而是返回匹配后面的索引。
  - `matchstr( {expr}, {pat}[, {start} [, {count}]] }`: 与 match 相同，只是返回的是匹配的字符串。如果没有匹配则返回空字符串。
  - `matchlist( {expr}, {pat}[, {start} [, {count}]] }`: 与 matchstr 相同，只是返回的是一个 List，第一个元素是匹配的字符串，后面 d 元素是匹配的子串，如“\1”，“\2"。（正则表达式的反向引用）。
- `stridx( {haystack}, {needle} [, {start}] )`: 查找第一个匹配的子串，返回字串瘦子符索引。
- `strlen( {expr} )`: 返回给定字符串的长度，以字节为单位。
- `substitute( {expr}, {pat}, {sub}, {flags})`: 字符串替换，与命令：substitute 类似。
- `strpart( {src}, {start} [, {len}] )`: 返回字符串的子串。类似于其他语言中的 substr()。

#### 7.3.2. List 操作

- `get( {list}, {idx} [, {default}] )`: 返回 list 的第 idx 个元素。需要注意的是，即使索引值 idx 超出了有效范围，该函数仍然会返回一个值，这个值或者是 0，或者是给定的 default 参数。
- `len ( {expr} )`: 返回数组的长度。
- `empty( {expr} )`: 判断一个数组是否为空，等同于 `return len( {expr}) == 0`，但是效率比 `len()` 高。
- `insert( {list}, {item [, {idx} ])`: 在数组中插入一个元素，位于 idx 之前。如果 idx=0 或者不提供 idx 参数，就插入在开头。返回结果数组的引用。
- `add( {list}, {expr})`: 在数组末尾增加一个元素。返回结果数组的引用。
- `remove( {list}, {idx} [, {end] )`: 删除数组中的一个或多个元素。
- `copy( {expr} )`: 返回给定数组的浅拷贝。所有涉及引用类型的复制操作都会涉及到深浅拷贝的问题。
- `deepcopy( {expr} [, {noref} ] )`: 返回给定数组的深拷贝。如果有交叉循环引用，可能会导致深拷贝出错。尽量少使用深拷贝。
- `sort （{list} [, {func})`: 按照指定的规则对数组排序。
- `reverse ({list})`: 反序数组中的元素。
- `split（ {expr} [, {pattern} [, {keepempty}]] )`: 把一个字符串按照特定的边界标记进行分割，返回各个部分为元素的数组。
- `join （{list} [, {sep}])`: 把一个数组中的各个元素的字符串描述连接成一个大字符串返回。
- `range( {expr} [, {max} [, {stride}]])`: 返回一个整数数组。
- `max( {list} )`: 
- `min( {list} )`: 
- `index({list}, {expr} [, {start} [, {ic} ]] )`: 
- `count( )`: 

#### 7.3.3. Dictionary 操作

- `keys(dict)`: 
- `values(dict)`: 
- `items(dict)`:
- `empty(dict)`: 
- `len(dict)`: 
- `count(dict, str)`: 
- `max(dict)`: 
- `min(dict)`: 
- 

#### 7.3.4. 缓冲区操作

- `line({expr})`: 返回指定行的行数。{expr} 取值如下：
  - `.`: the cursor position
  - `$`: the last line in the current buffer
  - `'x`: position of mark x (if the mark is not set, 0 is returned)
  - `w0`: first line visible in current window (one if the display isn't updated, e.g. in silent Ex mode) 
  - `w$`: last line visible in current window (this is one less than "w0" if no lines are visible)
  - `v`: In Visual mode: the start of the Visual area (the cursor is the end).  When not in Visual mode returns the cursor position.  Differs from |'<| in that it's updated right away. 
- `col({expr})`: 返回指定位置的列号。

- `getline({lnum} [, {end}])`: 读取一行或者多行。
  - Without {end} the result is a String, which is line {lnum} from the current buffer.  Example: `getline(1)`.
    - When {lnum} is a String that doesn't start with a digit, line() is called to translate the String into a Number. To get the line under the cursor: `getline(".")`.
    - When {lnum} is smaller than 1 or bigger than the number of lines in the buffer, an empty string is returned.
                                              
  - When {end} is given the result is a |List| where each item is a line from the current buffer in the range {lnum} to {end}, including line {end}. {end} is used in the same way as {lnum}.  
    - Non-existing lines are silently omitted. 
    - When {end} is before {lnum} an empty |List| is returned.

- `setline({lnum}, {text})`: 设置一行或者多行的内容。
  - {lnum} is used like with |getline()|. When {lnum} is just below the last line the {text} will be added as a new line. Example: `call setline(5, strftime("%c"))`.
  - If this succeeds, 0 is returned.  If this fails (most likely because {lnum} is invalid) 1 is returned.
  - When {text} is a |List| then line {lnum} and following lines will be set to the items in the list.  Example: `:call setline(5, ['aaa', 'bbb', 'ccc'])`.

- `{range}a[ppend](!)`:
  - Insert several lines of text below the specified line. 
  - If the {range} is missing, the text will be inserted **after** the current line.      
  - Adding [!] toggles 'autoindent' for the time this command is executed.
                                                              
- `:{range}i[nsert](!)`:     
  - Insert several lines of text above the specified line. 
  - If the {range} is missing, the text will be inserted **before** the current line.     
  - Adding [!] toggles 'autoindent' for the time this command is executed.
                                                              
- `indent({lnum})`: 获取指定行的缩进量（开头空白）。
  - The result is a Number, which is indent of line {lnum} in the current buffer. The indent is counted in spaces, the value of 'tabstop' is relevant.  {lnum} is used just like in |getline()|.                             
  - When {lnum} is invalid -1 is returned. 

- `search({pattern} [, {flags} [, {stopline} [, {timeout}]]])`: 使用正则表达式查找指定行。

#### 7.3.5. 光标操作

- `cursor({lnum}, {col} [, {off}])`: 
- `setpos({expr}, {list})`: 
  ```viml
  let pos=[0,5,5,0]
  call setpos(".", pos)
  ```
- `getpos({expr})`: 

#### 7.3.6. 变量操作

- `type({expr})`: 返回给定变量名的数据类型。其中的参数可以是变量名，也可以是字面量常数值。返回值是一个 Number 类型的数字，含义如下：
  - `0`: Number
  - `1`: String
  - `2`: Funcref
  - `3`: List
  - `4`: Dictionary
  - `5`: Float
- `function({name})`: 返回一个指向名为 name 的函数的引用类型变量。函数的名字可以是内置函数，也可以是用户自定义函数。
- `getbufvar({expr}, {varname})`: 读取指定 buffer 内有效的指定变量的值。
- `setbufvar({expr}, {varname}, {val})`: 设置指定 buffer 内指定变量的值。
- `getwinvar({winnr}, {varname})`: 读取当前 tab 页中的指定窗口范围的变量。
- `setwinvar({winnr}, {varname}, {value})`: 设置当前 tab 页的指定窗口范围的变量值。
- `gettabvar({tabnr}, {varname}) 和 settabvar({tabnr}, {varname})`: 读取和设置 tab 页范围的变量。
- `gettabwinvar({tabnr},{winnr},{varname}) 和 settabwinvar({tabnr}, {winnr}, {varname}, {value})`: 读取和设置指定 tab 页中的指定 window 范围的变量。

## 8. Refer Links

[Stackoverflow: VimScript or VimL?](https://stackoverflow.com/questions/4398312/vimscript-or-viml)

[Vimscript 编程参考](https://www.ctolib.com/docs-learn-vim-c-ljkpbozt.html)

[VimScript 脚本语言学习 ------ 变量作用域、函数](https://blog.csdn.net/smstong/article/details/20775695?utm_source=blogxgwz4)

[VimScript 脚本语言学习 ------ 条件、循环](https://blog.csdn.net/smstong/article/details/20730587?utm_source=blogkpcl5)

[VimScript 脚本语言学习 ------ 常用的内置函数 ---（操纵 String）](https://blog.csdn.net/smstong/article/details/20791329)

[VimScript 脚本语言学习 ------ 常用的内置函数 ---（操纵 List）](https://blog.csdn.net/smstong/article/details/20831813)

[VimScript 脚本语言学习 ------ 常用的内置函数 ---（变量相关）](https://blog.csdn.net/smstong/article/details/20839991?utm_source=blogxgwz0)

[使用脚本编写 Vim 编辑器，第 1 部分 变量、值和表达式](https://www.ibm.com/developerworks/cn/linux/l-vim-script-1/index.html)

[使用脚本编写 Vim 编辑器，第 2 部分 用户定义函数](https://www.ibm.com/developerworks/cn/linux/l-vim-script-2/index.html?ca=drs-)

[使用脚本编写 Vim 编辑器，第 3 部分 内置列表](http://www.ibm.com/developerworks/cn/linux/l-vim-script-3/index.html?ca=drs-)

[使用脚本编写 Vim 编辑器，第 4 部分 字典](http://www.ibm.com/developerworks/cn/linux/l-vim-script-4/index.html?ca=drs-)

[使用脚本编写 Vim 编辑器，第 5 部分 事件驱动的脚本编写和自动化](http://www.ibm.com/developerworks/cn/linux/l-vim-script-5/index.html?ca=drs-)

[Vim 脚本代码规范](https://github.com/vim-china/vim-script-style-guide)