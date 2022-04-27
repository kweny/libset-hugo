---
title: Hugo 模块
linktitle: Hugo 模块概览
description: 如何使用 Hugo 模块。
date: 2017-02-01
publishdate: 2017-02-01
menu:
  docs:
    parent: "modules"
    weight: 01
weight: 01
sections_weight: 01
categories: [hugo modules]
keywords: [themes,modules]
draft: false
aliases: [/themes/overview/,/themes/]
toc: true
---

**原文：[Hugo Modules](https://gohugo.io/hugo-modules/)**

Hugo 模块（**Hugo Modules**）是 Hugo 的核心构建块。一个*模块*可以是你的主项目，也可以是一个提供了 **static**、**content**、**layouts**、**data**、**assets**、**i18n**、**archtypes**（Hugo 的 7 种组件类型）中一种或多种类型组件的较小的模块。

你可以任意组合模块，甚至挂载非 Hugo 项目的目录，形成一个大的、虚拟的联合文件系统。

Hugo 模块由 Go Modules 提供支持，有关 Go 模块的更多信息请参阅：

- [https://github.com/golang/go/wiki/Modules](https://github.com/golang/go/wiki/Modules)
- [https://blog.golang.org/using-go-modules](https://blog.golang.org/using-go-modules)

这是一个全新的功能，目前只有几个示例项目：

- [https://github.com/bep/docuapi](https://github.com/bep/docuapi) 是一个在测试此功能时移植到 Hugo Modules 的主题。这时一个很好的将非 Hugo 项目挂载到 Hugo 文件夹结构的示例。它甚至演示了在常规 Go 模板中的 JS Bundler 实现。
- [https://github.com/bep/my-modular-site](https://github.com/bep/my-modular-site) 是一个非常简单的用于测试的站点。
