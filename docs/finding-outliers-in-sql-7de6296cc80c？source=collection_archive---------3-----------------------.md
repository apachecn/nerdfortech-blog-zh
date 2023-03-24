# 查找 SQL 中的异常值

> 原文：<https://medium.com/nerd-for-tech/finding-outliers-in-sql-7de6296cc80c?source=collection_archive---------3----------------------->

*使用 SQL Server 使用中位数绝对偏差查找异常值*

在现实世界的场景中，我们经常得到倾斜的数据分布。也就是说，大部分数据都集中在一个区域，剩下的几个数据点远离大部分数据。在统计学中，发现异常值的常用方法是计算标准差，并识别超过 3 个标准差的数据点。然而，如果数据分布是偏斜的，中位数是比平均值更好的集中趋势的度量，四分位间距是比标准差更好的分布度量。这是因为均值和标准差将考虑数据集中的所有数据点，包括异常值。

在本文中，我们将通过一个例子来看看如何使用中位数绝对偏差来发现异常值。

![](img/47a3b0cb2f4f361f095714f221b96845.png)

[来源](https://www.cuemath.com/data/outlier/)

**问题定义**

识别数据中的异常值

**解决方案**

将“雇员”表中的时薪视为 30、10、20、15、100

*第一步:*计算薪水列的中位数

(10，15，20，30，100)的中位数是 20

*第二步:*计算中间值和每个值的绝对差值

**abs(工资中位数)** =10，5，0，10，80

*第三步:*求绝对差的中值

**中位数** (0，5，10，10，80)为 10，称为**中位数绝对偏差(MAD)**

*第四步:*求各数值的偏差与中位数偏差的**比值**。即**ABS(salary-MAD)/MAD**= ABS((10–10)/10，(15–10)/10，(20–10)/10，(30–10)/10，(100–10)/10))=(0，0.5，1，2，9)

我们可以清楚地注意到，100 的工资与 9 的比率是一个异常值，是 MAD 的 3 倍以上。

下面显示了在 MS SQL Server 中使用中位数绝对偏差来查找异常值的示例代码

```
with median
as
(SELECT distinct PERCENTILE_CONT(0.5)within group(order by Salary)
over() as median
FROM Employees),Deviation
as
(SELECT abs(Salary-median)as Deviation
FROM Employees 
JOIN median on 1=1)
,
MAD as
(
SELECT DISTINCT PERCENTILE_CONT(0.5) within group(order by deviation) over() as MAD
FROM Deviation)SELECT abs(Salary -MAD)/MAD as Ratio, Salary
FROM MAD 
JOIN Employees On 1=1
```

**结论**

在这篇文章中，我们能够找到超出中位数 3 个绝对偏差的异常工资。即使数据不呈正态分布，这种方法也是解释异常值的有效方法。