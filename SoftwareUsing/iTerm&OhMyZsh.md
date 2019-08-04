- [Mac 终端：iTerm2 + OhMyZsh](#mac-终端iterm2--ohmyzsh)
  - [1. iTerm 安装和配置](#1-iterm-安装和配置)
  - [2. OhMyZsh 安装和配置](#2-ohmyzsh-安装和配置)
  - [3. iTerm HotKeys](#3-iterm-hotkeys)
  - [4. Refer Links](#4-refer-links)

# Mac 终端：iTerm2 + OhMyZsh

## 1. iTerm 安装和配置

1. 下载 [iTerm](https://www.iterm2.com/downloads.html) 并解压安装。

1. 配置 iTerm 为默认终端

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/b621146812d9dc5736d9821e8ac2a5b5.jpg)

1. 在 preference 中设置全局热键

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/43f3fd3a3fab819562b177709290fb96.jpg)

1. 在 preference 中设置配色方案

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/cac49f38c481500f2255d6965a7cee80.jpg)

1. 下载 [Meslo](https://github.com/powerline/fonts/blob/master/Meslo%20Slashed/Meslo%20LG%20M%20Regular%20for%20Powerline.ttf) 字体安装后在 preference 中设置字体

    ![image](http://img.cdn.firejq.com/jpg/2019/8/3/bdd0807f9f21c8cd8801d673d076e16d.jpg)

## 2. OhMyZsh 安装和配置

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

## 3. iTerm HotKeys

窗口控制
- cmd + n : 新建窗口
- cmd + t : 新建 TAB
- cmd + d : 垂直新建 TAB
- cmd + shift + d : 水平新建 TAB

## 4. Refer Links

[Mac 终端配置，DIY 你的 Terminal （iTerm 2 + Oh My Zsh）](https://segmentfault.com/a/1190000012786464)

[打造高效个性 Terminal（一）之 iTerm](http://huang-jerryc.com/2016/08/11/%E6%89%93%E9%80%A0%E9%AB%98%E6%95%88%E4%B8%AA%E6%80%A7Terminal%EF%BC%88%E4%B8%80%EF%BC%89%E4%B9%8B%20iTerm/)

TODO:

[Oh-My-Zsh 操作 Git 的快捷键](https://segmentfault.com/a/1190000007145316)

[iTerm2 下配置 ssh 自动登录和使用 lrzsz 上传下载](https://juejin.im/post/5b016ae6f265da0b82631177)

[iTerm2 配置用于同时打开多个 ssh 会话（支持多集群，多机器管理）](https://blog.csdn.net/skyyws/article/details/80929071)

[Mac 下使用 iTerm2 让 SSH 记录远程服务器账号和密码](https://blog.csdn.net/shaobo8910/article/details/75514849)
