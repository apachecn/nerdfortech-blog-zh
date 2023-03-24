# 使用 docker 在单个主机上设置多节点

> 原文：<https://medium.com/nerd-for-tech/cassandra-multinode-setup-on-a-single-host-using-docker-fe3d8b844f52?source=collection_archive---------7----------------------->

![](img/43cfbea970941207f67be111a7bc1689.png)

这个帖子不是关于卡珊德拉数据库的细节。相反，它致力于如何使用 docker 在一台主机上启动 cassandra 的多节点设置。对于个人项目，让每个人都有多个主机系统来理解 NoSQL 多 DC 场景，这实在是不可行或负担不起的。

# 步伐

我假设您的系统中已经安装了 docker。

# 假设有一个新的开始

检查 docker 实例

![](img/3a1e34093c5c093ce6c3394fd11915e2.png)

# *创建第一个节点—暴露在端口 9042 上*

> *>*docker run-p 9042:9042—name my-Cassandra-1-m 2g-d Cassandra:3.11

![](img/0d693b398a79b92c71e8335ba04149ee.png)

# 检查 IP

使用上面的命令检查容器的 ip

> > docker inspect—format = ' { { . network settings . IP address } } ' my-Cassandra-1

![](img/db2d1e3ef7837232bf9801cfb543ba9e.png)

# 创建另一个节点并链接到 prev 节点—暴露在端口 9043 上

> > docker run—name my-CASSANDRA-2-m 2g-d-e CASSANDRA _ SEEDS = " $(docker inspect—format = ' { { . network settings . IP address } } ' my-CASSANDRA-1)" CASSANDRA:3.11

![](img/060bdc8decbecb20a828d7cf37785b5d.png)

# 检查 cassandra 节点状态

> > docker exec-I-t my-Cassandra-1 bash-c '节点工具状态'

![](img/cd190449b471731c8711529d294a382a.png)

# 通过客户端 cqlsh 使用 cassandra

> 通过客户端 cqlsh
> >docker run-it—link my-Cassandra-1—RM Cassandra:3.11 bash-c ' exec cqlsh<<IP>>'

在另一个 docker 容器中使用 cqlsh 客户端来处理数据库

就是这样。在一台主机上安装多节点就是这么简单。