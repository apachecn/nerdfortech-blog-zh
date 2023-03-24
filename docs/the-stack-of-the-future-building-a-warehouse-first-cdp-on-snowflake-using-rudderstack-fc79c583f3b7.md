# 未来的堆栈:使用方向舵堆栈在雪花上构建仓库优先的 CDP

> 原文：<https://medium.com/nerd-for-tech/the-stack-of-the-future-building-a-warehouse-first-cdp-on-snowflake-using-rudderstack-fc79c583f3b7?source=collection_archive---------21----------------------->

![](img/794e4731489b8ebb29b08f1444ebb4ec.png)

未来的客户数据堆栈使仓库成为一等公民，利用高级存储/计算功能和附加工具将仓库转变为客户数据平台(CDP)。

我们将从定义当前解决方案的问题开始，解释构建 CDP 的仓库优先方法，解释为什么雪花是理想的仓库，最后看一个数据工程团队使用 RudderStack 和雪花处理大规模客户数据需求的真实示例。

# 什么是仓库优先 CDP？

# 当前的 CDP 造成了数据孤岛，并阻碍了对客户的统一认识。

CDP 的想法很简单:收集您所有的客户数据，这样您就可以统一了解客户的旅程和他们的完整资料，然后根据这些数据采取行动。来自这种组合数据的洞察力对于整个组织的团队来说越来越具有强制性。

从您的数据工程师的角度来看，收集您的所有客户数据是一个难以解决的、不断发展的问题，这是您希望 CDP 帮助您完成的事情。但是激活您的客户数据的需求通常来自您组织中的其他团队，如营销和产品团队，这促使大多数 CDP 产品专注于激活组件，*而不是*数据收集。

因此，尽管大多数 CDP 都有某种程度的统一数据功能，但您的数据工程师面临着一些主要问题。首先，第三方 CDP 通常会产生额外的数据孤岛。从客户细分到客户旅程接触点，数据存在于不同的系统中(例如，营销部门有侧重于激活的 CDP，产品部门有客户旅程工具，等等。)，而且是‘困’在每一个。其次，由于数据孤岛问题，CDP 很少能够访问存储在安全系统中的有价值的内部数据。

最终，这些挑战会限制您的数据工程师为您的公司提供尽可能丰富的客户数据的能力。

# 仓库优先 CDP 为您的客户构建一个统一的视图，供整个公司使用。

对您的 CDP 采用的**仓库优先**方法将*仓库*置于您客户数据基础架构的中心，为您提供完全的所有权、对内部数据的访问以及最大的灵活性。它使您的数据工程师能够收集和提供最丰富、最全面的客户数据集，为整个组织的团队和工具系统提供信息。

传统上，客户数据堆栈中仓库的主要功能是作为存储来自各种来源的信息的容器。换句话说，您使用仓库作为数据的存储库，主要用于存储和分析用例。

然而，像雪花这样的现代仓库使团队能够构建远远超出数据存储的有趣用例。例如， [Panther](https://runpanther.io/) 使您能够将雪花用作健壮的安全数据湖。通过 [Tapad](https://www.tapad.com/) ，一个跨设备的广告和内容交付平台，你可以将你的第三方数据直接传输到雪花进行浓缩。 [MessageGears](https://messagegears.com/products/integrations/snowflake/) 是一个客户营销平台，运行在一个品牌的雪花客户数据环境之上，提供高级客户数据细分和激活功能。

RudderStack 使全面的数据收集和路由到雪花对您的数据工程师来说很容易，也使他们能够将相同的数据发送到下游团队和工具进行激活。(不久，RudderStack 将把 Snowflake 变成 Stack 其余部分的数据源……更多信息请见下文。)

总之，这些工具使您的数据工程团队能够构建和管理完全自主、高度可扩展且极其灵活的 CDP。

# 雪花是您的 CDP 的理想数据仓库

有多个数据仓库提供者可供选择，那么如果您正在构建一个仓库优先的 CDP，为什么雪花是理想的选择呢？

首先，Snowflake 将存储、计算和服务的费用分开，但将它们原生集成在一起。对于 CDP 使用情形，对您的存储和计算分别收费可以大大降低总体成本，因为您的数据工程师可以仔细管理来自不同团队的查询需求，而不必同时考虑全面存储的成本。此外，当涉及到您的事件数据时，有效负载总是包含时间元素。尽管公司通常存储所有历史数据，但团队通常使用更新的数据。将您的存储和计算费用分开，可以显著优化处理最新数据的使用情形，因为您可以只加载您需要的数据，而不会牺牲长期存储。

第二，雪花提供了高级功能，如支持半结构化数据和通过变量数据类型对对象进行列存储，这对数据工程师管理和交付客户数据非常有用。

然而，除了 CDP 细节之外，作为数据工程师，我们想列举一些我们喜欢雪花的事情(因为它也有 CDP 之外的用途):

*   **安全性** —雪花完全符合 GDPR 和 CCPA 标准，并对传输中的数据和静态数据使用先进的加密和数据屏蔽技术。他们的平台内数据治理也很棒。
*   **速度和性能** —能够使用近乎无限的专用计算资源来支持并发数据查询，而不会对速度产生任何影响，这对于像我们这样以极高规模运行的团队来说确实非常有价值。
*   **易于管理** — Snowflake 非常易于管理——我们在基础设施方面花费的时间越少，我们就越能专注于我们的产品。

# 方向舵堆栈+雪花—构建 CDP 的完美数据堆栈

一旦雪花就位，构建您的仓库优先 CDP 需要一个额外的工具层来进行全面的收集和激活。这就是方向舵堆栈的作用。

# 全面的数据收集，为您的客户提供统一的视图

RudderStack 为事件数据提供了 11 个 SDK，为结构化数据和事件数据提供了云资源，以及 HTTP-as-a-source，这意味着您可以用来自所有网站、应用程序甚至内部数据源的客户数据填充雪花。RudderStack 的收集和路由功能为您的数据工程师提供了一个完全托管的管道。

除了直接在 Snowflake 中查询见解之外，全面的数据集还支持在 Snowflake 之上的工具中以任何复杂程度进行分析和 BI 用例。

# 为下游团队和工具激活数据

除了让数据工程师更容易收集数据之外，RudderStack 还简化了 CDP 的激活功能，这需要将数据整合到公司营销、产品和其他团队使用的工具中。RudderStack 通过两种主要方式实现这一点:

## 云、流媒体和 webhook 目的地

通过 RudderStack 流向雪花的每一条数据也可以路由到任何数量的其他云目的地、流媒体服务或 webhooks。

云目的地通常包括 Indicative、AppsFlyer 或 Google Analytics 等分析工具，而 Kafka 和 Kinesis 等流媒体服务可以从 RudderStack 实时获取。许多目的地可以选择在云或设备模式下运行。在设备模式下，RudderStack 在包装器中加载原生 SDK，并将事件的副本直接发送到目的地——这极大地简化了脸书和谷歌广告脚本之类的用例。

对于数据工程师来说，这意味着他们可以通过向每个团队提供一致、高质量的客户数据，让每个团队满意，每个团队都可以使用自己选择的工具，而不会产生额外的数据孤岛。

## 即将推出:仓库即资源

几个月后，我们将推出一些功能，将雪花变成您客户数据堆栈的数据源*。请继续关注公告。*

**在我们的* [*分步指南*](https://rudderstack.com/blog/step-by-step-guide-how-to-set-up-a-warehouse-first-cdp-on-snowflake-using-rudderstack/) *中，学习如何使用方向舵堆栈在雪花上构建 CDP。**

# *案例研究:Mattermost*

*Mattermost 在其客户数据堆栈中实施了 RudderStack 和 Snowflake，以获得 CDP 功能。他们的核心挑战是构建一个完整的客户旅程图，并将该数据提供给产品团队。*

*Mattermost 的首席数据工程师 Alex Dovenmule 将他们的基础架构迁移到了雪花，因为他希望通过分离存储和计算资源来降低成本。通过这一过程，他意识到通过数据收集和激活基础架构实现同样的目标既可以降低成本，又可以增加重要的功能——他们以前的供应商存储了所有的客户数据，这造成了额外的孤岛并推高了成本。*

*因为有了雪花，付钱给第三方在孤岛中存储数据的副本没有意义，所以 Alex 实施了 RudderStack 来分离数据收集和存储功能(RudderStack 不持久存储任何数据，所以我们不必为存储空间付费)，从而实现了完全拥有的无孤岛客户数据管道。*

*有了收集和存储所有客户数据的能力，Mattermost 第一次能够看到他们的整个客户旅程。正如 Alex 所描述的，运行 RudderStack 和 Snowflake“彻底改变了产品经理计划、发布和衡量功能采用的方式。”*

*在我们的[技术案例研究](https://rudderstack.com/blog/mattermosts-data-stack-explained-how-they-leverage-unlimited-data-for-customer-analytics/)中，了解 Mattermost 如何使用 Rudderstack 和 Snowflake 构建他们的仓库优先 CDP，以及在我们的[业务案例研究](https://resources.rudderstack.com/case-studies/mattermost-roi)中，了解他们公司的团队如何受益于他们新统一的客户数据集。*

# *今天就从方向舵堆栈和雪花开始*

*RudderStack 和 Snowflake 相结合，为您提供完美的 CDP 数据平台。[注册 RudderStack Cloud 的 14 天免费试用](https://app.rudderstack.com/signup?type=freetrial)，构建仓库优先的 CDP，为您的营销和分析堆栈的每个部分提供完整、统一的数据。*

# *今天试试方向舵堆栈*

*开始构建更智能的客户数据管道。使用你所有的客户数据。回答更难的问题。向您的整个客户数据堆栈发送见解。今天就报名参加 [RudderStack Cloud Free](https://app.rudderlabs.com/signup?type=freetrial) 。*

*加入我们的 [Slack](https://resources.rudderstack.com/join-rudderstack-slack) 与我们的团队聊天，查看我们在 [GitHub](https://github.com/rudderlabs) 上的开源报告，订阅[我们的博客](https://rudderstack.com/blog/)，在社交上关注我们: [Twitter](https://twitter.com/RudderStack) ， [LinkedIn](https://www.linkedin.com/company/rudderlabs/) ， [dev.to](https://dev.to/rudderstack) ， [Medium](https://rudderstack.medium.com/) ， [YouTube](https://www.youtube.com/channel/UCgV-B77bV_-LOmKYHw8jvBw) 。不要错过任何更新。今天就订阅我们的博客吧！*

**原载于【https://rudderstack.com】[](https://rudderstack.com/blog/the-stack-of-the-future-building-a-warehouse-first-cdp-on-snowflake-using-rudderstack)**。****