# 决策树和分裂函数(基尼系数、信息增益和方差减少)

> 原文：<https://medium.com/nerd-for-tech/decision-trees-and-splitting-functions-gini-information-gain-and-variance-reduction-23192f639048?source=collection_archive---------6----------------------->

**先决条件:**

请使用博文《在 R 中实现决策树——分类问题》中使用的数据集，并运行 R 脚本。在脚本中，查找— text(output.tree，pretty=0)。

在上述代码后添加以下代码-

fancyRpartPlot(output.tree)

安装以下软件包— RcolorBrewer、rattle 和 rpart.plot