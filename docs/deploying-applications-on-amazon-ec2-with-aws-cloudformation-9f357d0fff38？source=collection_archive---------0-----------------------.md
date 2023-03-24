# 使用 AWS Cloudformation 在 Amazon EC2 上部署应用程序

> 原文：<https://medium.com/nerd-for-tech/deploying-applications-on-amazon-ec2-with-aws-cloudformation-9f357d0fff38?source=collection_archive---------0----------------------->

## 成功安装应用程序后，使用 AWS::CloudFormation::Init、cfn-init 和 cfn-signal 来完成 AWS Cloudformation 堆栈。

# 云形成堆栈

一个*栈*是 AWS 资源的集合，您可以将它作为一个单元来管理。换句话说，您可以通过创建、更新或删除堆栈来创建、更新或删除资源集合。*自动气象站云形成*使用…