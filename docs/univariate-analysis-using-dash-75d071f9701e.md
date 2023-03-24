# 使用破折号的单变量分析

> 原文：<https://medium.com/nerd-for-tech/univariate-analysis-using-dash-75d071f9701e?source=collection_archive---------19----------------------->

作为 EDA 系列的一部分，我想向您介绍一种简单的方法，使用浏览器以交互方式进行单变量分析。

Plotly 和它的扩展 Dash 每月被下载 500 万次。

[](https://plotly.com/dash/) [## 仪表板概述

### 有了 Dash Enterprise，过去需要前端、后端和开发团队的全栈 AI 应用程序…

plotly.com](https://plotly.com/dash/) 

我将使用非常基本的 Titanic 数据集，但替换数据集只是 2 行代码的更改。

# 假设

1.  您已经清理了数据集
2.  变量被转换成正确的数据类型。

一旦这些步骤完成，使用 dash 我们可以设置一个简单的基于浏览器的界面来进行单变量分析。

***装破折号***

```
pip install dash dash-renderer dash-html-components dash-core-components plotly
```

设置代码可在[这里](https://github.com/SCK22/Visualizations/blob/master/Dash/univariate_analysis_dash.py)获得。

W 当您运行[univariate _ analysis _ dash . py](https://github.com/SCK22/Visualizations/blob/master/Dash/univariate_analysis_dash.py)文件时，您将看到类似如下的输出

![](img/907c28ffc21d33cb98eccde2fd2482fe.png)

打开上面提到的网址，瞧，你很容易就有一个网络应用程序在你的系统上运行了。

您可以从下拉列表中选择一列并查看分布情况。

![](img/e91390852aca50ccc29377285d555eaf.png)

要更改数据集，在文件中，更改读取数据集的路径和显示标题，就可以开始了！

一段视频展示了我们如何轻松使用 Dash 界面。

你喜欢这篇文章吗？请在评论中告诉我，你可以在 LinkedIn 上联系我，我们可以就此进行交谈。

你也可以访问我的 [github](https://github.com/sck22) 链接，获得一些在 EDA 和模型构建中可能有用的代码片段。

[](https://github.com/SCK22/Visualizations) [## sck 22/可视化

### 可视化的数据从各种来源，包括 data.gov.in 空气质量指数观看视频，如果你有加载新的数据从…

github.com](https://github.com/SCK22/Visualizations) 

在[松弛](http://mldiscussionwithck.slack.com)时与我联系