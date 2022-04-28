---
title: 使用 Hugo 模块
linktitle: 使用 Hugo 模块
description: 如何使用 Hugo 模块来构建和管理您的站点。
date: 2019-07-24
categories: [hugo modules]
keywords: [install, themes, source, organization, directories,usage,modules]
menu:
  docs:
    parent: "modules"
    weight: 20
weight: 20
sections_weight: 20
draft: false
aliases: [/themes/usage/,/themes/installing/,/installing-and-using-themes/]
toc: true
---

**原文：[Use Hugo Modules](https://gohugo.io/hugo-modules/use-modules/)**

## 先决条件

{{< gomodules-info >}}


## 初始化新模块 {id=initialize-a-new-module}

使用 `hugo mod init` 命令初始化一个新的模块。如果模块路径无法被猜测出来，则必须将其作为参数提供，例如：

```bash
hugo mod init github.com/gohugoio/myShortcodes
```

另请参阅 [CLI 文档]({{< relref "/commands/hugo_mod_init" >}})。


## 为主题（theme）使用模块 {id=use-a-module-for-a-theme}

将模块应用于主题的最简单方法是在配置中将其导入。

1. 初始化 Hugo 模块系统：`hugo mod init github.com/<your_user>/<your_project>`
2. 导入主题：

{{< code-toggle file="config" >}}
[module]
  [[module.imports]]
    path = "github.com/spf13/hyde"
{{< /code-toggle >}}


## 更新模块 {id=update-modules}

当您将模块作为导入添加到配置中时，将会下载并添加该模块，参阅[模块导入]({{< relref "/hugo-modules/configuration#module-config-imports" >}})。

要更新或管理版本，可以使用 `hugo mod get` 命令。

一些梨子:

### 更新所有模块

```bash
hugo mod get -u
```

### 递归更新所有模块

{{< new-in "0.65.0" >}}

```bash
hugo mod get -u ./...
```

### 更新一个模块

```bash
hugo mod get -u github.com/gohugoio/myShortcodes
```
### 获取特定版本

```bash
hugo mod get github.com/gohugoio/myShortcodes@v1.0.7
```

另请参阅 [CLI 文档]({{< relref "/commands/hugo_mod_init" >}})。


## 在模块中进行和测试更改 {id=make-and-test-changes-in-a-module}

对项目中导入的模块进行本地开发的一种方法是在 `go.md` 中添加一个指向到一个本地目录的 `replace` 指令：

```bash
replace github.com/bep/hugotestmods/mypartials => /Users/bep/hugotestmods/mypartials
```

如果你使用 `hugo server` 运行，将会重新加载配置，并将 `/Users/bep/hugotestmods/mypartials` 放入监视列表。

注意，从 v0.77.0 开始，您可以使用模块配置的 [`replacements`]({{< relref "/hugo-modules/configuration#module-config-top-level" >}}) 选项。{{< new-in "0.77.0" >}}


## 打印依赖图谱 {id=print-dependency-graph}

在相应的模块目录中执行 `hugo mod graph` 命令，将会打印依赖关系图，包括 vendoring、模块 replacement 或者禁用状态，例如：

```
hugo mod graph

github.com/bep/my-modular-site github.com/bep/hugotestmods/mymounts@v1.2.0
github.com/bep/my-modular-site github.com/bep/hugotestmods/mypartials@v1.0.7
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myassets@v1.0.4
github.com/bep/hugotestmods/mypartials@v1.0.7 github.com/bep/hugotestmods/myv2@v1.0.0
DISABLED github.com/bep/my-modular-site github.com/spf13/hyde@v0.0.0-20190427180251-e36f5799b396
github.com/bep/my-modular-site github.com/bep/hugo-fresh@v1.0.1
github.com/bep/my-modular-site in-themesdir

```

另请参阅 [CLI 文档]({{< relref "/commands/hugo_mod_init" >}})。


## Vendor 你的模块 {id=vendor-your-modules}

`hugo mod vendor` 会将所有模块依赖项写入一个 `_vendor` 文件夹，并将其用于所有后续构建。

注意：

* 你可以在模块树中的任何层级运行 `hugo mod vendor`。
* Vendoring 不会储存 `themes` 文件夹中存储的模块。
* 大多数命令都接受一个 `--ignoreVendorPaths` 标志，使用 Glob 路径模式来匹配，意思是不要将 `_vendor` 中的模块用于匹配到的模块。注意，在 Hugo 0.75 之前，此标志名为 `--ignoreVendor` 意思是“全忽略或全不忽略”。{{< new-in "0.75.0" >}}

另请参阅 [CLI 文档]({{< relref "/commands/hugo_mod_init" >}})。


## 整理 go.mod， go.sum {id=tidy-gomod-gosum}

运行 `hugo mod tidy` 以删除 `go.mod` 和 `go.sum` 中未使用的条目。

另请参阅 [CLI 文档]({{< relref "/commands/hugo_mod_init" >}})。


## 清理模块缓存 {id=clean-module-cache}

运行 `hugo mod clean` 以删除整个模块的缓存。

注意，您还可以配置使用 `maxAge` 配置 `modules` 缓存，参阅[文件缓存]({{< relref "/getting-started/configuration#configure-file-caches" >}})。

另请参阅 [CLI 文档]({{< relref "/commands/hugo_mod_init" >}})。
