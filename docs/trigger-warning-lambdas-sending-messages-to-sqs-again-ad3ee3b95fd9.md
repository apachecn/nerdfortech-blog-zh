# 触发警告:Lambda 又向 SQS 发送信息了！

> 原文：<https://medium.com/nerd-for-tech/trigger-warning-lambdas-sending-messages-to-sqs-again-ad3ee3b95fd9?source=collection_archive---------2----------------------->

## PYTHON，BOTO3，LAMBDA

## 对于这个项目，我创建了一个 Lambda 函数来向 SQS 队列发送消息。

![](img/1b63f3737f38351cdec79d822cb874f0.png)

**目标:**
使用 Python 创建标准的 SQS 队列
创建 Lambda 函数
修改 Lambda 以当前时间向 SQS 队列发送消息
创建 API 网关…