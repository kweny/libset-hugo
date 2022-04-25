---
title: 基本用法
linktitle: 基本用法
description: Hugo 的 CLI 功能齐全且易于使用，即使对于命令行经验非常有限的人也是如此。
date: 2017-02-01
publishdate: 2017-02-01
lastmod: 2017-02-01
categories: [getting started]
keywords: [usage,livereload,command line,flags]
menu:
  docs:
    parent: "getting-started"
    weight: 40
weight: 40
sections_weight: 40
draft: false
aliases: [/overview/usage/,/extras/livereload/,/doc/usage/,/usage/]
toc: true
---

**原文：[Basic Usage](https://gohugo.io/getting-started/usage/)**

本文描述的是在开发 Hugo 项目时将会使用的最常用的命令。想要全面了解 Hugo 的 CLI（Command Line Interface，命令行界面）请参阅[命令行参考][commands]。

## 测试安装

当您已经[安装了 Hugo][install]，确保它在您的 `PATH` 中，就可以通过以下 `help` 命令测试 Hugo 是否已正确安装。


```
hugo help
```

在控制台中看到的输出应类似于以下内容：

```
hugo is the main command, used to build your Hugo site.

Hugo is a Fast and Flexible Static Site Generator
built with love by spf13 and friends in Go.

Complete documentation is available at https://gohugo.io/.

Usage:
  hugo [flags]
  hugo [command]

Available Commands:
  check       Contains some verification checks
  config      Print the site configuration
  convert     Convert your content to different formats
  env         Print Hugo version and environment info
  gen         A collection of several useful generators.
  help        Help about any command
  import      Import your site from others.
  list        Listing out various types of content
  new         Create new content for your site
  server      A high performance webserver
  version     Print the version number of Hugo

Flags:
  -b, --baseURL string         hostname (and path) to the root, e.g. https://spf13.com/
  -D, --buildDrafts            include content marked as draft
  -E, --buildExpired           include expired content
  -F, --buildFuture            include content with publishdate in the future
      --cacheDir string        filesystem path to cache directory. Defaults: $TMPDIR/hugo_cache/
      --cleanDestinationDir    remove files from destination not found in static directories
      --config string          config file (default is path/config.yaml|json|toml)
      --configDir string       config dir (default "config")
  -c, --contentDir string      filesystem path to content directory
      --debug                  debug output
  -d, --destination string     filesystem path to write files to
      --disableKinds strings   disable different kind of pages (home, RSS etc.)
      --enableGitInfo          add Git revision, date and author info to the pages
  -e, --environment string     build environment
      --forceSyncStatic        copy all files when static is changed.
      --gc                     enable to run some cleanup tasks (remove unused cache files) after the build
  -h, --help                   help for hugo
      --i18n-warnings          print missing translations
      --ignoreCache            ignores the cache directory
  -l, --layoutDir string       filesystem path to layout directory
      --log                    enable Logging
      --logFile string         log File path (if set, logging enabled automatically)
      --minify                 minify any supported output format (HTML, XML etc.)
      --noChmod                don't sync permission mode of files
      --noTimes                don't sync modification time of files
      --path-warnings          print warnings on duplicate target paths etc.
      --quiet                  build in quiet mode
      --renderToMemory         render to memory (only useful for benchmark testing)
  -s, --source string          filesystem path to read files relative from
      --templateMetrics        display metrics about template executions
      --templateMetricsHints   calculate some improvement hints when combined with --templateMetrics
  -t, --theme strings          themes to use (located in /themes/THEMENAME/)
      --themesDir string       filesystem path to themes directory
      --trace file             write trace to file (not useful in general)
  -v, --verbose                verbose output
      --verboseLog             verbose logging
  -w, --watch                  watch filesystem for changes and recreate as needed

Use "hugo [command] --help" for more information about a command.
```

## `hugo` 命令

最常见的用法可能是直接在当前目录执行 `hugo` 命令，它会将当前目录作为输入来构建网站内容。

默认情况下会将网站内容生成到 `public/` 目录中，也可以通过自定义[站点配置][config]中的 `publishDir` 选项来更改输出目录。

`hugo` 命令将网站渲染到 `public/` 目录，并做好部署到 Web 服务器的准备：

```
hugo
0 draft content
0 future content
99 pages created
0 paginator pages created
16 tags created
0 groups created
in 90 ms
```

## 草稿、未来和过期内容 {#draft-future-and-expired-content}

Hugo 允许您在内容的 [front matter][]（前言设定，也叫文档头，即 Markdown 文档中最上方定义的内容） 中设置 `draft`、`publishdate`、`expirydate` 选项，默认情况下，带有这些选项的内容不会被发布：

1. `publishdate` 表示该内容在未来的指定日期发布。
2. `draft: true` 表示草稿内容。
3. `expirydate` 表示该内容过了指定日期后不会再发布。

在本地开发和部署期间，可以通过在命令 `hugo` 和 `hugo server` 上添加以下标志来开启上述三种内容的发布。也可以在[站点配置][config]中使用同名选项（不带 `--`）设置布尔值来开启这些内容的发布。

1. `--buildFuture`
2. `--buildDrafts`
3. `--buildExpired`

## LiveReload（实时重载）

Hugo 内置了 [LiveReload](https://github.com/livereload/livereload-js)，无需安装额外的软件包。在开发站点时，可以使用 `hugo server` 命令运行服务器并实时查看更改：

```
hugo server
0 draft content
0 future content
99 pages created
0 paginator pages created
16 tags created
0 groups created
in 120 ms
Watching for changes in /Users/yourname/sites/yourhugosite/{data,content,layouts,static}
Serving pages from /Users/yourname/sites/yourhugosite/public
Web Server is available at http://localhost:1313/
Press Ctrl+C to stop
```

它会运行一个功能齐全的 Web 服务器，同时监视[项目组织结构][dirs]中以下目录文件的添加、删除或更改操作：

* `/static/*`
* `/content/*`
* `/data/*`
* `/i18n/*`
* `/layouts/*`
* `/themes/<CURRENT-THEME>/*`
* `config`

当进行更改时，Hugo 将自动重建站点，重建完成后，LiveReload 会通知浏览器以静默方式刷新页面。

多数时候 Hugo 的构建都非常快，除非一直盯着浏览器，否则您可能不会注意到更改。这意味着可以在第二台显示器（或当前显示器的另一半）上保持浏览器打开状态来查看页面变更，而无需离开编辑器。

{{% note "Closing `</body>` Tag"%}}
Hugo 是在您的模板 `</body>` 关闭标签之前注入的 LiveReload `<script>`，因此如果此标签不存在，它将无法工作。
{{% /note %}}

## 自动重定向到您刚保存的页面

当您同时编辑多个文档并希望尽可能地实时预览变更时，如果手动切换浏览器标签显然是很麻烦的。幸运的是 Hugo 提供了一个简单的嵌入式的解决方案，即命令行标志 `--navigateToChanged`。

## 禁用 LiveReload

LiveReload 通过将 JavaScript 脚本注入到 Hugo 生成的页面来工作。这个脚本会创建一个从浏览器到 Hugo 服务端的 web socket 连接。

以下方法可以禁用 LiveReload：

```
hugo server --watch=false
```

或者

```
hugo server --disableLiveReload
```

也可以在站点配置中添加以下选项来禁用：

{{< code-toggle file="config" >}}
disableLiveReload = true
{{< /code-toggle >}}

## 部署您的网站

使用 `hugo server` 命令进行本地开发调试，之后，您需要使用 `hugo` 命令（没有后面的 `server`部分）来重建站点。然后可以将生成的 `public/` 目录复制到您的生产 Web 服务器上，以此来部署您的站点。

由于 Hugo 生成的是静态网站，因此您可以使用任何 Web 服务器托管在任何地方。有关 Hugo 社区贡献的托管和自动化部署的方法，请参阅[托管和部署][hosting]。

{{% warning "Generated Files are **NOT** Removed on Site Build" %}}
`hugo` 命令不会删除已经生成的文件。这意味着您应该在运行 `hugo` 命令之前手动删除 `public/` 目录（或者通过命令行标志或配置文件来指定发布目录）。如果您不删除的话，可能会有在生成的站点中留下错误文件（例如草稿或未来内容）的风险。
{{% /warning %}}


[commands]: {{< relref "/commands" >}}
[config]: {{< relref "/getting-started/configuration" >}}
[dirs]: {{< relref "/getting-started/directory-structure" >}}
[front matter]: {{< relref "/content-management/front-matter" >}}
[hosting]: {{< relref "/hosting-and-deployment" >}}
[install]: {{< relref "/getting-started/installing" >}}
