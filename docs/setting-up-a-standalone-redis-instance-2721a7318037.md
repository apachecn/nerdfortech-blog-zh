# 设置独立的 Redis 实例

> 原文：<https://medium.com/nerd-for-tech/setting-up-a-standalone-redis-instance-2721a7318037?source=collection_archive---------0----------------------->

![](img/761772f831bd7ef7bcdcd33fb4bf8f66.png)

Redis 设置

谈到缓存，Redis 是一个非常流行的内存数据库。这篇文章介绍了在 docker 中设置 Redis 实例的基本配置。

# 目录结构

创建一个名为`redisdb`的目录，并确保以下文件结构。

redisdb/
├──数据/
├── redis.conf

`data`目录将包含随时间拍摄的快照(如`.rdb`和/或`.aof`)。

`redis.conf`文件将包含实例配置。

# 配置

一般来说，下面的配置似乎很重要。其余的可以保留默认值。

## 网络

```
bind 0.0.0.0       # bind to all available interfaces
protectedmode no   # since we're not serving over localhost
port 6379          # default porttcp-backlog 511    # high backlog value helps avoid slow client 
                     connection issues, helpful in 
                     high-request-per-second environmentstimeout 0          # disable idle connection timeout
tcp-keepalive 300
```

## 一般

因为我们将在容器中运行它，所以不需要服务的后台进程。

```
daemonize no       
supervised no
databases          # valid between 1-16\. Indexed with 0
```

## 给…拍快照

这些设置将从内存中生成数据的快照，并以文件扩展名`.rdb`将它们保存在我们根据指定的策略在上面创建的`data/`目录下。

```
save 900 1                      # save <seconds> <writes>
save 300 10
save 60 10000stop-writes-on-bgsave-error yes # stop accepting writes if
                                  snapshotting fails. This limits 
                                  the risk of losing data.
```

## 安全性

基本的安全性包括设置服务器密码。

```
requirepass PASSWORD # set the server password
```

但是，如果数据是敏感的，仅仅使用服务器密码进行身份验证是有风险的。因为一个`FLUSHALL`命令就可以删除一切。它可以通过[访问控制列表](https://redis.io/topics/acl) (ACL)来缓解，该列表包含用户名/密码以及允许的命令列表。

## **内存管理(重要)**

由于 Redis 是一个内存数据库，您不希望它淹没运行它的节点，从而危及所有其他服务。合适的内存管理策略可以扭转局面。

驱逐政策

**驱逐政策**

`noeviction`:当达到内存限制并且客户端试图执行可能导致使用更多内存的命令时，返回错误。

`allkeys-lru`:通过尝试首先移除最近较少使用的(LRU)键来驱逐键，以便为添加的新数据腾出空间。

`volatile-lru`:通过尝试首先移除最近较少使用的(LRU)密钥来驱逐密钥，但是仅在具有到期设置的密钥中，以便为添加的新数据腾出空间。

`allkeys-random`:随机驱逐密钥，为添加的新数据腾出空间。

`volatile-random`:随机驱逐密钥，以便为添加的新数据腾出空间，但只驱逐设置了过期的密钥。

`volatile-ttl`:驱逐设置过期的密钥，首先尝试驱逐生存时间(TTL)较短的密钥。

可用驱逐策略的列表可以在[这里找到](https://redis.io/topics/lru-cache)。

## 坚持

Append Only File ( `aof`)是另一种持久性模式，与`rdb`相比，它提供了更好的持久性，因为它创建了类似于时间点快照的东西。

Redis 持久性

与`aof`文件相关的一些问题是，

*   AOF 文件大小与写入次数成正比。会越来越大。
*   AOF 文件越大，服务器重启的时间就越长。
*   因此，我们需要一种不时压缩 AOF 的方法。同样以非阻塞的方式，在服务器运行时接收读和写查询。

> 因此，只有当您拥有非常敏感的数据并且无法承受丢失时，才考虑此选项。

[这里是`rdb`和`aof`选项详细比较的链接](https://redis.io/topics/persistence)。

# 启动实例

在`redisdb/`目录下运行以下命令，因为本地路径在`-v`选项中必须是绝对路径。

启动 Redis 实例

# 测试连接

测试 Redis 连接