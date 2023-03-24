# 将 CLI 工具转换为 Github 操作

> 原文：<https://medium.com/nerd-for-tech/converting-cli-tool-to-github-action-fb1ec1bf4ae9?source=collection_archive---------0----------------------->

*原载于 2020 年 6 月 23 日 https://www.natigbabayev.com**的* [*。*](https://www.natigbabayev.com/2020-06-23/converting-cli-tool-to-github-action)

在本文中，我将分享我为我使用的一个 CLI 工具创建 Docker 容器动作的旅程。

# 什么是“Github Actions”？

GitHub Actions 允许您直接在存储库中构建端到端的持续集成(CI)和持续部署(CD)功能。如果你不熟悉 Github 动作，你可以在这里阅读更多关于它的内容[。](https://help.github.com/en/actions/getting-started-with-github-actions/about-github-actions)

## 以下是构建和发布 Docker 容器操作的步骤:

## 步骤 1:学习 CLI 工具

首先，我们需要决定我们想要创建 Github 动作的工具，并研究我们需要什么样的设置来在我们的 Docker 映像中运行它。在我的例子中，使用`openjdk:jre-slim`就足以运行 [detekt](https://detekt.github.io/detekt/cli.html) 。

在这一步，一个建议是，我们应该尝试找到轻量级的图像，以便`docker pull`更快。

因此，让我们在存储库中创建名为`Dockerfile`的文件，并添加以下内容:

```
FROM openjdk:jre-slim
```

## 步骤 2:将 CLI 工具下载到 Docker 映像

在这一步中，我们需要将工具下载到 Docker 映像中。为此，我们将使用`ADD`命令。

`ADD`命令需要一个源和一个目的地:

```
ADD source destination
```

现在，我们可以将 CLI 工具添加到 docker 映像中:

```
ADD https://github.com/detekt/detekt/releases/download/v1.10.0-RC1/detekt /usr/local/bin/detekt
```

下载后，我们需要使我们的文件可执行:

```
...
RUN chmod +x /usr/local/bin/detekt
```

## 步骤 3:向 Docker 映像添加入口点

现在我们需要通过添加[入口点](https://docs.docker.com/engine/reference/builder/#entrypoint)来完成 Docker 图像:

```
...RUN cd $GITHUB_WORKSPACEENTRYPOINT ["detekt"]
```

注意，我也添加了`cd $GITHUB_WORKSPACE`。`GITHUB_WORKSPACE`是一个环境变量，它告诉我们工作目录，以便我们可以在正确的目录下运行脚本。

现在我们已经完成了`Dockerfile`变更，我们可以进入下一步了。你可以在这里找到完整的文档。

## 第四步:发布

当我们完成`Dockerfile`时，我们可以发布我们的行动。为此，我们需要定义`action.yml`并添加名称、描述、作者等信息。你可以在这里查看我创建的`action.yml`。添加`action.yml`后，您需要创建`README.md`，它会给出一些关于您的动作、使用示例等信息。接下来的步骤是，在 GitHub 上将我们的更改发布到存储库之后，我们需要导航到存储库的主页并起草一个新的版本。这里可以阅读更多[。](https://help.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace)

## 🎉恭喜你！🎉

如果您正确地遵循了这些步骤，那么您创建的操作应该会发布在 GitHub Marketplace 上。你可以在这里找到样本库。