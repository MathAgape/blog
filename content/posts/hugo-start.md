+++
date = '2025-02-23T17:54:22+08:00'
title = 'Hugo 本地一步建站'
categories = ["Web-Dev"]
tags = ["Hugo", "Git"]
+++

本站使用 Hugo + GitHub Pages 搭建。
**Hugo** 是一个基于 Go 语言的**静态网站生成器**。只需挑选一款喜欢的**主题**，具备一点 **git** 基础，就能在几分钟内轻松完成本地建站。

<!--more-->

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

我选择了 [Mainroad](https://themes.gohugo.io/themes/mainroad/) 这款主题。

---

### 生成网站

打开**终端**，先进入要本地建站的目录，例如```cd C:\hugo```。

使用以下命令即可**一步建站**：
```PowerShell
hugo new site blog
cd  blog
git init
git submodule add https://github.com/vimux/mainroad.git themes/mainroad
echo "theme = 'mainroad'" >> hugo.toml
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

---

### 添加文章

打开**终端**，先进入本地建站的目录，例如```cd C:\hugo\blog```。

使用以下命令即可**添加文章**：
```PowerShell
hugo new content content/posts/my-first-post.md
```

用编辑器打开 my-first-post.md（我使用 **VS Code** 编辑器），会发现一些以 `+++` 包围的内容，称为 **Front Matter** 。

在 **Front Matter** 中，注意到 `draft = true`。这意味着该文档为**草稿**状态，使用 `hugo server` 命令无法看到这篇文档，而要使用以下命令才能预览到草稿：
```PowerShell
hugo server -D
```

ho！这就是使用 Hugo 快速本地建站并添加文章的全部内容，是不是非常简单呢？