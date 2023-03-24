# 结构化查询语言

> 原文：<https://medium.com/nerd-for-tech/sql-e8aedd1889f6?source=collection_archive---------15----------------------->

## 我的数据科学之旅(第 4.2 部分)

数据库是任何相关信息的集合。DBMS 是帮助用户创建和维护数据库的特殊软件。

![](img/edd1d369327595c721081b54918e87d6.png)

托拜厄斯·菲舍尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

## **两种类型的数据库**

1.  关系数据库(SQL):将数据组织到一个或多个表中。每个表格都有列和行。一个唯一的键标识每一行。关系数据库是存储和提供对彼此相关的数据点的访问的数据库，例如 my SQL、oracle、Postgre oracle、Maria DB。
2.  非关系数据库(NoSQL):用除了表、键值、文档、图形、灵活的表之外的任何东西来组织数据，例如 MongoDB、dynamo DB、Apache Cassandra

# TCL 集团股份有限公司（TCL Corporation 的缩写）

事务控制语言，使用 DML(数据操作语言)管理数据库中的事务。这就像从一个账户到另一个账户的货币交易；这笔钱必须从一个账户借记到另一个账户贷记。不能同时借记或贷记。

# DQL

用于获取数据的数据查询语言。DQL 只包含一个命令选择

# 民法博士

数据控制语言用于授予或撤销特权；在数据库中进行的任何操作都需要特权。包含两个命令 GRANT，REVOK

```
GRANT CREATE SESSION TO username
-- grant user permission to create sessionGRANT CREATE TABLE to username
-- grant user permision to create tablesGRANT sysdba TO username
--grant all permissionREVOKE CREATE SESSION FROM usernameGRANT privillage_name ON object_name To userREVOKE privillage_name ON object_name FROM user
```

# DoctorofModernLanguages 现代语言博士

数据操作语言。它用于操作数据库中的数据，例如插入、更新、删除。DML 命令未提交(任何更改都可以回滚，提交会永久保存更改)

保存点:临时保存，以便回滚到保存点

回滚:将数据库恢复到上次提交的状态

# 数据定义语言

用于定义数据库模式(创建或修改数据库对象的结构)的数据定义语言，例如，创建、删除、更改、截断、重命名。

# 结构化查询语言

SQL 是一种标准语言，代表基于英语的结构化查询语言，用于与 RDBMS 交互。

## **SQL 的子集**

1.  数据定义语言(DDL)—用于定义数据库结构，如创建、更改和删除对象。
2.  数据操作语言(DML) —它允许您访问和操作数据。它帮助你插入，更新，删除和检索数据库中的数据。
3.  数据控制语言(DCL) —它允许您控制对数据库的访问。示例—授予、撤销访问权限。

# 问题

给 RDBMS 的一组指令，告诉 RDBMS 要检索什么信息

可以存储在数据库中的数据类型

1.  数字/整数
2.  小数(m，n) m 总数，小数后的总数
3.  varchar(10)最长为 10 的可变长度字符串
4.  固定长度的 char(10)字符串
5.  日期
6.  时间戳

# SQL 语法

## 挑选

```
SELECT column1, column2 FROM table_name;
SELECT * FROM table_name;
```

## 选择不同

返回不同值的列表

```
SELECT DISTINCT column1, column2 FROM table_name;SELECT COUNT(DISTINCT column1) FROM Customers;
returns the count of unique
```

## 在哪里

用于过滤

```
SELECT column1, column2
FROM table_name
WHERE condition;
```

## **操作员**

=(等于)、>大于[、](https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_greater_than)、<小于[、](https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_less_than)、> =大于等于[、](https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_greater_than2)、< =小于等于[、](https://www.w3schools.com/sql/trysql.asp?filename=trysql_op_less_than2)、<、>不等，介于一个范围之间，就像搜索模式一样，在。

**与，或，非**

```
SELECT column1, column2
FROM table_name
WHERE condition1 AND condition2;SELECT column1, column2
FROM table_name
WHERE condition1 OR condition2;SELECT column1, column2
FROM table_name
WHERE NOT condition;SELECT * FROM table_name
WHERE condition1 AND (condition2 OR condition3);
```

**IN，NOT IN**(IN 运算符允许您在 WHERE 子句中指定多个值。IN 运算符是多个条件的简写)

```
SELECT column_name
FROM table_name
WHERE column_name IN (value1, value2);
```

## **之间**

```
SELECT column_name
FROM table_name
WHERE column_name BETWEEN value1 AND value2;
```

## **喜欢**

搜索指定的模式

```
SELECT column1
FROM table_name
WHERE column LIKE pattern;
```

Pattern (pattern inside " ") (%- 1 个或多个字符，_-只有一个字符)

a%:以

%a:以 a 结尾

%a%:查找任何位置都有“a”的值

_a%:查找第二个位置有“a”的值

a_%:以“a”开头的值，至少有两个字符

a__%:以“a”开头，至少有三个字符

a%z:以“a”开头，以“z”结尾。

## 以...排序

按升序(默认)或降序排序

```
SELECT column1, column2
FROM table_name
ORDER BY column1,column2 ASC|DESC;
-- orders by column1 first then column2, column1 can be ASC and column2 can be DESC
```

## 分组依据

将具有相同值的行分组到汇总行中

```
SELECT column_name
FROM table_name
GROUP BY column_name
```

Group by 与聚合函数(COUNT、MAX、MIN、SUM、AVG)一起使用，按一个或多个列对结果集进行分组。

## 数数

计算条目的数量

```
SELECT COUNT(column_name)
FROM   table_name
```

## **有**

```
SELECT SUM(column_name)
FROM   table_name
WHERE  CONDITION
GROUP BY column_name
HAVING (arithematic function condition);
```

## 桌子

```
CREATE TABLE Student (
    Student_ID NUMBER PRIMARY KEY,
    Name VARCHAR(20),
    Major VARCHAR(20)
    PRIMARY KEY(Student_ID) -- 2 ways to create primary key
); 
```

## 形容

```
DESCRIBE Student;
-- describes the table, tells Null and type
```

## 翻桌

```
DROP TABLE Student;
-- delete the table
```

从数据库中删除整个表结构和记录

## 创建索引

在表中创建索引，索引用于快速检索数据，用户看不到索引。与没有索引的表相比，更新有索引的表会花费一些时间，因为索引也需要更新

```
CREATE UNIQUE INDEX index_name
ON table_name ( column1, column2);DROP INDEX
ALTER TABLE table_name
DROP INDEX index_name;
```

## 下降指数

```
ALTER TABLE table_name
DROP INDEX index_name;
```

## 截断表格

```
TRUNCATE TABLE table_name;
```

它会删除表中的所有记录，但不会删除表。表的结构被保留。如果表包含外键并且不能回滚，则该表不能被截断。Truncate 比 delete 快，因为它不扫描记录。度假村自动增量

## 删除

```
DELETE FROM table_name WHERE condition;
```

仅删除记录不会将表从数据库中删除。它可以回滚，记录自动增量

## 更改表格

```
ALTER TABLE table_name {ADD|DROP|MODIFY} column_name {data_ype};ALTER TABLE table_name RENAME TO new_table_name;
```

## 明显的

```
SELECT DISTINCT column1
FROM table_name
```

## 顶端

```
SELECT TOP number|percent column_name
FROM table_nameSELECT TOP 3 * FROM CUSTOMERS;
--Top 3 customer
```

## 插入

```
INSERT INTO table_name( column1, column2)
VALUES(value1, value2);
```

## 更新

```
UPDATE table_name
SET column1 = value1, column2 = value2,
WHERE condition;
```

## 创建数据库

```
CREATE DATABASE database_name;
```

## 删除数据库

```
DROP DATABASE database_name;
```

## 使用

```
USE DATABASE database_name;
```

## 犯罪

```
COMMIT;
```

## 反转

```
ROLLBACK
```

# 经营者

*   算术运算符(+、-、*、/、%)
*   比较运算符(=，< , >，<>(不等于)，< = ,> =)
*   逻辑运算符(ALL、AND、ANY、BETWEEN、EXISTS、IN、LIKE、NOT、OR、ISNULL、UNIQUE)

# 表示

表达式是值、运算符和计算值的 SQL 函数的组合。这些就像公式，它们是用查询语言编写的。您还可以使用它们在数据库中查询一组特定的数据。

# 限制

SQL 约束用于指定表中数据的规则。这些用于限制必须存储在数据库中的数据类型，并提高存储在数据库中的数据的准确性和可靠性。2 种类型的约束列级和表级。

*   NOT NULL-列不能为 NULL
*   唯一-确保所有值都是唯一的
*   主关键字
*   外键
*   支票
*   默认“文本”-当没有指定时，提供默认值
*   指数
*   自动增量

# 键

**超级键:**超级键是一个或多个键的集合，可以用来唯一地标识表中的记录

**候选键:**超级键的子集。候选关键字是一个或多个字段/列的集合，可以唯一地标识表中的记录。一个表中可以有多个候选键。每个候选键都可以作为主键。包含空值

**主键:**主键是一个表的一个或多个字段/列的集合，唯一地标识数据库表中的一条记录。它不能接受空的、重复的值。只有一个候选键可以是主键。

*超级键是可以唯一标识每行的所有列的集合，候选键是有资格用作主键但尚未使用的键，最后，主键是满足成为主键的所有要求的特定候选键。*

**备选键:**备选键是可以作为主键使用的键。它是当前不是主键的候选键。

**组合键:**组合键组合一个表的多个字段/列来表示一个主键。

**唯一键:**唯一键是一个表的一个或多个字段/列的集合，其唯一地标识数据库表中的记录。它类似于主键，但只能接受一个空值，并且不能有重复值。

**代理键:**当没有唯一标识符时，代理键用于代替主键。代理键没有任何实际的表示，例如 S.No

**自然关键字:**自然关键字是在现实世界中具有用途或映射的主键，例如，标识号

# 连接

SQL Joins 子句用于组合数据库中两个或多个表的记录。联接是一种通过使用两个表的公共值来组合两个表中的字段的方法。

## 内部连接

返回在两个表中都有匹配值的记录(仅指定行和列)

```
SELECT table1.column1, table2.column2
FROM table1 INNER JOIN table2
ON table1.common_field = table2.common_field;
```

**左连接**

返回两个表中的公共行以及左表中的所有行

```
SELECT table1.column1, table2.column2
FROM table1 LEFT JOIN table2
ON table1.common_field = table2.common_field;
```

**右连接**

返回两者中的公共行加上右表中的所有行

```
SELECT table1.column1, table2.column2
FROM table1 RIGHT JOIN table2
ON table1.common_field = table2.common_field;
```

**全面加入**

返回左表和右表的公共行

```
SELECT table1.column1, table2.column2
FROM table1 FULL JOIN table2
ON table1.common_field = table2.common_field;
```

**自我加入**

SQL 自连接用于将一个表连接到自身，就好像该表是两个表一样

```
SELECT a.column_name, b.column_name
FROM table1 a, table1 b
WHERE a.common_field = b.common_field;
```

a 和 b 指的是同一张表

# 联盟

组合两个或多个 select 语句的结果，不返回重复值。UNION ALL 包含重复值

```
SELECT column1
FROM table1 
WHERE condition

UNION

SELECT column1 
FROM table1 
WHERE condition
```

# 别名

别名为列或表提供一个临时名称

```
SELECT column1 FROM table_name AS alias_nameSELECT column_name AS alias_name FROM table_name
```

# 指数

索引通过最小化处理查询时所需的磁盘访问次数来优化数据库的性能。索引加快了 select 查询和 where 子句的速度，但是减慢了数据输入的速度。用户看不到索引

**创建索引**

```
CREATE INDEX index_name ON table_name;CREATE INDEX index_name ON table_name (column_name);
-- indexing on a single columnCREATE UNIQUE INDEX index_name on table_name (column_name);
-- unique index allows only unique valueCREATE INDEX index_name on table_name (column1, column2);
-- index in two or more column
```

应该避免在具有频繁或大量更新或插入操作的表中使用索引

当对表进行索引时，会创建一个单独的表，称为索引表，它将数据存储在[搜索关键字，数据引用]中

# 索引的类型

## 主要的

密集:索引表中的条目数与主表中的条目数相同

稀疏:主表将被分成块，索引表将只包含该特定表的第一行

## 使聚集

对于非唯一键，如 dept id，其中许多学生可以有同一个系，主表根据非唯一聚类分成块；索引表将指向该块。

## 副手

两级索引主(ram)和辅助(硬盘)

主表指向辅表，辅表指向主表

# 扳机

当某些事件发生时自动执行的存储程序

```
CREATE/REPLACE TRIGGER trigger_name
BEFORE/AFTER/INSTED OF
INSERT/UPDATE/DELETE
OF column_name
ON table_name
REFERENCING OLD AS o NEW AS n
FOR EACH ROW
WHEN(condition)
DECLARE
BEGIN
END; 
```

# 视图

虚拟表，视图中没有记录。它只保存表的定义，从表中获取数据并显示出来。视图是作为数据库中的永久查询对象创建的

```
CREATE VIEW as view_name
SELECT column1
FROM table_name
WHERE condition;DROP VIEW ;
```

视图用于隐藏由连接引起的阴谋

# 选择语句

控制语句

```
CASE
WHEN condn1 THEN result1
WHEN condn2 THEN result2
ELSE result
```

# 正常化

最小化冗余的过程，划分表并链接它们。

4 种范式

1NF，2NF，3NF，BCNF

## 1 范式

1NF 的规则

*   每个单元格都应该有一个值属性(原子值)。如果一行中某个特定特性有多个值，则该行将被拆分

```
ID     Name      Age     Subject
1      A         20      E,M1NF
ID     Name      Age     Subject
1      A         20      E
1      A         20      M
```

*   对于该特定列，不应更改属性的数据类型
*   每一行应该有一个唯一的属性
*   顺序不重要

## 2 范式

1 NF +

*   不应该存在部分依赖关系(所有列应该完全依赖于一个主键)(部分依赖关系=候选键)

## 3 范式

2 NF+

*   没有传递依赖关系(X →Y→Z)

## Boyce-**Codd 范式** (BCNF)(3.5 NF)

3 NF+

*   LHS 应该是候选键或超级键

```
Table
column1 column2
column1 should be a  candidate key or a super key
```

# 反规格化

反规范化是一种数据库优化技术，在这种技术中，我们向一个或多个表中添加冗余数据。这可以帮助我们避免关系数据库中代价高昂的连接

连接是昂贵的。需要更多的资源来检索数据。

规范化会组织数据，但由于存在连接，检索数据的速度很慢。

。感谢您的阅读