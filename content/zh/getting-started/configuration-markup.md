---
title: 配置标记
linktitle: 配置标记
description: 如何处理 Markdown 和其他标记相关的配置。
date: 2019-11-15
categories: [getting started,fundamentals]
keywords: [configuration,highlighting]
menu:
  docs:
    parent: "getting-started"
    weight: 65
weight: 65
sections_weight: 65
slug: configuration-markup
toc: true
---

**原文：[Configure Markup](https://gohugo.io/getting-started/configuration-markup/)**

## 配置标记（Markup）

{{< new-in "0.60.0" >}}

关于 Hugo 中与默认 Markdown 处理程序相关的设置，请参阅 [Goldmark](#goldmark)。

以下是 Hugo 中所有与标记相关的的配置及其默认设置：

{{< code-toggle config="markup" />}}

**详细信息参阅下面的各部分。**

### Goldmark

[Goldmark](https://github.com/yuin/goldmark/) 从 Hugo 0.60 开始引入，作为 Markdown 的默认解析器。它速度快，符合 [CommonMark](https://spec.commonmark.org/0.29/) 标准，且非常灵活。请注意，Goldmark 与 Blackfriday 的功能集并不相同，相比 Blackfriday 它提供了很多新的功能同时也缺失一些功能，但我们会努力在后续版本中弥补这些差距。

下面是默认配置：

{{< code-toggle config="markup.goldmark" />}}

关于上述配置中 extensions 部分的详细信息，请参阅 [Goldmark 文档的这一部分](https://github.com/yuin/goldmark/#built-in-extensions)。

部分设置的解释：

unsafe
: 默认情况下，Goldmark 不会渲染原始 HTML 和潜在危险的链接。如果您有很多内联 HTML 和/或 JavaScript，可能需要开启 `unsafe` 设置。

typographer
: 这个扩展使用像 [smartypants](https://daringfireball.net/projects/smartypants/) 这样的排版实体来代替标点符号。

attribute
: 通过在一对大括号中添加属性列表（`{.myclass class="class1 class2" }`）并将其放在它所修饰的 Markdown 元素之后，来为标题和块启用自定义属性支持。修饰标题时应与标题同一行，修饰块时应在块的下方起新行。

{{< new-in "0.81.0" >}} 在 Hugo 0.81.0 中，支持为表格、列表、段落等 Markdown 块添加属性（如 CSS class）。

如一个带有 CSS class 的引用块：

```md
> foo
> bar
{.myclass}
```

目前还存在一些限制：对于表格，目前只能将其应用于整个表格（不能单独对某行或某单元格使用）；对于列表，仅适用于 `ul`/`ol` 节点，例如——

```md
* Fruit
  * Apple
  * Orange
  * Banana
  {.fruits}
* Dairy
  * Milk
  * Cheese
  {.dairies}
{.list}
```
请注意，对 [code fences]({{< relref "/content-management/syntax-highlighting#highlighting-in-code-fences" >}}) （代码块）使用时，属性必须在开始标记之后，和其它高亮处理指令放在一起，例如——

````
```go {.myclass linenos=table,hl_lines=[8,"15-17"],linenostart=199}
// ... code
```
````

autoHeadingIDType ("github") {{< new-in "0.62.2" >}}
: 用于创建自动 ID（锚点名称）的策略。可用的类型有 `github`、`github-ascii` 和 `blackfriday`。`github` 生成与 GitHub 兼容的 ID；`github-ascii` 在重音规范化后删除非 ASCII 字符；`blackfriday` 使用像 [Blackfriday](#blackfriday) 一样的 ID 策略，Blackfriday 是 Hugo 0.60 之前的默认 Markdown 引擎。请注意，如果使用 Goldmark 作为默认 Markdown 引擎，该选项的配置也将作为[锚定]({{< relref "/functions/anchorize" >}})模板函数中使用的策略。

### Blackfriday

[Blackfriday](https://github.com/russross/blackfriday) 是 Hugo 的默认 Markdown 渲染引擎，现在被 Goldmark 取代。但您仍然可以使用它：只需在顶级 `markup` 配置中将 `defaultMarkdownHandler` 设置为 `blackfriday`。

下面是默认配置：

{{< code-toggle config="markup.blackFriday" />}}

### Highlight（高亮）

下面是默认的 `highlight` 配置。其中一些选项可以在具体的代码块中单独设置，请参阅[语法高亮]({{< relref "/content-management/syntax-highlighting" >}})。

{{< code-toggle config="markup.highlight" />}}

关于 `style` 配置项，可以参阅这些样式库演示：

* [Short snippets](https://xyproto.github.io/splash/docs/all.html)
* [Long snippets](https://xyproto.github.io/splash/docs/longer/all.html)

对于 CSS，请参阅[生成语法高亮 CSS]({{< relref "/content-management/syntax-highlighting#generate-syntax-highlighter-css" >}})。

### 目录 {id="table-of-contents"}

{{< code-toggle config="markup.tableOfContents" />}}

这些设置仅适用于 Goldmark 渲染器：

startLevel
: 标题级别，取值从 1（`h1`）开始，设置从哪个级别开始渲染目录。

endLevel
: 停止目录渲染的标题级别（包含）。

ordered
: 是否生成有序列表而不是无序列表。

## Markdown 渲染钩子 {id="markdown-render-hooks"}

参阅 [Markdown 渲染钩子]({{< relref "/templates/render-hooks" >}})。

