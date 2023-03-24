# 使用 Gradle 101 进行依赖性管理

> 原文：<https://medium.com/nerd-for-tech/dependency-management-with-gradle-101-6c36fbeeeac4?source=collection_archive---------7----------------------->

几年前，在软件领域，很多时候需要第三方工具来完成任何扩展功能的特定目标，有时以一种有效的方式将它们组合在一起是一件非常头疼的事情。

开源软件背后的理念就是协作和透明。依赖关系调用也被称为“库”，所以基本上，开源项目也使用另一个项目，它也有其他依赖关系，或者它如何被称为传递依赖关系。

开发人员已经了解到拥有一个标准化的构建工具是开发工作流程的一个重要部分，尤其是对于团队来说。

## 格拉德勒

Gradle 是一个为 Java 开发的工具，可以自动将代码打包给用户。简而言之，处理项目中存在的所有直接和可传递的依赖关系。

Gradle 将构建过程作为任务来处理，任务在内部运行单元测试并使用特定的元数据创建 jars 文件。这种功能也存在于其他工具中，如 Maven 或 Apache 的 Ant。

它公开了一种基于 Groovy 的 DSL 或特定于领域的语言(非常类似于 Java，但更灵活)来使用它。它有自己的惯例，但是如果你来自基于 C 语言或者甚至是相同的 Java，就不难理解。

## 演示项目

*注意:我将使用 IntelliJ IDEA 作为 IDE*

这个项目的目标是将 CSV 库集成到我们的控制台 Java 应用程序中。

1.  在 IDE 已经打开的情况下，转到`New Project`，选择`Gradle`和`Java`
2.  添加您的`GroupID`(基础包)，在我的情况下将:`com.davidlares.reviews`
3.  加上你的`ArtifactID`(你的罐子的名字)，对我来说就是:`reviews`

此时，IDE 将创建一个项目，一切都将为我们设置好。

## 项目结构

一个简单的 Gradle 文件将包含以下文件。

`build.gradle`是一个配置文件，包含项目信息、所需的任何插件，以及用于处理直接和可传递依赖关系的存储库。在向导中定义了`group`值的地方，`version`是文件的版本，`apply plugin: 'java'`定义了文件夹的布局以及如何打包

`repositories()`是项目中定义的一种方法，它指示允许哪个存储库 Gradle 下载文件。更详细的信息可以在[这里](https://docs.gradle.org/current/userguide/declaring_repositories.html)找到，它有很多替代品。

第二个文件是`gradlew`，它是一个 Gradle 包装器。如果有人想亲自动手做你的项目，你可以安装一个 Gradle 的自安装程序吗

第三个文件是`gradlew.bat`，是相同的文件，但用于 Windows 操作系统。

你可以下载最后一个文件并运行它们，这将安装 Gradle 工具及其项目依赖项

当然，作为我们项目的一部分。我们在一个`/src`目录中有两个文件夹，一个是包含 Java 类的`main`文件夹，另一个是作为我们测试环境的`test`文件夹

此时，我们已经准备好编码和处理外部依赖项了。

## 添加依赖关系

您可能已经知道，Gradle 下载依赖项和传递依赖项，而且还提供了一个包缓存系统，可以在许多其他项目或库之间共享

如果我们详细描述我们的`build.gradle`,你将得到具有这个基本语法的`repositories()`方法

```
repositories() {
  mavenCentral()
}
```

这是一个闭包方法，它从 mavenCentral 存储库中返回一个调用。所以，这一切都发生在构建项目的时候。

由于我们需要一个 CSV 库，我们将使用 Apache commons CSV 库，[这里是库的链接。指令表明有一种方法可以从 Maven 仓库中提取它](https://commons.apache.org/proper/commons-csv/)

下面是文件库文档(Maven 版本)中的 XML 脚本

```
<dependency>
  <groupId>org.apache.commons</groupId>
  <artifactId>commons-csv</artifactId>
  <version>1.8</version>
</dependency>
```

因此，我们需要将它包含到`dependencies`闭包中

`testCompile`配置指令包含任意数量的命名参数。同时,`compile`部分设置要安装的库

您需要在那里添加以下内容:`compile group: 'org.apache.commons', name; 'commons-csv', version: '1.8'`(基于描述 XML 的信息。在此之后，您将需要刷新项目并构建它，但它可以是自动的，您可以在 IDE 的 Gradle projects 选项卡下启用自动导入选项

还有一种添加依赖项的简单方法:`compile ‘org.apache.commons:commons-csv:1.8’`

您可以通过在 ide 的`CLI`中运行`./gradlew dependencies`来检查是否列出了依赖项，如果成功，您将看到“dependencies”闭包下安装的每个依赖项的依赖树及其对应的语义版本。

Gradle 的另一个很酷的地方是，你可以用 Github/Gitlab 或其他什么东西给`gradle.build` 文件版本化，每个人都可以得到相同依赖项的副本，而不用直接查看`jar`文件。

## 最后，怎么用？

在我们的`src/main`文件夹中，添加一个`Main.java`文件，直接使用这个库。

我的包叫`Reviews`，完整路径是`com.davidlares.reviews`

这是我为`Main.java`写的剧本:

```
public class Main {
 public static void main(String[] args) {
  try {
   CSVPrinter p = new CSVPrinter(System.out, CSVFormat.EXCEL);
   p.printRecord("David", "Lares", 22, "Backend");
  } catch (IOException e) {
   e.printStackTrace();
  } 
 } 
}
```

IDE 将自动处理导入语句。

这很简单，对吗？