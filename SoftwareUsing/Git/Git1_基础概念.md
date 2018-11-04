- [Git 基础概念](#git-基础概念)
  - [1. 基础概念](#1-基础概念)
  - [2. 协议支持](#2-协议支持)
    - [2.1. 本地协议](#21-本地协议)
    - [2.2. HTTP 协议](#22-http-协议)
      - [2.2.1. 智能 (smart) HTTP 协议](#221-智能-smart-http-协议)
      - [2.2.2. 哑 (dumb) HTTP 协议](#222-哑-dumb-http-协议)
    - [2.3. SSH 协议](#23-ssh-协议)
    - [2.4. Git 协议](#24-git-协议)
  - [3. 环境变量](#3-环境变量)
  - [4. Refer Links](#4-refer-links)

# Git 基础概念

## 1. 基础概念

Git 是一个分布式版本控制软件。

## 2. 协议支持

Git 可以使用四种主要的协议来传输资料：
- 本地协议（Local）
- HTTP 协议
- SSH（Secure Shell）协议、
- Git 协议

使用不同协议的最直接差别体现在 URL 上。

### 2.1. 本地协议

最基本的协议是本地协议（Local protocol），其中的远程版本库就是硬盘内的另一个目录。

这常见于团队每一个成员都对一个共享的文件系统（例如一个挂载的 NFS）拥有访问权，或者比较少见的多人共用同一台电脑的情况。 后者并不理想，因为你的所有代码版本库如果长存于同一台电脑，更可能发生灾难性的损失。

如果你使用共享文件系统，就可以从本地版本库克隆（clone）、推送（push）以及拉取（pull）。 像这样去克隆一个版本库或者增加一个远程到现有的项目中，使用版本库路径作为 URL。 例如，克隆一个本地版本库，可以执行如下的命令：
```
$ git clone /opt/git/project.git
```
或
```
$ git clone file:///opt/git/project.git
```

NOTE
- 如果直接指定路径，Git 会尝试使用硬链接（hard link）或直接复制所需要的文件，效率更高。
- 如果指定 `file://`，Git 会触发平时用于网路传输资料的进程，那通常是传输效率较低的方法。 指定 `file://` 的主要目的是取得一个没有外部参考（extraneous references）或对象（object）的干净版本库副本。通常是在从其他版本控制系统导入后或一些类似情况需要这么做。

### 2.2. HTTP 协议

Git 通过 HTTP 通信有两种模式。 Git 1.6.6 版本引入了一种新的、更智能的协议，新版本的 HTTP 协议一般被称为“智能” HTTP 协议，旧版本的一般被称为“哑” HTTP 协议。

#### 2.2.1. 智能 (smart) HTTP 协议

“智能” HTTP 协议的运行方式和 SSH 及 Git 协议类似，只是运行在标准的 HTTP/S 端口上并且可以使用各种 HTTP 验证机制，这意味着使用起来会比 SSH 协议简单的多，比如可以使用 HTTP 协议的用户名／密码的基础授权，免去设置 SSH 公钥。

智能 HTTP 协议或许已经是最流行的使用 Git 的方式了，它即支持像 `git://` 协议一样设置匿名服务，也可以像 SSH 协议一样提供传输时的授权和加密。 而且只用一个 URL 就可以都做到，省去了为不同的需求设置不同的 URL。 如果你要推送到一个需要授权的服务器上（一般来讲都需要），服务器会提示你输入用户名和密码。 从服务器获取数据时也一样。

事实上，类似 GitHub 的服务，你在网页上看到的 URL （如 https://github.com/schacon/simplegit)，和你在克隆、推送（如果你有权限）时使用的是一样的。

#### 2.2.2. 哑 (dumb) HTTP 协议

如果服务器没有提供智能 HTTP 协议的服务，Git 客户端会尝试使用更简单的“哑” HTTP 协议。哑 HTTP 协议里 web 服务器仅把裸版本库当作普通文件来对待，提供文件服务。 哑 HTTP 协议的优美之处在于设置起来简单。 基本上，只需要把一个裸版本库放在 HTTP 跟目录，设置一个叫做 post-update 的挂钩就可以了（见 Git 钩子）。 此时，只要能访问 web 服务器上你的版本库，就可以克隆你的版本库。

下面是设置从 HTTP 访问版本库的方法：
```
$ cd /var/www/htdocs/
$ git clone --bare /path/to/git_project gitproject.git
$ cd gitproject.git
$ mv hooks/post-update.sample hooks/post-update
$ chmod a+x hooks/post-update
```
Git 自带的 post-update 挂钩会默认执行合适的命令（git update-server-info），来确保通过 HTTP 的获取和克隆操作正常工作。 这条命令会在你通过 SSH 向版本库推送之后被执行；然后别人就可以通过类似下面的命令来克隆：
```
$ git clone https://example.com/gitproject.git
```

### 2.3. SSH 协议

架设 Git 服务器时常用 SSH 协议作为传输协议。SSH 协议也是一个验证授权的网络协议。

通过 SSH 协议克隆版本库，你可以指定一个 `ssh://` 的 URL：
```
$ git clone ssh://user@server/project.git
```
或者使用一个简短的 scp 式的写法：
```
$ git clone user@server:project.git
```
若不指定用户，Git 会使用当前登录的用户名。

### 2.4. Git 协议

Git 协议是包含在 Git 里的一个特殊的守护进程；它监听在一个特定的端口（9418），类似于 SSH 服务，但是访问无需任何授权。 要让版本库支持 Git 协议，需要先创建一个 git-daemon-export-ok 文件 —— 它是 Git 协议守护进程为这个版本库提供服务的必要条件 —— 但是除此之外没有任何安全措施。 要么谁都可以克隆这个版本库，要么谁也不能。 这意味着，通常不能通过 Git 协议推送。 由于没有授权机制，一旦你开放推送操作，意味着网络上知道这个项目 URL 的人都可以向项目推送数据。因此，极少会有人这么做。

- 优点：
  
  目前，Git 协议是 Git 使用的网络传输协议里最快的。 如果你的项目有很大的访问量，或者你的项目很庞大并且不需要为写进行用户授权，架设 Git 守护进程来提供服务是不错的选择。它使用与 SSH 相同的数据传输机制，但是省去了加密和授权的开销。
- 缺点：
  
  缺乏授权机制。把 Git 协议作为访问项目版本库的唯一手段是不可取的。 一般的做法里，会同时提供 SSH 或者 HTTPS 协议的访问服务，只让少数几个开发者有推送（写）权限，其他人通过 `git://` 访问只有读权限。 Git 协议也许也是最难架设的。 它要求有自己的守护进程，这就要配置 xinetd 或者其他的程序，这些工作并不简单。 它还要求防火墙开放 9418 端口，但是企业防火墙一般不会开放这个非标准端口。而大型的企业防火墙通常会封锁这个端口。

## 3. 环境变量

Git 有三种环境变量，分别以配置文件的形式存放在三个不同的地方：
- 系统环境变量。
  - 系统变量对所有用户都适用。
  - 存放在 git 的安装目录下：`%Git%\etc\gitconfig`。
  - 若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。

- 用户环境变量
  - 用户变量只适用于该系统用户。
  - 存放在系统用户目录下，例如 Windows 10 中，存放在：`C:\Users\firej\.gitconfig`。
  - 若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。

- 本地环境变量
  - 本地变量只对当前项目有效。
  - 存放在当前项目的 git 目录中的配置文件（也就是工作目录中的 `.git/config` 文件）。
  - 若使用 `git config` 时用 `--local` 选项，读写的就是这个文件。

每个级别配置的优先级都高于上层的相同配置，例如 `.git/config` 里的配置会覆盖`%Git%/etc/gitconfig` 中的同名变量。

可以在命令行中使用 git config 工具查看这些变量：
```
查看所有环境变量
$ git config --list 

查看系统环境变量（针对所有用户）
$ git config --system --list 

查看用户环境变量（针对当前用户）
$ git config --global --list 

查看本地环境变量（针对当前项目）
$ git config --local --list 

编辑环境变量
$ git config --[system/global/local] [varname] [yourname] 
$ git config --[system/global/local] --unset [varname] [yourname] 
```

## 4. Refer Links
