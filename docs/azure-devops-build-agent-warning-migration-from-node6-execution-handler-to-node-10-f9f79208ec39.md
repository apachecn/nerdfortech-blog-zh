# Azure DevOps 构建代理警告—从节点 6 执行处理程序迁移到节点 10

> 原文：<https://medium.com/nerd-for-tech/azure-devops-build-agent-warning-migration-from-node6-execution-handler-to-node-10-f9f79208ec39?source=collection_archive---------3----------------------->

## 微软建议:什么都不做！

在撰写本文时，使用最新版本的构建代理 2.195.1(或之前的版本 2.195.0)在 Azure DevOps 中运行的管道当前将显示以下警告:

> # #[警告]此任务使用节点 6 执行处理程序，该处理程序将很快被弃用。如果您是任务的开发人员，请考虑向节点 10 处理程序[https://aka.ms/migrateTaskNode10](https://aka.ms/migrateTaskNode10)的迁移指南。如果您是用户，请随意…