# 使用 dbt 和 RudderStack 的客户会话分析

> 原文：<https://medium.com/nerd-for-tech/customer-session-analysis-using-dbt-and-rudderstack-144fb7932213?source=collection_archive---------6----------------------->

![](img/2e2a39aed4ea1a84b6ad612870aa9fd9.png)

任何客户数据平台最重要和最基本的一个方面是能够跟踪单个用户在浏览你的网站、应用、营销生态系统、社交平台等时的步骤。RudderStack 可以轻松地从每个平台收集这些数据，并将其发送到您的云数据仓库进行高级建模和分析。

在本练习中，我们将浏览一个示例 dbt，该示例 dbt 对您的仓库中由 RudderStack 行为数据支持的两个关键分析用例非常有用(代码可通过我们的 Git repo 或 hub.getdbt.com/rudderlabs 获得)。

1.  将匿名活动(首次识别呼叫前)链接到现在已知的用户，以及在子域之间或跨设备加入同一用户的不同匿名 id
2.  基于与用户(per #1)相关联的任何活动(在我们的例子中是跟踪呼叫)以及事件之间的时间间隔来定义会话

在这两种情况下，拥有原始数据以获得可见性对于支持分析非常有帮助，对于解决身份解析和会话定义方面的问题也非常有帮助。

还值得指出的是，在这个例子中，我们将会话定义为一些时间增量。在其他用例中，您可能希望将术语“会话”替换为“漏斗阶段”，漏斗阶段可以定义为用户在您的仓库中执行了一些特定的行为(例如，跟踪呼叫)或其他一些数据点(例如，返回的包)。例如，如果您想构建一个基于阶段的电子商务销售漏斗，您可以轻松地用事件映射来替换会话逻辑，事件映射定义结账阶段，如销售线索是否在登录页面上转换和/或他们是否在购物车中放置了商品。

# 概观

这个 dbt 项目构建在源表`tracks`之上，默认情况下，源表是在所有 RudderStack 仓库目的地中创建的。您还可以包括寻呼呼叫或仓库中的其他活动，如客户服务单、入站呼叫、退回的包裹等。只要该活动有时间戳。

来自`tracks`表的数据用于首先创建会话抽象，然后准备由客户在会话过程中触发的 5 个事件的序列。这个序列代表了客户的旅程。你可以通过给`dbt_tracks_flow.sql`添加必要的代码来增加旅程中的事件数量。

# 如何使用此存储库

这个项目是在 [dbt 云](https://cloud.getdbt.com/)上创建的。因此没有包含连接信息的`profiles.yml`文件。可以导入这个导入截图

想要在命令行界面(CLI)模式下执行模型的开发人员需要按照这里[提供的说明](https://docs.getdbt.com/docs/running-a-dbt-project/using-the-command-line-interface/)创建额外的配置文件。

**注意:**虽然这段代码已经针对 Google BigQuery 进行了测试，但它也可以用于其他支持 RudderStack 的数据仓库，如 Amazon Redshift 和 Snowflake，只需对特定于平台的分析公式进行微小的调整。

# 项目组成部分

以下文件包含在 dbt 模型中，并将在下面引用。您可以复制这个 repo 并将其直接加载到您自己的 cloud dbt 项目中，或者按照链接离线检查代码。

*   [**dbt_project.yml**](https://github.com/rudderlabs/dbt-customer-journey-analysis/blob/1.0.1/dbt_project.yml)—每个 dbt 项目都有一个 dbt _ project . yml 文件。这些是在 YAML 编写的，定义了通用的约定和属性。对于我们的项目，这个页面的亮点包括名称、版本以及我们希望我们的模型被具体化为视图。
*   [**【dbt _ aliases _ mapping . SQL**](https://github.com/rudderlabs/dbt-customer-journey-analysis/blob/1.0.1/models/dbt_aliases_mapping.sql)—创建匿名 id 到已知用户 id 的映射或“人行横道”,以便曾经由匿名用户执行的活动可以包含在已知用户的旅程中。这对于跨设备识别用户以及跨不同子域执行活动的用户非常重要。
*   [**dbt _ mapped _ tracks . SQL**](https://github.com/rudderlabs/dbt-customer-journey-analysis/blob/1.0.1/models/dbt_mapped_tracks.sql)—利用上面的别名映射模型，将所有事件链接到现在已知的用户 id。我们还设置了阶段，通过测量事件之间的时间将“会话”定义为任意分钟数，并将其存储在空闲时间字段中。
*   [**dbt _ session _ tracks . SQL**](https://github.com/rudderlabs/dbt-customer-journey-analysis/blob/1.0.1/models/dbt_session_tracks.sql)—我们根据映射的 tracks 模型为每个用户定义会话。我们将会话定义为 30 分钟的空闲时间，标识开始时间、序列号和下一个会话开始时间(如果有)。
*   [**dbt _ track _ facts . SQL**](https://github.com/rudderlabs/dbt-customer-journey-analysis/blob/1.0.1/models/dbt_track_facts.sql)—我们根据开始和下一个会话开始窗口之间的用户和时间戳，将来自映射轨迹模型的事件分配给它们各自的会话。
*   [**【dbt _ tracks _ flow . SQL**](https://github.com/rudderlabs/dbt-customer-journey-analysis/blob/1.0.1/models/dbt_tracks_flow.sql)—这里我们利用分析函数来显示关于每个会话的更多有用信息，比如前 5 个事件。您可以在这里添加无限的内容，包括识别构成转换的特定跟踪调用，或者汇总事件的数量、用于检查交易的总金额等。
*   [**tracks . yml**](https://github.com/rudderlabs/dbt-customer-journey-analysis/blob/1.0.1/models/tracks.yml)—YAML 文件，用于定义我们要用于 dbt_alias_mapping 模型的模式和跟踪表。

**注意:**这些模型中的每一个都被具体化为表格。因为这些会话是基于时间的，所以我们也可以使用基于表中最新事件时间戳的增量表。

# 命令序列

dbt 模型在新一轮运行中的执行顺序如下:

`dbt_aliases_mapping`

这个模型/表有两个属性/列— alias 和 dbt_visitor_id。此表捕获一个或多个 anonymous_id 值(别名)和一个 user_id (dbt_visitor_id)之间的联系。要更深入地了解身份映射和绘图，请查看我们的创始人关于身份绘图 ID 解析的博文。

`dbt_mapped_tracks`

该表包含 event_id、anonymous_id、dbt_visitor_id、timestamp、event 和 idle_time_minutes 列。

事件字段表示实际的事件名称。时间戳对应于事件实际生成的时刻。idle_time_minutes 捕获当前事件和前一事件之间的时间间隔。您可以添加来自跟踪事件或仓库中其他非 rudder 活动的任何字段。

`dbt_session_tracks`

此表定义了每个用户的会话。在下一步中，我们将把事件分配给这些会话。此会话包含 session_id、dbt_visitor_id、session_start_at、session_sequence_number 和 next_session_start_at 列。

dbt_mapped_tracks 表中的数据首先由 dbt_visitor_id 进行分区。然后将其进一步划分为事件组，其中一个组内的时间间隔(即空闲时间分钟)不超过 30 分钟。换句话说，如果某个事件的 idle_time_minutes 超过 30 分钟，则会创建一个新组。

**需要记住的一些要点:**

*   这些连续事件组就是会话。可以在模型定义中修改 idle_time_minutes 的值。
*   session_sequence_number 表示特定用户的会话顺序。
*   会话标识的形式为会话序列号 dbt 访问者标识。

`dbt_track_facts`

该表包含列 anonymous_id、时间戳、事件 id、事件、会话 id、dbt_visitor_id 和 track_sequence_number。

在此表中，来自 dbt_session_tracks 的信息与 dbt_mapped_tracks 表中的记录相关联。现在，每个事件都与一个 session_id 相关联，并且在会话中，事件被分配一个 track_sequence_number。

`dbt_tracks_flow`

此表中的列包括事件标识、会话标识、跟踪序列编号、事件、dbt 访问者标识、时间戳、事件 2、事件 3、事件 4 和事件 5。这实际上是一个表，其中每个事件和 4 个后续事件在每个记录中表示。

**注意:**请记住将 tracks.yml 和 dbt_aliases_mapping.sql 中的模式更改为您的数据库模式。

# 后续步骤

一旦您在数据仓库中的 RudderStack 数据上释放了 dbt 的威力，您就可以使用 RudderStack 仓库操作将这些信息发送回您的生态系统。查看我们的博客 [RudderStack 的 Data Stack: Deep Dive](https://rudderstack.com/blog/rudderstacks-data-stack-deep-dive) 来看看我们内部是如何做的，以及[加入我们的 Slack](https://rudderstack.com/join-rudderstack-slack-community/) 来看看其他团队是如何利用这个工具的。

*本博客最初发布于:* [*https://rudder stack . com/blog/customer-session-analysis-using-dbt-and-rudder stack/*](https://rudderstack.com/blog/customer-session-analysis-using-dbt-and-rudderstack/)