
- [Homebrew](#Homebrew)
  - [1. 更换为国内源](#1-更换为国内源)
  - [2. Refer Links](#2-Refer-Links)

# Homebrew

TODO:

## 1. 更换为国内源

homebrew 主要分两部分：git repo（位于 GitHub）和二进制 bottles（位于 bintray），这两者在国内访问都不太顺畅。可以替换成国内的镜像，git repo 国内镜像就比较多了，可以自行查找。

- [更换为清华源](https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/)

  ```shell
  cd "$(brew --repo)"
  git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git

  cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git

  brew update
  ```

- [更换为中科大源](https://mirrors.ustc.edu.cn/help/homebrew-core.git.html)

  ```shell
  cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
  git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
  ```

## 2. Refer Links

[Homebrew 有比较快的源（mirror）吗？](https://www.zhihu.com/question/31360766)