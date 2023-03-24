# 使用 DynamoDb 之前应该考虑的事情

> 原文：<https://medium.com/nerd-for-tech/things-you-should-consider-before-using-dynamodb-85b97daf83f5?source=collection_archive---------0----------------------->

![](img/b7dfb214033fb301fcfaccd0b6cd35ea.png)

本文設有[中文版](/@crizantlai/使用 dynamodb-前你該考慮的事情-4bb1235d4521)。

L 最近，我一直在用 DynamoDb 做一个项目，这是我第一次见到这个 AWS 服务。它只是不像`MongoDB`那样是一个常规的 NoSQL 数据库，我遇到了一些问题(或特性？)中，我将与大家分享我处理这些问题的方式(如果我知道如何处理的话)。

## 没有空字符串