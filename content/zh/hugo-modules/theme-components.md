---
title: 主题组件
linktitle: 主题组件
description: Hugo 通过主题组件（Theme Components）提供高级主题支持。
date: 2017-02-01
categories: [hugo modules]
keywords: [themes, theme, source, organization, directories]
menu:
  docs:
    parent: "modules"
    weight: 50
weight: 50
sections_weight: 50
draft: false
aliases: [/themes/customize/,/themes/customizing/]
toc: true
---

**原文：[Theme Components](https://gohugo.io/hugo-modules/theme-components/)**

{{% note %}}
本节内容可能包含已过时且正在重写的信息。
{{% /note %}}

从 Hugo 0.42 开始，您可以将主题配置为任意多个主题组件的组合：

{{< code-toggle file="config">}}
theme = ["my-shortcodes", "base-theme", "hyde"]
{{< /code-toggle >}}

支持嵌套，让某个主题组件在它自己的 `config.toml` 中引入其它主题组件（主题继承）。[^1]

在上面的 `config.toml` 中定义的主题示例中，创建了一个具有 3 个主题组件的主题，优先级从左到右（从上到下）。

对于任意给定的文件、数据条目，Hugo 将首先在项目中查找，然后是 `my-shortcode`、`base-theme`，最后在 `hyde`。

Hugo 采用两种不同的算法来合并文件系统，具体取决于文件类型：

* 对于 `i18n` 和 `data` 文件，Hugo 使用文件中的 翻译 id 和 数据 key 进行深度合并。
* 对于 `static`、`layouts`（模板）和 `archetypes` 文件，在文件层合并。所以最左边的文件将优先选择。

上述 `theme` 定义中使用的名称必须与 `your-site/themes` 中的文件夹匹配，例如 `your-site/themes/my-shortcodes`。Hugo 后续版本计划改进为使用一个 URL scheme 以使其可以自动解析。

每个主题组件都可以有自己的配置文件，如 `config.toml`，但目前可以配置的内容仅限于：

* `params`（全局的和每种语言的）
* `menu`（全局的和每种语言的）
* `outputformats` 和 `mediatypes`

这里同样应用左优先规则：相同 ID 的 param/menu 等最左侧的会成为最终选项。对于上述配置 Hugo 提供了一些隐藏的和实验性的命名空间（namespace）支持，我们在未来也会努力改进，但仍然鼓励主题作者创建自己的命名空间以避免命名冲突。

[^1]: 对于托管在 [Hugo Themes Showcase](https://themes.gohugo.io/) 上的主题，需要将组件添加为指向 `exampleSite/themes` 目录的 git 子模块（git submodules）。

