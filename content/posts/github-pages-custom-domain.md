+++
date = '2025-02-25T17:10:49+08:00'
title = 'GitHub Pages × 阿里云 域名绑定'
categories = ["Web-Dev"]
tags = ["Hugo", "Git", "Github Pages"]
+++

在域名解析中添加两条记录（A 与 CNAME），即可将**阿里云**购买的域名与 **Github Pages** 绑定。

<!--more-->

---

### 阿里云域名解析

打开**终端**，用`ping`获取自己 GitHub Pages 主机的**IP地址**，例如：
```PowerShell
ping mathagape.github.io
```

访问[阿里云域名注册与管理平台](https://wanwang.aliyun.com/domain/)，点击右上角的菜单进入**域名控制台**。

再从左侧菜单进入**域名列表**，并点击要绑定的域名，我要绑定的域名是**mathagape.com**。
再从左侧菜单进入**域名解析**，再**解析设置**下添加**A记录**与**CNAME记录**：

- **A记录：** 主机记录`@`，记录值是前面ping出来的那个**IP地址**。
- **CNAME记录：**：主机记录`www`，记录值是自己 GitHub Pages 的原域名。

检查这两条记录的状态为绿色的**启用**。

![](https://mathagape.github.io/blog/images/aliyun-domain-records.png)

---

### GitHub Pages 自定义域名

打开 GitHub Pages 仓库的 **Settings**，点击左侧菜单的 **Pages**，在右侧 **Custom domain** 下填入要绑定的域名，点击 **Save**，等待出现**绿勾 DNS check successful**的提示。

![](https://mathagape.github.io/blog/images/github-pages-custom-domain.png)

---

### 注意

当完成域名绑定之后，需要将 **hugo.toml** 的`baseurl`改为绑定的新域名，否则主题将无法正常渲染。
我的域名是mathagape.com，故改为：
```TOML
baseurl = "https://mathagape.com/"
```
改完之后要过一会，清除浏览器缓存，并刷新页面。

现在，即使打开原来的 GitHub Pages 网址 <https://mathagape.github.io/blog/> ，浏览器也会自动跳转到自定义域名 <https://mathagape.com>；当然，也支持通过网址<https://www.mathagape.com>来访问。

ho！这就是自定义域名的全部过程！