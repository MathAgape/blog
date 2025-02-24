+++
date = '2025-02-23T17:54:22+08:00'
title = '从零开始的 Hugo 本地建站'
weight = 1
categories = ["Web-Dev"]
tags = ["Hugo"]
+++

本站使用 Hugo + GitHub Pages 搭建。

Hugo 是一个基于 Go 语言的静态网站生成器。几乎不需要编程基础，只要掌握 Markdown 和 Git，就能轻松搭建和维护个人博客或文档网站。

GitHub Pages 则是 GitHub 提供的免费静态网站托管服务，可以直接从 GitHub 仓库部署 Hugo 生成的静态网站。

---

- 我的系统：Windows 11
- 我的终端：PowerShell 7
- Hugo 官方文档参考：[Quick start](https://gohugo.io/getting-started/quick-start/)

### 注意

- 如果你是 Windows 用户，务必使用 **PowerShell 7**。
- 务必要安装 hugo (extended)。

不遵循以上两点，亲测会遇到 bug。

---

### 安装 Hugo

打开**终端**，使用一下命令即可安装 Hugo：
```PowerShell
winget install Hugo.Hugo.Extended
```

安装完成后，可用```PowerShell hugo version```验证。

更多安装方式，可参考官方文档：[Install Hugo on Windows](https://gohugo.io/installation/windows/)

---

### 挑选主题

访问[Hugo 官方主题库](https://themes.gohugo.io/)，挑选一个适合自己需求的主题。

基于我对搜索和 LaTeX 的需求，我选择了[Relearn](https://themes.gohugo.io/themes/hugo-theme-relearn/)这款主题。

---

### 生成网站

打开**终端**，先进入要本地建站的目录，例如```cd C:\hugo```。

使用一下命令即一步建站：
```PowerShell
hugo new site my-new-site
cd my-new-site
git init
git submodule add https://github.com/McShelby/hugo-theme-relearn.git themes/hugo-theme-relearn
echo "theme = 'hugo-theme-relearn'" >> hugo.toml
hugo server
```

按住 ctrl ，点击生成的地址（形如http://localhost:1313/），即可预览网站效果。
按 ctrl+c ，即可停止开发服务器的运行。

##### 小知识：http://localhost:1313/是什么？

http://localhost:1313/这个地址叫做 **本地开发服务器地址（Localhost Address）**，它是 **Hugo 本地服务器** 启动后提供的一个访问网址。具体来说：

- **localhost**：表示你的本机（即你的电脑）。相当于**127.0.0.1**，是一个特殊的 IP 地址，专门用于指向自己的计算机。
- **1313**：端口号，Hugo 默认使用 **1313 端口** 作为本地服务器端口，你可以修改它（例如 `hugo server -p 8080` 会改用 8080 端口）。
- **http://localhost:1313/**：这个 URL 允许你在本机测试 Hugo 生成的网站，而无需先发布到互联网。

当你运行 `hugo server` 时，Hugo 会启动一个临时 Web 服务器，在你修改内容后自动刷新页面，让你能实时预览网站效果。

