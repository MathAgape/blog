+++
date = '2025-02-23T19:17:10+08:00'
title = 'Hugo × GitHub Pages 自动化部署'
categories = ["Web-Dev"]
tags = ["Hugo", "Git"]
+++

当你已经知道如何用 Hugo 完成本地建站，接下来就可以将它部署到互联网上了！
**GitHub Pages 托管**是最为简单的办法之一，无需购买服务器，快捷、免费、稳定，尤其是 **GitHub Actions 自动化部署** 功能特别方便！

<!--more-->

---

- 我的系统：Windows 11
- 我的终端：PowerShell 7
- Hugo 官方文档参考：[Host on GitHub Pages](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

---

### 创建仓库

访问 [github](https://github.com/new) 创建一个新仓库，可以命名为**blog**。
我的仓库地址就是：<https://github.com/MathAgape/blog.git>

注意，要保持仓库公开。
我自己测试了一下，GitHub Pages 不支持私有仓库，否则无法设置自动化部署。（付费好像可以）

---

### 推送本地站点

打开**终端**，进入本地站点的目录：
```PowerShell
cd C:\hugo\blog
```

之前生成网站都时候已经执行过`git init`，故若要将本地站点的全部内容推送到远程，只需执行以下命令：
```PowerShell
git remote add origin https://github.com/MathAgape/blog.git
git add .
git commit -m "Initial commit of Hugo site"
git branch -M main
git push origin main
```

这样，就完成了远程仓库的初始化。
打开浏览器访问仓库，刷新查看是否推送成功。若成功，就可以继续自动化部署的设置了。

---

### 自动化部署

点击网页上的 **Settings > Pages** ，将 **Source** 从 **Deploy from a branch** 改成 **GitHub Actions** 。

![改 Source](https://mathagape.github.io/blog/images/hugo-githubpages-source.png)

打开**终端**（注意当前的工作目录要在`C:\hugo\blog`），用以下命令新建一个空的文件 **hugo.yaml**：
```PowerShell
mkdir -p .github/workflows
touch .github/workflows/hugo.yaml
```

在 **hugo.yaml** 中填入如下代码：
```YAML
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.144.2
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          HUGO_CACHEDIR: ${{ runner.temp }}/hugo_cache
          HUGO_ENVIRONMENT: production
          TZ: America/Los_Angeles
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

注意：检查 **HUGO_VERSION** 以及 **branches** 要与自己实际情况匹配，其余不要改动。

保存对 **hugo.yaml** 的修改，再打开**终端**，推送刚刚的修改：
```PowerShell
git add .
git commit -m "Add GitHub Actions for Hugo deployment”
git push origin main
```

打开浏览器访问仓库，点击 **Actions**。

![查看工作流](https://mathagape.github.io/blog/images/hugo-githubpages-actions.png)

你会看到工作流的状态，等到前面的图标变成绿勾，就代表完成部署了。点击绿勾右边的链接。

![部署完成](https://mathagape.github.io/blog/images/hugo-githubpages-done.png)

再点击 **deploy** 下的网址，就能看到 GitHub Pages 了！

![GitHub Pages 页面链接](https://mathagape.github.io/blog/images/hugo-githubpages-link.png)

---

### 日常维护

对我来说 Hugo + GitHub Pages 的组合非常轻量，便于日常维护。

打开终端，注意当前的工作目录要在`C:\hugo\blog`。

增减修改 Markdown 文档后，若要本地预览，执行以下命令即可：
```PowerShell
hugo server
```

注意，如果看不到新内容，请检查 MarkDown 文档的开头里是否包含了`draft = true`，如果有，删除它。

本地预览若满意，则使用以下命令，即可完成远程更新：
```PowerShell
git add .
git commit -m "wip"
git push
```

ho！这样就完成了本地 Hugo 站点到 GitHub Pages 的托管！大家都能看到你的网站啦！