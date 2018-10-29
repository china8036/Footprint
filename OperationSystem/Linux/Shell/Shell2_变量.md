

# Shell 变量

## 1. 相关命令

TODO:

- `declare`
  - `declare`: 显示所有 shell 变量和函数。
  - `declare a='beijing' b='shanghai'`: 在当前 shell 中创建变量并赋值。
  - `declare -p a b`: 显示指定变量的值。
  - `declare -r a='beijing'`: 创建只读变量 a。
- `set`
- `unset`
- `env`
- `readonly`

特殊替换
- `${var:-word}`: 若变量 var 为空或已被 unset，返回 word 值。
- `${var:=word}`: 若变量 var 为空或已被 unset，返回 word 值并把 word 值赋值给 var。
- `${var:+word}`: 若变量 var 已被定义，返回 word 值。
- `${var:?msg}`: 若变量 var 为空或已被 unset，将 msg 输出到标准错误输出。
- `${var%pattern}`: 若 var 以 pattern 匹配的模式结尾，则删除 var 中右边最短的匹配。
- `${var%%pattern}`: 若 var 以 pattern 匹配的模式结尾，则删除 var 中右边最长的匹配。
- `${var#pattern}`: 若 var 以 pattern 匹配的模式开头，则删除 var 中左边最短的匹配。
- `${var##pattern}`: 若 var 以 pattern 匹配的模式开头，则删除 var 中左边最长的匹配。

## 2. 预定义变量

### 2.1. 环境变量

**环境变量在当前 Shell 和所有子 Shell 中生效**，必要的时候 shell 脚本也可以定义环境变量。环境变量的变量值可以更改，部分值由系统设定，部分由用户设定。

常用环境变量：
- `HOME`: 用户主目录。
- `PATH`: 环境变量的路径，用冒号分隔。如：`PATH=/bin:/usr/bin:/usr/local/bin`.
- `TERM`: 终端类型。
- `PWD`: 当前所在的绝对路径。
- `PS1`: 主提示符，可更改。默认情况下 root 用户 `PS1=#`，普通用户 `PS1=$`。
- `PS2`: 辅助提示符，即在命令末尾输入 `\` 后回车显示的提示符，用于继续输入命令，默认为 `>`。
- `SHELL`: Shell 解释器的路径。
- `MAIL`: 系统邮箱路径。
- `LOGNAME`: 当前登录的用户名。
- `UID`: 当前用户的 UID。
- `LANG`: 当前系统的主语系。

### 2.2. 特殊变量

普通变量的变量名只能包含数字、字母和下划线，如果一个变量的变量名中包含其他特殊含义的字符，这样的变量被称为特殊变量。

**预定义特殊变量是只读的，不能由用户重新定义**。

- 命令行参数
  - `$n`: 表示传递给脚本或函数的指定参数，也称为位置变量。其中，n 是一个数字，表示第几个参数。例如，第一个参数是 $1，第二个参数是 $2。
    - 特别的，`$0` 表示当前脚本的文件名。
    - 当 n 超过一位数时，需要包含在 `{}` 中，如 `${12}`。
    - 可以使用 `shift` 命令移动参数位置，每执行一次表示将所有位置参数的位置向左移动一位（除了 `$0`）。
  - `$#`: 表示传递给脚本或函数的参数个数。
  - `$*`: 表示传递给脚本或函数的所有参数，即以"$1" "$2" ... "$n" 的形式输出所有参数。当被双引号 (" ") 包含时，"$*" 会将所有的参数作为一个整体，以"$1 $2 ... $n"的形式输出所有参数。
  - `$@`: 表示传递给脚本或函数的所有参数，即以"$1" "$2" ... "$n" 的形式输出所有参数。当被双引号 (" ") 包含时，"$@" 会将各个参数分开，以"$1" "$2" ... "$n" 的形式输出所有参数。
- 返回值
  - `$?`: 表示上个命令的退出状态或函数的返回值。大部分命令执行成功会返回 0，失败返回 1。
- PID
  - `$$`: 表示当前 Shell 进程的 PID。对于 Shell 脚本，就是这个脚本所在的进程的 PID。
  - `$!`: 表示上个后台命令的 PID。
- `!$` 返回上一个命令的最后一个字符串。

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

## 3. 用户自定义变量

**用户自定义变量只在当前 Shell 生效**。

### 3.1. 命名规则

- 区分大小写。
- 只能包含数字、字母、下划线。
- 不能以数字开头。
- 为与命令想区别，一般使用大写字母来命名自定义变量。
- 没有长度限制。

### 3.2. 创建和赋值

Shell 变量是弱类型变量，因此不需要显式声明。

Shell 变量的数据类型可以是数字、字符串（默认），**当变量值为数字时，才允许完成比较和整数操作**:
```shell
a=5
b=3
declare c=$a+$b && echo $c # 5+3，默认为字符串拼接
declare -i c=$a+$b && echo $c # 8，使用 -i 声明为整数类型，可进行算术运算
```

Shell 变量默认值为空，因此若只创建而不赋值，初始值为空而不为 0。

Shell 变量的创建和赋值通常有以下方式：

- 使用 `declare` 命令
  - `-`: 设置变量属性。
  - `+`: 取消变量类型属性。
  - `-x`: 设置环境变量。
  - `-a`: 设置普通数组。
  - `-A`: 设置关联数组。
  - `-i`: 设置整型。
  - `-r`: 设置只读。
  - `-p`: 显示变量类型。

- 直接赋值：`var=value`
  - `=` 两边不可出现空白字符。
  - 若 value 中包含空白字符，需要包含在引号中。

- 命令行参数赋值：`$n`

- 命令输出结果赋值：反引号或 `$(cmd)`

- 使用 `read` 命令：每次读取一个输入行知道遇到换行符为止（并将该换行符替换为空字节）。
  - `read -p "Please input your value:" var`:
  - `read var1 var2 ...`: 若输入 < 变量个数，没有赋值的变量取空值；若输入 > 变量个数，多余的值全部组成一个字符串后赋值给最后一个变量。
  - `read file_name`: 从文件读取变量值。

### 3.3. 读取变量

在变量名前使用 `$` 表示引用变量的值。

在需要时可以使用 `{}` 来识别变量名边界，如 `echo "This is ${order}th"`。

### 3.4. 删除变量

使用 `unset var_name` 从 Shell 内存中销毁该变量，但无法删除只读变量。

### 3.5. 输出变量

- 使用 `echo` 直接输出
  - `echo -n` 关闭自动插入换行符（默认开启）。
  - `echo -e` 开启对转义字符进行识别替换（默认关闭），如 `echo -e "\e[1; 31m hahahaha \e[0m"` 可输出红色的字符 hahahaha。

- 使用 `printf` 格式化输出
  - eg: `printf "The numbers are %.3f, %.2f, %.4f.\n" 32 16 3.14`

## 4. 数组

### 4.1. 创建和赋值

**Shell 仅支持一维数组，各项之间直接用空格分隔，且没有限制数组的大小**。需要注意的是，**在 Shell 中支持普通数组（索引为整数）和关联数组（索引为字符串）**。

- 一次性赋值

  eg:
  ```shell
  student=("Jack" "Mike" "Anny" "John")
  ```

- 逐项赋值

  eg:
  ```shell
  student[0]="Jack"
  student[1]="Mike"
  student[2]="Anny"
  student[3]="John"
  ```
  NOTE: 赋值时若跳过了某一项，则该项值为空，且此时被跳过的 / 未被赋值的下项不属于该数组，如：
  ```shell
  stu[0]="aaa"
  stu[2]="ccc"
  echo ${#stu[*]} # 结果为 2
  ```

### 4.2. 读取

- 读取数组元素个数：`${#stu[*]}` 或 `${#stu[@]}`
- 读取数组中某个元素的长度：`${#stu[i]}`
- 读取全部元素：
  
  eg: 对于 `stu=("a" "b" "c")`
  - 直接输出：
    ```shell
    echo ${stu[*]}
    # 或
    echo ${stu[@]}
    # 结果
    # a b c
    ```
  - 将数组全部元素读取成 n 个元素：
    ```shell
    list=(${stu[@]})
    # 或
    list=("${stu[@]}")
    # 或
    list=(${stu[*]})
    # 结果：
    # list = '([0]="a" [1]="b" [2]="c")'
    ```
  - 将数组全部元素读取成 1 个元素：
    ```shell
    all=("${stu[*]}")
    # 结果：
    # all = '([0]="a b c")'
    ```
  - while 读取：
    ```shell
    i=0
    while [$i -lt ${#stu[*]}]
    do
      echo ${stu[$i]}
      i=$(($i+1))
    done
    ```

NOTE: 直接 `echo $stu` 相当于 `echo ${stu[0]}`，输出的是数组第一个元素。

## 5. 字符串

- 字符串拼接  
  ```shell
  name='world'
  echo "Hello", $naem, "!"
  echo "Hello ${name}!"
  ```

- 字符串长度：`${#string}`

- 截取字符串
  ```shell
  str='Hello world!'
  echo ${str:0:3} # Hel
  echo ${str:2} # llo world!
  ```

- 查找子串
  ```shell
  str='Hello world!'
  echo `expr index "$str" "H"` # 1
  echo `expr index "$str" "o w"` # 5
  ```

## 6. Refer Links

[Shell 特殊变量：Shell $0, $#, $*, $@, $?, $$ 和命令行参数](http://c.biancheng.net/cpp/view/2739.html)
