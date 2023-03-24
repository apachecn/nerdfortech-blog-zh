# 使用 webhook jenkins 在 s3 上托管一个静态网站

> 原文：<https://medium.com/nerd-for-tech/host-a-static-website-on-s3-using-webhook-jenkins-beb6328d6a5f?source=collection_archive---------16----------------------->

**提示:阅读 jenkins 的 web hook**

**注意:这是一个管道方法**

**安:**

步骤 1:设置简单的 S3 铲斗

![](img/18f21b0f5b15e4ff6e87f7de85f5fc12.png)

步骤 2:这里我们使用自动化管道方法

[https://github.com/kushagra67414/jenkins-static-s3-deploy](https://github.com/kushagra67414/jenkins-static-s3-deploy)

它包括 Jenkins 管道和 s3 存储桶允许公众访问 index.html 和 error.html 文件所需的策略

![](img/5deff9b13a1c2321508017f2761c8786.png)

步骤 3:管道设置

在 Jenkins 创建一个新的管道项目。

![](img/4927b45ab32984d48f292ad72b1b588a.png)

管道= >

![](img/5deff9b13a1c2321508017f2761c8786.png)

在 git 存储库中添加一个 webhook。

![](img/f86e53b4ba55f956e3bd2c03a33fbfde.png)

如果此 repo 中的推送提交将创建一个版本，则需要 webhook 配置。

步骤 4:运行管道来测试它是否工作。它应该上传文件到 S3 桶。

首先，配置作业。

![](img/240f1f8ba967c83cd5d98470ab680fe4.png)![](img/c69a1b57be12877037f44f01d6f6bcde.png)

步骤 5:配置作业后，构建项目，它必须将数据存储在 s3 存储桶中，但它可能会显示错误，因为我们没有在 jenkins 用户中配置 aws cli。

我们正在配置 aws cli，因为管道由 aws cli 命令组成，当管道执行时，它会检查 awi cli 环境

转到 jenkins 用户，配置 aws cli 以执行管道中的命令。

首先转到用户:

>苏—詹金斯

> aws 配置 root 用户的 add secret key 和 access key，或者使用 s3 bucket 的策略创建一个 iam 用户来访问它并提供 iam 用户的凭证。

最佳实践是创建一个 iam 用户。

![](img/8411ee852047ec3e1583e66561dd76c3.png)

步骤 6:重启 jenkins 并构建项目

命令:systemctl 重新启动 jenkins

控制台输出:

![](img/6bf00573b6dfef3a8981f366a620b5ea.png)![](img/cc6deba365efca206ea2a8a8ea316113.png)![](img/19d684cca4c78911fa88289c67f020c4.png)