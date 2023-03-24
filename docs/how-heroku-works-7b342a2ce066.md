# Heroku 是如何工作的

> 原文：<https://medium.com/nerd-for-tech/how-heroku-works-7b342a2ce066?source=collection_archive---------21----------------------->

![](img/bc03167c61f2eee2a5e22ef22402bc84.png)

我开始在空闲时间研究 Heroku，将其作为创建个人应用程序的一个选择。事实上，我将 AWS 生态系统中的一个现有应用程序转换到 Heroku，这是 DZone.com 上的一个系列:

[从 AWS 转移到 Heroku](https://dzone.com/articles/moving-away-from-aws-and-onto-heroku)

从 Heroku 的一个全新想法开始，我也能够快速创建一个基于健身的 SaaS 解决方案，这在 DZone.com 上的另一个系列中有记录:

[使用 Heroku 快速构建多租户 SaaS 产品](https://dzone.com/articles/using-heroku-to-quickly-build-a-multi-tenant-saas)

在我的几个应用程序中使用 Heroku 已经一年多了，我想我应该后退一步，描述 Heroku 是如何工作的，并就为什么 Salesforce 拥有的解决方案应该成为您下一个项目的考虑因素提供一些想法。

# 什么是 Heroku？

Heroku 成立于 2007 年，是一个平台即服务(PaaS)生态系统，目前支持 Ruby、Java、Node.js、Scala、Clojure、Python、PHP 和 Go 编程语言(以及对许多其他语言的社区支持)。因为它能够支持多种语言来完成相同的结果，Heroku 被认为是一个[多语言](https://en.wikipedia.org/wiki/Polyglot_(computing))平台。

在最高层次上，Heroku 旨在满足寻找住处的应用程序的需求。开发人员从创建 Heroku“应用”开始，并通过标准化的 git 存储库引入他们的原始代码。Heroku 只是从这里开始——构建和部署应用程序，然后根据需要提供给用户。这包括静态资源(如 Angular 或 React.js 应用程序)，可以从 Node.js 实现中提供。

Heroku 提供超过 175 种附加服务来补充每个应用程序，包括:

*   数据库服务
*   正常运行时间警报
*   消息服务
*   自动备份
*   搜索
*   韵律学
*   邮件传递

下面是一个我的个人应用程序，它利用了 Heroku 中的数据库、安全性和消息传递功能:

![](img/fc786adee63f2a9d5f7600ce3c65f8fd.png)

Heroku 尽一切可能让用户专注于在他们的应用程序中构建特性和功能。采用这种方法，您可以避免学习、实施、管理和支持 DevOps 驱动的应用程序部署路径所需的时间。

Heroku 自 2010 年以来一直是 Salesforce 公司，是 2.12 亿美元收购的一部分。事实上，Salesforce 首席执行官 Marc Benioff 表示:

> *“*[*有了 Heroku，我们的平台可以成为下一代云计算的基石。”*](https://www.salesforce.com/news/press-releases/2011/01/03/salesforce-com-completes-acquisition-of-heroku/) *Marc Benioff——sales force 董事长兼首席执行官*

# Heroku 的特征

Heroku 不仅仅是一个部署和存放应用程序的地方，它提供了补充应用程序托管和管理的所有必要方面。在这篇文章中，我将关注 Heroku 的四个有趣的方面。

# 1.构建包和 Slugs

Buildpacks 是 Heroku 生态系统内部的机制，它编译应用程序代码并创建“slug”slug 是应用程序的预打包副本(包括任何必要的运行时层),经过优化后可以发布到 dyno 中。为了更好地理解这个概念，请看下图:

![](img/ad059c79f58945e6254cdeebf117b961.png)

作为在 Heroku 中创建新应用程序的一部分，从包含要部署的源代码的存储库中创建一个 git remote。准备就绪后，发出一个标准化的`git push`命令，同时指定目标主机和分支。在上面的示例中，发出`git push heroku master`命令将当前分支推送到“heroku”远程中的“主”分支。

这将触发 Heroku 开始处理请求。第一步是扫描项目，了解 Heroku 配置中是否指定了 buildpack。如果使用上述受支持的语言之一，这一步是不必要的，因为 Heroku 会自动选择正确的构建包。如果您没有使用官方支持的语言，您可以创建一个定制的构建包，或者您可以从 [Elements Marketplace](https://elements.heroku.com/) 的 6000 多个第三方构建包中选择。

然后，buildpack 告诉 Heroku 如何检索运行应用程序所需的所有依赖项和运行时。此操作的结果是创建一个应用程序段。Heroku 然后将这个 slug 部署到一个(或多个)dynos 上，并运行 buildpack 或用户指定的命令来启动应用程序。

# 2.动态、配置变量和版本

在上一节中，我们使用 buildpack 和 slug 在 Heroku 中启动了一个新的应用程序。应用程序本身运行在 Heroku dyno 中，这是 Heroku 平台的核心。Dynos 是基于 Linux 的容器，它们是独立的，旨在根据用户指定的命令执行代码。(Heroku 提出了术语 dyno，因为当时 Docker 还没有提出或推广术语 container。)由于这种设计，Heroku 允许 dynos 按比例放大(或缩小)以满足容量需求。

为了避免在应用程序的源代码中包含敏感的配置信息，Heroku 提供了一种存放配置变量的机制。对于命令行用户，`heroku config`命令提供以下信息:

```
╭─me@mbp ~/new-dyno ‹master›
╰─$ heroku config
=== new-dyno Config Vars
DATABASE_URL:      mysql://someUser:somePassword@someHost.net/someService?reconnect=true
CLIENT_ID:         myAuthClientId
CLIENT_SECRET:     myAuthSecreteId
```

Heroku 仪表盘的“设置”选项卡中有一个基于网络的补充版本:

![](img/32ad93178f8564afa21648bb0ffee753.png)

每当部署代码、更改配置变量或修改附加资源时，Heroku 都会创建一个新版本并重启 dyno 应用程序。这是 Heroku 的一个经常被忽视的特性，因为 Heroku 总是提供轻松回滚到先前状态的能力。

发布历史可以在 Heroku 仪表板中获得，也可以通过`heroku releases`命令行获得:

```
╭─me@mbp ~/new-dyno ‹master›
╰─$ heroku releases
=== new-dyno Releases - Current: v3
v3  Deploy 2b93a215                            me@example.com  2021/03/23 07:49:35 -0400
v2  Set CLIENT_ID, CLIENT_SECRET config vars   me@example.com  2021/03/19 07:26:34 -0400
v1  Deploy 40e37807                            me@example.com  2021/03/19 06:48:00 -0400
```

# 3.Heroku 附件

产品或平台的适应性在很大程度上依赖于库或服务，只需点击鼠标按钮或发出简单的命令行语句即可获得这些库或服务。目前，Heroku 包括超过 175 个附加产品，涵盖超过 24 个不同类别的附加产品。下面是 Heroku Marketplace 的当前截图，在这里可以查看附加组件并立即添加到现有的应用程序中。

![](img/2648055ce18fa93bd259d6bf30d90bfa.png)

大多数 Heroku 附加组件提供免费使用或试用期。这两种选择都允许开发人员在进行长期投资之前更好地了解产品。

就我个人而言，我已经利用免费试用层来比较在同一领域竞争的解决方案。事实上，加载项可以像安装时一样快速删除。

# 4.记录和路由

Heroku 通过自动整理和路由来自应用程序每个部分的日志到一个通道来简化日志记录。这提供了真正全面和可扩展的以应用程序为中心的日志记录。

Heroku 使用 [Logplex](https://devcenter.heroku.com/articles/logplex) 进行分布式日志路由和整理。使用命令行界面，可以发出以下命令来查看当前日志:

```
╭─me@mbp ~/new-dyno ‹master›
╰─$ heroku logs --tail 2021-03-28T20:45:30 app[web.1]: 2021-03-28 20:45:30.020  INFO 4 --- [scheduling-1] c.g.j.f.services.SmsService : sendSmsSummaryMessages() completed
2021-03-28T20:50:00 app[web.1]: 2021-03-28 20:50:00.019  INFO 4 --- [scheduling-1] c.g.j.f.services.SmsService : start date=03/28/2021, time=08:50 PM
2021-03-28T20:50:00 app[web.1]: 2021-03-28 20:50:00.019  INFO 4 --- [scheduling-1] c.g.j.f.services.SmsService : end date=03/30/2021, time=02:50 AM
```

利用附加组件的功能，可以向应用程序引入 Coralogix 日志记录、Logentries、LogDNA、Papertrail 和 Sumo 逻辑等工具，以便提供全功能的逻辑分析和调试。

# 谁应该(和不应该)使用 Heroku？

在多个选项争夺客户的情况下，下表说明了 Heroku 是(和不是)一个选项的情况:

![](img/dbd653ea42edf8744e2d92d78362fcfc.png)

根据我个人对 30 年来 IT 行业的分析，我可以肯定地说，Heroku 提供了必要的解决方案集，可以满足目前运行的 80%以上的应用程序的需求。唯一的限制因素是执行必要的分析和规划，以利用 Heroku 提供的服务。

一旦 Heroku 成为目的地，每个应用程序都将开始受益于可以应用于未来特性和功能的时间。

# 结论

如果我要为任何 IT 专业人士起草一份简明的使命陈述，那会非常简单:

> *“将您的时间集中在提供扩展您知识产权价值的特性/功能上。将框架、产品和服务用于其他一切。”*

基本上，如果你的工作是构建一个应用程序来帮助小部件做一些事情，那么就投入你的时间和精力来最大化地赋予小部件竞争优势。避免花费时间去做框架、产品或服务已经提供的事情——要知道他们可能总是比你能提供更好的解决方案——因为这就是他们存在的原因。

在将 Heroku 用于我的应用程序之前，我发现自己花费了不必要的时间试图找出 Heroku 已经为我做了(并且做得非常好)的方面。这种努力带来了一个严重的后果，那就是我无法以正确的方式分配有限的时间。现在，一年过去了，我对坚持上面简单的使命所取得的成果感到惊讶。

从我能找到的最新信息来看，我的应用程序是目前在 Heroku 上运行的 900 多万个应用程序的一部分——显然是下一代云计算的一部分。

祝你今天过得愉快！