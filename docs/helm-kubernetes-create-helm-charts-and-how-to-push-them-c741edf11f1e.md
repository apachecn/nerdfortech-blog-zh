# Helm & Kubernetes |创建掌舵图以及如何推动它们

> 原文：<https://medium.com/nerd-for-tech/helm-kubernetes-create-helm-charts-and-how-to-push-them-c741edf11f1e?source=collection_archive---------4----------------------->

![](img/c4bc5ba2c003e1191882eaa901771a16.png)

戴维·诺克斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在这篇文章中，我想谈谈什么是 Helm，如何安装和使用它，以及如何将它的图像推送到 artifactory。这听起来像是一大堆东西，实际上也是，但这是 CI/CD 中非常重要的一步，所以我会尽可能解释清楚。

# 什么是头盔

简而言之， [Helm](https://helm.sh) 是 Kubernetes 的一个包管理器，它允许开发者和运营商更容易地将应用和服务打包、配置和部署到 Kubernetes 集群上。

Helm 有机会为 Kubernetes 设置变量，然后您可以将这些变量分配给不同的 Kubernetes 对象。

或者用[官网](https://helm.sh)的话说:

> Helm 帮助您管理 Kubernetes 应用程序——Helm 图表帮助您定义、安装和升级最复杂的 Kubernetes 应用程序。
> 
> 图表易于创建、版本化、共享和发布，所以开始使用 Helm，停止复制和粘贴。

# 如何安装头盔

[家酿](https://brew.sh/index_de):

```
$ brew install helm
```

[巧克力](https://chocolatey.org/):

```
$ choco install kubernetes-helm
```

来源:

```
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

有关如何安装 Helm 的更多信息，请访问[官方网站](https://helm.sh/docs/intro/install/)。

# 头盔如何工作

要使用 Helm，您的电脑上需要安装 Kubernetes。如果情况不是这样，安装 [MiniKube](https://kubernetes.io/de/docs/setup/minikube/) 或者在 Docker 桌面中启用 Kubernetes 集群。

## 单线安装

Helm 管理您的所有 Kubernetes 文件，并安装、更改和删除其所有内容。Helm 会自动应用其“模板”目录中的每个 Kubernetes Yaml 文件。只需一个简单的 cli 命令就可以安装整个 K8s 应用程序:

```
$ helm install APP_NAME CHART_NAME
```

## 中央值存储

这还不是全部。Helm 有一个 [Values.yaml 文件](https://helm.sh/docs/chart_template_guide/values_files/)，其中可以存储定义 Kubernetes 对象所需的所有数据。然后，您的 K8s 文件可以引用这个 Values.yaml 文件并使用这些值。使用 Helm 你可以防止在不同的 K8s 资源中复制粘贴 10 次完全相同的名字。我们稍后会看一个例子，所以请继续阅读。

## 部署时覆盖值

使用 Helm，您可以在部署时覆盖 Values.yaml 文件的一些值。您可以通过添加— set 标志并在后面添加一个键和值来实现这一点。[安装命令](https://helm.sh/docs/helm/helm_install/#options)可能看起来像这样:

```
$ helm install APP_NAME CHART_NAME --set deployment.image=nginx
```

## 包装图表

有了 Helm，你不仅有机会在本地运行你的 Helm-Charts，不，你还可以打包它们并上传到你的个人 Helm-Repo。如果你有一个舵图准备部署，你只需要运行:

```
$ helm package CHART_NAME --version=MAJOR.MINOR.PATCH
```

你实际上不必使用 M.M.P 版本控制，但是使用[语义版本控制](https://semver.org/lang/de/)确实是一个好的实践。

无聊的理论说够了，我们先来举个小例子。

# Nginx 示例

首先，这个例子非常简单，只需在集群或 MiniKube 上部署一个带有服务的 Nginx 应用程序。

正如您在这里看到的，实际上只有一个绑定了服务的基本部署。现在，我们将尝试为这个应用程序创建一个新的舵图。

```
$ helm create HELM-CHART-NAME
```

这将在项目的根目录下创建一个新的舵图。如果您打开该文件夹，您将看到它包含几个子目录和 yaml 文件。

1.  第一个是图表目录。你暂时不必理会它。
2.  第二个是[。helmignore](https://helm.sh/docs/chart_template_guide/helm_ignore_file/) 文件。它具有与类似的属性。git 忽略 git 的文件。您可以配置打包时哪些文件不会添加到您的 helm hart 中。
3.  Chart.yaml 文件包含一些关于图表本身和应用程序版本的信息。每次改完都可以改。
4.  values.yaml 文件包含模板目录中文件所需的所有值。
5.  模板目录包含 Helm 执行的每个文件。有些文件是默认存在的，在大多数情况下你可以忽略它们。如果部署成功，它们可以帮助您创建入口或生成控制台输出。

## 使用 values.yaml 文件

1.  将 K8s 文件添加到模板目录中。
2.  查看您的 K8s 文件，搜索您可以替换的值，通常有很多，并将它们添加到您的 values.yaml 文件中。

这看起来比这个大 K8s 文件简单多了，对吧？这会儿没用，所以我们也需要改变其他文件，使用这个文件的值。

您可以使用来引用这些值。值，然后是文件中定义的层次结构。通过这种方式，您可以确保在您需要的地方有相同的值，并且打字错误几乎是过去的事情。

## 安装您的第一个图表

至此，您已经配置了第一个图表。您现在可以安装它:

```
$ helm install nginx-app nginx-chart
```

如果一切正常，您应该能够在 localhost:30001 上看到 nginx 欢迎屏幕

如果您想升级您的部署，您可以只使用设置命令来改变，例如图像。

```
$ helm upgrade nginx-app --set nginx.deployment.image=nginx:1.7
```

在那之后，你现在应该看到豆荚被换出了正确的图像。

## 打包您的图表

如果您不仅希望在本地部署图表，还必须将它们打包并推送到 artifactory。重要的是，您已经将所有不想打包的文件添加到。helmignore 文件。如果已经是这种情况，只需运行:

```
$ helm package nginx-chart --version=1.0.0
```

这将在舵图表的基础上创建一个新的舵包。看起来像是:

```
nginx-chart-1.0.0.tgz
```

## 推动你的图表

为了推动我们的图表，我们可以使用旋度。在我看来这是最简单的方法。

```
$ curl -u username:password -T nginx-chart-1.0.0.tgz \
[https://YOUR-HELM-CHAR](https://YOUR-HELM-CHART)T-REPO/nginx-chart-1.0.0.tgz
```

另一种方法是直接使用头盔:

```
$ helm push nginx-chart-1.0.0.tgz \ 
[http://](https://YOUR-HELM-CHART-REPO)[username:password](https://YOUR-HELM-CHART-REPO)@[YOUR-HELM-CHAR](https://YOUR-HELM-CHART)T-REPO
```

在这之后，你应该可以在你的头盔-海图-注册表中看到你推送的海图。

# 反射

# 什么进展顺利？

这个包装非常好用。它在第一次尝试时工作得非常好，并且不需要任何配置。此外，初始化工作相当不错，不需要太多的变化。

# 有哪些需要改进的地方？

唯一不顺利的是，我在部署中使用了错误的 values.yaml 文件值。因为您可以在以后描述 Kubernetes 对象，或者直接在您的 IDE 中看到它，如果某个值是可能的，那么解决它是相当容易的。

我希望你能学到一些东西。快乐驾驶！😊