# 用于数据科学的 SQL 不仅仅是 SELECT *

> 原文：<https://medium.com/nerd-for-tech/sql-for-data-science-is-way-more-than-select-4474ce3790ae?source=collection_archive---------5----------------------->

![](img/894beb11fe69b020c82e80ba3d094b95.png)

照片由[文森特·索罗门](https://unsplash.com/@vincentiu?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

你已经完成了数据科学课程，你已经学习了 Python、统计学、线性代数、机器学习、深度学习，你已经掌握了探索数据的主要库，你已经用 Kaggle 数据进行了大量练习，制作了一个令人惊叹的作品集，并且获得了一份数据科学家的工作！
你很兴奋**第一周上班，打开。csv 或。xlsx 文件与熊猫，并开始探索数据！但是，等等…
**“在哪里。csv/。xlsx 文件？？?"****

**如今，所有公司都将数据存储在云(AWS、Google Cloud、Azure)的结构化或非结构化数据库中，因此，对于数据科学家来说，掌握 SQL 和 NoSQL 语言以从这些数据库中正确提取信息至关重要。
不幸的是，在您的数据科学课程中教授的关于 SQL 的一切都是简单的查询，就像著名的`SELECT * FROM table`！嗯，它确实工作，**但是请相信我**，当你要处理数 Pb 的信息**时，你不会希望**将两个不同的表格导入到有熊猫的`merge`中(事实上，根据你的 PC 配置，你不能这样做)。**

**出于这个原因，我决定用我在过去 6 个月的数据科学家工作中学到的一些 SQL 技巧来写这篇文章。**

## ****1。子查询(带有 where 子句的 Select 语句调用另一个 Select 语句)****

**假设您在一家宠物商店工作，您想知道所有养狗的顾客的姓名、年龄和地址，但是顾客信息在一个表中，而宠物信息在另一个表中。你可以把两个表都拿来，然后用 Python 合并，**但是你可以做得更好！**您可以使用*子查询*从表 2 中提取所有有狗的客户名称，然后使用这些客户名称作为`WHERE`子句的参数来提取所需的信息，如下例所示:**

```
SELECT column_name, column_age, column_address
FROM table1
WHERE column_name IN (SELECT column_name
                      FROM table2
                      WHERE column_pet_type = 'dog')
ORDER BY column_age DESC
```

**第二个选择提取所有养狗的客户的名字，并将这些名字用作第一个`SELECT`的`WHERE`子句的参数。确保子查询`SELECT`语句将返回一个单独的列，否则第一个`SELECT`语句将不起作用。
这样，从数据库中提取信息所需的时间将比提取所有信息然后连接表更快。**

## ****2。临时表****

**SQL Server 中的临时表是临时存在于服务器上的数据库表。它将普通表中的数据子集存储一段时间。让我们来看一个例子:
假设您想要具有特定姓名和特定年龄的所有客户的 ID，不幸的是，您的表没有年龄，但是包含出生日期。您可以将出生日期列转换为年龄，并将该信息存储在临时表中，然后在新查询中使用该临时表。**

```
SELECT column_id, 
       column_name, 
       DATEDIFF(hour, column_birth_date, GETDATE())/8766 AS Age
INTO #TemporaryTable
FROM table1SELECT column_id
FROM #TemporaryTable
WHERE column_name = 'Daniel'
AND Age = 33
```

**在查询的第一部分中，我使用函数`DATEDIFF`将列 birth_date 转换为 Age，然后存储信息`INTO #TemporaryTable.` 。在第二部分中，我使用我创建的临时表来查询从临时表中提取的具有特定年龄的列 ID。**

## ****3。参数化查询****

****参数化查询**(也称为准备好的语句)是一种预编译 SQL 语句的方法，因此您只需提供需要插入到语句中的“参数”即可执行。这通常是用来避免 SQL 注入的攻击。
SQL 注入是一个 web 安全漏洞，使得攻击者能够干扰应用程序对其数据库的查询。它通常允许攻击者查看他们通常无法检索的数据。在许多情况下，攻击者可以修改或删除这些数据，从而导致应用程序的内容或行为发生持久变化。
在下面的例子中，我将“姓名”和“年龄”作为参数传递给我的查询，使用的语法适用于 Microsoft Server SQL ( `%(parameter)s`)，对于其他数据库，请检查它们的文档以查看正确的语法。**

```
import pandas as pd
import pymssqlquery = """SELECT column_name, column_address, 
                  column_email, column_age
           FROM table1
           WHERE column_name = %(name)s
           AND column_age = %(age)s"""conn = pymssql.connect(host, user, password, database)
dataframe = pd.read_sql(query, conn, 
                        params = dict(name = 'Daniel', age = 33))
#Or if you execute the query using cursor
cur = conn.cursor()
cur.execute(query, params = dict(name = 'Daniel', age = 33))
```

# **结论**

**如果您想提高代码性能，深入了解 SQL 基础知识，本文只涵盖了 SQL 能为用户提供的很小一部分。我希望你喜欢这篇文章，并在此过程中学到一些新东西。如果是这样，请务必在评论中告诉我。**