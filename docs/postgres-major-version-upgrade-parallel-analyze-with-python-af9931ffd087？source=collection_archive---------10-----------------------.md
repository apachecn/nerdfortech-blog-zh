# Postgres —使用 Python 的并行作业

> 原文：<https://medium.com/nerd-for-tech/postgres-major-version-upgrade-parallel-analyze-with-python-af9931ffd087?source=collection_archive---------10----------------------->

![](img/dba4fa38dcff7f6106f1aafe62666ea1.png)

最近，我们的团队收到了 AWS 针对我们的 Aurora Postgres 集群的多份通知，内容涉及 Postgres 版本 9.6.x 的弃用。我们必须将这些数据库升级到更高的主要版本(Aurora Postgres v10.x.x)。

这里有一点很重要，Postgres 在主要版本升级中不复制统计数据。这意味着对于一个关键的查询可能会出现计划翻转，这将导致应用程序延迟变得更高，并影响整体数据库性能(CPU 峰值等)。

很明显，我想到了在升级后并行分析这些表(仅在停机时间内)。为了使这一步更顺利，我创建了一个小的 Python 脚本来并行运行这些命令。我们可以向该实用程序提供一个文件，其中包含我们希望以并行方式运行的所有查询。

**用法:**

```
python3 pg_parallel_sql.py -e pgdb-pg.db -d pgdb -po 5200 -u useradmin -pa 48 -f analyze.sql -eoe-h, --help    : show this help message and exit                      
-e, --endpoint: endpoint identifier of Postgres Cluster [Mandatory ]   
-d, --database: Postgres database name  [ Mandatory ]                      
-po, --port   : Postgres port number [ Optional, Default: 8192 ]    
-u, --user    : Username for authentication [ Mandatory ]                 
-pa,--parallel: Parallel processes to run analyze commands
                [Optional, [1-96]]      
-f, --file     : filename containing all the queries [ Mandatory ]      
                Queries should be termiated by ";"                     
-eoe, --exitonerror: Exits on 1st sql query error otherwise continue     
                     [Optional, Default: False ]
```

**输出:**

```
2021-04-12 16:56:25,969 INFO:Schemas by Table count
+--------------------+------------+
|     SchemaName     | TableCount |
+--------------------+------------+
|   aws_oracle_ext   |     7      |
| information_schema |     7      |
|       public       |     23     |
|     pg_catalog     |     55     |
|       appschema    |    151     |
|       admin        |    259     |
+--------------------+------------+
2021-04-12 16:56:29,538 INFO:Processing Queries in file analyze.sql
100%|█████████████████████████████| 145/145 [00:54<00:00,  2.68it/s]
2021-04-12 16:57:24,201 INFO: 0 error(s) in log file /tmp/pg_parallel_sql.log
2021-04-12 16:57:24,202 INFO:Total execution time: 0:00:58.232558
```

密码

**更新:**

上面的 python 脚本可以方便地并行运行任务并显示进度，尽管还有其他方法来完成相同的任务。

1.  这是一个很好的客户端实用程序，可以在集群范围内运行 vacuum/analyze 命令。它支持“—分阶段分析”和“—作业”参数，以增量和并行方式运行分析命令。
2.  并行运行命令的另一个有趣且简单得多的方法是使用“xargs”命令。

```
xargs -d "\n" -n 1 -P 20 psql database_name username -c < list_of_commands.txt
```

ref:[https://markandruth . co . uk/2016/05/26/running-lots-of-postgres-commands-in-parallel](https://markandruth.co.uk/2016/05/26/running-lots-of-postgres-commands-in-parallel)

**结束语:**这是完成工作的一个基本而快速的脚本，但不是一个非常高效的脚本。它还可以进一步改进，比如对数据库连接使用连接池，分块读取文件以避免内存问题，然后可能是一个更好的日志记录程序。