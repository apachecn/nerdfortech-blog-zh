# 使用 CSV 文件将 SQL DB 迁移到 MySQL

> 原文：<https://medium.com/nerd-for-tech/migrate-sql-db-to-mysql-using-a-csv-file-7cbcaea984cf?source=collection_archive---------3----------------------->

![](img/b61cb45fe64ca2b736e4d8fb4bdcb5b5.png)

MS SQL 是微软的专有 RDBMS，MySQL 是 Oracle 支持的免费开源 RDBMS。随着越来越多的组织从专有软件过渡到开源软件，从 MS SQL 到 MySQL 数据库的迁移势头越来越大。

这不是一个人应该迁移的实际方式，但我只是避免了许多灰色区域，比如—

1.  模式转换
2.  数据丢失风险
3.  时间消耗

## 关于数据库的信息:

1.  我试图迁移的表有超过 40 列。
2.  这个表格有你能想到的所有标准分隔符。
3.  表中到处都是空值。
4.  最后，有超过 100，000 行。

## 迁移步骤:

1.  通过转到任务→导出数据…导出 MS SQL 表
2.  选择本机客户端作为源
3.  选择平面文件作为目标。将分隔格式和代码页设置为拉丁文 I
4.  在下一步中，保留默认的行分隔符，并将列分隔符更改为一个罕见的 ASCII 值(*一个逻辑上不会出现在实际数据中的字符*
5.  接下来，打开 MySQL Workbench 并连接到数据库。在连接之前，在 Advanced 选项卡下，将此选项添加到 others 窗口中的连接器选项中:OPT_LOCAL_INFILE=1
6.  现在执行下面的命令—

```
LOAD DATA LOCAL INFILE 'path\\to\\the\\file' INTO TABLE table_name
FIELDS TERMINATED BY '<rare ASCII Char>' 
LINES TERMINATED BY '\r\n';
```

这个 SQL 语句加载用了 4-5 秒，执行后显示了一个感叹号。为了验证行数，我必须对表进行 count(*)，但是得到了 MS SQL 表的确切行数。

这个过程为我节省了很多时间。但是，有一些限制，在某些情况下不可能迁移所有的表。

但是如果失败了，请在评论中告诉我，我会和你联系，努力解决这个问题。你也可以在下面的 LinkedIn 上联系我—

[](http://linkedin.com/in/kuharan) [## 库哈兰·博米克

### 数据工程师:Python、Powershell、shell 脚本、AWS Lambda、Azure 数据工厂、Azure ML、Azure Logic Apps、run books……

linkedin.com](http://linkedin.com/in/kuharan) 

谢谢你撑到了最后。祝大家 2021 新年快乐。