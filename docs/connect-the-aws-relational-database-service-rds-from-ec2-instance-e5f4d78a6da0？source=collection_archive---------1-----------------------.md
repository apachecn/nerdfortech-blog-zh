# 从 EC2 实例连接 AWS 关系数据库服务(RDS)

> 原文：<https://medium.com/nerd-for-tech/connect-the-aws-relational-database-service-rds-from-ec2-instance-e5f4d78a6da0?source=collection_archive---------1----------------------->

创建 EC2 实例并连接 RDS PostgreSQL 数据库实例。

考虑到以下实例已经创建，
1。RDS PostgreSQL DB 实例(参见[本篇](https://er-kalpanasharma.medium.com/creating-an-aws-rds-mysql-instance-and-connect-to-it-using-python-ea6292df3e1c)文章)
2。EC2 实例

# **使用 ssh 客户端连接到 EC2 实例:**

> 1.打开一个 SSH 客户端。
> 2。找到您的私钥文件。用于启动此实例的密钥是{permissionkey}。pem
> 3。使用其公共 DNS 连接到实例:{公共 dns}
> **示例:**
> ssh -i "{permissionkey}。PEM "[Ubuntu @](mailto:ubuntu@ec2-3-131-94-210.us-east-2.compute.amazonaws.com){公共 dns}

# **连接 ssh 客户端后，更新操作系统并安装 PostgreSQL 客户端。**

> sudo apt-get 更新&& sudo apt-get 升级
> sudo apt-get 清除 postgresql*
> sudo apt-get -f 安装
> sudo apt-get 安装 postgresql

# **连接 PostgreSQL 客户端，**

> root @ IP-{ IP address }:/home/Ubuntu # psql—host = { RDS Endpoint }—port = 5432—username = { RDS DB 实例的用户名} — password — dbname=test_db

然后，输入 RDS 数据库实例的密码

一旦连接到 PostgreSQL 客户端，您将能够访问 test_db 数据库。

> **test_db= >** 创建部门表(id integer，name varchar)；
> 创建表格
> 
> **test_db= >** 插入部门(id，name)值(1，' IT ')；
> 插入 0 1
> 
> **test _ db =>**SELECT * FROM 部门；
> id | name
> —+—
> 1 | IT
> (1 行)
> 
> **test_db= >** 创建表用户(id integer，name varchar，department varchar)；
> 创建表格
> 
> **test_db= >** 插入用户(id，姓名，部门)值(1，' Amol '，1)；
> 插入 0 1
> 
> **test _ db =>**SELECT * FROM users；
> id |名称|部门
> —+—+——
> 1 | Amol | 1
> (1 行)

也可以使用 DB 工具连接 RDS DB 实例，[https://docs . AWS . Amazon . com/Amazon RDS/latest/user guide/CHAP _ getting started。CreatingConnecting.PostgreSQL.html](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_GettingStarted.CreatingConnecting.PostgreSQL.html)