- [Git 常用操作](#git-常用操作)
  - [1. 安装](#1-安装)
  - [2. 配置](#2-配置)
    - [2.1. 设置当前用户的用户名和 email](#21-设置当前用户的用户名和-email)
    - [2.2. 为 GitHub 账号添加 SSH Keys](#22-为-github-账号添加-ssh-keys)
    - [2.3. 设置换行符](#23-设置换行符)
    - [2.4. 设置代理](#24-设置代理)
    - [2.5. 配置提交日志模版](#25-配置提交日志模版)
    - [2.6. 配置命令别名](#26-配置命令别名)
    - [2.7. 配置 Buffer 大小](#27-配置-buffer-大小)
    - [2.8. 配置代理](#28-配置代理)
    - [2.9. 其它常用配置](#29-其它常用配置)
    - [2.10. 中文显示配置](#210-中文显示配置)
    - [2.11. .gitignore 文件配置](#211-gitignore-文件配置)
    - [2.12. .editorconfig 文件配置](#212-editorconfig-文件配置)
  - [3. 基本命令](#3-基本命令)
  - [4. 常用操作](#4-常用操作)
    - [4.1. 手动创建一个项目](#41-手动创建一个项目)
    - [4.2. 提交本地修改到远程仓库](#42-提交本地修改到远程仓库)
    - [4.3. 使用他人提供的 ssh 公钥 push 到他人远程仓库](#43-使用他人提供的-ssh-公钥-push-到他人远程仓库)
    - [4.4. fork 别人的项目后从原项目拉取最新代码](#44-fork-别人的项目后从原项目拉取最新代码)
  - [5. Refer Links](#5-refer-links)

# Git 常用操作

## 1. 安装

- Windows

  在 https://git-scm.com/ 下载 git for windows，安装即可。

- Ubuntu
  ```
  apt install git
  ```
- MacOS
  ```
  brew install git
  ```

## 2. 配置

[git-config(1) Manual Page](https://git-scm.com/docs/git-config)

[《Pro Git》8.1 Customizing Git - Git Configuration](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration)

### 2.1. 设置当前用户的用户名和 email

```
git config --global user.name "Your Name"
git config --global user.email "youremail@domain.com"
```
Home 目录下会新建一个 `.gitconfig` 文件，配置信息就保存于这个文件中。

### 2.2. 为 GitHub 账号添加 SSH Keys

以公钥认证方式访问 SSH 协议的 Git 服务器时无需输入口令，而且更安全（访问 HTTP 协议的 Git 服务器时，比如提交修改，每次都需要输入口令）。

1. 创建 SSH key
    ```
    $ ssh-keygen -t rsa -C youremail@163.com
    ```
    系统会提示 key 的保存位置（一般是~/.ssh 目录）和指定口令，保持默认，连续三次回车即可。

1. Copy SSH Key

    然后用 vim 打开 id_rsa.pub 文件内的内容，粘帖到 github 帐号管理的添加 SSH key 界面中。
    ```
    vim ~/.ssh/id_rsa.pub
    ```

1. 添加到 GitHub

    登录 github-> Accounting settings 图标 -> SSH key-> Add SSH key-> 填写 SSH key 的名称（可以起一个自己容易区分的），然后将拷贝的 `~/.ssh/id_rsa.pub` 文件内容粘帖 -> add key”按钮添加。

1. 测试
  ```
  ssh -T git@github.com
  ```

### 2.3. 设置换行符

[Git 多平台换行符问题 (LF or CRLF)](http://kuanghy.github.io/2017/03/19/git-lf-or-crlf)

[GitHub 第一坑：换行符自动转换](https://github.com/cssmagic/blog/issues/22)

[GitHub Help: Dealing with line endings](https://help.github.com/en/articles/dealing-with-line-endings)

假如你正在 Windows 上写程序，又或者你正在和其他人合作，他们在 Windows 上编程，而你却在其他系统上，在这些情况下，你可能会遇到行尾结束符问题。这是因为在不同平台上文本文件使用不同的换行符来表示一行的结束：
- Windows: CRLF
- UNIX/Linux: LF
- MacOS: CR

相关 Git 配置选项：
- core.autocrlf

  Git can handle this by **auto-converting CRLF line endings into LF** when you add a file to the index, and **vice versa** when it checks out code onto your filesystem. You can turn on this functionality with the `core.autocrlf` setting.

  - `git config --global core.autocrlf false`: 关闭自动转换功能。
  - `git config --global core.autocrlf true`: 适用于 Windows, this **converts LF endings into CRLF when you check out code**. 即只在从 Git 服务器 check out 代码到本地时，将 LF 转换为 CRLF。
  - `git config --global core.autocrlf input`: 适用于 Linux or macOS, this **converts CRLF to LF on commit but not the other way**. This setup should leave you with CRLF endings in Windows checkouts, but LF endings on macOS and Linux systems and in the repository. 即只在将代码从本地 commit 到 Git 服务器时（所谓的 "input"），将 CRLF 转换为 LF。

- core.safecrlf

  该选项用于检查文件是否包含混合换行符，其有三个可选项：
  - true: 拒绝提交包含混合换行符的文件。
  - false: 允许提交包含混合换行符的文件。
  - warn: 提交包含混合换行符的文件时给出警告。
  ```
  git config --global core.safecrlf true   // 禁止提交混合换行符
  ```

常用 Git 相关配置搭配：
```
git config --global core.autocrlf input
git config --global core.safecrlf false
```

P.S.

使用 dos2unix 命令行工具将代码仓库中的所有文件转换为 LF 换行符并保持原文件的修改时间不变：
```
find . -name "*.md" | xargs dos2unix -k
```

### 2.4. 设置代理

设置针对个别 URL 的代理：
```
git config --global http.http://github.com.proxy http://127.0.0.1:1080
git config --global https..https://github/com.proxy https://127.0.0.1:1080
```

设置全局代理：
```
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy https://127.0.0.1:1080
```

撤销代理：
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```

查看当前的代理配置
```
git config --global --get http.proxy
git config --global --get https.proxy
```

### 2.5. 配置提交日志模版

在 home 目录（`C:\Documents and Settings\username`）创建一个提交日志模版`.gitmessage.txt`，模版内容如下：
```
-- 新功能 = 【模块】XXXX   【变更说明】XXX

--bug=bugid 【背景 / 原因 / 解决方案】XXX

--crid=XXX【若有 codereview，则这个 xxx 需要填写 codereview ID, 界面中不带#的那串数字】
```
修改全局配置文件：
```
git config --global commit.template C:\Documents and Settings\.gitmessage.txt
```

### 2.6. 配置命令别名

```
# 使 git st == git status
git config --global alias.st status
# co 表示 checkout
git config --global alias.co checkout
# ci 表示 commit
git config --global alias.ci commit
# br 表示 branch
git config --global alias.br branch
# 配置一个 git last，让其显示最后一次提交信息
git config --global alias.last 'log -1'
# 配置 log 格式
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### 2.7. 配置 Buffer 大小

在对一些历史悠久的大型项目（如 Linux 项目）进行 git 操作时，可能会出现由于 Buffer 不够大导致的错误：
```
error: RPC failed; curl 18 transfer closed with outstanding read data remaining
```

通过以下配置增大 postBuffer（此处设置为 500MB）即可：
```
git config --global http.postBuffer 524288000
```

### 2.8. 配置代理

[git 代理配置](https://www.jianshu.com/p/27365d2542d7)

```
git config --global https.proxy https://proxyuser:proxypassword@ip/domain:port
git config --global http.proxy http://proxyuser:proxypassword@ip/domain:port
```

配置全局代理：
```
git config --global http.proxy http://127.0.0.1:1080

git config --global https.proxy https://127.0.0.1:1080

git config --global --unset http.proxy

git config --global --unset https.proxy
```

配置局部代理：
```
git conifg --global http.https://git.code.xx.com http://127.0.0.1:1080
git conifg --global http.http://git.code.xx.com http://127.0.0.1:1080
```

### 2.9. 其它常用配置

```
git config --global credential.helper store // 操作一次后，会在 ~/ 目录下生成 .config 和 .git-credentials 两个文件
```

### 2.10. 中文显示配置

[Git 中文显示问题解决](https://xstarcd.github.io/wiki/shell/git_chinese.html)

- 避免 git gui 中中文乱码
  ```
  git config --global gui.encoding utf-8
  ```

- 避免 git status 显示中文乱码
  ```
  git status
  On branch master
  Your branch is up to date with 'origin/master'.

  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

    renamed:    "PenetrationTesting/Steganography/DigitalWatermarking/DW_\345\237\272\347\241\200.md" -> "PenetrationTesting/Steganography/DigitalWatermarking_\345\237\272\347\241\200.md"
    new file:   Programming/Matlab/Matlab Prime.pdf
    new file:   "Programming/Matlab/Matlab_Script1_\345\237\272\346\234\254\350\257\255\346\263\225.md"
    new file:   "Programming/Matlab/Matlab_Script2_VC\346\267\267\347\274\226.md"
    new file:   "Programming/Matlab/Matlab_\345\270\270\347\224\250\346\223\215\344\275\234.md"
    new file:   "Programming/Matlab/matlab\345\205\245\351\227\250\346\225\231\347\250\213.pdf"
  ```
  解决方法：
  ```
  # 不对 0x80 以上的字符进行 quote，解决 git status/commit 时中文文件名乱码
  git config --global core.quotepath off
  ```

### 2.11. .gitignore 文件配置

[廖雪峰 Git 教程：忽略特殊文件](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013758404317281e54b6f5375640abbb11e67be4cd49e0000)

介绍：
- `.gitignore` 配置文件用于配置不需要加入版本管理的文件，配置好该文件可以为我们的版本管理带来很大的便利。
- 不需要从头写。gitignore 文件，GitHub 已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore。

应用场景：
- 忽略操作系统自动生成的文件，比如缩略图等。
- 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如 Java 编译产生的。class 文件。
- 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件或存放数据库密码的配置文件。

配置语法：
- 以井号“#”表示注释。
- 以斜杠“/”开头表示目录。
- 以星号“*”通配多个字符。
- 以问号“?”通配单个字符。
- 以方括号“[]”包含单个字符的匹配列表。
- 以叹号“!”表示不忽略（跟踪) 匹配到的文件或目录。
- git 对于 .ignore 配置文件是按行从上到下进行规则匹配的，意味着如果前面的规则匹配的范围更大，则后面的规则将不会生效。
- 若想将 `.gitignore` 中配置匹配的忽略文件强制 commit，可以用 `-f` 强制添加到 Git：`$ git add -f App.class`。
- 可以用 git check-ignore 命令检查匹配的规则：`$ git check-ignore -v App.class`，Git 会显示。gitignore 的第 3 行规则忽略了该文件。

e.g.
- 规则：
  ```
  fd1/*
  ```
  说明：忽略目录 fd1 下的全部内容。无论是根目录下的 /fd1/ 目录，还是某个子目录 /child/fd1/ 目录，都会被忽略。

- 规则：
  ```
  /fd1/*
  ```
  说明：忽略根目录下的 /fd1/ 目录的全部内容。

- 规则：
  ```
  /*
  !.gitignore
  !/fw/bin/
  !/fw/sf/
  ```
  说明：忽略全部内容，但是不忽略 .gitignore 文件、根目录下的 /fw/bin/ 和 /fw/sf/ 目录。

### 2.12. .editorconfig 文件配置

https://www.zhihu.com/question/19960028

https://github.com/editorconfig/editorconfig/wiki

http://www.alloyteam.com/2014/12/editor-config/

[Crete A Plugin for yourself](https://github.com/editorconfig/editorconfig/wiki/Plugin-How-To)

EditorConfig 帮助开发人员定义和维护一致的编码风格在不同的编辑器和 IDE。EditorConfig 项目包含一个文件格式定义编码风格和文本编辑器插件的集合。EditorConfig 文件易于阅读并且他们与版本控制器很好地合作。

EditorConfig 文件使用 INI 格式，目的是可以与 Python ConfigParser Library 兼容，但是允许在分段名（section names）中使用“and”。分段名是全局的文件路径，格式类似于 gitignore。斜杠 (/) 作为路径分隔符，#或者；作为注释。注释应该单独占一行。**EditorConfig 文件使用 UTF-8 格式、CRLF 或 LF 作为换行符**。

当打开一个文件时，EditorConfig 插件会在打开文件的目录和其每一级父目录查找。editorconfig 文件，直到有一个配置文件 root=true。

EditorConfig 配置文件从上往下读取，并且路径最近的文件最后被读取。匹配的配置属性按照属性应用在代码上，所以最接近代码文件的属性优先级最高。

- 通配符：
  ```
  *	匹配除 / 之外的任意字符串
  **	匹配任意字符串
  ?	匹配任意单个字符
  [name]	匹配 name 字符
  [!name]	匹配非 name 字符
  {s1,s2,s3}	匹配任意给定的字符串 (since 0.11.0)
  ```

- [支持的属性](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties)

  NOTE: 不是每种插件都支持所有的属性，具体可见 [Wiki](https://github.com/editorconfig/editorconfig/wiki/EditorConfig-Properties)。

  - `indent_style`: tab 为 hard-tabs，space 为 soft-tabs。
  - `indent_size`: 设置整数表示规定每级缩进的列数和 soft-tabs 的宽度（译注：空格数）。如果设定为 tab，则会使用 tab_width 的值（如果已指定）。
  - `tab_width`: 设置整数用于指定替代 tab 的列数。默认值就是 indent_size 的值，一般无需指定。
  - `end_of_line`: 定义换行符，支持 lf、cr 和 crlf。
  - `charset`: 编码格式，支持 latin1、utf-8、utf-8-bom、utf-16be 和 utf-16le，不建议使用 uft-8-bom。
  - `trim_trailing_whitespace`: 设为 true 表示会除去换行行首的任意空白字符，false 反之。
  - `insert_final_newline`: 设为 true 表明使文件以一个空白行结尾，false 反之。
  - `root`: 表明是最顶层的配置文件，发现设为 true 时，才会停止查找。editorconfig 文件。

  推荐不要指定某些 EditorConfig 属性。比如，tab_width 不需要特别指定，除非它与 indent_size 不同。同样的，当 indent_style 设为 tab 时，不需要配置 indent_size，这样才方便阅读者使用他们习惯的缩进格式。另外，如果某些属性并没有规范化（比如 end_of_line），就最好不要设置它。

实例：
```
# EditorConfig is awesome: http://EditorConfig.org

# top-most EditorConfig file
root = true

[*]
# Unix-style newlines with a newline ending every file
end_of_line = lf
insert_final_newline = true
# Set default charset
charset = utf-8
# 2 space indentation
indent_style = space
indent_size = 2
```

## 3. 基本命令

- clone

  复制一个已创建的仓库：
  ```
  $ git clone ssh://haorooms@domain.com/blog.git
  ```
  NOTE:
  - 浅复制

    git clone 默认会下载项目的完整历史版本，如果你只关心最新版的代码，而不关心之前的历史信息，可以使用 git 的浅复制功能：
    ```
    $ git clone --depth=1 https://github.com/xxxx/xxxx.git
    ```
    `--depth=1` 表示只下载最近一次的版本，使用浅复制可以大大减少下载的数据量。

  - 如果之后又想获取完整历史信息，可以使用：
    ```
    $ git fetch --unshallow
    ```

  - 只克隆指定分支
    ```
    $ git clone --branch <branch_name> <remote-address>
    ```

  - 如果你只是想下载最新的代码看看，也可以直接从 GitHub 上下载打包好的 zip 文件，这比浅复制更快，因为它只包含了最新的代码文件，而且是经过 ZIP 压缩的。

- init

  创建一个新的本地仓库：
  ```
  $ git init
  ```

- status

  显示工作路径下已修改的文件：
  ```
  $ git status
  ```

- diff

  显示与上次提交版本文件的不同：
  ```
  $ git diff
  ```

- add

  把当前所有修改添加到下次提交中：
  ```
  $ git add
  ```
  把对某个文件的修改添加到下次提交中：
  ```
  $ git add -p <file>
  ```

- commit

  提交本地的所有修改：
  ```
  $ git commit -a
  ```
  提交之前已标记的变化：
  ```
  $ git commit
  ```
  附加消息提交：
  ```
  $ git commit -m 'message here'
  ```
  提交，并将提交时间设置为之前的某个日期：
  ```
  git commit --date="`date --date='n day ago'`" -am "Commit Message"
  ```
  修改上次提交：请勿修改已发布的提交记录
  ```
  $ git commit --amend
  ```
  <!-- TODO: git commit 规范 -->

- checkout

  切换分支：
  ```
  $ git checkout branch2
  ```
  创建新分支，同时切换到该新分支：
  ```
  $ git checkout -b <branch>
  ```

- stash

  把当前分支中未提交的修改移动到其他分支
  ```
  $ git stash
  $ git checkout branch2
  $ git stash pop
  ```

- log

  从最新提交开始，显示所有的提交记录（显示 hash， 作者信息，提交的标题和时间）：
  ```
  $ git log
  ```
  显示所有提交（仅显示提交的 hash 和 message）：
  ```
  $ git log --oneline
  ```
  显示某个用户的所有提交：
  ```
  $ git log --author="username"
  ```
  显示某个文件的所有修改：
  ```
  $ git log -p <file>
  ```

- blame

  查看什么用户，在什么时间，修改了文件的什么内容：
  ```
  $ git blame <file>
  ```

- revert

  重置一个提交（通过创建一个截然不同的新提交）
  ```
  $ git revert <commit>
  ```
  将 HEAD 重置到指定的版本，并抛弃该版本之后的所有修改：
  ```
  $ git reset --hard <commit>
  ```
  将 HEAD 重置到上一次提交的版本，并将之后的修改标记为未添加到缓存区的修改：
  ```
  $ git reset <commit>
  ```
  将 HEAD 重置到上一次提交的版本，并保留未提交的本地修改：
  ```
  $ git reset --keep <commit>
  ```

- push

  将本地分支推送到远程分支
  ```
  $ git push 《远程仓库地址 / 别名》 《本地分支名》:《远程分支名》
  ```
  `-f` 强制推送，即利用强覆盖方式用你本地的代码替代 git 仓库内的内容
  ```
  git push –f 《远程仓库地址 / 别名》 《本地分支名》:《远程分支名》
  ```

  实例：

  提交本地 test 分支作为远程仓库 origin 的 master 分支
  ```
  $ git push origin test:master
  ```
  提交本地 test 分支作为仓库 origin 远程的 test 分支
  ```
  $ git push origin test:test
  ```
  删除远程仓库 origin 的分支：将冒号“:”左边的分支设置为空，将删除：右边的远程分支
  ```
  $ git push origin :test
  ```

- pull

  pull 命令用于取回远程仓库指定分支的更新，再与本地的指定分支合并；
  ```
  $ git pull 《远程仓库地址 / 别名》 《远程分支名》:《本地分支名》
  ```

  实例：
  - 取回 origin 主机的 next 分支，与本地的 master 分支合并：
    ```
    $ git pull origin next:master
    ```
  - 如果远程分支是与当前分支合并，则冒号及冒号后面的部分可以省略：
    ```
    $ git pull origin next
    ```
    实质上，这等同于先做 git fetch，再做 git merge。
    ```
    $ git fetch origin next:tmp # 从 origin 远程仓库获取最新版本的代码，并下载到一个新分支 tmp 中
    $ git diff tmp # 查看当前分支（master）与 tmp 的不同，解决冲突
    $ git merge tmp # 将 tmp 分支与当前分支（master）合并
    $ git branch -d tmp # 删除临时分支 tmp
    ```
  - 如果当前分支与远程分支存在追踪关系，git pull 就可以省略远程分支名，本地的当前分支自动与对应的 origin 主机”追踪分支”(remote-tracking branch) 进行合并：
    ```
    $ git pull origin
    ```
  - 如果当前分支只有一个追踪分支，连远程主机名都可以省略：
    ```
    $ git pull
    ```
    上面命令表示，当前分支自动与唯一一个追踪分支进行合并。

  - 如果合并需要采用 rebase 模式，可以使用 `–rebase` 选项：
    ```
    $ git pull --rebase 《远程主机名》 《远程分支名》:《本地分支名》
    ```

- branch

  基本用法：
  - 查看所有分支（包括本地分支和远程分支）
    ```
    $ git branch -a
    ```
  - 基于当前分支创建新分支：
    ```
    $ git branch <new-branch>
    ```
  - 基于远程分支创建新的可追溯的分支：
    ```
    $ git branch --track <new-branch> <remote-branch>
    ```
  - 删除指定本地分支：
    ```
    $ git branch -d <branch>
    ```
  - 在本地删除指定远程分支：
    ```
    $ git branch -r -d 《远程仓库地址 / 别名》/《远程分支名》
    ```

    ![image](http://img.cdn.firejq.com/jpg/2018/11/2/2f9a513a4f227edd2aaa4c05f318ba8e.jpg)

  - 删除远程仓库中的指定分支

    可使用 push 的 hack 用法：将冒号“:”左边的分支设置为空，将删除`:`右边的远程分支。
    ```
    $ git push origin :patch-1 # 删除远程分支 patch-1
    ```

  - 手动指定本地分支与远程分支建立追踪关系
    ```
    $ git branch --set-upstream master origin/next
    ```
    实际上，在 git clone 的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的 master 分支自动”追踪”origin/master 分支。

- remote

  查看远程仓库：
  ```
  $ git remote -v
  ```
  添加远程仓库到 git 中并设置别名：
  ```
  $ git remote add 《远程仓库别名》 《远程仓库 URL 地址》
  ```

  例：
  ```
  $ git remote add firejq git@github.com:firejq/jq666.git
  ```
  删除远程仓库：
  ```
  $ git remote rm origin
  ```

## 4. 常用操作

### 4.1. 手动创建一个项目

1. 在 GitHub 上创建一个 repo，得到 git 地址。
1. 克隆到本地：`git clone https://github.com/firejq/xxxxx.git`，执行完毕后，便成功克隆了远程仓库到本地，即将本地仓库与远程仓库绑定完成。
1. 在本地仓库中（即该项目目录下），添加 / 修改 / 删除文件后，同步推送到远程仓库：
    ```
    $ git add -A
    $ git commit -m “提交修改的标识内容”
    $ git push
    ```
    NOTE: git add 操作时，若不加上 `-A` 参数，只会提交添加和修改的操作，不会提交删除文件的操作。

### 4.2. 提交本地修改到远程仓库

![image](http://img.cdn.firejq.com/jpg/2019/4/5/ca96904842d5c3cf9a69ec52be241267.jpg)

1. git clone 已存在 GitHub 上的 Repository（在新建的~/MyTestFolder 目录中）:
    ```
    git clone https://github.com/zhchnchn/ZhchnchnTest.git
    ```

1. 修改一个文件，然后提交
    ```
    $ vim README.md
    $ git status
    $ git add README.md // 修改标识
    $ git status
    $ git commit -m "Edit by WorkUbuntu 1204" // 提交注释
    $ git status
    $ git remote add origin https://github.com/zhchnchn/ZhchnchnTest.git
    ```
    这时会报错误：
    ```
    fatal: remote origin already exists.
    ```

    解决办法：
    ```
    $ git remote rm origin
    ```

    然后再次执行
    ```
    $ git remote add origin https://github.com/zhchnchn/ZhchnchnTest.git
    ```

1. 之后，需要将修改 push 到 GitHub 上
    ```
    git push -u origin master
    ```

    执行该条命令后，会要求输入 GitHub 账户的用户名和密码。

1. 提交完成后，查看 GitHub 上的 Repository，会发现内容修改成功。

### 4.3. 使用他人提供的 ssh 公钥 push 到他人远程仓库

要 push 到另外一个远程仓库：（要先在 C:\Users\xxx\.ssh\config 文件中配置 host 信息）:

![image](http://img.cdn.firejq.com/jpg/2018/11/2/63c431433e6d01963d731b8f88beb441.jpg)

配置多个密钥（在 C:\Users\xxx\.ssh\config 文件中）:

![image](http://img.cdn.firejq.com/jpg/2018/11/2/3ac18c4f6cc89de3a4a69b15b4eef70b.jpg)

```
Host // 标识名称
HostName // 远程仓库的主机地址
IdentityFile// 对应的密钥的本地路径
~/ 指向 C:\Users\jieqian\ （当前用户的工作目录）
```

克隆远程仓库到本地
1. 先 cd 到某个文件夹下（在该文件夹下生成克隆仓库）

1. ![image](http://img.cdn.firejq.com/jpg/2018/11/2/dd78634f3883c98126756b17f42bace4.jpg)

    如果没有在 config 文件中配置 shabby 的 host，shabby 要改成 github.com（远程仓库主机地址）。

### 4.4. fork 别人的项目后从原项目拉取最新代码

1. clone 你 fork 后的项目到本地

    ![image](http://img.cdn.firejq.com/jpg/2018/11/2/cf62a3eca02975c12b4fc95547e0bed5.jpg)

    ![image](http://img.cdn.firejq.com/jpg/2018/11/2/53bbb87d9e430342afbfa5c870c5b073.jpg)

1. 添加原项目地址作为上游的远程仓库
    1. 查看当前远程仓库：`git remove -v`
    1. 添加原项目地址作为上游的远程仓库（upstream 是远程仓库的别名，可自定义）：`git remove add upstream https://github.com/xxx/xxx.git`
    1. 再次查看远程仓库：`git remove -v`
    1. 查看所有分支：`git branch -a`

1. 从原项目拉取最新代码到本地的对应分支（会根据原项目分支创建新分支）：`git fetch upstream`，再次查看当前所有分支：`git branch -a`.

1. 查看新代码与当前代码的异同，处理冲突：`git diff upstream/master master`

1. 将最新代码的分支 upstream/master 与当前分支 master 合并：`git merge upstream/master master`

1. 推送最新代码到自己的远程仓库：`git push origin`

## 5. Refer Links

[git Reference](https://git-scm.com/docs)

[简明 Git 命令速查表](http://www.codeceo.com/article/git-command-guide.html)

[git 常用命令及常见问题](https://xianyulaodi.github.io/2017/03/31/git%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4%E6%80%BB%E7%BB%93/)

[Git 的奇技淫巧](https://github.com/521xueweihan/git-tips)

[Angular git commit 规范](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#commit)
