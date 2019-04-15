- [MacOS 常用命令](#macos-常用命令)
  - [1. dscacheutil](#1-dscacheutil)
  - [2. mdfind](#2-mdfind)
  - [3. locate.updatedb](#3-locateupdatedb)
  - [4. Refer Links](#4-refer-links)

# MacOS 常用命令

## 1. dscacheutil

刷新 DNS 缓存：
```
sudo dscacheutil -flushcache
```

## 2. mdfind

mdfind 命令就是 Spotlight 功能的终端界面，这意味着如果 Spotlight 被禁用，mdfind 命令也将无法工作。mdfind 命令非常迅速、高效。

可以通过下面的命令搜索文件名包含 "button" 的文件：
```
mdfind -name "button"
```

由于 mdfind 就是 Spotlight 功能的终端界面，因此还可以使用 mdfind 寻找文件和文件夹的内容。例，寻找所有包含 Pearson 文字的文件：
```
mdfind "Pearson"
```

mdfind 命令还可以通过 `-onlyin` 参数搜索特定文件夹的内容。例，搜索 Library 文件夹中所有 plist 文件：
```
mdfind -onlyin ~/Library plist
```

## 3. locate.updatedb

在 MacOS 上，`/usr/libexec/locate.updatedb` 命令相当于 Linux 平台上的 `updatedb` 命令，对 `locate` 命令起到更新索引数据库的作用。

e.g.
```
sudo /usr/libexec/locate.updatedb
```

为便于使用，可以通过 `ln` 建立软链接：
```
sudo ln -s /usr/libexec/locate.updatedb /usr/local/bin/updatedb
```

## 4. Refer Links
