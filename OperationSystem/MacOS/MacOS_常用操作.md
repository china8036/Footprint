- [MacOS 常用操作](#macos-常用操作)
  - [1. 显示隐藏文件](#1-显示隐藏文件)
  - [2. 复制文件路径](#2-复制文件路径)
  - [3. 刷新 DNS 缓存](#3-刷新-dns-缓存)
  - [4. 文件名乱码](#4-文件名乱码)
  - [5. 读写 NTFS](#5-读写-ntfs)
  - [6. 与 Windows 配合使用](#6-与-windows-配合使用)
  - [7. Refer Links](#7-refer-links)

# MacOS 常用操作

## 1. 显示隐藏文件

`Command+Shift+.` 可以显示隐藏文件、文件夹，再按一次，恢复隐藏。

## 2. 复制文件路径

`Command+Option+c`

## 3. 刷新 DNS 缓存

[Flushing your DNS cache in Mac OS X and Linux](https://help.dreamhost.com/hc/en-us/articles/214981288-Flushing-your-DNS-cache-in-Mac-OS-X-and-Linux)

- Mac OS X 12 (Sierra) and later:
  ```
  sudo killall -HUP mDNSResponder; sudo killall mDNSResponderHelper; sudo dscacheutil -flushcache
  ```

- Mac OS X 11 (El Capitan) and OS X 12 (Sierra):
  ```
  sudo killall -HUP mDNSResponder
  ```

- Mac OS X 10.10 (Yosemite), Versions 10.10.4+:
  ```
  sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
  ```

## 4. 文件名乱码

[下载的文件名总是「乱码」？这里有各平台的解决方法](https://sspai.com/post/44360)

## 5. 读写 NTFS

[macOS 读写 NTFS，及 EXT 分区格式化和挂载](https://zhih.me/macos-mount-ntfs-ext/)

## 6. 与 Windows 配合使用

[如何优雅地同时使用 mac 和 windows 工作？](https://www.jianshu.com/p/6b747fae1075)

[Mac To Win | 不完全迁移体验指北](https://sspai.com/post/45742)

[microsoft remote desktop 使用](https://jingyan.baidu.com/article/219f4bf7fe65e9de442d38b6.html)

## 7. Refer Links
