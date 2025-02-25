+++
date = '2025-02-24T20:53:36+08:00'
title = 'Hugo × LaTeX 网页数学公式渲染'
categories = ["Web-Dev"]
tags = ["Hugo", "Git", "LaTeX"]
+++

学会基本的 Hugo 本地建站和 GitHub Pages 部署之后，对于数学爱好者来说，最重要的就是让网页实现数学公式渲染。
我选择使用 **JavaScript显示引擎 MathJax**，支持**渲染 LaTeX 数学公式**。
涉及修改主题，附**Fork 主题并替换子模块**教程。

<!--more-->

---

- 我的系统：Windows 11
- 我的终端：PowerShell 7
- Hugo 官方文档参考：[Mathematics in Markdown](https://gohugo.io/content-management/mathematics/)

---

### 效果预览

This is an inline \(a^*=x-b^*\) equation.

These are block equations:

\[a^*=x-b^*\]

\[ a^*=x-b^* \]

\[
a^*=x-b^*
\]

These are also block equations:

$$a^*=x-b^*$$

$$ a^*=x-b^* $$

$$
a^*=x-b^*
$$

**以上效果对应的 MarkDown 代码：**
```MarkDown
This is an inline \(a^*=x-b^*\) equation.

These are block equations:

\[a^*=x-b^*\]

\[ a^*=x-b^* \]

\[
a^*=x-b^*
\]

These are also block equations:

$$a^*=x-b^*$$

$$ a^*=x-b^* $$

$$
a^*=x-b^*
$$
```

---

### 修改站点配置文件

打开**hugo.toml**，添加以下代码：
```TOML
[markup]
  [markup.goldmark]
    [markup.goldmark.extensions]
      [markup.goldmark.extensions.passthrough]
        enable = true
        [markup.goldmark.extensions.passthrough.delimiters]
          block = [['\[', '\]'], ['$$', '$$']]
          inline = [['\(', '\)'], ['$', '$']]
[params]
  math = true
```

---

### Fork 主题并替换子模块

之前一步建站的时候，是直接使用他人仓库的主题；由于本教程会涉及涉及修改主题，故需先 fork 那个主题到自己仓库。
这是原仓库地址：<https://github.com/Vimux/Mainroad>，点击网页右上角的 **Fork** 即可。 
这是我 fork 后的仓库地址：<https://github.com/MathAgape/Mainroad>

接下来，需要删除原来的主题子模块，**终端**命令如下：
```PowerShell
Remove-Item -Recurse -Force themes\mainroad
git submodule deinit themes/mainroad
git rm themes/mainroad
```

然后，就可以添加自己 fork 的仓库为主题子模块，完成子模块替换：
```PowerShell
git submodule add --force https://github.com/MathAgape/Mainroad.git themes/mainroad
```

确保子模块已正确初始化并更新：
```PowerShell
git submodule update --init --recursive
```

`cd C:\hugo\blog\themes\mainroad`进入**主题目录**（即**子模块**目录），用`git remote -v`检查远程分支，地址应该要是自己 fork 后的那个仓库地址。
如果地址不对，可像这样设置远程地址：
```PowerShell
git remote set-url origin https://github.com/MathAgape/Mainroad.git
```

把刚刚的修改都推送至远程仓库：
```PowerShell
git add .
git commit -m "Update submodule"
git push origin main
```

这样，就完成了子模块替换，不再直接使用别人的主题，而是使用自己 fork 的主题，以便于在原主题上进行修改。

---

### 修改主题文件

打开主题目录下的 **layouts\partials** 目录，注意到已经存在 **mathjax.html** 文件，故只需在这个文件中添加代码：
```HTML
<script id="MathJax-script" async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>
<script>
  MathJax = {
    tex: {
      displayMath: [['\\[', '\\]'], ['$$', '$$']],  // block
      inlineMath: [['\\(', '\\)'], ['$', '$']]                  // inline
    },
    loader:{
      load: ['ui/safe']
    },
  };
</script>
```

再打开主题目录下的 **layouts\_default** 目录，打开 **baseof.html** 文件，在这个文件的`<head>`下添加代码：
```HTML
  {{ if .Param "math" }}
    {{ partialCached "math.html" . }}
  {{ end }}
```

把刚刚的修改都推送至远程仓库：
```PowerShell
git add .
git commit -m "modify mathjax.html”
git push origin master
```

最后，`cd C:\hugo\blog`回到站点目录，更新子模块：
```PowerShell
git submodule update --remote --recursive
```

此时可写一篇带有 LaTeX 标记的文章，并用`hugo server`查看数学公式的渲染是否正常，若是正常，就把刚刚的修改都推送至远程仓库：
```PowerShell
git add .
git commit -m "Update submodule"
git push origin main
```

ho！这样，就完成了 Hugo 配置 MathJax 渲染 LaTeX的步骤，我们就可以随意书写数学公式啦！