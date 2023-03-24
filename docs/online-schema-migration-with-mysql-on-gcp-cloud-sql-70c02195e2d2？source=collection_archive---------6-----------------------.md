# 在 GCP 上使用 MySQL 进行在线模式迁移(云 SQL)

> 原文：<https://medium.com/nerd-for-tech/online-schema-migration-with-mysql-on-gcp-cloud-sql-70c02195e2d2?source=collection_archive---------6----------------------->

这是一篇关于在 Cloud SQL 上使用 MySQL 尝试在线模式迁移的笔记。我尝试了这两种工具:

*   pt-在线-模式-更改
*   gh-ost

# 条款

我在以下条件下用 MySQL 测试了它们:

*   云 SQL 第二代
*   MySQL 版本 5.7(截至本文撰写时默认)
*   [启用二进制日志记录](https://cloud.google.com/sql/docs/mysql/backup-recovery/pitr)
*   通过[云 SQL 代理](https://cloud.google.com/sql/docs/mysql/sql-proxy)访问

![](img/321fab4c91acf4a732f26d283f925a6b.png)

# [在线 DDL](https://dev.mysql.com/doc/refman/5.7/en/innodb-online-ddl-operations.html)

执行在线迁移时，首先想到的是[在线 DDL](https://dev.mysql.com/doc/refman/5.7/en/innodb-online-ddl-operations.html) 。但是，有[限制](https://dev.mysql.com/doc/refman/5.7/en/innodb-online-ddl-limitations.html)。

在线 DDL 的主要优势是你不需要依赖任何额外的工具，因为 MySQL 为你提供了所有的工具。在某些情况下，您可以访问正在迁移的表(并发 DML ),并执行不需要大量磁盘空间或 I/O 的就地迁移。

然而，关于在线 DDL 有一个主要的问题:即使你用`LOCK=None`运行一个查询，MySQL 也只能在主服务器上在线执行，阻止副本应用 DML，直到主服务器完成你的 DDL。因此，在大型表上运行 DDL 会导致显著的复制延迟。

此外，无法限制或暂停迁移过程，因此无法控制主服务器上的负载。

这些问题不是 Cloud SQL 特有的，而是 MySQL (InnoDB)普遍存在的。

如果您不关心加载或复制延迟，在线 DDL 可能是一个不错的选择，但是如果您运行的服务提供更高的服务级别，这种情况就不常见了。因此，我最近试用了两个在线迁移工具，看看它们是否可以用于云 SQL。

# [pt-online-schema-change(pt-OSC)](https://www.percona.com/doc/percona-toolkit/3.0/pt-online-schema-change.html)

Percona 的 pt-osc 是一个在线模式迁移工具，已经流行了很多年。

它用所需的模式创建一个新表，然后开始从原始表中复制行。它还创建触发器来镜像在迁移到新表的过程中原始表中发生的更改。最后，pt-osc 重命名该表，在原始表和新表之间切换。

## 使用云 SQL 运行 pt-osc

当我尝试使用 Cloud SQL 运行 pt-osc 时，我必须注意以下几点。

**1)** `**log_bin_trust_function_creators**` **必须启用**

当 pt-osc 创建触发器时，它需要一个拥有超级权限的用户，但是[这在 Cloud SQL](https://cloud.google.com/sql/faq#grantall) 中是不可能的。相反，您可以在实例中启用`[log_bin_trust_function_creators](https://dev.mysql.com/doc/refman/5.7/en/replication-options-binary-log.html#sysvar_log_bin_trust_function_creators)`来允许所有用户创建存储函数和触发器。你可以在这里找到如何更改云 SQL [上的数据库标志。](https://cloud.google.com/sql/docs/mysql/flags)

**2)用**明确指定副本`**--check-slave-lag**`

pt-osc 监控复制延迟，如果检测到大于`--max-lag`的延迟，则暂停并调节自身。默认情况下，pt-osc 试图通过在主服务器上运行`SHOW SLAVE HOSTS`和`SHOW PROCESSLIST`来寻找副本。但是，只要您让 pt-osc 通过云 SQL 代理连接到 MySQL 服务器，pt-osc 就应该无法访问从 MySQL 服务器检索到的主机信息。您会看到以下警告消息:

```
No slaves found. See — recursion-method if host localhost has slaves.
Not checking slave lag because no slaves were found and — check-slave-lag was not specified.
```

pt-osc 提供了一种显式传递副本主机信息的方式。我可以通过添加一个类似于`—-check-salve-lag D=foo,h=127.0.0.1,P=33001`的标志让它识别复制品。要查看这是否成功，您可以检查日志。

```
Will check slave lag on:
localhost -> 127.0.0.1:33001
```

**pt-OSC 命令执行示例**

```
$ pt-online-schema-change \
  --alter=“DROP COLUNN bar ”D=test,t=foo,h=127.0.0.1,P=33000,u=root,p=pass \
  --check-slave-lag="D=test,h=127.0.0.1,P=33001,u=root,p=pass" \
  --print \
  --execute
```

# [gh-ost](https://github.com/github/gh-ost)

gh-ost 被设计成无触发器的，以避免由触发器引起的问题。你可以在这里看到触发器会导致什么样的问题。

gh-ost 的在线迁移是这样进行的:gh-ost 创建一个具有所需模式的新表，并将原始表中的行复制到新表中。为了将原始表中发生的更改镜像到新表中，gh-ost 连接到 MySQL 服务器并接收 binlog 事件，就像它是一个副本服务器一样。然后，它从 binlog 事件中提取更改，并将它们应用到主数据库中的新表。最后，它执行被称为切换的最后阶段，在那里它交换表。

由于无触发设计，gh-ost 为[提供了更灵活的节流](https://github.com/github/gh-ost/blob/master/doc/throttle.md)和[一些交互式命令](https://github.com/github/gh-ost/blob/master/doc/interactive-commands.md)，可以在不暂停迁移过程的情况下控制其行为。

## 使用云 SQL 运行 gh-ost

当我尝试使用 Cloud SQL 运行 gh-ost 时，我必须注意以下几点。

**1)用** `**--allow-on-master**` **标志**运行

对于 gh-ost，建议从副本服务器读取 binlog 事件，以减少主服务器的负载。它还提供了一种安全测试迁移过程的方法，通过让 gh-ost 连接到副本服务器(使用`— migrate-on-replica`或`--test-on-replica`选项)，您不必接触主服务器。

然而，由于[云 SQL 不允许创建副本](https://cloud.google.com/sql/docs/mysql/replication#rr-info)的副本，因此无法让 gh-ost 使用云 SQL 从副本接收二进制日志。([就像这里提到的](https://github.com/github/gh-ost/issues/770#issuecomment-517305392)，使用外部副本也是可能的)

要让 gh-ost 连接到主机，需要`--allow-on-master`标志。更多详情请参见本文件。

**2)带着** `**--gcp**` **旗跑**

在没有`--gcp`选项的情况下，通过云 SQL 代理运行时，我遇到了以下错误:

```
FATAL Unexpected database port reported: 3306
```

[文档说它是用于第一代](https://github.com/github/gh-ost/blob/master/doc/command-line-flags.md#gcp)，但是如果您通过隧道连接运行它[(即，如果为连接指定的端口不同于实际服务的端口)](https://github.com/github/gh-ost/issues/631#issuecomment-431639248)，它似乎是必需的。

**GH-ost 命令执行示例**

```
$ gh-ost \
  --max-load=Threads_running=25 \
  --critical-load=Threads_running=1000 \
  --chunk-size=1000 \
  --throttle-control-replicas=127.0.0.1:33001 \
  --max-lag-millis=3000 \
  --user=root \
  --password=pass \
  --host=127.0.0.1 \
  --port=33000 \
  --allow-on-master \
  --database=test \
  --table=foo \
  --verbose \
  --alter="DROP COLUMN bar" \
  --default-retries=120 \
  --panic-flag-file=/tmp/host.panic.flag \
  --serve-socket-file=/tmp/ghost.test.foo.sock \
  --gcp \
  --execute
```

# 编后记

我发现这两个工具都可以与云 SQL 一起工作。下一步是在我的生产环境中运行它。如果我在整个操作过程中发现任何特定于云 SQL 和模式迁移的问题，我将更新此注释。

引用作品:

*   [MySQL 5.7 在线 DDL 操作](https://dev.mysql.com/doc/refman/5.7/en/innodb-online-ddl-operations.html)
*   [错误#73196 允许在主服务器和从服务器上同时运行 ALTER TABLE](https://bugs.mysql.com/bug.php?id=73196)
*   [pt-在线-模式-改变](https://www.percona.com/doc/percona-toolkit/3.0/pt-online-schema-change.html)
*   [亚马逊 RDS 和 pt-online-schema-change](https://www.percona.com/blog/2016/07/01/pt-online-schema-change-amazon-rds/)
*   [为什么无触发？](https://github.com/github/gh-ost/blob/master/doc/why-triggerless.md)
*   [运行在谷歌云 SQL 上](https://github.com/github/gh-ost/issues/770)
*   [gh-ost 备忘单](https://github.com/github/gh-ost/blob/master/doc/cheatsheet.md)
*   [在非标准 mysql 端口上通过 ssh 隧道使用 gh-ost？【问题】#631](https://github.com/github/gh-ost/issues/631)