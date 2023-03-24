# 在 R-回归问题中实现决策树(使用 rPart)

> 原文：<https://medium.com/nerd-for-tech/implementing-decision-trees-in-r-regression-problem-using-rpart-c74cbd9e0b7b?source=collection_archive---------1----------------------->

决策树通常用于因变量(响应变量)和自变量(解释变量/预测变量)之间的关系本质上是非线性的回归问题。

让我们考虑一个地区的农作物产量。作物产量取决于降雨量。在理想条件下，它们是线性相关的(如果作物的所有生长条件都具备)。让我们引入另一个变量“温度”。农作物产量现在也依赖于“温度”,但是…