# 仪表板—如何在数据分析中可视化

> 原文：<https://medium.com/nerd-for-tech/dashboarding-how-to-visualize-in-data-analytics-42f5c30bf4e?source=collection_archive---------6----------------------->

我目前在一家新公司担任数据分析师。分析是你进入“科学”数据之前的第一步。你想了解更多关于数据科学的知识吗？检查一下我以前的文章。数据分析和数据科学的区别在[这里](https://www.northeastern.edu/graduate/blog/data-analytics-vs-data-science/)解释。

![](img/48d3ac2288ef10cc7578ca74ae4b6ac3.png)

照片由[克里斯·利维拉尼](https://unsplash.com/@chrisliverani?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为数据分析师，你的工作是以最简单的方式向业务人员展示收集的数据。他们想看看他们的公司或产品表现如何。没有人希望在理解他们公司的产品行为之前，查看 [Excel 表](https://www.perfectxl.com/excel-glossary/what-is-a-spreadsheet/)或 [CSV](https://www.howtogeek.com/348960/what-is-a-csv-file-and-how-do-i-open-it/) 文件(字符分隔值或逗号分隔文件)中成百上千的数据点。简而言之，我们做脏活，并把它做得尽可能漂亮。我们就像数据的提炼者。

在我作为分析师的整个生命周期中，以最简单和最容易理解的方式显示数据对我来说是必不可少的。我已经用各种软件完成了这项工作。

什么是[仪表板](https://www.logianalytics.com/resources/bi-encyclopedia/dashboards-dashboarding/)？

[仪表板](https://www.datapine.com/blog/data-dashboards-definition-examples-templates/)包括创建公司或产品数据的概览。仪表板是汇总所有相关信息的地方。它将关键点可视化，有助于做出明智的决策。由数据和/或业务分析师和经理来确定需要在仪表板上显示的与公司目标相关的关键绩效指标。

可以有不同类型的[仪表盘](https://www.idashboards.com/guides/what-is-a-dashboard/)。不是一刀切。

您可以使用的一个常用数据分析和可视化工具是 Microsoft Excel。它非常强大，相信我，在 Excel 中有很多你不知道的事情可以做。我仍然一直看到和听到 Excel 的功能，我很惊讶。但是老实说，我不太喜欢使用它(抱歉，微软)。只是因为我相信其他软件有更漂亮的图表。Excel 也不能很好地处理大型数据点(200，000 多行——或者我只是有一台很慢的 PC？)所以我用我信任的老朋友 [pandas](https://pandas.pydata.org/docs/user_guide/index.html) (python)来完成数据清理任务，然后转换回 xlsx (excel)来创建我的图表。我知道那很久了。

有些在线应用程序允许你存储数据，并自动将数据可视化。如果你真的了解我，你会知道我完全支持自动化。我不是来受苦的。

[元数据库](https://www.metabase.com/)是一个分析工具，它连接到一家公司的[数据库](https://clifflolo.medium.com/storing-data-3ad883e1154f)，帮助他们存储和分析他们的数据。它附带了实时仪表盘的功能，并为团队提供了根据数据做出决策的能力——该死的宣传，现在让我来做我的影响者演出吧。

在这里导入数据后，它会运行自动生成的 SQL 查询，让您更好地理解数据。此外，您可以编写自己的查询或使用拖放式可视化查询构建器来生成更具体的查询，以回答与数据相关的问题。你可以通过不同的特性过滤你的数据，求和，聚合，而不用费力去想哪个代码适合什么。好玩！相信我吧。

另一个我正在学习使用的是 [Redash](https://redash.io/) 。Redash 是另一个帮助你理解数据的软件。这是一个连接到数据源的平台，有助于构建用于数据可视化的仪表板。在我的下一篇文章中，我将带您了解如何在构建仪表板时将数据源连接到 Redash。

专业提示:阅读这篇关于如何提高你的[仪表板技术](/geekculture/visualising-data-for-organisations-5b6b0dcdcfef)的文章。

下次见！