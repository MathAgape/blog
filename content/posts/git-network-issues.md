+++
date = '2025-02-22T09:06:36+08:00'
title = '解决使用 Git 的网络问题（国内）'
categories = ["Tech-Tips"]
tags = ["Git", "VPN"]
+++

已经使用了 **VPN** ，浏览器也已经可以顺利上网了，但在**终端**使用 git 命令总遇到网络问题，非常不方便。但只需进行以下两项网络设置，就能在愉快使用 **git** 了！

<!--more-->

---

- 我的系统：Windows 11
- 特别感谢：青羽（提出如何设置 VPN）

---

### 设置 VPN

![Clash](https://mathagape.github.io/blog/images/git-network-issues-clash-settings.png)

特别地，如果无法开启**服务模式**，可参考这篇[Clash for Windows 手动安装 Service Mode](https://blog.arnozeng.com/archives/service-mode-setup-manually.html)解决。

---

### 添加 hosts

打开```C:\Windows\System32\drivers\etc```，找到**hosts**文件。要修改这个文件需要管理员权限，一个简单的办法是，把它复制到桌面，用记事本打开这个文件，在最后一行写入```185.199.108.133 raw.githubusercontent.com```，保存，替换原来那个，重启电脑。

---

### 测试

打开**终端**，用```ping```测试终端是否已经像浏览器一样使用 VPN 。
ho！这就是在国内也能流畅使用 git 的办法！