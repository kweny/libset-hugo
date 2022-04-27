---
title: 配置 Hugo 模块
linktitle: 配置 Hugo 模块
description: 描述 Hugo 模块的配置选项。
date: 2019-07-24
categories: [hugo modules]
keywords: [themes, source, organization, directories]
menu:
  docs:
    parent: "modules"
    weight: 10
weight: 10
sections_weight: 10
toc: true
---

**原文：[Configure Modules](https://gohugo.io/hugo-modules/configuration/)**

## 模块配置：顶层 {id="module-config-top-level"}

{{< code-toggle file="config">}}
[module]
noVendor = ""
proxy = "direct"
noProxy = "none"
private = "*.*"
replacements = ""
workspace = ""
{{< /code-toggle >}}

noVendor {{< new-in "0.75.0" >}}
: 在 Vendoring 时，跳过特定的模块，使用 Glob 路径模式匹配，例如 "github.com/**"。

vendorClosest {{< new-in "0.81.0" >}}
: 启用后，会选择最靠近的依赖模块，默认行为是选择第一个。注意，给定的模块路径仍然只能有一个依赖，因此一旦使用就不能重新定义。

{{% translator-note %}}
Vendor 是 Go 语言中的概念，它是 Go 的一种依赖管理方式：将依赖的包（特指外部包）复制到当前工程下的 vendor 目录，然后当进行 `go build` 或 `go run` 的时候，会优先到这个 vendor 目录中查找依赖包。

在 Hugo 中，使用 `hugo mod vendor` 命令会将当前模块所依赖的模块写入到 `_vendor` 文件夹，以便用于后续构建。

即，noVendor 就是在下载或复制模块的依赖时使用 Glob 路径模式表达式的方式排除特定的模块；开启 vendorClosest 选项就是当为某个模块引入它所依赖的模块时，选择最靠近它的那个，通常就是列出来的第一个。
{{% /translator-note %}}

proxy
: 定义用于下载远程模块的代理服务器。默认为 `direct`，表示类似 "git clone" 的方式。

noProxy
: 指定特定的模块不使用 proxy 所定义的代理。Glob 路径模式匹配，多个以逗号分隔。

private
: 匹配 Glob 路径模式（逗号分隔）的是私有模块。

workspace {{< new-in "0.83.0" >}}
: 指定工作区文件。该选项将启用 Go 语言的工作区模式。也可以使用操作系统环境变量的方式设置，如 `export HUGO_MODULE_WORKSPACE=/my/hugo.work`。工作区模式仅适用于 Go 1.18 及以上版本。

replacements {{< new-in "0.77.0" >}}
: 逗号分隔的 Glob 路径模式与本地目录的映射关系，如 `github.com/bep/my-theme -> ../.., github.com/bep/shortcodes -> /some/path`，通常用于模块的临时本地开发。也可以使用操作系统环境变量来定义，如 `HUGO_MODULE_REPLACEMENTS="github.com/bep/my-theme -> ../.."`。可以使用绝对路径和相对路径，如果使用相对路径的话，应该是相对于 [themesDir]({{< relref "/getting-started/configuration#themesdir" >}})。

{{% translator-note %}}
Go 1.18 引入了一个叫做工作区模式（workspace mode）的特性。这个特性对于在**本地同时开发多个相互间存在依赖关系的模块**非常有帮助。

假如同时开发两个模块 `example.com/main` 和 `example.com/util`，且 `example.com/main` 依赖了 `example.com/util`。在 Go 1.18 之前有两种方式：

1. 每当修改了 `example.com/util`，都要将其提交到代码仓库并打好 tag，然后在 `main` 中使用 `go get -u` 之类的方式将新的 `util(tag)` 更新下来。
2. 在 `example.com/main` 中编写一个 `go.mod` 文件，其中包含一条 replace 指令如 `replace example.com/util => /path/to/util`，这样在构建 `main` 时直接使用本地指定路径的 `util`（可以是绝对路径，也可以是相对于 `main` 的相对路径）。

第 1 种方式操作频繁且效率低下。于是有了第 2 种方式，省去了提交 `util` 并在 `main` 更新的操作，但这种方式需要我们在 `main` 提交之前将 `go.mod` 中的 `replace` 指令删掉，否则其它开发者下载并构建 `main` 时会有问题（比如其他人本地没有 `util`，或者路径不一样）。

为了解决这些痛点，Go 1.18 新增了工作区模式，在该模式下，不再需要 `go.mod` 中的 `replace` 指令，而是创建一个 `go.work` 文件，在这个文件中设置所有模块的本地路径。这样不会对 `main` 的 `go.mod` 文件造成侵入，不影响其代码提交，而 `go.work` 文件也不需要提交到仓库。

那么，在 Hugo 中的 workspace 选项也就是 `go.work` 的作用；而 replacements 选项就相当于 `go.mod` 中的 `replace` 指令。
{{% /translator-note %}}

请注意，上述的几个配置选项对应了 Go Modules 中的概念。其中一些选项可以很自然地设置为操作系统环境变量，比如以 proxy 选项为例：

```
env HUGO_MODULE_PROXY=https://proxy.example.org hugo
```

{{< gomodules-info >}}

## 模块配置: hugoVersion

如果您的模块需要特定版本的 Hugo 才能工作，可以在该 `module` 部分中指定，当用户使用太旧/太新的版本时将会收到警告。

{{< code-toggle file="config">}}
[module]
[module.hugoVersion]
  min = ""
  max = ""
  extended = false
{{< /code-toggle >}}

上面这些配置项都是可选的。

min
: 支持的最低 Hugo 版本，如 `0.55.0`

max
: 支持的最高 Hugo 版本，如 `0.55.0`

extended
: 是否需要扩展版（extended）的 Hugo。

## 模块配置: imports

{{< code-toggle file="config">}}
[module]
[[module.imports]]
  path = "github.com/gohugoio/hugoTestModules1_linux/modh1_2_1v"
  ignoreConfig = false
  ignoreImports = false
  disable = false
[[module.imports]]
  path = "my-shortcodes"
{{< /code-toggle >}}

path
: 可以是有效的 Go Module 路径，如 `github.com/gohugoio/myShortcodes`，也可以是存储在主题文件夹中的模块的目录名称。

ignoreConfig
: 如果启用，任何模块配置文件（如 `config.toml`）都不会被加载。注意，模块的依赖传递也会被停止。

ignoreImports {{< new-in "0.80.0" >}}
: 如果启用，将不遵循该模块的 `imports`。

{{% translator-note %}}
`ignoreImports` 选项的作用是中断依赖传递，假如我们开发的一个模块 import 了另外一个模块 A，而 A 自己也 import 了一些其它的模块，那开启 `ignoreImports` 就会只引入 A 而忽略 A 自己所 import 的那些模块。

`ignoreImports` 其实是对针对 `ignoreConfig` 的一个完善。`ignoreConfig` 虽然也可以中断依赖传递，但同时也会忽略 `menu`、`params` 等其它设置，而 `ignoreImports` 则只忽略 `imports` 设置。
{{% /translator-note %}}

disable
: 设置为 `true` 则在在 `go.*` 文件中保留其版本信息的情况下禁用该模块。

noMounts {{< new-in "0.84.2" >}}
: 对于该 import 项不要挂载任何文件夹。

noVendor
: 对于该 import 项不进行 Vendor（只在主项目中允许）。

{{% translator-note %}}
关于什么是 Vendor 在前文有介绍。
{{% /translator-note %}}

{{< gomodules-info >}}


## 模块配置: mounts

{{% note %}}
在 Hugo 0.56.0 引入 `mounts` 配置时，我们谨慎的保留了原有的 `contentDir`、`staticDir` 之类的配置，以确保旧版本站点都能继续工作。但是您不应该同时配置这两者：如果您添加了一个 `mounts` 部分，应该删除旧的 `contentDir`、`staticDir` 等设置。
{{% /note %}}

{{% warning %}}
当添加了一个自定义挂载（mount），那么涉及的 target 根路径的默认挂载将会被忽略：确保显式地添加它。
{{% /warning %}}

{{% translator-note %}}
例如添加了一个挂载 `source="rediscuss/dist", target="assets/rediscuss"` 将会覆盖默认挂载 `source="assets", target="assets"`。

如果要保留默认的 `assets` 挂载，可以先显式地挂载一遍 `assets`，如——

```toml
module:
  mounts:
  - source: assets
    target: assets
  - source: rediscuss/dist
    target: assets/rediscuss
```
{{% /translator-note %}}

**默认 mounts**
{{< code-toggle file="config">}}
[module]
[[module.mounts]]
    source="content"
    target="content"
[[module.mounts]]
    source="static"
    target="static"
[[module.mounts]]
    source="layouts"
    target="layouts"
[[module.mounts]]
    source="data"
    target="data"
[[module.mounts]]
    source="assets"
    target="assets"
[[module.mounts]]
    source="i18n"
    target="i18n"
[[module.mounts]]
    source="archetypes"
    target="archetypes"
{{< /code-toggle >}}

source
: 挂载的源目录。对于主项目，可以是项目相对路径，或者绝对路径，甚至是软链接（symbol link）；对于其它模块则必须是项目相对路径。

target
: 应该挂载到 Hugo 的虚拟文件系统中的位置。必须以 Hugo 的组件文件夹之一开头：`static`、`content`、`layouts`、`data`、`assets`、`i18n`、`archetypes`，例如 `content/blog`。

lang
: 语言代码，如 "en"。仅用于 `content` 挂载或者多主机模式下的 `static` 挂载。

includeFiles (string or slice)
: 用一个或多个 Glob 模式来匹配要引入的文件或目录。如果匹配模式和 `excludeFiles` 不冲突，则匹配到的文件将会是要挂载的文件。

Glob 模式是从 `source` 根目录开始的文件名匹配，即使是在 Windows 系统上，也应该使用 Unix 风格的斜杠（`/`），`/` 匹配根目录，`**` 可以递归匹配多级路径，如 `posts/**.jpg`。

搜索不区分大小写。

{{< new-in "0.89.0" >}}

excludeFiles (string or slice)
: 一个或多个 Glob 模式来匹配要排除的文件。

