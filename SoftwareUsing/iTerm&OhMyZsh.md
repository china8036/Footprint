- [Mac 终端：iTerm2 + OhMyZsh](#mac-终端iterm2--ohmyzsh)
  - [1. 安装配置](#1-安装配置)
    - [1.1. iTerm 安装和配置](#11-iterm-安装和配置)
    - [1.2. OhMyZsh 安装和配置](#12-ohmyzsh-安装和配置)
  - [2. 使用技巧](#2-使用技巧)
    - [2.1. iTerm HotKeys](#21-iterm-hotkeys)
    - [2.2. 使 iTerm 记住 SSH 连接账号密码](#22-使-iterm-记住-ssh-连接账号密码)
    - [2.3. 配置 szrz](#23-配置-szrz)
  - [3. Refer Links](#3-refer-links)

# Mac 终端：iTerm2 + OhMyZsh

## 1. 安装配置

### 1.1. iTerm 安装和配置

1. 下载 [iTerm](https://www.iterm2.com/downloads.html) 并解压安装。

1. 配置 iTerm 为默认终端

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/b621146812d9dc5736d9821e8ac2a5b5.jpg)

1. 在 preference 中设置全局热键

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/43f3fd3a3fab819562b177709290fb96.jpg)

1. 在 preference 中设置配色方案

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/cac49f38c481500f2255d6965a7cee80.jpg)

1. 在 preference 中设置高亮显示光标所在行

    在 profile-color 中勾选 cursor-guide 即可。

1. 下载 [Meslo](https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf) 字体安装后在 preference 中设置字体

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/bdd0807f9f21c8cd8801d673d076e16d.jpg)

1. 设置显示命令执行时间

    在 view 中勾选 show timestamps 即可。

### 1.2. OhMyZsh 安装和配置

1. 下载安装 [OhMyZsh](https://ohmyz.sh/)

1. 配置 ~/.zshrc

    - 设置主题
      ```
      ZSH_THEME="agnoster"
      ```
    - 插件安装
      - 自动提示 & 命令补全
        ```
        brew install zsh-syntax-highlighting
        cd ~/.oh-my-zsh/custom/plugins
        git clone git://github.com/zsh-users/zsh-autosuggestions
        cd zsh-autosuggestions
        vim zsh-autosuggestions.zsh
        ```
        修改 `ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=10'`，最后在 `~.zshrc` 中：
        ```
        plugins=(zsh-autosuggestions git)
        source /xxx/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh # /xxx/ 代表 .zshrc 所在的路径
        ```
      - 代理切换
        ```
        git clone https://github.com/escalate/oh-my-zsh-proxy-plugin.git "$(HOME)/.oh-my-zsh/custom/plugins/proxy"
        ```
        Enable plugin in `~/.zshrc`
        ```
        plugins=(proxy)
        ```
        创建以下文件，分别在各个文件中设置代理（no_proxy 设置为空即可）
        ```
        ~/.proxy/http_proxy
        ~/.proxy/https_proxy
        ~/.proxy/ftp_proxy
        ~/.proxy/rsync_proxy
        ~/.proxy/no_proxy
        ```
        至此即可
        - 通过 `lsproxy` 来查看所有代理
        - 通过 `proxy` 来使用 http_proxy
        - 通过 `noproxy` 来取消代理

     - 路径前缀的 `XX@XX` 太长：

      编辑 `~/.oh-my-zsh/themes/agnoster.zsh-theme` 主体文件，将里面的 `build_prompt` 下的 `prompt_context` 字段在前面加 `#` 注释掉即可。

`～/.zshrc` Demo: TODO:
```

```

P.S. 卸载 oh my zsh：`uninstall_oh_my_zsh`.

## 2. 使用技巧

### 2.1. iTerm HotKeys

窗口控制
- cmd + n : 新建窗口
- cmd + t : 新建 TAB
- cmd + d : 垂直新建 TAB
- cmd + shift + d : 水平新建 TAB

按住 cmd 键：
- 可以拖拽选中的字符串。
- 点击 url：调用默认浏览器访问该网址。
- 点击文件：调用默认程序打开文件。
- 如果文件名是 filename:42，且默认文本编辑器是 Macvim、Textmate 或 BBEdit，将会直接打开到这一行。
- 点击文件夹：在 finder 中打开该文件夹。
- 同时按住 option 键，可以以矩形选中，类似于 vim 中的 ctrl + v 操作。

切换 tab：
- cmd+←, cmd+→, cmd+{, cmd+}。
- cmd+ 数字直接定位到该 tab。

查找：
- cmd + f

### 2.2. 使 iTerm 记住 SSH 连接账号密码

在 ～/.ssh/ 中创建文件：~/.ssh/remembered_hosts/example.exp，内容如下：

```shellwget https://raw.githubusercontent.com/mmastrac/iterm2-zmodem/master/iterm2-send-zmodem.sh
wget https://raw.githubusercontent.com/mmastrac/iterm2-zmodem/master/iterm2-recv-zmodem.sh

作者：wait4friend
链接：https://juejin.im/post/5b016ae6f265da0b82631177
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
#!/usr/bin/expect
set host 服务器 IP
set port 服务器 ssh 端口
set user 服务器账号
set password 服务器密码

spawn ssh $user@$host -p $port
expect {
  "*yes/no*" {send "yes\r";exp_continue;}
  "*assword:*" {send "$password\r"}
}
interact
```

在 iTerm 中创建 profile，login shell 中设置 `expect ~/.ssh/remembered_hosts/example.exp`，需要连接使打开该 profile 即可。

### 2.3. 配置 szrz

Mac 上 iTerm 原生不支持 rz/sz 命令，也就是不支持 Zmodem 来进行文件传输，不过只要通过简单的配置就可以实现。

1. brew install lrzsz

1. cd /usr/local/bin && wget https://raw.githubusercontent.com/xmvper/iterm2-zmodem/master/iterm2-send-zmodem.sh && wget https://raw.githubusercontent.com/xmvper/iterm2-zmodem/master/iterm2-recv-zmodem.sh && chmod +x iterm2-recv-zmodem.sh iterm2-send-zmodem.sh

    这两个脚本实际是使用 AppleScript 来弹出文件选择窗口，然后把选中的文件名称传递给 rzsz 命令。

1. 在 iTerm 里面配置触发器，当监控到特定字符串的时候执行刚才下载的两个文件。打开 iTerm2 -> Preferences -> Profiles 选择 Advanced 设置 Triggers ，点击 Edit，分别添加以下 2 个 trigger：
    ```
    Regular expression: rz waiting to receive.\*\*B0100
    Action: Run Silent Coprocess
    Parameters: /usr/local/bin/iterm2-send-zmodem.sh

    Regular expression: \*\*B00000000000000
    Action: Run Silent Coprocess
    Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
    ```
1. 重新打开该 profile 即可。

## 3. Refer Links

[iTerm2 官方文档](https://www.iterm2.com/documentation.html)

[Mac 终端配置，DIY 你的 Terminal （iTerm 2 + Oh My Zsh）](https://segmentfault.com/a/1190000012786464)

[打造高效个性 Terminal（一）之 iTerm](http://huang-jerryc.com/2016/08/11/%E6%89%93%E9%80%A0%E9%AB%98%E6%95%88%E4%B8%AA%E6%80%A7Terminal%EF%BC%88%E4%B8%80%EF%BC%89%E4%B9%8B%20iTerm/)

[Mac 下使用 iTerm2 让 SSH 记录远程服务器账号和密码](https://blog.csdn.net/shaobo8910/article/details/75514849)

[iTerm2 配置用于同时打开多个 ssh 会话（支持多集群，多机器管理）](https://blog.csdn.net/skyyws/article/details/80929071)

[iTerm2 下配置 ssh 自动登录和使用 lrzsz 上传下载](https://juejin.im/post/5b016ae6f265da0b82631177)

TODO:

[Oh-My-Zsh 操作 Git 的快捷键](https://segmentfault.com/a/1190000007145316)