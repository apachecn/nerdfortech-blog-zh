# SQL 101:创建表、插入数据和执行查询

> 原文：<https://medium.com/nerd-for-tech/sql-101-create-a-table-insert-data-and-perform-queries-83368766b9fd?source=collection_archive---------22----------------------->

![](img/1d635f826fbe9573297cb0db7a16f5b7.png)

## 什么是 SQL？

SQL 代表**结构化查询语言**，可以读作“S-Q-L”或“sequel”。SQL 是一种在关系数据库管理系统( [RDBMS](https://www.sisense.com/glossary/relational-database/) )中交流和管理数据的语言。它允许我们通过在数据库中使用 SELECT、delete、INSERT、update、create 和 DROP 语句来执行所有的 [CRUD](https://www.sumologic.com/glossary/crud/#:~:text=CRUD%20Meaning%3A%20CRUD%20is%20an,%2C%20read%2C%20update%20and%20delete.) 功能(创建、读取、更新、删除)。

下面的例子将假设你有关系数据库的基础知识。不同的关系数据库(SQLite、PostgreSQL、MySQL)并不支持所有相同的 [*SQL 数据类型*](https://www.journaldev.com/16774/sql-data-types) *。对于下面的例子，我们将使用 SQLite 数据库中的数据类型。*

## 创建表格

对于这个例子，我们将创建一个简单的`students`表，其中包含列`id` `name` `grade` `gpa` `tardies`，并为每个列分配一个数据类型。我们将使用一条 SQL 语句使用`CREATE TABLE`在数据库中创建新的`students`表:

```
CREATE TABLE students
  (id INTEGER PRIMARY KEY,
   name TEXT,
   grade INTEGER,
   gpa FLOAT,
   tardies INTEGER);
```

## 将数据插入表中

现在我们的`students`表存在，但它完全是空的。让我们看看如何使用`INSERT`语句向其中添加一些学生数据:

```
INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Jake", 12, 3.5, 4);INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Emily", 10, 3.7, 2);INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Sam", 11, 3.5, 3);INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Jordan", 10, 3.6, 2);INSERT INTO students (name, grade, gpa, tardies)
  VALUES ("Victoria", 9, 3.2, 3);
```

每个`INSERT`将在我们的`students`表中创建一个新的数据行。我们首先指定表的名称，然后添加我们想要添加数据的列。`VALUES`分别是将要插入指定列的数据。注意，我们不必为学生指定一个`id`，因为 ordered `id`将为每个新的数据行自动创建，在这个表中是一个学生。

## 查询表

从表中查询特定数据有许多复杂的方法。让我们从一些基础开始。每个查询都将以一个`SELECT`语句开始。

首先，让我们看看如何使用[通配符](https://data-flair.training/blogs/sql-wildcard/) `*`选择表中的每一列，从`students`表中获取所有数据:

`SELECT * FROM students`

让我们看看如何通过指定`name`列从表中只获得学生的名字:

`SELECT name FROM students;`

简单吧？让我们增加一点复杂性，并使用`ORDER BY`按字母顺序对姓名进行排序(默认为升序):

`SELECT name FROM students ORDER BY name;`

现在，让我们从`name`和`gpa`列中选择数据，并使用`DESC`按 GPA 从低到高排序:

`SELECT name, gpa FROM students ORDER BY gpa DESC`

让我们看看如何通过实现一个`WHERE`语句和大于运算符来获得 GPA 高于 3.5 的学生的姓名:

`SELECT name FROM students WHERE gpa>3.5`

我们如何更改上述语句以获得所有 GPA 大于 3.5 且在 10 年级或更高年级的学生的数据(姓名、年级、GPA、迟到)？我们可以将[位运算符](https://docs.microsoft.com/en-us/sql/t-sql/language-elements/bitwise-operators-transact-sql?view=sql-server-ver15) `AND`与`*`通配符一起使用:

`SELECT * FROM students WHERE gpa>3.5 AND grade>=10`

现在，来点不同的。让我们看看如何在 SQL 中使用[聚合函数](https://www.guru99.com/aggregate-functions.html)对行执行简单的计算。这些函数将始终返回一个值:

怎样才能从`students`表中得到最高的 GPA？

`SELECT MAX(gpa) FROM students`

最低的 GPA 怎么样？

`SELECT MIN(gpa) FROM students`

而平均的`students` GPA 呢？

`SELECT AVG(gpa) FROM students`

`students`总共有多少拖期？

`SELECT SUM(tardies) FROM students`

10 年级学生的平均绩点怎么样？

`SELECT AVG(gpa) FROM students WHERE grade==10`

## 结论

这是对 SQL 世界的一个非常基本的介绍。要继续学习 SQL，请查看我的博客 [SQL 102:连接和连接表](https://kylefarmer85.medium.com/sql-102-joins-and-join-tables-d71d0dfc2f4)！