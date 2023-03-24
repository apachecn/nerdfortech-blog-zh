# 在舵栈喂狗:跟踪计划第 1 部分

> 原文：<https://medium.com/nerd-for-tech/dogfooding-at-rudderstack-tracking-plans-part-1-f84d61daada0?source=collection_archive---------8----------------------->

![](img/4d64fd8b90b3a238107f78db2f5a76ab.png)

# 什么是跟踪计划，为什么需要跟踪计划？

在 RudderStack，我们经常谈论拥有自己的数据的重要性，以及通过完整的数据构建强大的分析可以带来的竞争优势。数据信任是这种结构的基础。为了信任数据，您必须信任提供该数据的工具。这就是为什么我们建立了我们的数据治理 API 和新的跟踪计划功能，我们正准备测试。

方向舵堆栈跟踪计划是我们的数据治理 API 中的最新产品，也是迄今为止我们请求最多的功能之一。与允许您在运行中转换数据的 RudderStack 转换不同，跟踪计划允许您在一开始就计划和规定数据应该是什么样子。跟踪计划解决了与流数据相关的三个基本问题:

1.  **缺失或配置不当的数据会破坏下游 SaaS 应用程序和数据仓库**。这会导致自动化活动执行不佳和仪表板损坏等问题。
2.  **糟糕的命名和重复的事件和属性。**这造成了下游工具和数据仓库的混乱和错误映射。
3.  **上游数据提供者做出改变，导致事件流改变。**这导致了上面的 1 和 2，很少或没有提前通知或修复它的能力。

# 跟踪计划如何解决这些问题

RudderStack 跟踪计划允许您为每个跟踪、分组和识别调用定义特定的事件名称和属性。此外，您可以分配与每个特性或属性关联的数据类型，并指定该特性或属性是否是必需的。跟踪计划 API 还支持版本控制，以便更好地控制数据流。

有了跟踪计划后，您可以使用现有的数据治理 API 来评估入站事件、有效负载样本和元数据，以便与您的计划进行比较。你也可以使用我们随追踪计划一起发布的 RudderTyper 工具。[ruder typer](https://docs.rudderstack.com/data-governance/rudder-typer)是一个工具，用于根据您发布的跟踪计划规范生成强类型的[ruder stack](https://rudderstack.com/)分析库包装器，这意味着您的数据在捕获后将符合您定义的模式。

# 未来会怎样？

这正是我们需要你帮助的地方。我们目前正在与一些 Alpha 客户合作，并在我们自己的 RudderStack 生产实例中使用跟踪计划。我们在路线图上的是关于我们想要跟踪什么类型的错误或模式违规，以及如何处理它们的决定。虽然不是一成不变的，但这里有一个到目前为止我们正在思考的秘密:

违规类型描述操作发生意外事件未定义架构的事件。未计划的属性事件已定义，但属性或特性在架构中不存在匹配的数据类型特定属性的数据类型与架构中定义的不匹配必需的字段缺少负载缺少架构中所需的属性集

一旦完全构建了该功能，对这些违规中的每一个采取的动作可以包括以下一个或多个:

1.  拒绝整个有效载荷
2.  接受整个有效载荷，并通过警告标志将其发送到下游目的地
3.  将整个有效负载重新路由到 S3 桶(也称为“死信队列”)
4.  从有效负载中移除模式中未定义的附加属性
5.  为架构中缺少的必需字段插入默认值
6.  基于模式比较的更多高级选项如下:[https://ajv.js.org/options.html](https://ajv.js.org/options.html)

# 那么，你从哪里开始呢？

如果您有兴趣参与，请通过 [Slack](https://rudderstack.com/join-rudderstack-slack-community) 与我们联系，或者给我们发一封[电子邮件](mailto:katie@rudderstack.com)。

同时，让我们看看典型的 SaaS 企业是如何使用 RudderStack 设计和实现跟踪计划的。作为 RudderStack 数据治理 API 的一部分，跟踪计划首先是通过代码来管理的，但是我们知道设计计划需要开发人员和非开发人员的共同努力，所以我们设计了一个跟踪计划模板 Google Sheet 来帮助团队起步。

第一步是获得一份 RudderStack 跟踪计划模板，该模板将很快推出。这将帮助您和您的团队组织您想要从每个 RudderStack 源中捕获的各种事件和字段。该表单要求您的帐户拥有用户访问令牌。有关如何创建用户令牌的帮助，请查看我们的[访问令牌用户文档](https://docs.rudderstack.com/adding-a-new-user-transformation-in-rudderstack/api-access-token)。

下一步是创建您认为可能需要的事件和属性的愿望列表。这第一步的目标不是创建一个最终的列表，而是主要查看不同利益相关者之间的数据需求交集，并开始为您的公司构建数据架构。在本练习中，从销售和营销漏斗或执行摘要报告等现有的高级范例开始可能会有所帮助，因为它们的基本指标通常已经达成一致。从您已经知道需要度量的内容开始是一个很好的方法，可以开始研究如何度量它，更具体地说，数据首先来自哪里，以及将度量什么属性或特性(例如，必需的键和数据类型)。

例如，让我们以一家 SaaS 企业为例，它有一个衡量以下各项的漏斗:

阶段团队独特的网站访问者营销—付费数字销售—参与 Sales 营销—参与机会/免费试用销售—外包产品激活/POC 销售—销售工程师客户销售—咖啡饮料产品使用客户成功

现在，我们已经定义了每个阶段，让我们更深入地研究需要创建和跟踪哪些数据元素来重现我们的漏斗并分配数据源。需要注意的是，在某些情况下，例如定义营销合格销售线索(MQL)，可能有多个信息来源有助于确定任何一个特定销售线索，但在此表中，我们定义了保留该信息的系统，因此，如果我们需要执行审计，Salesforce(在此示例中)是我们确认该特定销售线索是否标记为 MQL 的系统。由于我们正在定义每个指标，我们将把它分配到我们的谷歌表上的跟踪计划。

漏斗步骤来源度量跟踪计划访问营销网站和应用程序独特的匿名 id 页面视图(营销)页面视图(应用程序)潜在客户营销网站和应用程序每个域独特的电子邮件地址计数通知提交(营销)应用程序注册(应用程序)mqlsalesforce 已检查 MQL 的 Salesforce 潜在客户计数(未删除)N/A (SFDC 云提取)机会/免费试用 sales force Opp Type = initial n/A(SFDC 云提取)产品激活应用程序用户是否已创建连接已创建客户销售机会=关闭赢得机会产品

我们的一些指标将来自数据仓库中的[ruder stack Cloud Extract](https://rudderstack.com/product/cloud-extract)源或其他非 ruder stack 表，因此不会在我们的事件数据跟踪计划中定义。

# 制定跟踪计划

在上面的漏斗图中，我们定义了六个不同的事件和三个我们想要建立的不同跟踪计划。这并不意味着定义了你的跟踪计划的全部，但足以让你开始使用这些工具。

RudderStack 源(跟踪计划)用户操作名称 RudderStack 事件名称市场营销网站页面视图页面 _ 视图市场营销网站表单 Submitform _ submitApplicationPage 页面视图页面 _ 视图应用程序应用程序 signup app _ signup application connection created connection _ createdSalesforce web hook * Opportunity Wonopp _ won

**通常情况下，Salesforce 和其他 SaaS 工具会每 24 小时使用 RudderStack Cloud Extract 提取一次数据，但是像将机会标记为成功这样的关键事件非常重要，足以触发通过 Webhook 源传回的实时事件。*

定义了源和事件之后，我们现在需要确定每个事件的属性和属性类型。这些现在应该添加到跟踪计划谷歌表。每个源都应该有自己的从“导入模板”复制的选项卡。下面的选项卡是我们创建的营销站点选项卡的副本。

事件名称描述属性名称属性类型属性描述请求' d page _ view 用户访问 UTM 参数的页面 link_sourcestringValue 定义为？link _ source = { value } of form _ submit user 提交一个表单 page _ titlestringTitle of the pager page _ urlstringgurl of the pager form _ idstring 表单的 ID(在 Sanity 中配置)RlabelstringLabel for Google Analytics events(如果需要)ocategorystring category for Google Analytics events(如果需要)Outm _ sourcestringOptional 可选 utm 参数 soutm _ mediumstring 可选 utm 参数 soutm _ campaignstring 可选 utm 参数 soutm _ contentstring 可选 utm 参数 _ textstringThe 可选 utm

创建了营销网站源计划的基础后，我们现在可以通过在 Google 表单中配置附加设置将它上传到 RudderStack(当我们发布该功能时会有更多相关信息)。

跟踪计划 Google Sheet 的一个令人兴奋的部分是，你可以从 RudderStack 跟踪计划 API 下载最新版本的跟踪计划，然后上传你所做的任何更改，确保每个参与该计划的人都有最新的更改。

一旦跟踪计划通过 Google Sheet 上传到 API，您就可以开始使用 RudderTyper 了。下载说明和教程将提供给测试版的参与者。

# 追踪计划只是拼图的一部分

尽管 RudderStack 跟踪计划非常有用(并且已经为我们的团队和测试版用户所用)，但是应该注意的是，总会有这样的情况，一旦数据从数据源到达，您仍然需要对其进行转换，以便根据各种下游目的地工具的需求进行丰富、过滤或处理。跟踪计划和[转换](https://rudderstack.com/product/transformations)齐头并进，以确保稳定可靠的数据馈送。

有时，您可能不确定如何处理从您的源中流出的特定变化的事件，在这种情况下，将它们发送到备份桶(如亚马逊 S3 或谷歌云存储)是一个优雅的解决方案。查看我们的文档，了解更多关于如何利用各种[云存储平台](https://docs.rudderstack.com/destinations/storage-platforms)的信息。

# Beta 注册

随着我们继续让开发人员完全控制他们的数据和工具的使命，我们认可并感谢我们的客户做出的帮助改进产品的承诺，我们感谢您。如果你想了解更多关于如何注册的信息，请联系 katie@rudderstack.com 的[或通过](mailto:katie@rudderstack.com) [Slack](https://rudderstack.com/join-rudderstack-slack-community) 联系我们。

*本博客最初发布于:* [*https://rudder stack . com/blog/dogfooding-at-rudder stack-tracking-plans-part-1*](https://rudderstack.com/blog/dogfooding-at-rudderstack-tracking-plans-part-1)