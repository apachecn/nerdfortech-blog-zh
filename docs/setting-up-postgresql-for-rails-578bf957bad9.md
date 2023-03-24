# 为 Rails 设置 PostgreSQL

> 原文：<https://medium.com/nerd-for-tech/setting-up-postgresql-for-rails-578bf957bad9?source=collection_archive---------1----------------------->

新手指南:致我们未来的 ROR 开发者！

![](img/09435b0b34421fa0c63021c6b48c5350.png)

为新项目设置 PostgresSQL 非常简单。只需在终端中发出几个命令，就大功告成了。

我仍然记得，在我的第一个 ROR 项目中，我被分配了一个使用 PostgreSQL 数据库而不是 SQLite 的任务。由于我是 rails 和数据库技术的新手，我花了几个小时试图弄清楚如何设置它。

作为一名好奇的开发人员，您可能会想到一个问题，“既然我已经设置了 SQLite，为什么还要使用 PostgreSQL 数据库？”这个问题的答案非常宽泛，当我们考虑其他现有的数据库技术，如 MongoDB、MySQL 等时，为我们的应用程序选择哪一种会变得更加混乱。

我不会在这里回答这个问题，因为它超出了本文的范围，但是，这里有一些文章的链接，这些文章对这个问题进行了全面的解释，后一篇文章将指导您在系统中安装 PostgreSQL。

[](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems) [## SQLite vs MySQL vs PostgreSQL:关系数据库管理系统的比较

### 关系数据模型在数据库管理中占主导地位，它以行和列的表格来组织数据

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/sqlite-vs-mysql-vs-postgresql-a-comparison-of-relational-database-management-systems) [](https://fulcrum.rocks/blog/why-use-postgresql-database/#:~:text=Postgres%20allows%20you%20to%20store,tasks%20and%20create%20integral%20environments.&text=We%27ll%20compare%20Postgres%20with,choose%20PostgreSQL%20over%20other%20options.) [## 为什么在 2021 年用 PostgreSQL 作为我下一个项目的数据库

### 什么是 PostgreSQL 数据库？数据完整性超级重要。在所有事情上。句号。在现代技术中…

支点.岩石](https://fulcrum.rocks/blog/why-use-postgresql-database/#:~:text=Postgres%20allows%20you%20to%20store,tasks%20and%20create%20integral%20environments.&text=We%27ll%20compare%20Postgres%20with,choose%20PostgreSQL%20over%20other%20options.) 

现在，让我们开始吧。

为了确保您已经在系统中正确安装了 PostgreSQL，请运行以下命令:

```
psql --version
```

它将给出您安装在系统中的 PostgreSQL 版本的输出。

要在您的 web 应用程序中使用 PostgreSQL 数据库，您需要创建一个 Postgres 用户。

如果你还没有在 Postgres 上设置用户，下面是设置新用户的方法:

```
sudo -u postgres createuser -s username
```

用您想保留的用户名替换`username`。

要为此用户设置密码，请登录 PostgreSQL 命令行客户端:

```
sudo -u postgres psql
```

输入以下命令设置密码:

```
\password username
```

将`username`替换为您的，然后会提示您输入两次密码。输入您想要保留的密码。

确保用户已经创建，该命令将给出所有用户的列表:

```
\du
```

然后通过以下方式退出 PostgreSQL 客户端:

```
\q
```

您的 Postgres 用户已经设置完毕。现在我们需要告诉 rails 使用这个用户来创建 Postgres 数据库。在此之前，如果您还没有设置 rails 应用程序，请遵循以下步骤:

确保使用正确的 ruby 版本来创建新的 rails 应用程序。对于这个设置，我将使用 2.7.2 版本，这可能会生成 rails 版本 6.1.4.1 的 rails 应用程序。

```
$ rvm use 2.7.2$ rails new app_name --database=postgresql
```

额外的`— database=postgresql`命令告诉 rails 使用 Postgres 数据库，而不是 SQLite(rails 默认使用 SQLite)。它会在你的 gem 文件中添加 Postgres gem，`gem pg`。现在在你的 gem 文件中安装所有的 gem:

```
bundle install
```

导航到`config/database.yml`文件，然后**在`default: &default`下添加**该代码

```
default: &default
  adapter: postgresql
  encoding: unicode
  host: localhost
  username: username
  password: your_password
   pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
```

添加您想要用来为所有 3 个环境创建数据库的 Postgres 用户的凭据。

在我的例子中，我希望所有 3 个环境(开发、生产和测试)都使用`username` Postgres 用户和在`default: &default`下给出的其他`key:value`对。

如果您想为不同的环境使用不同的用户，那么您需要为各自的环境显式地给出凭证`key:value`。比如:

```
development:
  <<: *default
  username: username
  password: your_passwordtest:
  <<: *default
  username: username_2
  password: your_password_2
```

您也可以通过`database`键更改这些环境的数据库名称。

完成这个设置后，我们需要告诉 Rails 通过以下方式创建这些 Postgres 数据库:

```
rails db:create
```

瞧啊。！就是这样！

**注意:**您不必为您创建的每个 rails 应用程序创建一个新的 Postgres 用户，您可以使用一个现有的用户，只需将其凭证添加到`database.yml`中，如图所示。

尽情享受吧！

[我们连线吧！](https://juzershakir.github.io/)

# 额外资源

[](https://www.digitalocean.com/community/tutorials/understanding-sql-and-nosql-databases-and-different-database-models) [## 了解 SQL 和 NoSQL 数据库以及不同的数据库模型|数字海洋

### 自古以来，计算机最需要和依赖的功能之一就是内存…

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/understanding-sql-and-nosql-databases-and-different-database-models)