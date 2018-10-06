- [Shell 基础](#shell-基础)
  - [1. 元字符 (Meta Charactor)](#1-元字符-meta-charactor)
  - [2. 通配符 (Wildcard)](#2-通配符-wildcard)
  - [3. 转义符](#3-转义符)
  - [4. 重定向](#4-重定向)
    - [4.1. 输入重定向](#41-输入重定向)
    - [4.2. 输出重定向](#42-输出重定向)
  - [5. Refer Links](#5-refer-links)

# Shell 基础

## 1. 元字符 (Meta Charactor)

Shell 有一系列自己的特殊字符 / 元字符：
- `IFS`	由 `<space>` 或 `<tab>` 或 `<enter>` 三者之一产生。
- `CR`	由 `<enter>` 产生。
- `=`	设定变量。
- `$`	作变量或运算替换。
- `>`	重导向 stdout。 
- `<`	重导向 stdin。 
- `|`	命令管道。 
- `&`	重导向 file descriptor ，或将命令置于后台执行。 
- `( )`	将其内的命令置于 nested subshell 执行，或用于运算或命令替换。 
- `{ }`	将其内的命令置于 non-named function 中执行，或用在变量替换的界定范围。
- `;`	在前一个命令结束时，而忽略其返回值，继续执行下一个命令。 
- `&&`	在前一个命令结束时，若返回值为 true，继续执行下一个命令。 
- `||`	在前一个命令结束时，若返回值为 false，继续执行下一个命令。 
- `!`	执行 history 列表中的命令。

## 2. 通配符 (Wildcard)

通配符是由 Shell 预处理的，Shell 会将其当作路径或文件名去在磁盘上搜寻可能的匹配。若符合要求的匹配存在，则进行代换（路径扩展)，否则就将该通配符作为一个普通字符传递给目标命令程序。在通配符被处理后，Shell 会完成该命令的重组，然后再继续处理重组后的命令，直至执行该命令。

Shell 常见通配符：
- `*`: 匹配任意字符串，包括空字符串。
- `?`: 匹配任意单个字符。
- `[abc]`: 匹配 `a` 或者 `b` 或者 `c` 字符。
- `[!abc]`: 匹配除了 `a，b，c` 这 3 个字符之外的任意一个字符。
- `[a-z]`: 匹配 26 个英文小写字符中任意一个。
- `[!a-z]`: 匹配除了“26 个英文小写字符”之外的任意一个字符。
- `{str1, str2, str3}`: 匹配给定字符串序列中的任意一个字符串。
  
NOTE: 
- 以上通配符都不包含对 `/` 字符的匹配。
- 字符范围所能匹配的字符跟 `locale` 设置息息相关。在有些 `locale` 设置中，`[a-d]` 等价于 `[abcd]`，而在有些 `locale` 设置中，`[a-d]` 等价于 `[aBbCcDd]`。
- 若希望匹配文件名以 `.` 开头的文件，无法使用通配符匹配 `.` 字符，必须显示指定 `.` 字符才能匹配成功。
  ```shell
  # wrong
  ls *
  ls [.]*
  ls [.-9]*

  # right
  ls .*
  ls .[a]*
  ls .[a-z]*
  ```

eg:
```shell
# 列出所有文件名不以 a 开头的文件
ls -l [!a]*

# 列出所有文件名以 txt 为后缀的文件
ls -l *.txt

# 列出所有文件名以 txt 为后缀且只包含一个字符的文件
ls -l ?.txt

# 列出所有文件名以 txt 为后缀且只包含一个数字的文件
ls -l [0-9].txt

# 列出文件名为 aa.txt、bb.txt、cc.txt 的文件
ls -l {aa,bb,cc}.txt
```

## 3. 转义符

转义符可以让通配符、元字符变成普通字符，而不起特殊作用。

> There are three quoting mechanisms: the escape character, single quotes, and double quotes.

Shell 提供转义符有三种：
- `\`: 普通转义，去除其后紧跟的元字符或通配符的特殊意义。
- `''`: 硬转义，其内部所有的 shell 元字符、通配符都会被转义。注意，硬转义中不允许出现`'`（单引号）。
- `""`:	软转义，其内部只允许出现特定的 shell 元字符 `$` 用于参数代换，`（尖引号）用于命令执行。

## 4. 重定向

- 标准输入：`/dev/stdin`，用 0 表示。
- 标准输出：`/dev/stdout`，用 1 表示。
- 标准错误输出：`/dev/stderr`，用 2 表示。
- 空设备文件：`/dev/null `，相当于无法恢复的回收站。

### 4.1. 输入重定向

- `command < file`: 将文件中的内容作为命令输入。
- `command << string`: 将到指定字符串之前的所有内容都作为命令输入。

eg:
```bash
$ ls /home/firejq

workplace

$ ls << "EOF"    
> /home 
> /firejq      
> EOF

redis                                             
```

### 4.2. 输出重定向

- stdout 重定向
  - `command > file`: stdout 重定向到文件中（覆盖），stderr 重定向到屏幕上。
  - `command >> file`: stdout 重定向到文件中（追加），stderr 重定向到屏幕上。
- stderr 重定向
  - `command 2> file`: stderr 重定向到文件中（覆盖），stdout 重定向到屏幕上。
  - `command 2>> file`: stderr 重定向到文件中（追加），stdout 重定向到屏幕上。
- stdout 和 stderr 分别重定向
  - `command > file1 2> file2`: stdout 重定向到 file1（覆盖），stderr 重定向到 file2（覆盖）。
  - `command >> file1 2>> file2`: stdout 重定向到 file1（追加），stderr 重定向到 file2（追加）。
- stdout 和 stderr 统一重定向
  - `command &> file` 或 `command > file 2>&1`: stdout 和 stderr 都重定向到文件中（覆盖），屏幕上没有输出。
  - `command &>> file` 或 `command >> file 2>&1`: stdout 和 stderr 都重定向到文件中（追加），屏幕上没有输出。

NOTE:
- `cmd >a 2>a` 和 `cmd >a 2>&1` 有什么不同？
  - `cmd >a 2>a`: stdout 和 stderr 都直接送往文件 a，使用了 FD1、FD2 两个互相竞争使用文件 a 的管道。a 文件会被打开两遍，会导致 stdout 和 stderr 互相覆盖。
  - `cmd >a 2>&1`: stdout 直接送往文件 a，而 stderr 继承了 FD1 的管道之后，再被送往文件 a，也就是说只使用了一个管道 FD1，但已经包括了 stdout 和 stderr。a 文件只被 FD1 打开一遍，不会导致 stdout 和 stderr 互相覆盖。

- 为什么 `command > file 2>&1` 中的 `2>&1` 要写在后面？

  用 `strace` 进行分析可以看到：
  - `command > file 2>&1` 实现重定向的关键系统调用序列是：
    ```
    open(file) == 3 
    dup2(3,1) 
    dup2(1,2)
    ```
    首先是 `command > file` 将标准输出重定向到 file 中， `2>&1` 是标准错误拷贝了标准输出的行为，也就是同样被重定向到 file 中，最终结果就是标准输出和错误都被重定向到 file 中。 
  - `command 2>&1 >file` 实现重定向的关键系统调用序列是： 
    ```
    dup2(1,2) 
    open(file) == 3 
    dup2(3,1)
    ```
    `2>&1` 标准错误拷贝了标准输出的行为，但此时标准输出还是在终端。`>file` 后输出才被重定向到 file，但标准错误仍然保持在终端。

## 5. Refer Links

[Linux Shell 通配符模式表达式](https://dslztx.github.io/blog/2017/05/10/Linux-Shell%E9%80%9A%E9%85%8D%E7%AC%A6%E6%A8%A1%E5%BC%8F%E8%A1%A8%E8%BE%BE%E5%BC%8F/)

[Linux Shell 通配符、元字符、转义符使用实例介绍](https://www.cnblogs.com/chengmo/archive/2010/10/17/1853344.html)

TODO:

http://www.ruanyifeng.com/blog/2018/09/bash-wildcards.html