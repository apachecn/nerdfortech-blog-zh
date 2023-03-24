# 如何在 Ubuntu 20.04 上安装 PostgreSQL

> 原文：<https://medium.com/nerd-for-tech/how-to-install-postgresql-on-ubuntu-20-04-8182c06483f2?source=collection_archive---------2----------------------->

![](img/ff3f515399b385394d35c4becdbfb0c9.png)

# 先迈出第一步

我们将使用普通的旧的 Ubuntu 库[到](https://releases.ubuntu.com/20.04/) [PostgreSQL](https://www.postgresql.org/) 。这是在 Ubuntu 及其版本上最简单、最方便的安装方式。步骤 1:不要忘记更新您的存储库:

```
sudo apt-get update
```

之后，你就可以开始安装了。
第二步:安装 PostgreSQL

```
sudo apt install postgresql postgresql-contrib
```

如果一切顺利，您应该检查已安装服务的状态，以确保它正在运行:

```
sudo service postgresql status
```

如果它没有运行，您可以使用以下命令启动:

```
sudo service postgresql start
```

第三步:创建一个新用户

你可能想要创建一个新用户，最简单的方法是使用交互模式:

```
sudo -u postgres createuser --interactive
```

系统将提示您输入新角色的名称，以及该角色是否应该是超级用户:

```
Enter name of role to add: test_user Shall the new role be a superuser? (y/n) y
```

**步骤 4:** 为新创建的用户(或默认 *postgres* 账户)分配密码

为新创建的角色分配一个密码可能是个好主意。最简单的方法是使用 PostgreSQL 提示符:

```
sudo -u postgres psql
```

出现提示后，您可以键入*\ password previous _ picked _ user _ name*

```
postgres=# \password test_user
```

遵循同样的策略，您可以更改默认的 *postgres* 用户密码:

```
postgres=# \password postgres
```

第五步:创建你的第一个数据库

当您完成设置您的用户时，您可以创建一个新的数据库(在我的例子中是 *testDb* ):

```
sudo -u postgres createdb testDb
```

**步骤 6:** 将步骤 5 中创建的数据库分配给步骤 4 中用户创建的数据库

创建新数据库后，剩下要做的就是将以前创建的用户设置为数据库的新所有者:

```
ALTER DATABASE testDb OWNER TO test_user;
```

这就是开始使用 PostgreSQL 所需要做的一切。尽情享受吧！

# 资源

1.  [在 Windows 上设置 WSL2、PostgresQL 和 Phoenix live view](https://dev.to/ohaleks/set-up-wsl2-postgresql-and-phoenix-liveview-on-windows-3ol5)
2.  [如何在 Ubuntu 20.04 上安装 PostgreSQL【快速入门】](https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart)
3.  如何更改 PostgreSql 数据库的所有者？

*原载于 2020 年 11 月 23 日*[*https://Mike vas . tech*](https://mikevas.tech/how-to-install-posgresql-ubuntu-20-04/)*。*