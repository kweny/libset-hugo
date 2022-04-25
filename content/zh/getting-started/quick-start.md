---
title: 快速开始
linktitle: 快速开始
description: 使用漂亮的 Ananke 主题创建一个 Hugo 站点。
date: 2013-07-01
publishdate: 2013-07-01
categories: [getting started]
keywords: [quick start,usage]
authors: [Shekhar Gulati, Ryan Watters]
menu:
  docs:
    parent: "getting-started"
    weight: 10
weight: 10
sections_weight: 10
draft: false
aliases: [/quickstart/,/overview/quickstart/]
toc: true
---

**原文：[Quick Start](https://gohugo.io/getting-started/quick-start/)**

{{% note %}}
本快速入门在示例中使用 macOS。有关如何在其它操作系统上安装 Hugo 的说明，请参阅[安装](/getting-started/installing)。

建议[安装 Git](https://git-scm.com/downloads) 来运行本教程。

对于其它学习 Hugo 的方法，如书籍或视频教程，请参阅[外部学习资源](/getting-started/external-learning-resources/)页面。
{{% /note %}}

## 第 1 步：安装 Hugo

{{% note %}}
Homebrew 和 MacPorts 是 macOS 的包管理器，可以分别从 [brew.sh](https://brew.sh/) 和 [macports.org](https://www.macports.org/) 安装。如果您使用的是 Windows 等其它操作系统，请参阅[安装](/getting-started/installing)。
{{% /note %}}

```bash
brew install hugo
# 或者
port install hugo
```

验证是否安装成功：

```bash
hugo version
```

{{< asciicast ItACREbFgvJ0HjnSNeTknxWy9 >}}

## 第 2 步：创建新站点

```bash
hugo new site quickstart
```

上面的命令将在一个名为 `quickstart` 的文件夹中创建一个新的 Hugo 站点。

{{< asciicast 3mf1JGaN0AX0Z7j5kLGl3hSh8 >}}

## 第 3 步：添加主题（Theme）

关于主题的选用，可以在 [themes.gohugo.io](https://themes.gohugo.io/) 查阅主题列表，本快速入门使用 [Ananke 主题](https://themes.gohugo.io/gohugo-theme-ananke/)。

首先，从 GitHub 下载主题并将其添加到您站点的 `themes` 目录中：

```bash
cd quickstart
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

然后，将主题添加到站点配置中：

```bash
echo theme = \"ananke\" >> config.toml
```

{{< asciicast 7naKerRYUGVPj8kiDmdh5k5h9 >}}

## 第 4 步：添加一些内容

您可以手动创建内容文件（例如：`content/<CATEGORY>/<FILE>.<FORMAT>`）并在其中编写元数据，您也可以使用 `new` 命令来创建内容文件，该命令可以自动添加一些元数据（例如标题和日期）：

```
hugo new posts/my-first-post.md
```

{{< asciicast eUojYCfRTZvkEiqc52fUsJRBR >}}

编辑新创建的内容文件，它将以如下内容开头：

```markdown
---
title: "My First Post"
date: 2019-03-26T08:47:11+01:00
draft: true
---

```

{{% note %}}
其中 `draft: true` 表示该内容文件是草稿，草稿不会被部署。当内容编写完成后，可以将其改为 `draft: false` 以使其可以正式发布。更多信息参阅[这里](/getting-started/usage/#draft-future-and-expired-content)。
{{% /note %}}

## 第 5 步：启动 Hugo 服务器

现在，在启用[草稿](/getting-started/usage/#draft-future-and-expired-content)的情况下启动 Hugo 服务器：

{{< asciicast BvJBsF6egk9c163bMsObhuNXj >}}

```
▶ hugo server -D

                   | EN
+------------------+----+
  Pages            | 10
  Paginator pages  |  0
  Non-page files   |  0
  Static files     |  3
  Processed images |  0
  Aliases          |  1
  Sitemaps         |  1
  Cleaned          |  0

Total in 11 ms
Watching for changes in /Users/bep/quickstart/{content,data,layouts,static,themes}
Watching for config changes in /Users/bep/quickstart/config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

**使用浏览器访问新站点：[http://localhost:1313](http://localhost:1313)。**

随意编辑或添加新内容，只需要在浏览器中刷新即可快速查看更改（若浏览器存在缓存时，您可能需要在浏览器中强制刷新，如 Ctrl-R）。

## 第 6 步：自定义主题

新站点看起来不错，但在向公众发布之前，需要对其进行一些调整。

### 站点配置

使用文本编辑器打开配置文件 `config.toml`：

```
baseURL = "https://example.org/"
languageCode = "en-us"
title = "My New Hugo Site"
theme = "ananke"
```

`title` 用于设置站点名称，可以使用更个性化的名称替换其值。如果您已经准备好了一个域名，可以使用 `baseURL` 设置，注意，运行本地开发服务器时不需要此值。

{{% note %}}
**提示：** 在 Hugo 服务器运行时对站点配置或站点中的任何其它文件进行更改，将立即在浏览器中看到变化，若浏览器不自动刷新，可能需要[清除缓存](https://kb.iu.edu/d/ahic)。
{{% /note %}}

有关特定于主题的配置选项，请参阅[主题站点](https://github.com/theNewDynamic/gohugo-theme-ananke)。

**如果需要进一步的主题定制，请参阅[定制主题](/themes/customizing/)。**

## 第 7 步：构建静态页面

很简单，只需要执行：

```
hugo -D
```

默认情况下将输出到 `./public` 目录中（可以使用 `-d` 或 `--destination` 标志来更改，或者在配置文件中设置 `publishdir` 选项）。
