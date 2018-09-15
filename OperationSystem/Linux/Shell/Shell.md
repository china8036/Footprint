- [Shell](#shell)
  - [1. 元字符 (Meta Charactor)](#1-元字符-meta-charactor)
  - [2. 通配符 (Wildcard)](#2-通配符-wildcard)
  - [3. 转义符](#3-转义符)
  - [4. 特殊变量](#4-特殊变量)
  - [5. Refer Links](#5-refer-links)

# Shell

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

## 4. 特殊变量

普通变量的变量名只能包含数字、字母和下划线，如果一个变量的变量名中包含其他特殊含义的字符，这样的变量被称为特殊变量。

- 命令行参数
  - `$n`: 表示传递给脚本或函数的指定参数。其中，n 是一个数字，表示第几个参数。例如，第一个参数是 $1，第二个参数是 $2。特别的，`$0` 表示当前脚本的文件名。
  - `$#`: 表示传递给脚本或函数的参数个数。
  - `$*`: 表示传递给脚本或函数的所有参数，即以"$1" "$2" ... "$n" 的形式输出所有参数。当被双引号 (" ") 包含时，"$*" 会将所有的参数作为一个整体，以"$1 $2 ... $n"的形式输出所有参数。
  - `$@`: 表示传递给脚本或函数的所有参数，即以"$1" "$2" ... "$n" 的形式输出所有参数。当被双引号 (" ") 包含时，"$@" 会将各个参数分开，以"$1" "$2" ... "$n" 的形式输出所有参数。
- 返回值
  - `$?`: 表示上个命令的退出状态或函数的返回值。大部分命令执行成功会返回 0，失败返回 1。
- PID
  - `$$`: 表示当前 Shell 进程的 PID。对于 Shell 脚本，就是这个脚本所在的进程的 PID。

eg:
```shell
#!/bin/bash

echo "File Name: $0"
echo "First Parameter : $1"
echo "First Parameter : $2"
echo "Quoted Values: $@"
echo "Quoted Values: $*"
echo "Total Number of Parameters : $#"
```
执行脚本：
```
$ ./test.sh AAA BBB

  File Name : ./test.sh
  First Parameter : AAA
  Second Parameter : BBB
  Quoted Values: AAA BBB
  Total Number of Parameters : 2
```

eg:
```shell
#!/bin/bash
echo "\$*=" $*
echo "\"\$*\"=" "$*"
echo "\$@=" $@
echo "\"\$@\"=" "$@"
echo "print each param from \$*"
for var in $*
do
    echo "$var"
done
echo "print each param from \$@"
for var in $@
do
    echo "$var"
done
echo "print each param from \"\$*\""
for var in "$*"
do
    echo "$var"
done
echo "print each param from \"\$@\""
for var in "$@"
do
    echo "$var"
done
```
执行脚本：
```
$ ./test.sh "a" "b" "c" "d"

  $*=  a b c d
  "$*"= a b c d
  $@=  a b c d
  "$@"= a b c d
  print each param from $*
  a
  b
  c
  d
  print each param from $@
  a
  b
  c
  d
  print each param from "$*"
  a b c d
  print each param from "$@"
  a
  b
  c
  d
```

## 5. Refer Links

[Shell 特殊变量：Shell $0, $#, $*, $@, $?, $$ 和命令行参数](http://c.biancheng.net/cpp/view/2739.html)

[Linux Shell 通配符模式表达式](https://dslztx.github.io/blog/2017/05/10/Linux-Shell%E9%80%9A%E9%85%8D%E7%AC%A6%E6%A8%A1%E5%BC%8F%E8%A1%A8%E8%BE%BE%E5%BC%8F/)

[Linux Shell 通配符、元字符、转义符使用实例介绍](https://www.cnblogs.com/chengmo/archive/2010/10/17/1853344.html)