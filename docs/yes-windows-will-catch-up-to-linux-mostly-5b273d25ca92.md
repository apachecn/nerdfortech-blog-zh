# 是的，Windows 将在很大程度上赶上 Linux。

> 原文：<https://medium.com/nerd-for-tech/yes-windows-will-catch-up-to-linux-mostly-5b273d25ca92?source=collection_archive---------11----------------------->

![](img/d5621e87d15614bee555c76621189ec1.png)

彼得罗·马蒂亚在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

您在哪个平台上开发将取决于项目需求。但在大多数情况下，大多数开发人员可能会告诉你，Linux 在开发或创造力方面远远优于 Windows(有些人甚至会提出超越这两者的观点)。Linux 更快，更安全，维护隐私，更新更快。

Linux 更安全，因为它是开源的，不像当前的 Windows。贡献者捕捉漏洞的速度比黑客攻击发行版的速度更快。

Linux 比 Windows 运行得更快，因为与 Windows 不同，文件系统更有组织性。对于大多数发行版来说，它也更加轻量级，而 Windows 则更加臃肿。基本上，与 Linux 相比，Windows 通常有更多不必要的程序在后台运行。

在某种程度上，你可以说 Linux 的 CLI 工具也更好，但你不能再这样说了，因为现在你可以在命令提示符/PowerShell 中使用 Bash。喘息！

那么……关于上面的那一点，你为什么这样命名呢？？自 2018 年以来，微软比以往任何时候都更加接受开源的理念。为了大力支持这个想法，微软发布了 wsl 1(Linux 的 Windows 子系统)。这第一次让开发人员能够在 Windows 中安装和使用 bash。现在，你仍然不能使用许多你能在真正的 Linux 环境中使用的 o '工具，但是这是一个开始。哦，我提到过 WSL 也是开源的吗？

今年，Windows 发布了 WSL2，这是对 WSL1 的全面革新。与“伪造”linux 环境的 WSL1 不同，WSL2 使用由微软开发和维护的完整 Linux 内核。这个内核使用在 kernel.org 的[找到的基础内核。它也像它的前辈一样完全开源。](http://kernel.org)

WSL2 带来的另一个巨大优势是执行系统调用的能力。系统调用是 Linux 程序与内核交互的方式，它基本上是一个 API。这在 WSL1 中是不可行的，我认为部分原因是它没有使用完整的内核(如果有内核的话)。有了这个能力，我们现在可以使用很多在 Linux 中习以为常的 bash 工具，比如 Node、Flask、Docker 等等。

除非微软再来一次 180 度大转弯，大幅缩减他们的计划，否则在下一个十年，我们将看到 Windows 基本上 Linux 化。这让我很兴奋，因为我是一个 Windows 用户……嗯，从来没有。我确实喜欢 Linux，但出于几个显而易见的原因，我永远不会 100%放弃 Windows。一个是游戏，另一个是游戏。此外，加密挖掘在 Windows 上设置起来更容易一些，还有一些其他的 Windows 软件必须具备。

WSL 为 Windows 完全采用开源软件打开了大门，我预测在接下来的十年内，WSL 将完全包围 Windows 的核心(而不是取代它)。这意味着您可以同时使用 Windows 和 Linux 软件，而无需运行 VM 实例。但这是否意味着 Windows 将开源？部分原因。我不认为我们会看到 Windows 100%免费。显然，我们可以免费使用 WSL，而且 WSL 可能会支持很多程序。

我不认为我们真的会看到像 Linux 发行版一样开发不同风格的 Windows，但是我们会看到不同类型的 Linux 子系统，它们会模仿更流行的发行版甚至一些全新发行版的行为。

现在，这并不意味着 Windows 将超越 Linux。我提到的在当前状态下使用 Linux 优于 Windows 的另一个优势是隐私。我想现在我们大多数人都意识到微软从用户那里收集数据。现在对我来说，这并不真正困扰我，但对许多人来说。在未来的某个时刻，你可以说 Windows 可以和 Linux 在性能和安全性方面并驾齐驱，在 Linux 社区考虑在他们现有的发行版上采用 Windows 之前，MSFT 将不得不停止收集数据并清除其数据库中收集的所有数据。

但尽管如此，我认为重要的是认识到 MSFT 采用 Linux 和开源软件的方向，以及它将来可能登陆 Windows 的方向，特别是如果你从事操作系统开发的话。