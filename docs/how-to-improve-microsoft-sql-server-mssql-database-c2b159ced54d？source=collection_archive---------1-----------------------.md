# 如何提高 Microsoft SQL Server 数据库性能

> 原文：<https://medium.com/nerd-for-tech/how-to-improve-microsoft-sql-server-mssql-database-c2b159ced54d?source=collection_archive---------1----------------------->

![](img/206d5c4c42ac24fec46ea73f7e03e50e.png)

来源:Pixabay

有时，我们的数据库甚至外部数据库需要在数据结构方面进行一些优化。

根据我的经验，我们可以采取三种方法:一种更基本、更快速，另一种更深入，包括重构和重新规划数据库，最后一种是前两种方法的混合。

在这篇文章中，我想分享这个微软数据库引擎的基本和快速的方法。如果你想看到类似的内容，但对于 MySQL/MariaDB，请查看我的另一篇文章:[如何提高 MYSQL / MariaDB 数据库性能](https://pirex360.medium.com/how-to-improve-mysql-mariadb-database-performance-181f270d7fbe)。

我有很多项目需要将现有的客户端 MSSQL 数据库集成到一个定制的 web 应用程序中，原因有很多，比如获取客户、供应商、产品和各种数据。有时我会遇到性能问题，瓶颈是外部数据库，在大多数情况下使用这种方法可以获得可见的结果，但这总是取决于目标数据库的状态。

数据库中的索引在这种方法中起着主要作用，在大型数据库中，重建索引可能会将数据库的性能带到另一个级别，遗憾的是，索引的糟糕实现也可能会使数据库变慢(这是一个更高级的主题，不在本文讨论之列)。

因此，让我们加快我们的数据库！
*注意:它是安全的，您的数据库数据没有风险！*

您可以使用以下 SQL 命令手动按表重建(只需更改数据库表的 table_name):

```
ALTER INDEX ALL ON table_name REBUILD ;
```

或者，您可以对整个数据库执行这个更高级的脚本，这将获取所有数据库表，并对每个表使用上面的命令，只需将第 7 行中的 database_name 替换为您的目标数据库名:

```
DECLARE [@Database](http://twitter.com/Database) NVARCHAR(255)   
DECLARE [@Table](http://twitter.com/Table) NVARCHAR(255)  
DECLARE [@cmd](http://twitter.com/cmd) NVARCHAR(1000)DECLARE DatabaseCursor CURSOR READ_ONLY FOR  
    SELECT name FROM master.sys.databases   
    WHERE name IN ('database_name')  -- databases
    AND state = 0 -- database is online
    AND is_in_standby = 0 -- database is not read only for log shipping
    ORDER BY 1OPEN DatabaseCursorFETCH NEXT FROM DatabaseCursor INTO [@Database](http://twitter.com/Database)  
    WHILE @@FETCH_STATUS = 0  
    BEGINSET [@cmd](http://twitter.com/cmd) = 'DECLARE TableCursor CURSOR READ_ONLY FOR SELECT ''['' + table_catalog + ''].['' + table_schema + ''].['' +  
       table_name + '']'' as tableName FROM [' + [@Database](http://twitter.com/Database) + '].INFORMATION_SCHEMA.TABLES WHERE table_type = ''BASE TABLE'''-- create table cursor  
       EXEC ([@cmd](http://twitter.com/cmd))  
       OPEN TableCursorFETCH NEXT FROM TableCursor INTO [@Table](http://twitter.com/Table)   
       WHILE @@FETCH_STATUS = 0   
       BEGIN
          BEGIN TRY   
             SET [@cmd](http://twitter.com/cmd) = 'ALTER INDEX ALL ON ' + [@Table](http://twitter.com/Table) + ' REBUILD' 
             PRINT [@cmd](http://twitter.com/cmd) -- uncomment if you want to see commands
             EXEC ([@cmd](http://twitter.com/cmd)) 
          END TRY
          BEGIN CATCH
             PRINT '---'
             PRINT [@cmd](http://twitter.com/cmd)
             PRINT ERROR_MESSAGE() 
             PRINT '---'
          END CATCHFETCH NEXT FROM TableCursor INTO [@Table](http://twitter.com/Table)   
       ENDCLOSE TableCursor   
       DEALLOCATE TableCursorFETCH NEXT FROM DatabaseCursor INTO [@Database](http://twitter.com/Database)  
    END  
    CLOSE DatabaseCursor   
    DEALLOCATE DatabaseCursor
```

就是这样，执行并等待一段时间(取决于您的数据库大小)…然后测试您的查询的性能。

希望这些信息能帮助到别人。