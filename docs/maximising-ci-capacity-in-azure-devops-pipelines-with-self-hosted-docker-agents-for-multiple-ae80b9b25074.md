# 借助面向多个工作流的自托管 Docker 代理，最大化 Azure DevOps 管道中的 CI 容量

> 原文：<https://medium.com/nerd-for-tech/maximising-ci-capacity-in-azure-devops-pipelines-with-self-hosted-docker-agents-for-multiple-ae80b9b25074?source=collection_archive---------5----------------------->

如果你像我一样，在 Azure DevOps 中管理一个自托管代理池，并且发现你想要更多的容量，那么 Docker 可能是你的解决方案。通过在 Docker 中运行 Azure Pipelines 代理，您可以利用可用的资源，而无需引入新的虚拟机，也不会增加代理池的运行和维护成本。使用 Docker，您可以在一台机器上运行多个构建代理实例，并利用它来扩展您的 CI 容量。

# 文档文件

让我们快速浏览 docker 文件，最终[满足了我们所有的用例](https://github.com/KrylixZA/Azure-Pipelines-Docker-Agents/blob/main/build-agent/windows/Dockerfile)。

## 基地

最初的步骤使用转义字符“` ”,因为默认的“\”会干扰安装路径，这使事情变得简单了一些。我以 mcr.microsoft.com/dotnet/framework/runtime:4.8 的[和](https://hub.docker.com/_/microsoft-dotnet-framework-runtime)为基础图片。我已经测试了各种其他基础映像，例如 [windows-servercore](https://hub.docker.com/_/microsoft-windows-servercore) 、 [windows-nanoserver](https://hub.docker.com/_/microsoft-windows-nanoserver) 和 [dotnet](https://hub.docker.com/_/microsoft-dotnet) 映像，但所有这些都缺少一些工具或功能。另一个潜在的基础映像是[mcr.microsoft.com/dotnet/framework/sdk:4.8](https://hub.docker.com/_/microsoft-dotnet-framework-sdk/)映像，它附带安装了 Visual Studio 2019 构建工具&测试工具，但没有附带 SQL Server 数据工具，这为我排除了它。自己安装 Visual Studio 比尝试修补已经安装的程序要容易得多。

如果您使用[mcr.microsoft.com/dotnet/framework/sdk:4.8](https://hub.docker.com/_/microsoft-dotnet-framework-sdk/)作为您的基本映像，您可以跳过这段代码中安装 Visual Studio 构建工具以及 Visual Studio 工具代理的部分。[你可以在微软的文档](https://docs.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-build-tools?view=vs-2019)中阅读更多关于各种 Visual Studio 工作流和组件的信息。

## 饭桶

接下来我需要安装 Git。我选择使用 MinGit 安装。

MinGit 带来了一个有趣的问题，默认的`gitconfig`文件有一个指向自身的无限循环，所以我简单地下载了那个文件并删除了循环，将它复制到我的 Docker 映像中并覆盖了默认文件。你可以在 GitLab 上阅读更多关于这个问题[的报道。我的`gitconfig`文件是 GitHub](https://gitlab.com/gitlab-org/gitlab/-/issues/239013#note_400463268) 上的[。](https://github.com/KrylixZA/Azure-Pipelines-Docker-Agents/blob/main/build-agent/windows/gitconfig)

## NodeJS

接下来我需要安装 NodeJS。

我需要支持新的和遗留的节点模块，并且发现 NodeJS 10.x 的最新版本已经足够了。如果您不需要支持遗留系统，我强烈建议您使用 NodeJS 的最新稳定版本。

## Java 语言(一种计算机语言，尤用于创建网站)

为了支持 Java 构建，以及管道中的各种静态分析工具，Java JDK 和 JRE 都是必要的。令人欣慰的是，开放的 JDK 和开放的 JRE 是现成的，易于安装。但是，有必要设置这两个环境变量并更新路径以引用安装。

## 努格特

安装 NuGet.exe 并将其作为路径的一部分对于设置为运行的代理非常有用。NET 构建，尽管这不是完全必要的，因为有管道任务来安装可以添加到`azure-pipelines.yml`中的 NuGet。将 NuGet 直接安装到代理上的主要好处是，您可以使用 docker 文件中的`nuget` CLI 来验证您可能拥有的任何私有提要。

## 码头工人

下一步假设在启动容器时，主机的 Docker 安装被安装到正在运行的容器中。这允许各种构建过程在管道中运行 Docker 任务。如果主机的 Docker 安装没有安装到运行容器中，当从 Azure DevOps 查看 Docker 代理时，运行 Docker 的功能将完全不存在。

## 无头铬合金

有必要在 Docker 代理中直接安装一个无头浏览器来运行 UI 测试。我选择用 Chocolatey 安装谷歌浏览器。

基本的 Windows 映像不再安装任何字体——然而，出于明显的原因，为了让 Chrome 工作，字体需要安装在操作系统上，这是`[FontsToAdd.tar](https://github.com/KrylixZA/Azure-Pipelines-Docker-Agents/blob/main/build-agent/windows/FontsToAdd.tar)`和`[Add-Font.ps1](https://github.com/KrylixZA/Azure-Pipelines-Docker-Agents/blob/main/build-agent/windows/Add-Font.ps1)`负责的。

## 清理和设置入口点

最后一部分只是清理所有临时安装目录和文件，然后为正在运行的容器设置入口点。

`[start.ps1](https://github.com/KrylixZA/Azure-Pipelines-Docker-Agents/blob/main/build-agent/windows/start.ps1)`文件与微软提供的[文件略有不同，Azure Pipelines 代理的安装和运行目录不再是`C:\azp\agent`而是`C:\agent`。](https://docs.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops#create-and-build-the-dockerfile)

## 完整的文档

# Docker 中心图像

如果你需要，你可以从 [Docker Hub](https://hub.docker.com/r/krylixza/azure-pipelines-windows-build-agent) 拉 Docker 镜像。

```
docker pull krylixza/azure-pipelines-windows-build-agent:1.0.0.38
```