# 如何将客户端/本地缓存与 Redis 等分布式缓存同步

> 原文：<https://medium.com/nerd-for-tech/how-to-sync-client-side-local-cache-with-distributed-cache-like-redis-5ec39160cab8?source=collection_archive---------1----------------------->

很少有应用程序架构像 Redis 一样需要客户端(本地)缓存和分布式缓存的结合。这种混合解决方案可以减少网络调用或数据库调用，并明显提高性能。

这里的主要挑战是保持实际数据、客户端(本地)缓存和分布式缓存的一致性。没有标准解决方案能有效解决这种混合解决方案的相关问题，如内存优化、广播模式。

Redis 6.0.0 发布了一个特性，即 Redis 服务器辅助的客户端缓存，带有**客户端跟踪**命令。由于这是一个新发布的功能，Redis 支持像 Jedis 这样的 java 客户端，Radisson 尚未提供对该功能的支持。其他语言可能也是如此。

如果你想了解关于**客户端跟踪**命令的全部细节，以下是最好的资源。

1.  【https://redis.io/topics/client-side-caching 
2.  [https://redis.io/commands/client-tracking](https://redis.io/commands/client-tracking)
3.  [https://www.youtube.com/watch?v=fcgFT1Nca14&ab _ channel = redis labs](https://www.youtube.com/watch?v=fcgFT1Nca14&ab_channel=RedisLabs)

希望我会在下一篇文章中创建一个 POC 并与大家分享。