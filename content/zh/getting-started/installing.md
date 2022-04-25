---
title: 安装 Hugo
linktitle: 安装 Hugo
description: 在 macOS、Windows、Linux、OpenBSD、FreeBSD 以及任何可以运行 Go 编译器工具链的机器上安装 Hugo。
date: 2016-11-01
publishdate: 2016-11-01
lastmod: 2018-01-02
categories: [getting started,fundamentals]
authors: ["Michael Henderson"]
keywords: [install,pc,windows,linux,macos,binary,tarball]
menu:
  docs:
    parent: "getting-started"
    weight: 30
weight: 30
sections_weight: 30
draft: false
aliases: [/tutorials/installing-on-windows/,/tutorials/installing-on-mac/,/overview/installing/,/getting-started/install,/install/]
toc: true
---

**原文：[Install Hugo](https://gohugo.io/getting-started/installing/)**

{{% note %}}
有很多关于“Hugo 是用 Go 语言编写的讨论”，但您不需要安装 Go 就可以使用 Hugo。只需要获取一个预编译的二进制文件！
{{% /note %}}

Hugo 是用 [Go](https://golang.org/) 编写的，支持多个平台。最新版本可以在 [Hugo Releases][releases] 找到。

Hugo 目前为以下系统提供预编译的二进制文件：

- 适用于 x64、i386 和 ARM 架构的 macOS（Darwin）。
- Windows
- Linux
- OpenBSD
- FreeBSD

此外，在任何可以运行 Go 工具链的地方，都可以通过 Hugo 源码来编译二进制文件，例如 DragonFly BSD、OpenBSD、Plan 9、Salaris 等其它操作系统。请参阅 <https://golang.org/doc/install/source> 来了解 Go 语言支持的目标操作系统和编译架构的完整组合。


## 快速安装

### 二进制（跨平台）

从 [Hugo Releases][releases] 下载适合您平台的版本。下载后，二进制文件可以从任何地方运行，无需将其安装到全局位置，这适用于没有特权帐户的系统，如共享主机。

理想情况下，您应该将其安装在 `PATH` 中的某个位置以便于使用，如 `/usr/local/bin`。

### Docker

Hugo 目前不提供官方 Docker 镜像，但推荐这些最新的发行版：https://hub.docker.com/r/klakegg/hugo/。


### Homebrew (macOS)

如果您在 macOS 上使用 [Homebrew][brew]，则可以使用以下命令安装 Hugo：

{{< code file="install-with-homebrew.sh" >}}
brew install hugo
{{< /code >}}

更详细的说明请阅读下面的在 macOS 和 Windows 上的安装指南。

### MacPorts (macOS)

如果您在 macOS 上使用 [MacPorts][macports]，则可以使用以下命令安装 Hugo：

{{< code file="install-with-macports.sh" >}}
port install hugo
{{< /code >}}

### Homebrew (Linux)

如果您在 Linux 上使用 [Homebrew][linuxbrew]，则可以使用以下命令安装 Hugo：

{{< code file="install-with-linuxbrew.sh" >}}
brew install hugo
{{< /code >}}

Linux 上的 Homebrew 安装指南可以在[这里][linuxbrew]找到。

### Chocolatey (Windows)

如果您在 Windows 上使用 [Chocolatey][] 进行软件包管理，则可以使用以下命令安装 Hugo：

{{< code file="install-with-chocolatey.ps1" >}}
choco install hugo -confirm
{{< /code >}}

或者，如果您需要包含 Sass/SCSS 的扩展版本：

{{< code file="install-extended-with-chocolatey.ps1" >}}
choco install hugo-extended -confirm
{{< /code >}}

### Scoop (Windows)

如果您在 Windows 上使用 [Scoop][] 进行包管理，则可以使用以下命令安装 Hugo：

```bash
scoop install hugo
```

或者使用以下命令安装扩展版本：

```bash
scoop install hugo-extended
```

### 源码

#### 依赖工具

- [Git][installgit]
- [Go（至少 1.11 版）](https://golang.org/dl/)

#### 从 GitHub 获取源码

从 Hugo 0.48 开始，Hugo 使用 Go 1.11 中内置的 Go Modules 支持来构建。最简单的入门方法是在 GOPATH 之外的目录中克隆 Hugo，如下例所示：

{{< code file="from-gh.sh" >}}
mkdir $HOME/src
cd $HOME/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
go install --tags extended
{{< /code >}}

`--tags extended` 标志表示支持 Sass/SCSS 的扩展版本，如果您不需要，可以删除该标志。

{{% note %}}
如果您是 Windows 用户，请将环境变量 `$HOME` 替换为 `%USERPROFILE%`。
{{% /note %}}

## macOS

### 假设

1. 您知道如何打开 macOS 终端。
2. 您使用的是现代 64位 Mac。
3. 您将 `~/Sites` 作为站点的起点（`~/Sites` 用于示例目的，如果您对命令行和文件系统足够熟悉，那么按照说明操作应该没有问题）。

### 选择适合您的方法

在 Mac 上安装 Hugo 的三种方法：

1. 包管理器，如 [Homebrew][brew]（`brew`）或 [MacPorts][macports]（`port`）。
2. 分发包/压缩包（如 tarball）。
3. 从源码构建。

在 Mac 上安装 Hugo 没有“最佳”方式，您应该使用最适合您的方法。

### 利弊

上述每种方法各有利弊：

1. **包管理器**：使用包管理器是最简单的安装方法，所需的维护工作最少，没有明显的缺点。默认将使用最新的版本，因此在下一个版本发布之前不会修复 bug（除非您在 Homebrew 中使用 `--HEAD` 选项安装它）。版本发布可能会滞后几天，因为需要与其它团队协调。尽管如此，如果您想在稳定、官方使用的源上工作，这是推荐的安装方法。包管理器运行良好，易于更新。

2. **Tarball**：从 tarball 下载和安装也很容易，尽管它比 Homebrew 需要更多的命令行。更新也很简单：您只需要使用新的二进制文件重复安装过程。这使您可以灵活地在计算机上拥有多个版本。如果您不想使用 `brew`，那么 tarball/binary 是一个不错的选择。

3. **从源码构建**：从源码构建所需要的工作量最多。其优点是您不必等待版本发布就能使用新功能或修复 bug。缺点是您需要花费更多的时间来管理设置。

{{% note %}}
从源码构建对于经验丰富的用户更有吸引力，本指南将更多地关注通过 Homebrew 和 Tarball 安装 Hugo。
{{% /note %}}

### 使用 Brew 安装 Hugo

{{< youtube WvhCGlLcrF8 >}}

#### 第 1 步：安装 `brew`（如果您还没有安装）

访问 `brew` 网站 <https://brew.sh/>，按照那里的说明进行操作。最重要的一步是从命令行安装：

{{< code file="install-brew.sh" >}}
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{{< /code >}}

#### 第 2 部：运行 `brew` 命令安装 `hugo`

使用 `brew` 安装 Hugo 非常简单，如下所示：

{{< code file="install-brew.sh" >}}
brew install hugo
{{< /code >}}

如果 Homebrew 工作正常，应该会看到类似于下面的内容：

```
==> Downloading https://homebrew.bintray.com/bottles/hugo-0.21.sierra.bottle.tar.gz
######################################################################### 100.0%
==> Pouring hugo-0.21.sierra.bottle.tar.gz
🍺  /usr/local/Cellar/hugo/0.21: 32 files, 17.4MB
```

{{% note "Installing the Latest Hugo with Brew" %}}
如果您想要保持最新的开发版本，请将 `brew install hugo` 替换为 `brew install hugo --HEAD`。
{{% /note %}}

通常 `brew` 应该会更新您的环境变量路径以包含 Hugo，您可以打开一个新的终端窗口并运行一些命令来确认：

```
$ # 显示 hugo 可执行文件的路径
which hugo
/usr/local/bin/hugo

# 显示安装的版本
ls -l $( which hugo )
lrwxr-xr-x  1 mdhender admin  30 Mar 28 22:19 /usr/local/bin/hugo -> ../Cellar/hugo/0.13_1/bin/hugo

# 验证 hugo 是否正确运行
hugo version
Hugo Static Site Generator v0.13 BuildDate: 2015-03-09T21:34:47-05:00
```

### 用 Tarball（压缩包）安装 Hugo

#### 第 1 步：确定位置

从 tarball 安装时，您必须决定是将二进制文件安装在 `/usr/local/bin` 中还是您的主目录中。对于这点人们通常有三个看法：

1. 在 `/usr/local/bin` 中安装以便系统上的所有用户都可以使用。这是一个相当标准的可执行文件的安装位置。缺点是您可能需要提升权限才能将软件放入该位置。此外，如果您的系统上有多个用户，他们都将运行相同的版本。如果您想尝试新版本，这有时可能会是一个问题。

2. 在 `~/bin` 中安装，只有您（当前用户）可以执行它。安装在这个位置易于执行和维护，不需要提升权限。缺点是只有您（当前用户）可以运行 Hugo。如果您的系统上还有其他用户，他们必须维护自己的副本，这可能导致他们运行不同的版本。当然这样使您可以更轻松地尝试不同的版本。

3. 将其安装在您的站点目录（`Sites`）中。如果您只有一个正在构建的站点，这是一个不错的选择，它把所有东西都放在一个地方。如果想尝试新版本，可以复制整个站点目录并更新其中的 Hugo 可执行文件。

上述三个选择都适用，为了简洁起见，本指南重点介绍选项 #2。

#### 第 2 步：下载 Tarball

1. 在浏览器中打开 <https://github.com/gohugoio/hugo/releases>。

2. 查找标有“Latest Release”（最新版本）的绿色标签来查找当前最新版本。

3. 下载适用于 Mac 的 tarball。名称类似于 `hugo_X.Y_osx-64bit.tgz`，其中 `X.Y` 表示版本号。

4. 默认情况下，tarball 将保存到您的 `~/Downloads` 目录中。如果选择其它位置，那么下面步骤中的目录也要更改。

#### 第 3 步：确认下载的文件

验证下载过程中 tarball 没有损坏：

```
tar tvf ~/Downloads/hugo_X.Y_osx-64bit.tgz
-rwxrwxrwx  0 0      0           0 Feb 22 04:02 hugo_X.Y_osx-64bit/hugo_X.Y_osx-64bit.tgz
-rwxrwxrwx  0 0      0           0 Feb 22 03:24 hugo_X.Y_osx-64bit/README.md
-rwxrwxrwx  0 0      0           0 Jan 30 18:48 hugo_X.Y_osx-64bit/LICENSE.md
```

`.md` 文件是 Hugo 的文档，另外一个（如 `.tgz`）则是可执行文件。

#### 第 4 步：安装到 `bin` 目录中

```
# 创建一个目录（如果没有）
mkdir -p ~/bin

# 切换到该目录
cd ~/bin

# 解压 tarball
tar -xvzf ~/Downloads/hugo_X.Y_osx-64bit.tgz
Archive:  hugo_X.Y_osx-64bit.tgz
  x ./
  x ./hugo
  x ./LICENSE.md
  x ./README.md

# 验证是否正确运行
./hugo version
Hugo Static Site Generator v0.13 BuildDate: 2015-02-22T04:02:30-06:00
```

您可能需要将 bin 目录添加到 `PATH` 环境变量中。使用 `which` 命令将检查是否可以找到 `hogo` 并打印它的完整路径，如果找不到则不会打印任何内容。

```
# 检查 hugu 是否在环境变量 PATH 中
which hugo
/Users/USERNAME/bin/hugo
```

如果 `hugo` 不在您的 `PATH`：

1. 确定您的默认 shell（zsh 或 bash）：

   ```
   echo $SHELL
   ```

2. 编辑您的个人设置

   如果您的默认 shell 是 zsh：

   ```
   nano ~/.zprofile
   ```

   如果您的默认 shell 是 bash：

   ```
   nano ~/.bash_profile
   ```

3. 插入一行以添加 `$HOME/bin` 到现有的 `PATH`：

   ```
   export PATH=$PATH:$HOME/bin
   ```

4. 按 Control-X，然后按 Y 保存文件。

5. 关闭当前终端并打开一个新的终端以获取更改。再次运行 `which hugo` 命令以检查是否生效。

至此，您已经成功安装 Hugo。

### 在 Mac 上从源码构建

如果想自己编译 Hugo，您需要安装 Go（又名 Golang）。可以[直接通过 Go 官方网站](https://golang.org/dl/)或使用以下命令利用 Homebrew 安装 Go：

```
brew install go
```

#### 第 1 步：获取源码

如果要编译特定版本的 Hugo，请访问 <https://github.com/gohugoio/hugo/releases> 并下载相应版本的源代码。如果要使用绝对最新源码（可能包括 bug）来编译 Hugo，请克隆 Hugo 代码库：


```
git clone https://github.com/gohugoio/hugo
```

{{% warning "Sometimes \"Latest\" = \"Bugs\""%}}
直接克隆 Hugo 代码既有好处也有坏处。使用 Hugo 的前沿版本，容易受到最新功能以及最新 bug 的影响。如果您在最新版本中发现 bug，请在 [GitHub 上创建 issue](https://github.com/gohugoio/hugo/issues/new)，感谢您的反馈。
{{% /warning %}}

#### 第 2 步：编译

将包含源码的目录设为您的工作目录，然后获取 Hugo 的依赖项：

```
mkdir -p src/github.com/gohugoio
ln -sf $(pwd) src/github.com/gohugoio/hugo

go get
```

上述操作将获取依赖项的最新版本。如果 Hugo 构建失败，则可能是依赖项引入了破坏性更改的原因。

一旦正确配置了源码目录，就可以使用以下命令编译 Hugo：

```
go build -o hugo main.go
```

然后将编译得到的 `hugo` 可执行文件放到您的 `$PATH` 中，就可以开始使用 Hugo 了。

## Windows

以下内容旨在成为在 Windows PC 上安装 Hugo 的完整指南。

{{< youtube G7umPCU-8xc >}}

### 假设

1. 您将 `C:\Hugo\Sites` 用作新项目的起点。
2. 您将 `C:\Hugo\bin` 用于存储 Hugo 可执行文件。

### 设置您的目录

您需要一个地方来存储 Hugo 可执行文件、您的[内容][content]以及生成的 Hugo 网站：

1. 打开 Windows 资源管理器。
2. 创建一个新文件夹：`C:\Hugo`（假设您想在 C 驱动器上使用 Hugo，当然也可以放在其它任何地方）。
3. 在 Hugo 文件夹中创建一个子文件夹：`C:\Hugo\bin`。
4. 在 Hugo 中创建另一个子文件夹：`C:\Hugo\Sites`。

### 技术用户

1. 从 [Hugo Releases][releases] 下载最新的 Hugo 可执行文件压缩包。
2. 将所有内容提取到 `..\Hugo\bin` 文件夹中。
3. 在 PowerShell 或您首选的 CLI 中，cd 到 `C:\Hugo\bin`，并使用命令行 `set PATH=%PATH%;C:\Hugo\bin` 将可执行文件添加到环境变量中。重启命令行后如果 `hugo` 命令不起作用，则可能需要以管理员身份运行命令提示符。

### 非技术用户

1. 访问 [Hugo Releases][releases] 页面。
2. 最新版本在顶部公布，滚动到发版公告底部以查看下载。他们都是 ZIP 文件。
3. 找到靠近底部的 Windows 文件（他们按字母顺序排列，因此 Windows 排在最后），下载 32 位或 64 位文件，具体取决于您使用的是 32 位还是 64 位 Windows（如果您不知道，[请看这里](https://esupport.trendmicro.com/en-us/home/pages/technical-support/1038680.aspx)）。
4. 将 ZIP 文件移动到 `C:\Hugo\bin` 文件夹中。
5. 打开 ZIP 文件并提取其内容。确保将内容解压到同一 `C:\Hugo\bin` 文件夹中（Windows 将默认解压到当前目录，除非您明确告诉它解压到其它位置）。
6. 您现在拥有三个新文件：Hugo 可执行文件（`hugo.exe`）、`LICENSE` 和 `README.md`。

现在您需要将 Hugo 添加到您的 Windows 环境变量中：

#### 对于 Windows 10 用户：

- 鼠标右键点击**开始**按钮。
- 单击**系统**。
- 单击单击右侧的**高级系统设置**。
- 单击底部的**环境变量**按钮。
- 在用户变量部分，选中变量为 Path 的行，并单击**编辑**按钮。
- 单击**浏览**按钮并选择 `hugo.exe` 所在的目录。如果您按照上述说明进行操作，这个路径就是 `C:\Hugo\bin`。
- 单击`新建`按钮。
- 输入 `hugo.exe` 解压缩的文件夹路径，如果您按照上述说明进行操作，这个路径就是 `C:\Hugo\bin`。*这个路径应该是 Hugo 所在的文件夹路径（`C:\Hugo\bin`），而不是二进制文件路径（`C:\Hugo\bin\hugo.exe`）。*。
- 在每个窗口单击**确定**退出。

#### 对于 Windows 7 和 8.x 用户：

Windows 7 和 8.1 不包含 Windows 10 中提供的的简易路径编辑器，因此建议这些平台上的非技术用户安装免费的第三方路径编辑器，例如 [Windows Environment Variables Editor]。

### 验证可执行文件

运行一些命令以验证可执行文件是否安装成功，然后构建一个示例站点以开始使用。

#### 1. 打开命令提示符

在命令提示符下，键入 `hugo help` 并按<kbd>回车</kbd>键，您应该可以看到以下开头的输出：

```
hugo is the main command, used to build your Hugo site.

Hugo is a Fast and Flexible Static Site Generator
built with love by spf13 and friends in Go.

Complete documentation is available at https://gohugo.io/.
```

如果这样，那么安装就完成了。否则请仔细检查放置 `hugo.exe` 文件的路径，并确认在环境变量中配置的路径是否正确。如果您仍然没有得到上述预期输出，请搜索 [Hugo 论坛][forum]，看看其他人是否已经解决了我们的问题。如果没有，请在“Support”（支持）类别中添加问题，并描述（包含）您的命令和输出。

在命令提示符下，更改当前工作目录为 `Sites` 目录：

```
C:\Program Files> cd C:\Hugo\Sites
C:\Hugo\Sites>
```

#### 2. 运行命令

运行以下命令以生成新站点，这里使用 `example.com` 作为站点的名称：

```
C:\Hugo\Sites> hugo new site example.com
```

您现在应该得到一个 `C:\Hugo\Sites\example.com` 目录，切换到该目录并列出其中的内容，应该得到类似于以下内容的输出：

```
C:\Hugo\Sites> cd example.com
C:\Hugo\Sites\example.com> dir
Directory of C:\hugo\sites\example.com

04/13/2015  10:44 PM    <DIR>          .
04/13/2015  10:44 PM    <DIR>          ..
04/13/2015  10:44 PM    <DIR>          archetypes
04/13/2015  10:44 PM                83 config.toml
04/13/2015  10:44 PM    <DIR>          content
04/13/2015  10:44 PM    <DIR>          data
04/13/2015  10:44 PM    <DIR>          layouts
04/13/2015  10:44 PM    <DIR>          static
               1 File(s)             83 bytes
               7 Dir(s)   6,273,331,200 bytes free
```

### Windows 安装疑难解答

[@dhersam][] 制作了一个关于常见问题的精彩视频：

{{< youtube c8fJIRNChmU >}}

## Linux

### Snap 软件包

在任何[支持 Snaps 的 Linux 发行版][snaps]中，您都可以使用以下命令安装 Sass/SCSS 扩展版本：


    snap install hugo --channel=extended

若要安装不支持 Sass/SCSS 的非扩展版本：

    snap install hugo

使用 `snap refresh hugo --channel=extended` 或 `snap refresh hugo --channel=stable` 命令可以在两者之间切换。

{{% note %}}
由于 Snap 的限制和安全模型，通过 Snap 安装 Hugo 只能写入用户目录 `$HOME` 以及用户拥有的 gvfs 挂载目录。更多信息可以查看 [GitHub 上的相关 issue](https://github.com/gohugoio/hugo/issues/3143)。
{{% /note %}}

### Debian 和 Ubuntu

[@anthonyfok](https://github.com/anthonyfok) 及 [Debian Go Packaging Team](https://go-team.pages.debian.net/) 上的贡献者维护了一个官方的 Hugo [Debian 软件包](https://packages.debian.org/hugo)，这个软件包在 [Ubuntu](https://packages.ubuntu.com/hugo) 分享，可以通过 `apt-get` 安装：

    sudo apt-get install hugo

其安装的版本取决于您的 Debian/Ubuntu 版本。在 Ubuntu bionic (18.04) 上会安装没有 Sass/SCSS 支持的非扩展版本，而在 Ubuntu disco (19.04) 上则会安装支持 Sass/SCSS 的扩展版本。

不推荐这种安装方式，因为在 Debian 和 Ubuntu 的 Linux 包管理器中的 Hugo 版本通常落后于[此处](https://github.com/gcushen/hugo-academic/issues/703)所述的几个版本。

### Arch Linux

您可以从 Arch Linux [社区](https://www.archlinux.org/packages/community/x86_64/hugo/)仓库安装 Hugo。同时适用于 Arch Linux 的衍生系统，如 Manjaro。

```
sudo pacman -Syu hugo
```

### Fedora，Red Hat 和 CentOS

Fedora 为 Hugo 维护了一个[官方软件包](https://apps.fedoraproject.org/packages/hugo)，可以使用以下命令安装：

    sudo dnf install hugo

对于最新版本，推荐使用由 Fedora Copr 上的 [@daftaupe](https://github.com/daftaupe) 所维护的 Hugo 包：

* <https://copr.fedorainfracloud.org/coprs/daftaupe/hugo/>

请参阅 [Hugo 论坛中的相关讨论][redhatforum]。

### openSUSE Tumbleweed

openSUSE 为 Tumbleweed 滚动发行版维护了一个[官方包](https://software.opensuse.org/package/hugo)，可以用以下命令安装：

````
sudo zypper install hugo
````

### Solus

Solus 的软件包仓库中包含 Hugo，可以使用以下方式安装：

```
sudo eopkg install hugo
```

### OpenBSD

OpenBSD 通过 `pkg_add` 方式提供了一个 Hugo 软件包：

    doas pkg_add hugo

## 升级 Hugo

升级 Hugo 很简单，如下载并替换新的可执行文件到您的放置目录 `PATH`，或使用 Homebrew 更新 `brew upgrade hugo`。

## 下一步

现在您已经安装了 Hugo，请阅读[快速入门指南][quickstart]并浏览文档的其余部分。如果您有任何疑问，请访问 [Hugo 论坛][forum] 直接向 Hugo 社区提问。


[brew]: https://brew.sh/
[macports]: https://www.macports.org/
[Chocolatey]: https://chocolatey.org/
[content]: /content-management/
[@dhersam]: https://github.com/dhersam
[forum]: https://discourse.gohugo.io
[mage]: https://github.com/magefile/mage
[dep]: https://github.com/golang/dep
[highlight shortcode]: /content-management/shortcodes/#highlight
[installgit]: https://git-scm.com/
[installgo]: https://golang.org/dl/
[linuxbrew]: https://docs.brew.sh/Homebrew-on-Linux
[quickstart]: /getting-started/quick-start/
[redhatforum]: https://discourse.gohugo.io/t/solved-fedora-copr-repository-out-of-service/2491
[releases]: https://github.com/gohugoio/hugo/releases
[Scoop]: https://scoop.sh/
[snaps]: https://snapcraft.io/docs/installing-snapd
[windowsarch]: https://esupport.trendmicro.com/en-us/home/pages/technical-support/1038680.aspx
[Windows Environment Variables Editor]: https://eveditor.com/
