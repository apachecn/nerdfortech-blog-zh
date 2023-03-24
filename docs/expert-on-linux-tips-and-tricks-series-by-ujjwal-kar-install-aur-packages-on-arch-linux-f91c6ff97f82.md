# Linux 专家，提示和技巧系列|在 Arch Linux 上手动和使用 Aurman 安装 AUR 软件包。

> 原文：<https://medium.com/nerd-for-tech/expert-on-linux-tips-and-tricks-series-by-ujjwal-kar-install-aur-packages-on-arch-linux-f91c6ff97f82?source=collection_archive---------12----------------------->

## Arch Linux 没有内置 aurman。我们必须手动安装。在本文中，我们将首先手动安装 aurman，然后使用 aurman 安装一个包。

AUR (Arch User Repository)是一个面向 Arch Linux 用户的社区驱动的资源库，包含 [PKGBUILDs](https://wiki.archlinux.org/title/PKGBUILD) (包描述)，允许你用 [makepkg](https://wiki.archlinux.org/title/Makepkg) 从源代码编译一个包，然后通过 [pacman](https://wiki.archlinux.org/title/Pacman#Additional_commands) 安装。

# 如何手动安装任何软件包(例如 aurman)？

## 步骤 1:安装这些软件包

确保安装了 base-devel 软件包组。还应该安装 git 来下载包。安装 setuptools 需要 python-pip

> sudo pacman-S base-devel git python-pip

## 步骤 2:安装 setuptools

使用 pip 安装 setuptools。

> pip 安装设置工具

## 第三步:下载 AUR 软件包

从 [AUR 网络界面](https://aur.archlinux.org)搜索并下载 [PKGBUILDs](https://wiki.archlinux.org/title/PKGBUILD)

> git 克隆[https://aur.archlinux.org/aurman.git](https://aur.archlinux.org/aurman.git)

## 第四步:安装奥曼

*   将目录更改为 aurman(下载的包)

> cd 奥曼

这个`PKGBUILD`可以用 *makepkg* 构建成可安装包，然后用 *pacman* 安装。

> makepkg -si — skippgpcheck

最后奥曼安装了。

## 步骤 5:运行以下命令

> 奥曼-休

搞定了。！！

# 如何使用 Aurman 安装包？

## *步骤 1:使用名称或描述搜索包？

假设我需要 nodejs。

> 奥曼-Ss 节点 js

你会发现许多包，检查他们的描述。

## *步骤 2:安装软件包

> 奥曼-S 节点

# 类似帖子

[](https://ujjwalkar.netlify.app/post/expert-on-linux-tips-and-tricks/) [## Linux 专家，提示和技巧| Ujjwal Kar

### 嗨，我是 Ujjwal Kar，在各种基于 linux 内核的操作系统上工作了很长时间，比如 Ubuntu，Ubuntu Linux Mint…

ujjwalkar.netlify.app](https://ujjwalkar.netlify.app/post/expert-on-linux-tips-and-tricks/)