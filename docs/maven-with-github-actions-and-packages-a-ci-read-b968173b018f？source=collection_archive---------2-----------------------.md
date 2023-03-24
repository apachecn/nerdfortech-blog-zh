# Maven 的 Github 操作和包——✅的一篇文章

> 原文：<https://medium.com/nerd-for-tech/maven-with-github-actions-and-packages-a-ci-read-b968173b018f?source=collection_archive---------2----------------------->

**GitHub 动作**🎬是一项免费使用的服务🤖您的 CI/CD 工作流程直接从 GitHub 控制台开始。

**GitHub 包**📦是一个托管工件存储库，用于存储和共享您的代码二进制文件⏩甚至是码头工人的照片。

![](img/8b59baf465ab86afba9030530365734d.png)

> 我们将深入一个为 Java/Spring 应用程序设置 Maven build 的例子。

> *PS:如果你对使用 Heroku* *的* ***CD 感兴趣，可以看看我之前的文章——《Heroku 云上的*[Spring app](/nerd-for-tech/spring-app-on-heroku-cloud-with-continuous-deployment-328ff8fbfe7a)*。***

要建立一个 GitHub 工作流，你只需要定义一个**触发点**和一个必须运行的**作业**。

首先，要设置 GitHub 操作，导航到 GitHub 存储库的“操作”标签→“新建工作流”→“自己设置工作流”。

这将打开一个 YML 文件，我们可以用它来定义 GitHub 动作定义。

# —第一部分:触发器

第一部分定义了“当”——即[的触发点](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#on)。

对于这个例子，我们将把触发条件定义为“推”到“主”分支，如下所示。

```
name: My project build  
on:   
    push:
    branches: 
        [ main ]
```

> 除了推送事件之外，您还可以配置工作流，以便在推送特定的文件夹/glob 模式、拉取请求或 corn 时触发。

# 第二部分:行动

这里是我们定义“[什么](https://docs.github.com/en/actions/learn-github-actions/introduction-to-github-actions#the-components-of-github-actions)部分的地方。用 DevOps 的术语来说，它相当于作业/管道阶段。

我们需要用一个跑步者来定义一个“作业”。GitHub 官方支持几个[托管的跑步者](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#github-hosted-runners)。

> 每个触发器创建一个用于运行作业中的步骤的一次性容器。

```
jobs:
   build:
     runs-on: ubuntu-latest
     steps:
```

工作是一系列步骤的集合。每个步骤可以是一个 shell 命令，也可以是一个“动作”。

> 社区中有许多预置的“动作”可用，例如，要在容器中设置 **java 和 maven** ，我们可以使用 [setup-java](https://github.com/actions/setup-java) 。

```
 - name: Set up JDK 11
       uses: actions/setup-java@v2
       with:
         java-version: '11'
         distribution: 'adopt'
         server-id: github
         settings-path: ${{ github.workspace }}
```

# —第三部分:构建

这是最后一步，我们为要发布的二进制文件指定 [Github 包注册表](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry#publishing-a-package)。

由于我们使用的是 maven，pom 中的以下条目定义了目标。

```
<distributionManagement>
    <repository>
      <id>github</id>
      <name>GitHub Packages</name>
      <url>https://maven.pkg.github.com/OWNER/REPOSITORY</url>
    </repository>
</distributionManagement>
```

现在，运行 maven deploy 应该会将二进制文件发布到 GitHub 包中。

> 请注意，您也可以在这里使用 maven 发布目标。

```
 - name: Publish to GitHub Packages Apache Maven
       run: mvn deploy -s $GITHUB_WORKSPACE/settings.xml
       env:
         GITHUB_TOKEN: ${{ github.token }}
```

# 例子

## 以下存储库是上述设置的一个示例。

[阿尔琼恰克里/夏洛克-埃罗库-poc](https://github.com/arjunchakri/sherlock-heroku-poc/actions)

工作流文件→ *。github/workflows/maven-publish . yml*

现在，我们已经成功整合了 GH 行动🎉！！
每一次代码更改都会构建一个新的二进制文件并发布到 GitHub 包中！！