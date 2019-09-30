- [Git 使用 GitHub 进行团队合作开发](#git-使用-github-进行团队合作开发)
  - [1. 一切从简](#1-一切从简)
  - [2. 不 Fork 仓库的方法](#2-不-fork-仓库的方法)
  - [3. Git Flow](#3-git-flow)
  - [4. GitHub 多人协作开发的三种方式](#4-github-多人协作开发的三种方式)
    - [4.1. Fork 方式](#41-fork-方式)
    - [4.2. 组织](#42-组织)
    - [4.3. 合作者](#43-合作者)
  - [5. Git 规范](#5-git-规范)
    - [5.1. commit 规范](#51-commit-规范)
  - [6. Refer Links](#6-refer-links)

# Git 使用 GitHub 进行团队合作开发

## 1. 一切从简

GitHub 的各项功能都非常简单，就是因为在实际的软件开发中，往往用不到那些复杂度极高的功能。

## 2. 不 Fork 仓库的方法

将 GitHub 利用到开源软件开发中的正常流程：
1. 在 GitHub 上进行 Fork。
1. 将 1 中的仓库 clone 至本地开发环境。
1. 在本地环境中创建特性分支。
1. 对特性分支进行代码修改并进行提交。
1. 将特性分支 push 到 1 中的仓库中。
1. 在 GitHub 上对 Fork 来源仓库发送 Pull Request。

在无法给不特定的多数人赋予提交权限的公开软件开发中，这种流程能够防止仓库收到计划之外的提交。然而在公司企业的开发中，开发者每天都要见面，要经常互相发送 Pull Request，这种流程就显得有些繁琐了。

因此，为适用于团队人员可以面对面交流的情形，出现了不 fork 仓库的合作方法，可以让每一名开发者都掌握着一个本地仓库和一个远程仓库，使整个开发流程变得简单：

![image](http://img.cdn.firejq.com/jpg/2018/11/2/bfa32585ab5eb1de1d396a5e240b2d24.jpg)

## 3. Git Flow

- 令 master 分支时常保持可以部署的状态。
- 进行新的作业时要从 master 分支创建新分支，新分支名称要具有描述性。
- 在 1 中新建的本地仓库分支中进行提交。
- 在 GitHub 端仓库创建同名分支，定期 push。
- 需要帮助或反馈时创建 Pull Request，以 Pull Request 进行交流。
- 让其他开发者进行审查，确认作业完成后与 master 分支合并。
- 与 master 分支合并后立刻部署。

[Git 工作流指南](https://github.com/xirong/my-git/blob/master/git-workflow-tutorial.md )

## 4. GitHub 多人协作开发的三种方式

https://gist.github.com/belm/6989341

### 4.1. Fork 方式

网上介绍比较多的方式（比较大型的开源项目，比如 cocos2d-x)。

开发者 fork 自己生成一个独立的分支，跟主分支完全独立，pull 代码后，项目维护者可根据代码质量决定是否 merge 代码。

### 4.2. 组织

组织的所有者可以针对不同的代码仓库建立不同访问权限的团队。

Accounts Settings => Organizations =>Create new Organizations 新建一个组织  然后添加项目成员，根据提示设置完毕即可。

新建一个 Repository  新建完毕后  进入 Repository 的 Settings =>Collaborators 在 Teams 下面点击刚创建的组织，比如 eveloper-51/owners。里面就可以添加或者 remove 组织成员。

### 4.3. 合作者

代码仓库的所有者可以为单个仓库增加具备只读或者读写权限的协作者。

合作者方式比较实用，也很方便，新建一个 Repository，完毕之后，进入 Repository 的 Settings，然后在 Manage Collaborators 里就可以管理合作者了。

其他合作者，实用 `ssh-keygen -C "YourEmail@example.com"` （这里的 email 使用 github 账号）生成公钥和私钥，在 Accounts Settings -> SSH keys 将公钥上传上去。

上传完成后，可使用 Tower(Mac 下 Git 管理工具）clone remote Repository 使用 SSH 方式登录（这里的私钥使用刚才生成的） 这样，其他合作者就可以正常的 push 代码了。

## 5. Git 规范

### 5.1. commit 规范

- bugfix: 线上功能 Bug
- sprintfix: 未上线代码修改（功能模块未上线部分 Bug）
- minor: 不重要的修改（换行、拼写错误等）
- feature: 新功能说明

## 6. Refer Links

《Github 入门与实践》