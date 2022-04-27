---
title: 配置 Hugo
linktitle: 配置 Hugo
description: 如何配置您的 Hogo 站点。
date: 2013-07-01
publishdate: 2017-01-02
lastmod: 2017-03-05
categories: [getting started,fundamentals]
keywords: [configuration,toml,yaml,json]
menu:
  docs:
    parent: "getting-started"
    weight: 60
weight: 60
sections_weight: 60
draft: false
aliases: [/overview/source-directory/,/overview/configuration/]
toc: true
---

**原文：[Configure Hugo](https://gohugo.io/getting-started/configuration/)**

## 配置文件

Hugo 使用站点根目录中的 `config.toml`、`config.yaml`、或 `config.json` 作为默认的站点配置文件。

可以使用命令行 `--config` 指定一个或多个配置文件来该覆盖默认值，如：

Examples:

```
hugo --config debugconfig.toml
hugo --config a.toml,b.toml,c.toml
```

{{% note %}}
`--config` 指定多个配置文件时，以逗号分隔。
{{% /note %}}

{{< todo >}}TODO: distinct config.toml and others (the root object files){{< /todo >}}

## 配置目录

除了使用单个配置文件之外，还可以使用 `configDir` 指定配置目录（默认 `config/`）来组织配置文件或多环境特定配置。

- 目录中每个文件代表一个配置根对象。例如 `params.toml` 配置 `[Params]`、`menu(s).toml` 配置 `[Menu]`、`languages.toml` 配置 `[Languages]` 等。

- 每个文件的内容必须是顶级的，如

  {{< code-toggle file="config" >}}
  [Params]
    foo = "bar"
  {{< /code-toggle >}}

  {{< code-toggle file="params" >}}
  foo = "bar"
  {{< /code-toggle >}}

- 每个目录包含一组文件，其中包含环境特有的设置。

- 文件可以本地化为特定语言。


```
├── config
│   ├── _default
│   │   ├── config.toml
│   │   ├── languages.toml
│   │   ├── menus.en.toml
│   │   ├── menus.zh.toml
│   │   └── params.toml
│   ├── production
│   │   ├── config.toml
│   │   └── params.toml
│   └── staging
│       ├── config.toml
│       └── params.toml
```

基于上面的结构，运行 `hugo --environment staging` 时，Hugo 将会使用 `config/_default` 中的所有配置，并在此基础上再合并 `config/staging` 中的配置。


{{% note %}}
默认情况下，`hugo server` 命令使用 __development__ 环境，`hugo` 命令使用 __production__ 环境。
{{%/ note %}}

## 从主题合并配置

{{< new-in "0.84.0" >}} 在 Hugo 0.84.0 版本中对配置的合并功能进行了改进，最大的变化/改进是，现在默认情况下可以对主题中的 `params` 选项进行深度合并。

`_marge` 选项的值可以是以下之一：

none
: 不合并。

shallow
: 浅合并，值使用新添加的键值。

deep
: 深度合并，使用新添加的键值，并且合并已有的键值。

{{% translator-note %}}
举例来说，假设你使用的主题中有一个配置：

```toml
[params]
[params.colours]
blue = "#337DFF"
green = "#68FF33"
red = "#FF3358"
```

在之前版本的 Hugo，要调整其中一个配置，如 `red` 的颜色，则需要复制整个 `[params.colours]` 下的配置，然后把 `red` 修改为你想要的值。

现在当开启 `deep` 的情况下，只需要设置想变更的选项，其它的选项会沿用主题中的值：

```toml
[params]
[params.colours]
red = "#fc0f03"
```
{{% /translator-note %}}

以下是当前默认的合并策略（在您的站点中不需要进行如下冗长的配置，只需要配置您需要修改的选项即可，未配置的将使用默认值）：

{{< code-toggle config="mergeStrategy" skipHeader=true />}}

## 所有配置设置 {id="all-configuration-settings"}

以下是 Hugo 定义的配置变量的完整列表，用户可以在站点配置文件中覆盖默认值。

### archetypeDir 

**默认值：** "archetypes"

Hugo 会到这个目录中查找原型文件（内容模板）。{{% module-mounts-note %}}

### assetDir

**默认值：** "assets"

Hugo 到这个目录查找供 [Hugo Pipes]({{< relref "/hugo-pipes" >}}) 使用的资产文件。{{% module-mounts-note %}}

### baseURL

站点根主机名（和路径），例如 https://bep.is/。

### blackfriday
参阅 [配置 Blackfriday]({{< relref "/configuration-markup#blackfriday" >}})

### build

参阅 [构建的配置](#configure-build)

### buildDrafts (false)

**默认值：** false

构建时是否包含草稿内容。

### buildExpired

**默认值：** false

构建时是否包含过期内容。

### buildFuture

**默认值：** false

构建时是否包含未来发布日期的内容。

### caches
参阅 [配置文件缓存](#configure-file-caches)

### cascade

{{< new-in "0.86.0" >}}

将默认配置值（前言设定 front matter）传递到内容树中的页面。站点配置中的选项和前言设定中的选项相同，请参阅 [Front Matter Cascade]({{< relref "/content-management/front-matter#front-matter-cascade" >}})。

### canonifyURLs

**默认值：** false

是否将相对 URL 转换为绝对 URL。

### contentDir

**默认值：** "content"

Hugo 从中读取内容文件的目录。{{% module-mounts-note %}}

### copyright

**默认值：** ""

站点的版权说明，通常显示在页脚。

### dataDir

**默认值：** "data"

Hugo 从中读取数据文件的目录。{{% module-mounts-note %}}

### defaultContentLanguage

**默认值：** "en"

内容的默认语言，若内容没有明确指定语言，则使用该默认语言。

### defaultContentLanguageInSubdir

**默认值：** false

是否在子目录中渲染默认语言的内容，例如默认语言的内容放在 `content/en`，然后将站点根目录 `/` 重定向到 `/en/`。

### disableAliases

**默认值：** false

是否禁用别名重定向的生成。注意，即使设置了 `disableAliases=true`，别名本身也会保留在页面上。这样做的动机是能够使用自定义格式在 `.htaccess` 或 Netlify 的 `_redirects` 之类的文件中生成 301 重定向。

### disableHugoGeneratorInject

**默认值：** false

默认情况下，Hugo 会在主页（仅主页）的 HTML head 中注入一个名为 generator 的 mata 标签。您可以将其关闭，但如果您保持开启，我们将不胜感激，因为这是观察 Hugo 人气上升的好方法。

### disableKinds

**默认值：** []

禁用指定类型的所有页面。此列表中允许的值：`"page"`、`"home"`、`"section"`、`"taxonomy"`、`"term"`、`"RSS"`、`"sitemap"`、`"robotsTXT"`、`"404"`。

### disableLiveReload

**默认值：** false

禁用浏览器窗口的自动实时重载。

### disablePathToLower

**默认值：** false

禁止将 url/path 转换为小写。

### enableEmoji

**默认值：** false

为页面内容启用 Emoji 表情支持，请参阅 [Emoji 表情符号备忘单](https://www.webpagefx.com/tools/emoji-cheat-sheet/)。

### enableGitInfo

**默认值：** false

为每个页面启用 `.GitInfo` 对象（如果 Hugo 站点使用 Git 进行版本控制）。启用后将使用内容文件的最后 git 提交日期更新每个页面的 `Lastmod` 参数。

### enableInlineShortcodes

**默认值：** false

启用行内短代码（shortcode）支持。请参阅[行内短代码]({{< relref "/templates/shortcode-templates#inline-shortcodes" >}})。

### enableMissingTranslationPlaceholders

**默认值：** false

如果缺少翻译，则显示占位符而不是默认值或空字符串。

### enableRobotsTXT

**默认值：** false

启用 `robots.txt` 文件的生成。

### frontmatter

见[配置 Front Matter](#configure-front-matter)。

### googleAnalytics

**默认值：**  ""

Google Analytics 的跟踪 ID。

### hasCJKLanguage

**默认值：** false

如果为 true，则自动检测内容中的中文/日文/韩文。这将使 `.Summery` 和 `.WordCount` 可以正确处理 CJK 语言。

### imaging

参阅[图像处理配置]({{< relref "/content-management/image-processing#imaging-configuration" >}})。

### languageCode

**默认值：**  ""

[RFC 5646](https://datatracker.ietf.org/doc/html/rfc5646) 定义的语言标记。内部 [RSS 模板](https://github.com/gohugoio/hugo/blob/master/tpl/tplimpl/embedded/templates/_default/rss.xml)使用此值填充其 `<language>` 元素。 该值未在其他地方使用。

### languages

参阅[配置语言]({{< relref "/content-management/multilingual#configure-languages" >}})。

### disableLanguages

参阅[禁用语言]({{< relref "/content-management/multilingual#disable-a-language" >}})。

### markup
参阅[配置标记]({{< relref "/getting-started/configuration-markup" >}})。{{< new-in "0.60.0" >}}

### mediaTypes
参阅[配置媒体类型]({{< relref "/templates/output-formats#media-types" >}})。

### menus

参阅[向菜单添加非内容条目]({{< relref "/content-management/menus#add-non-content-entries-to-a-menu" >}})。

### minify

参阅[配置 Minify](#configure-minify)。

### module

模块配置参阅[模块配置]({{< relref "/hugo-modules/configuration" >}})。{{< new-in "0.56.0" >}}

### newContentEditor

**默认值：** ""

创建新内容时使用的编辑器。

### noChmod

**默认值：** false

不要同步文件的权限模式。

### noTimes

**默认值：** false

不要同步文件的修改时间。

### outputFormats

参阅[配置输出格式](#configure-additional-output-formats).

### paginate

**默认值：** 10

[分页]({{< relref "/templates/pagination" >}})中每页默认的元素数量。

### paginatePath

**默认值：** "page"

分页时使用的路径元素(https://example.com/page/2)。

### permalinks

参阅[内容管理]({{< relref "/content-management/urls#permalinks" >}})。

### pluralizeListTitles

**默认值：** true

将列表中的标题复数化。

### publishDir

**默认值：** "public"

Hugo 将最终生成的站点静态文件（HTML 文件等）写入到该目录。

### related

参阅[关联内容]({{< relref "/content-management/related#configure-related-content" >}})。{{< new-in "0.27" >}}

### relativeURLs 

**默认值：** false

启用此选项可使所有相对 URL 相对于内容根。注意，这不会影响绝对 URL。

### refLinksErrorLevel

**默认值：** "ERROR"

当使用 `ref` 或 `relref` 解析页面链接但无法解析时，将使用此日志级别进行日志记录。有效值为 `ERROR`（默认）或 `WARNING`。任何 `ERROR` 都将导致构建失败（`exit -1`）。

### refLinksNotFoundURL

当在 `ref` 或 `relref` 中找不到页面引用时用作占位符的 URL。按原样使用。

### removePathAccents

**默认值：** false

从内容路径中的 [Composite Characters](https://en.wikipedia.org/wiki/Precomposed_character)（预组字符）中删除 [Non-spacing 标记](https://www.compart.com/en/unicode/category/Mn)。

```text
content/post/hügó.md --> https://example.org/post/hugo/
```

### rssLimit

**默认值：** -1 (不限)

RSS feed 中的最大项目数。

### sectionPagesMenu

参阅["懒人 section 菜单"]({{< relref "/templates/menu-templates#section-menu-for-lazy-bloggers" >}})。

### security

参阅[安全策略]({{< relref "/about/security-model#security-policy" >}})。

### sitemap

默认[站点地图配置]({{< relref "/templates/sitemap-template#configuration" >}})。

### summaryLength

**默认值：** 70

在 [.Summary]({{< relref "/content-management/summaries#hugo-defined-automatic-summary-splitting" >}})（摘要）中显示的文本长度。

### taxonomies

参阅[配置分类]({{< relref "/content-management/taxonomies#configure-taxonomies" >}})。

### theme

关于如何引入一个主题参阅[模块配置][Module Config]({{< relref "/hugo-modules/configuration#module-config-imports" >}})。

### themesDir

**默认值：** "themes"

Hugo 从中读取主题的目录。

### timeout 

**默认值：** "30s"

生成页面内容的超时时间，可以设置为 [duration](https://pkg.go.dev/time#Duration) 或者毫秒数。注意：超时设置是为了避免存在递归内容而无限等待。如果您的页面生成速度很慢（如需要大型图像处理或依赖远程内容），您可能需要将此值设得大一些。

### timeZone 

{{< new-in "0.87.0" >}}

时区（或位置），例如 `Europe/Oslo`，在解析前言设定中的日期时或者在[时间函数]({{< relref "/functions/time" >}})中，若没有提供时区信息，将以此作为默认值。有效值可能取决于操作系统，但应该会包括 `UTC`、`LOCAL` 以及 [IANA 时区数据库](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) 中的位置。

### title

站点标题。

### titleCaseStyle

**默认值：** "AP"

参阅[配置标题大小写样式](#configure-title-case)。

### uglyURLs

启用后，URL 的样式为 `/filename.html` 而不是 `/filename/`。

### watch

**默认值：** false

监听文件系统的变化并根据需要重新创建。

{{% note %}}
如果你在 \*nix 机器上开发你的站点，这里有一个从命令行查找配置选项的便捷方式：

```
cd ~/sites/yourhugosite
hugo config | grep emoji
```

显示输出如下：

```
enableemoji: true
```
{{% /note %}}

## 配置构建选项 {id="configure-build"}

{{< new-in "0.66.0" >}}

`build` 配置部分包含全局构建相关的配置选项。

{{< code-toggle file="config">}}
[build]
useResourceCacheWhen="fallback"
writeStats = false
noJSConfigInAssets = false
{{< /code-toggle >}}


useResourceCacheWhen
: 设置什么时候将 `/resources/_gen` 中缓存的资源用于 PostCSS 和 ToCSS。有效值为 `never`、`always` 和 `fallback`。最后一个值表示如果 PostCSS/extended 版本不可用，将尝试缓存。

writeStats {{< new-in "0.69.0" >}}
: 启用后，将在项目根目录写入一个名为 `hugo_satas.json` 的文件，其中包含一些有关构建的聚合数据，例如已发布的用于进行 [CSS pruning]({{< relref "/hugo-pipes/postprocess#css-purging-with-postcss" >}}) 的 HTML 实体列表。如果只想将其用于生产的构建，则应考虑将其放在 [config/production]({{< relref "/getting-started/configuration#configuration-directory" >}}) 中。值得一提的是，由于部分服务器构建的性质，当在服务器运行时添加或修改 HTML 实体时，都将添加新的 HTML 实体而不会删除旧的值，直到重启服务器或者运行 `hugo` 构建。

注意：该选项主要用于清除未使用的 CSS，主要考虑构建速度，可能存在误报（例如元素不是真正的 HTML 元素）。

noJSConfigInAssets {{< new-in "0.78.0" >}}
: Hugo 构建时可以在 `/assets` 文件夹中写入一个 `jsconfig.json` 文件，其中包含通过运行 [js.Build]({{< relref "/hugo-pipes/js" >}}) 得到的导入映射。此文件旨在帮助在 [VS Code](https://code.visualstudio.com/) 等代码编辑器中进行智能提示。使用 `noJSConfigInAssets` 选项可以将该功能关闭。注意，如果您不使用 `js.Build`，则不会写入任何文件。


## 配置服务器 {id="configure-server"}

{{< new-in "0.67.0" >}}

这部分仅适用于运行 `hugo server` 时，允许在开发过程中设置 HTTP 标头，可以用来测试内容安全策略等。配置格式和 [Netlify](https://docs.netlify.com/routing/headers/#syntax-for-the-netlify-configuration-file) 匹配，具有更强大一些的 [Glob 模式匹配](https://github.com/gobwas/glob)：

{{< code-toggle file="config">}}
[server]
[[server.headers]]
for = "/**"

[server.headers.values]
X-Frame-Options = "DENY"
X-XSS-Protection = "1; mode=block"
X-Content-Type-Options = "nosniff"
Referrer-Policy = "strict-origin-when-cross-origin"
Content-Security-Policy = "script-src localhost:1313"
{{< /code-toggle >}}

由于该配置仅在开发时有意义，因此通常配置于 `development` 环境：


{{< code-toggle file="config/development/server">}}
[[headers]]
for = "/**"

[headers.values]
X-Frame-Options = "DENY"
X-XSS-Protection = "1; mode=block"
X-Content-Type-Options = "nosniff"
Referrer-Policy = "strict-origin-when-cross-origin"
Content-Security-Policy = "script-src localhost:1313"
{{< /code-toggle >}}


{{< new-in "0.72.0" >}}

你还可以为服务器指定简单的重定向规则，语法类似于 Netlify。

注意，状态码 200 将触发 [URL 重写](https://docs.netlify.com/routing/redirects/rewrites-proxies/)，通常在 SPA（单页应用）中会想这样做，如：

{{< code-toggle file="config/development/server">}}
[[redirects]]
from = "/myspa/**"
to = "/myspa/"
status = 200
force = false
{{< /code-toggle >}}

{{< new-in 0.76.0 >}} 当设置 `force=true` 时，即使路径中存在内容也仍然会进行重定向。注意，在 Hugo 0.76 之前的版本 force 选项默认为 true，而现在则与 Netlify 的做法保持一致（目前 Netlify 默认为 false）。

## 配置标题大小写样式 {id="configure-title-case"}

设置 `titleCaseStyle` 选项以指定[标题]({{< relref "/functions/title" >}})模板功能和自动章节标题所使用的大小写样式。默认使用 [AP Stylebook](https://www.apstylebook.com/)（美联社风格），但你也可以将其设置为 `Chicago` 或者 `Go`（每个单词都以大写字母开头）。

# 配置环境变量 {id="configuration-environment-variables"}

HUGO_NUMWORKERMULTIPLIER
: 可以设置 Hugo 并行处理中使用的工作线程数。如果未设置，将使用逻辑 CPU 的数量。

## 配置查找顺序 {id="configuration-lookup-order"}

与模板[查找顺序][lookup order]类似，Hugo 有一组默认规则，用于在网站源代码目录的根目录中搜索配置文件：

1. `./config.toml`
2. `./config.yaml`
3. `./config.json`

在配置文件中，可以指示 Hugo 如何呈现网站，控制网站菜单，并定义站点参数。

## 示例配置 {id="example-configuration"}

下面是一个 Hugo 站点配置文件的典型示例。嵌套在 `params:` 下的值将填充 [`.Site.Params`][] 变量以在[模板][templates]中使用：

{{< code-toggle file="config">}}
baseURL: "https://yoursite.example.com/"
title: "My Hugo Site"
permalinks:
  posts: /:year/:month/:title/
params:
  Subtitle: "Hugo is Absurdly Fast!"
  AuthorName: "Jon Doe"
  GitHubUser: "spf13"
  ListOfFoo:
    - "foo1"
    - "foo2"
  SidebarRecentLimit: 5
{{< /code-toggle >}}

## 使用环境变量进行配置 {id="configure-with-environment-variables"}

除了已经提到的 3 个配置选项之外，还有一些可以通过操作系统环境变量定义的键值。

例如，以下命令可以在类 Unix 系统上设置网站的标题：

```
$ env HUGO_TITLE="Some Title" hugo
```

如果你是使用诸如 Netlify 之类的服务器来部署您的站点，这将非常有用。可以查看 Hugo 文档 [Netlify 配置文件](https://github.com/gohugoio/hugoDocs/blob/master/netlify.toml)的例子。


{{% note "Setting Environment Variables" %}}
环境变量名称必须以 `HUGO_` 为前缀，并且整个名称必须为大写。

要设置 params 选项，则以 `HUGO_PARAMS_` 开头。
{{% /note %}}

{{< new-in 0.79.0 >}} 如果您使用的是 snake_cased（蛇型）变量名，则上述方法将不起作用，因此从 0.79.0 决定以 `HUGO` 之后的第一个字符作为分隔符，这意味着您可以使用任何[允许](https://stackoverflow.com/questions/2821043/allowed-characters-in-linux-environment-variable-names#:~:text=So%20names%20may%20contain%20any,not%20begin%20with%20a%20digit.)的分隔符——如 `HUGOxPARAMSxAPI_KEY=abcdefgh` 的形式（定义了一个名为 API_KEY 的 params 选项，值为 abcdefgh）——来定义环境变量。

{{< todo >}}
Test and document setting params via JSON env var.
{{< /todo >}}

## 渲染时忽略内容和数据文件 {id="ignore-content-and-data-files-when-rendering"}

要在呈现站点时从 `content` 和 `data` 目录中排除特定文件，可以将 `ignoreFiles` 选项设置为一个或多个匹配文件路径的正则表达式。

忽略以 `.foo` 或 `.boo` 结尾的文件：

{{< code-toggle copy="false" >}}
ignoreFiles = ['\.foo$', '\.boo$']
{{< /code-toggle >}}

使用文件的绝对路径忽略一个文件：

{{< code-toggle copy="false" >}}
ignoreFiles = ['^/home/user/project/content/test\.md$']
{{< /code-toggle >}}

## 配置 Front Matter（前言设定，文档头） {id="configure-front-matter"}

### 配置日期

日期在 Hugo 中很重要，可以在 `config.toml` 中添加 `frontmatter` 部分来配置 Hugo 如何为内容页面分配日期。

默认配置为：

{{< code-toggle file="config" >}}
[frontmatter]
date = ["date", "publishDate", "lastmod"]
lastmod = [":git", "lastmod", "date", "publishDate"]
publishDate = ["publishDate", "date"]
expiryDate = ["expiryDate"]
{{< /code-toggle >}}

如果您的某些内容中有非标准的日期参数，则可以覆盖 `date` 设置，如：

{{< code-toggle file="config" >}}
[frontmatter]
date = ["myDate", ":default"]
{{< /code-toggle >}}

`:default` 是默认设置的快捷方式。如在上面的配置中，当文档的 Front Matter 中存在 `myDate` 字段时会将 `.Date` 设置为 `myDate` 字段所指定的日期值，否则将查找 `date`、`publishDate`、`lastmod` 字段并选择第一个有效的日期值。

在上面的配置列表中，以 “:” 开头的值是具有特殊含义的日期处理程序（见下文）。其它的只是 Front Matter 中日期参数的名称（不区分大小写）。另外需要注意的是，Hugo 有一些内置别名：`lastmod` => `modified`、`publishDate` => `pubdate`、`published` 和 `expireDate` => `unpublishdate`。举个例子，在 Front Matter 中使用 `pubDate` 来指定日期，默认情况下将分配给 `.PublishDate`。

特殊日期处理程序：

`:fileModTime`
: 从内容文件的上次修改时间戳中获取日期。

一个示例：

{{< code-toggle file="config" >}}
[frontmatter]
lastmod = ["lastmod", ":fileModTime", ":default"]
{{< /code-toggle >}}

上面的配置中，将首先提取 Front Matter 参数 `lastmod` 的日期值来设置 `.Lastmod`，然后是内容文件的修改时间戳，最后一个的 `:default` 在这里其实是不需要的，但如果配置了则会在 `:git`、`date` 和 `publishDate` 中查找有效日期。

`:filename`
: 从内容文件的文件名中获取日期。例如，文件名 `2018-02-22-mypage.md` 将提取日期 `2018-02-22`。此外如果未设置 `slug`，则文件名中的 `mypge` 将用作 `.Slug` 的值。

一个示例：

{{< code-toggle file="config" >}}
[frontmatter]
date  = [":filename", ":default"]
{{< /code-toggle >}}

上面的例子中，将首先尝试从文件名中提取 `.Date` 的值，若没有则查找 Front Matter 中的 `date`、`publishDate` 和 `lastmod` 字段。

`:git`
: 获取 Git 作者对内容文件最后一次修订的日期，只有当构建时使用了 `--enableGitInfo` 或在站点配置中设置了 `enableGitInof = true` 时有效。


## 配置附加输出格式 {id="configure-additional-output-formats"}

Hugo v0.20 开始可以将内容呈现为多种输出格式（如 JSON、AMP html 或 CSV）。请参阅[输出格式][Output Formats]。

## 配置 Minify {id="configure-minify"}

{{< new-in "0.68.0" >}}

默认配置：

{{< code-toggle config="minify" />}}

## 配置文件缓存 {id="configure-file-caches"}

从 Hugo 0.52 开始，您可以配置的不仅仅是 `cacheDir`。下面是默认配置：

{{< code-toggle >}}
[caches]
[caches.getjson]
dir = ":cacheDir/:project"
maxAge = -1
[caches.getcsv]
dir = ":cacheDir/:project"
maxAge = -1
[caches.getresource]
dir = ":cacheDir/:project"
maxAge = -1
[caches.images]
dir = ":resourceDir/_gen"
maxAge = -1
[caches.assets]
dir = ":resourceDir/_gen"
maxAge = -1
[caches.modules]
dir = ":cacheDir/modules"
maxAge = -1
{{< /code-toggle >}}

您可以在自己的 `config.toml` 中覆盖上面任意的缓存设置。

### 关键字解释

`:cacheDir`
: `:cacheDir` 提取配置选项 `cacheDir` 的值（也可以通过操作系统环境变量 `HUGO_CACHEDIR` 设置）。如果提取不到 `cacheDir` 的值，将 fallback 到 Netlify 上的 `/opt/build/cache/hugo_cache/` 或其它操作系统临时目录下的 `hugo_cache` 目录。这意味着，如果在 Netlify 上执行构建，则使用 `:cacheDir` 配置的所有缓存都将在下一次构建时保存和恢复。对于其它 CI 供应商，请阅读他们的文档。有关 CircleCI 示例，请参阅[此配置](https://github.com/bep/hugo-sass-test/blob/6c3960a8f4b90e8938228688bc49bdcdd6b2d99e/.circleci/config.yml)。


`:project`
: 当前 Hugo 项目的基础目录名称。默认情况下，每个项目都有独立的文件缓存，这意味着当执行 `hugo --gc` 时，将不会影响到同一台 PC 上运行的其它 Hugo 项目的相关文件。

`:resourceDir`
: 配置选项 `resourceDir` 的值。

maxAge
: 缓存条目被剔除之前的持续时间。-1 表示永久，0 表示关闭特定缓存。`maxAge` 使用 Go 的 `time.Duration` 类型，因此有效格式如 `10s`（10 秒）、`10m`（10 分）、`10h`（10 小时）。

dir
: 存储该缓存文件的绝对路径。允许的起始占位符是 `:cacheDir` 和 `:resourceDir`（见上文）。

## 配置文件的格式规范 {id="configuration-format-specs"}

* [TOML 规范][toml]
* [YAML 规范][yaml]
* [JSON 规范][json]

[`.Site.Params`]: {{< relref "/variables/site" >}}
[directory structure]: {{< relref "/directory-structure" >}}
[json]: https://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf "Specification for JSON, JavaScript Object Notation"
[lookup order]: {{< relref "/templates/lookup-order" >}}
[Output Formats]: {{< relref "/templates/output-formats" >}}
[templates]: {{< relref "/templates" >}}
[toml]: https://github.com/toml-lang/toml
[yaml]: https://yaml.org/spec/
[static-files]: {{< relref "/content-management/static-files" >}}
