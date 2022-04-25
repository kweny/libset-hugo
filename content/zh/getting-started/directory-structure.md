---
title: 目录结构
linktitle: 目录结构
description: Hugo 的 CLI 会创建一个项目目录结构，并将其作为构建过程的输入以创建一个完整的网站。
date: 2017-01-02
publishdate: 2017-02-01
lastmod: 2017-03-09
categories: [getting started,fundamentals]
keywords: [source, organization, directories]
menu:
  docs:
    parent: "getting-started"
    weight: 50
weight: 50
sections_weight: 50
draft: false
aliases: [/overview/source-directory/]
toc: true
---

**原文：[Directory Structure](https://gohugo.io/getting-started/directory-structure/)**

## 新站点脚手架

{{< youtube sB0HLHjgQ7E >}}

在命令行中通过 `hugo new site` 命令运行生成器会创建一个包含以下元素的目录结构：

```
.
├── archetypes
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```


## 目录结构说明

以下是对每个目录的概述，其中包含指向 Hugo 文档各个部分的链接。

[`archetypes`](/content-management/archetypes/)

使用 `hubo new` 命令创建的内容文件，将至少包含 `date`、`title`、和 `draft = true` 这些前言设定（front matter）。这可以节省时间并提高多内容类型的一致性。使用 [archetypes][] 可以自定义预置的前沿设定。

[`assets`][]

资源文件目录，存储所有需要 [Hugo Pipes]({{< ref "/hugo-pipes" >}}) 处理的文件。只有使用了 `.Permalink` 或 `.RelPermalink` 的文件才会被发布到 `public` 目录中。注意：默认情况下不会创建 `assets` 目录。

[`config`](/getting-started/configuration/)

Hugo 附带了大量的[配置指令][configuration directives]，这些配置指令以 JSON、YAML、或 TOML 格式的文件存储在[配置目录](/getting-started/configuration/#configuration-directory)中。`config` 目录是默认的配置目录，可以使用 `configDir` 来更改。默认不创建 `config` 目录，而是直接使用站点根目录下的 `config.toml` 单个配置文件。如果需要配置的指令很多，可以在配置目录中以多文件的方式组织。或者站点需要根据不同的环境配置不同的指令时，也可以在配置目录中为每个环境单独创建文件夹，配置目录中的每个根文件夹将作为每个环境的独立配置目录。

很多站点可能几乎不需要配置，但如果您需要更细致地告诉 Hugo 该如何构建您的站点，可以通过[配置指令][configuration directives]来进行配置。


[`content`][]

站点的所有内容都将位于 `content` 目录中。在 Hugo 中每个顶级文件夹都被视为一个[content section（内容部分）][content section]。举例来说，如果你的站点主要有三个 section：`blog`、`articles` 和 `tutorials`，那么你将使用三个目录来分别存放这个三个部分的内容：`content/blog`、`content/articles` 和 `content/tutorials`。Hugo 使用 section 来分配默认[内容类型][content types]。

[`data`](/templates/data-templates/)

`data` 目录用于存储 Hugo 在生成网站时可以使用的配置文件。如存储一些额外的数据以供 Hugo 在生成站点时使用，可以使用 YAML、JSON 或 TOML 格式来编写这些文件，这些数据文件不用于生成独立页面，而是用于对内容文件进行补充。此外还可以在该目录中创建从本地和动态源中加载自定义数据的[数据模板][data templates]文件。


[`layouts`][]

`layouts` 目录中以 `.html` 文件的形式存放模板。模板用于定义如何呈现内容视图，包括[列表页][lists]、[主页][homepage]、[Taxonomy 模板][taxonomy templates]、[Partial 模板][partials]、[单页模板][singles]等。


[`static`][]

`static` 存储所有静态内容，如图像、CSS、JavaScript 等。当 Hugo 构建站点时，static 目录中的所有文件将被按原样复制。使用 `static` 文件夹的一个很好的例子是[在 Google Search Console 上验证站点所有权][searchconsole]时，希望 Hugo 复制原始的 HTML 文件而不修改其内容。


{{% note %}}
从 **Hugo 0.31** 开始可以使用多个静态目录。
{{% /note %}}

resources
: 资源目录，缓存一些文件以加快构建速度。模板作者也可以使用它来分发构建的 SASS 文件。注意：默认情况下不创建 `resources` 目录。


[archetypes]: /content-management/archetypes/
[configuration directives]: /getting-started/configuration/#all-configuration-settings
[`content`]: /content-management/organization/
[content section]: /content-management/sections/
[content types]: /content-management/types/
[data templates]: /templates/data-templates/
[homepage]: /templates/homepage/
[`layouts`]: /templates/
[`static`]: /content-management/static-files/
[lists]: /templates/list/
[pagevars]: /variables/page/
[partials]: /templates/partials/
[searchconsole]: https://support.google.com/analytics/answer/1142414?hl=en
[singles]: /templates/single-page-templates/
[starters]: /tools/starter-kits/
[taxonomies]: /content-management/taxonomies/
[taxonomy templates]: /templates/taxonomy-templates/
[types]: /content-management/types/
[`assets`]: {{< ref "/hugo-pipes/introduction#asset-directory" >}}
