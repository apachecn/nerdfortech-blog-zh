# 将 MySQL 整数列转换为日期时间，同时保留数据

> 原文：<https://medium.com/nerd-for-tech/convert-mysql-integer-column-into-datetime-while-preserving-the-data-413040555061?source=collection_archive---------9----------------------->

我最近遇到了一个问题，我从一个 API 中获取整数值时间戳数据，我只是将它保存到一个表的整数列中。

与我一起工作的客户当时决定，他们需要将时间戳的整数列转换为人类可读的日期时间。我需要在保存数据的同时进行这种转换。

在谷歌搜索了一段时间后，我终于找到了一个有效的答案，我认为值得分享。

![](img/7dda4a92c5071c9bdaffe23cd7f3ca53.png)

大喊到 [@ikukevk](https://unsplash.com/@ikukevk) 供图

以下是需要的 5 个步骤:

1.  在同一个 mysql 表中创建一个类型为**日期时间**的*临时列*

2.在 mysql 表中，根据您的使用情况运行以下查询之一:

a)您有一个 10 位数的时间戳值，例如 1613759058

运行以下命令:

```
**UPDATE** table 
**SET** 
*temp_column* = **FROM_UNIXTIME**(*integer_timestamp_column*)
```

这将获取 *integer_timestamp_column* 值，并使用[**FROM _ UNIXTIME**](https://database.guide/from_unixtime-examples-mysql/)**将它们转换成日期时间。**然后将该值复制到 *temp_column 中。*

b)如果时间戳长于 10 位数(意味着它占毫秒或更多)，您需要转换它。我有一个 13 位的时间戳，所以我做了如下转换:

```
**FLOOR**(*integer_timestamp***/1000**)
```

首先，它除以 1000，剩下 3 位小数。然后应用**FLOOR****通过向下舍入将数字转换为整数(这是我们下一步需要的)。**

**然后运行:**

```
**UPDATE** table 
**SET** 
*temp_column* = **FROM_UNIXTIME**(**FLOOR**(*integer_timestamp_column***/1000**))
```

**再次将转换后的值从*整数时间戳列*复制到*临时列。***

**3.删除*整数 _ 时间戳 _ 列***

**4.将*临时列*重命名为*整数时间戳列***

**![](img/5b67f663d78b2e3327c9214a3ccbd0b7.png)**

**大喊到 [@lidyanada](https://unsplash.com/@lidyanada) 供图**

# **瞧，您已经将表中的整数列转换为日期时间列，同时保留了数据！**