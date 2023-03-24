# 詹金斯攻击框架

> 原文：<https://medium.com/nerd-for-tech/jenkins-attack-framework-40b7ce628e78?source=collection_archive---------12----------------------->

在我以前的雇主(FusionX/Accenture)，我为 RedTeam 安全人员编写了一个工具，以帮助攻击和利用 Jenkins 应用程序中的漏洞。这些攻击在很大程度上是众所周知的，但是手动执行非常耗时。

埃森哲慷慨地同意开源这个项目，我很高兴能与全世界分享它。除了该工具简化和支持的“bog 标准”攻击之外，它还为公众带来了一些新的攻击。这些攻击包括执行“幽灵作业”的能力，这些作业不会在 Jenkins 中显示为正在执行，并且可以无限期运行。这些作业允许具有“创建作业”权限的攻击者有效地获得 Jenkins 服务器上的持久性。结合其他工具，这种能力可用于通过 Jenkins 服务器进行数据透视。

该工具的另一个特性是能够以明文形式转储所有共享凭据，即使攻击者只有“创建作业”权限而没有管理员访问权限。最后一个独特的功能允许具有管理访问权限的攻击者为任意用户创建 API 令牌(不可能通过 Jenkins 菜单)。

这些以及更常见的攻击都在这个项目的自述文件中列出，可以从这里获得:[https://github.com/Accenture/jenkins-attack-framework](https://github.com/Accenture/jenkins-attack-framework)。埃森哲允许我访问该存储库并帮助维护它，并且鼓励 PRs 的新功能。这个工具被称为“框架”,因为它的设计使得添加额外的特性(插件)变得相当容易。

此外，请查看埃森哲的公告博客，了解该工具的更深入概述:[https://www . Accenture . com/us-en/blogs/cyber-defense/red-teaming-Jenkins-attack-framework](https://www.accenture.com/us-en/blogs/cyber-defense/red-teaming-jenkins-attack-framework)