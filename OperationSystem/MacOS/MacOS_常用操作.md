- [MacOS 常用操作](#macos-常用操作)
  - [1. 刷新 DNS 缓存](#1-刷新-dns-缓存)
  - [2. 文件名乱码](#2-文件名乱码)
  - [3. 读写 NTFS](#3-读写-ntfs)
  - [4. 与 Windows 配合使用](#4-与-windows-配合使用)
  - [5. Refer Links](#5-refer-links)

# MacOS 常用操作

## 1. 刷新 DNS 缓存

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

## 2. 文件名乱码

[下载的文件名总是「乱码」？这里有各平台的解决方法](https://sspai.com/post/44360)

## 3. 读写 NTFS

[macOS 读写 NTFS，及 EXT 分区格式化和挂载](https://zhih.me/macos-mount-ntfs-ext/)

## 4. 与 Windows 配合使用

[如何优雅地同时使用 mac 和 windows 工作？](https://www.jianshu.com/p/6b747fae1075)

[Mac To Win | 不完全迁移体验指北](https://sspai.com/post/45742)

## 5. Refer Links
