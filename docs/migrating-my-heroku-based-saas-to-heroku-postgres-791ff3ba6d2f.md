# 将我的基于 Heroku 的 SaaS 迁移到 Heroku Postgres

> 原文：<https://medium.com/nerd-for-tech/migrating-my-heroku-based-saas-to-heroku-postgres-791ff3ba6d2f?source=collection_archive---------36----------------------->

![](img/0c93efd7b33d030de2cd5485424100cf.png)

在我 30 多年的信息技术职业生涯中，我遇到过很多已经进入生产状态的概念验证或原型应用程序。这一直以来都是与被创作作品的初衷直接冲突的。

虽然这应该被认为是对出色工作的某种形式的赞美，但更多的时候，在那些原型开发实践中做出的决策并没有被贴上生产支持友好的标签。在某种程度上，我最初为我嫂子设计的基于[Heroku](https://www.heroku.com/)T2 的【SaaS】健身方案也存在同样的情况。

现在，是时候完成这部分旅程了。

# 概述健身之旅(迄今为止)

对于那些不熟悉的人，我在 2020 年疫情发作期间开始为我的嫂子(她是一名私人教练)构建一个健身应用程序。这个想法后来变成了在 Heroku 运行的 SaaS 解决方案——为美国各地的私人教练提供多租户设计。

以下是本系列中每篇文章的清单:

* [使用 Heroku 快速构建多租户 SaaS 产品](https://dzone.com/articles/using-heroku-to-quickly-build-a-multi-tenant-saas) —介绍和初步设计
* [将 Twilio 集成到我在 Heroku 的 SaaS 解决方案中](https://dzone.com/articles/integrating-twilio-into-my-saas-solution-in-heroku) —为最终用户介绍 SMS 功能
* [在我基于 Heroku 的 SaaS 解决方案中利用 cloud amqp](/git-connected/using-heroku-to-quickly-build-a-saas-product-part-3-leveraging-cloudamqp-419d8137f271)n—添加基于消息的发票处理

这些文章为以下路线图中提到的特性和功能铺平了道路:

![](img/15200568411e39614ac4f2d27607ed90.png)

1.0.5 版本将会有一些使用机器学习和人工智能的好东西，但是需要进行数据库转换来更好地准备 RESTful API。

# 目标 PostgreSQL

最初，我使用 MySQL 作为底层数据源，因为我的重点是收集应用程序初始版本的必要需求。由于我已经将 MySQL 用于我在 Heroku 上运行的另一个应用程序，所以很容易利用我现有的知识。

我知道我的数据层的最终状态将是 [Heroku Postgres](https://elements.heroku.com/addons/heroku-postgresql) ，因为 MySQL 有以下优点:

*   免费&开源(FOSS)——我的项目利用了一些令人惊叹的 OSS 框架；数据库从开源中获益是有意义的
*   **对象关系数据库** —可以定义对象和表继承(高级数据结构)
*   **非常适合复杂的查询**——将是目前正在设计的健身自动化功能的重点
*   **多版本并发控制(MVCC)** —无需读写锁即可与数据交互
*   **跨 Dynos 共享**——可以与多个应用程序共享一个付费的 Heroku Postgres 实例(比如我为我岳母编写的应用程序)

# 迁移到 Heroku Postgres

该过程的第一步是将健身客户端(用 [Angular](https://angular.io/) 编写，并通过 [Node.js](https://nodejs.org/en/) 在 Heroku 上提供)和服务(使用 [Spring Boot](https://spring.io/projects/spring-boot) )置于维护模式。这可以通过对每个 Dyno 使用以下 Heroku CLI 命令来完成:

```
heroku maintenance:on
```

以下步骤介绍了从 MySQL 转换到 Heroku Postgres 的整个过程。

**1。建立 Heroku Postgres 附加组件**

从 MySQL 迁移到 Heroku Postgres 的第一步是添加 Heroku Postgres my fitness API。这与执行以下 Heroku CLI 命令一样简单:

```
heroku addons:create heroku-postgresql:hobby-dev
```

爱好开发计划将是暂时的，直到我准备关闭我现有的 MySQL 数据库。例如，我们假设从 hobby-dev 实例返回了以下数据库结果:

```
Creating heroku-postgresql:hobby-dev on ⬢ sushi… free
Database has been created and is available
! This database is empty. If upgrading, you can transfer
! data from another database with pg:copy
Created postgresql-acute-52767 as DATABASE_URL
```

使用“heroku config”CLI 命令，可以从 Heroku 检索完整的 DATABASE_URL，其中包括访问数据库所需的系统帐户 ID 和密码。例如，我们假设检索了以下信息:

```
postgres://user:[password@somehost.compute-1.amazonaws.com](mailto:password@somehost.compute-1.amazonaws.com):5432/someDatabase
```

接下来，我将使用我的 MacBook Pro 在本地进行数据转换。第一步是在本地环境中设置 DATABASE_URL:

```
export DATABASE_URL=postgres://user:password@somehost:5432/someDatabase
```

使用' echo $DATABASE_URL '将验证环境变量是否可用。

**2。在本地安装 PostgreSQL】**

为了在本地执行迁移，我的 MacBook Pro 上安装了 [Postgres.app](https://postgresapp.com/) (支持 PostgreSQL 版本 13)。

接下来安装了 Postgres 命令行工具:

```
sudo mkdir -p /etc/paths.d && echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp
```

我重启了我的终端会话，然后使用以下命令验证 Postgres 工具运行正常:

```
which psql
```

对此的回应是:

```
/Applications/Postgres.app/Contents/Versions/latest/bin/psql
```

我验证了可以使用以下命令从 Heroku 访问 Postgres 信息:

```
heroku pg:info
```

收到了以下答复:

```
=== DATABASE_URL
Plan: Hobby-dev
Status: Available
Connections: 0/20
PG Version: 13.2
Created: 2021–05–03 04:13 UTC
Data Size: 7.9 MB
Tables: 0
Rows: 0/10000 (In compliance)
Fork/Follow: Unsupported
Rollback: Unsupported
Continuous Protection: Off
Add-on: postgresql-acute-52767
```

**3。使用 pgloader 迁移数据**

[pgloader](https://github.com/dimitri/pgloader) 是一个将数据从 MySQL 迁移到 PostgreSQL 的开源库。使用[自制软件](https://brew.sh/)，安装快速简单:

```
brew install pgloader
```

安装完成后，我可以使用以下命令运行 pgloader:

```
pgloader –version
```

该命令返回了以下信息:

```
pgloader version “3.6.2”
compiled with SBCL 2.0.11
```

接下来，我使用以下命令创建了健身数据库的 PostgreSQL 版本:

```
createdb fitness
```

使用数据库 URL 和我刚刚创建的新 PostgreSQL 版本，使用以下命令开始转换:

```
pgloader mysql://userId:password@hostname/oldFitnessDatabase postgresql://localhost/fitness
```

从那里，pgloader 命令毫不费力地完成了迁移。它提供了以下信息:

```
2021–05–03T06:05:38.013630+01:00 LOG pgloader version “3.6.2”
2021–05–03T06:05:38.087483+01:00 LOG Migrating from #<MYSQL-CONNECTION mysql:// userId:password@hostname/oldFitnessDatabase {1004FE7A53}>
2021–05–03T06:05:38.087632+01:00 LOG Migrating into #<PGSQL-CONNECTION pgsql://john.vester@localhost:5432/fitness {1005142EA3}>2021–05–03T06:05:42.536036+01:00 LOG report summary reset
… report information here …
Total import time ✓ 4830 231.6 kB 3.150s
```

我能够建立到本地 PostgreSQL 数据库的连接，并确认所有预期的数据都在那里。

**4。将数据输入 Heroku Postgres**

本地 PostgreSQL 数据库包含来自 MySQL 的真实数据，下一步是将这些更改推入 Heroku Postgres 实例。

为了加载数据，可以使用以下 CLI 命令创建数据库的压缩版本:

```
PGPASSWORD=mypassword pg_dump -Fc — no-acl — no-owner -h localhost -U userId fitness > fitness.dump
```

使用我现有的(但很少使用的)AWS S3 帐户，我将 fitness.dump 上传到 AWS S3，并使用以下命令生成了一个预签名的 URL:

```
aws s3 presign s3://myBucket/myKey
```

最后一步是将数据恢复到新创建的 Heroku Postgres 实例中:

```
heroku pg:backups:restore ‘<SIGNED URL>’ DATABASE_URL
```

DATABASE_URL 是指添加到本地环境中的属性。建立 Heroku Postgres 附加软件”一节。

我能够建立到 Heroku Postgres 数据库的连接，并确保所有预期的数据都在那里。

**5。更新 Spring Boot 服务**

数据库更新后，最后一步是更改 Spring Boot RESTful 服务中的“pom.xml”来删除它:

```
<dependency>
 <groupId>mysql</groupId>
 <artifactId>mysql-connector-java</artifactId>
 <scope>runtime</scope>
</dependency>
```

然后用这个替换它:

```
<dependency>
 <groupId>org.postgresql</groupId>
 <artifactId>postgresql</artifactId>
 <scope>runtime</scope>
</dependency>
```

启动 Spring Boot 服务没有导致任何错误，数据转换也没有任何问题。

该过程的最后一步是禁用健身客户端和服务的维护模式。这可以通过对每个 Dyno 使用以下 Heroku CLI 命令来完成:

```
heroku maintenance:off
```

# 结论

下面的文章展示了将现有的 MySQL 数据库迁移到 Heroku Postgres 是多么容易。当然，我强烈建议首先在非生产环境中执行和验证这些步骤。虽然该工具看起来是一流的，并且专注于没有任何问题地转换数据，但是在不会影响客户的环境中执行这样的任务总是很好的。

在我的场景中，从 MySQL 转换到 Heroku Postgres 的成本不是问题，因为价格差异每月只有几美分。然而，有了 Heroku Postgres 的实现，我就有可能为我岳母的应用程序从 MySQL 的免费版本迁移到使用相同的 Heroku Postgres 实例。

然而，同样的价格，我感到欣慰的是，我有一个数据库，专注于满足我在 Heroku 运行 SaaS 解决方案的健身应用程序之旅中面临的挑战性需求。

祝你今天过得愉快！