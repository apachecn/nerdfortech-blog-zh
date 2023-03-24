# Azure CLI 入门

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-azure-cli-1c3902c8d4fb?source=collection_archive---------1----------------------->

Azure CLI 是一个跨平台的命令行工具，内置于 Python 中，可以创建、更新和删除几乎所有的 Azure 资源。它为您提供了一个相当容易理解的 CLI，在幕后使用各种 Azure Rest APIs。

![](img/13301793e613fe8f3bbe099b58ffeb48.png)

照片由[韦斯·希克斯](https://unsplash.com/@sickhews?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**通过 Azure 云外壳的 Azure CLI**

如果你使用 Azure 云外壳来使用 Azure CLI，你不需要在你的本地机器上使用它，使用云外壳的一个优点是它防止你必须用 Azure CLI 进行认证。因为你已经通过 Azure 门户进行了身份验证，所以云外壳将身份验证“传递”给 Azure CLI，所以一切正常。

**本地 Azure CLI**

如果您选择在本地安装和运行 Azure CLI，您可以通过 local [安装这个](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)，然后您可以使用命令`az login`验证 Azure CLI。一旦您进行了身份验证，请确保使用`az account set`设置默认订阅。如果不设置默认订阅，则必须为发出的每个命令指定订阅。

运行以下命令来设置默认订阅，将`[subscription_name]`占位符更改为您的订阅名称。

```
az account set --subscription '[subscription_name]'
```

**命令:**

Azure CLI 是围绕**命令**的前提构建的。命令是执行某些动作的可执行程序。Azure CLI 都源于一个名为`az`的命令。你将使用 Azure CLI 运行的所有命令都以`az`命令开始。

`az`是启动 Azure CLI 的起点，然后指定一个**命令组**。

**查找命令:**

了解 Azure CLI 和语法的最佳方式之一是使用帮助系统。

每个 Azure CLI 命令组和命令都有一个通用的`--help`参数。`--help`命令显示与命令或组相关的所有选项。

> 注意，在根级别，命令组实际上是 az 命令本身。然后所有子群从`az`中脱离。

例如，如果您不确定此时您不知道什么，请键入以下命令:

```
az --help
```

您将看到`az`返回所有可用的组和子组。

现在，您可以深入到任何一个子组，了解更多信息。假设您想要管理资源组。向下滚动一点，您会看到一个名为`group`的子组。提供`group`作为`az`和`--help`的参数，看看会发生什么。该命令将是:

```
az group --help
```

在任何命令或命令组后继续使用`--help`来检查什么是可能的。

**获取示例:**

例子是学习的好方法。幸运的是，我们有了`az find`命令来发现使用各种命令和命令组的实际例子。

一旦你找到了你想要使用的命令组或命令，使用`az find`提供该组或命令如何使用的例子。例如，您可能需要对存储帐户执行一些操作。尝试以下命令，查看一些有用的示例。

```
az find "az storage"
```

也许你需要找一些例子来创建一个资源组。使用`--help`，您发现`az group`是您需要使用的命令组。一旦您知道了命令组或命令，就可以使用`az find`深入到各种示例中。

您可以看到，通过首先向`az find`提供命令组，您可以找到要使用哪些命令(`create`)。从那里，您可以向`az find`提供该命令，以显示与该命令相关的示例。例如，尝试以下命令:

```
az find "az group"
az find "az group create"
```

**Azure 脚本:**

尽管 Azure CLI 不像 PowerShell 那样是脚本语言，但您仍然可以构建一些方便的脚本。

由于 Azure CLI 只是不依赖于任何一种语言的可执行命令，所以您可以像创建任何其他命令一样创建 shell 脚本。Bash shell 脚本通常用于创建 Azure CLI 脚本，但是您也可以在 PowerShell 脚本甚至 Windows 批处理文件中组合 Azure CLI 命令。

如果你要创建基于 Azure CLI 命令的脚本，你应该使用 VS 代码并安装 [Azure CLI 工具 VS 代码扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)。该扩展在 Azure CLI 脚本中提供了语法着色、各种智能感知功能等。

创建 Azure CLI 脚本与单独运行命令没有太大区别。您仍然需要输入要运行的命令，但是要在脚本中执行，并使用扩展名`AZCLI`保存脚本。`AZCLI`扩展告诉 VS 代码该文件是一个 Azure CLI 脚本，并识别它。

Azure CLI 提供了一堆执行任务的命令，很容易放入您的 Devops 周期或自动化任务中。我们只触及了冰山一角，你可以参考参考部分的文件

**参考文献:**

[](https://docs.microsoft.com/en-us/rest/api/azure/) [## Azure REST API 参考文档

### 欢迎使用 Azure REST API 参考文档。表述性状态转移(REST)API 是服务…

docs.microsoft.com](https://docs.microsoft.com/en-us/rest/api/azure/) [](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) [## Azure 命令行界面(CLI)入门

### 欢迎使用 Azure 命令行界面(CLI)！本文介绍了 CLI，并帮助您完成常见任务…

docs.microsoft.com](https://docs.microsoft.com/en-us/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)