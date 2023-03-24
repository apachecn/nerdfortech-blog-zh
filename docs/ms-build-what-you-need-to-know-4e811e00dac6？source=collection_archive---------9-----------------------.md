# MS Build —您需要了解的内容

> 原文：<https://medium.com/nerd-for-tech/ms-build-what-you-need-to-know-4e811e00dac6?source=collection_archive---------9----------------------->

![](img/aeccb17fa5b4254241fda689d08dbaf2.png)

在现代世界中，应用程序和软件是使用诸如 C#和 Java 之类的高级编程语言编写的，这些语言是人类可以理解的，并且不能由计算机直接执行。

为了让计算机“执行”这些应用程序，用高级编程语言编写的代码必须转换成一组机器指令。然后，必须对其进行捆绑和部署，以便成功访问应用程序。

但是在实时中，应用程序程序员并不强调或专注于编译和打包部分。相反，他们更关注构建应用程序背后的逻辑。那么，高级代码到机器指令的转换和捆绑是如何处理的呢？

对于基于 C#的应用程序，微软提供了一个工具来处理这些活动。它被称为 MSBuild。让我们看看什么是 MSBuild，以及它如何在这个博客中有用。

**目录**

1.  什么是 MSBuild？
2.  什么项目文件包含？
3.  项目文件中的节
4.  结论

# 什么是 MSBuild？

直接来自于。在. Net 2.0 中，微软提供了构建引擎工具 MSBuild。MSBuild 是一个构建平台工具，有助于自动化创建软件产品的过程，包括编译源代码、打包、测试和部署源代码。

MSBuild 是随 Visual Studio IDE 安装程序一起提供的独立工具。也可以单独安装。从技术上讲，它不依赖于 Visual Studio，可以在不包含 Visual Studio 的计算机上运行。它只需要 csproj、vbproj、vcproj、vjsproj、proj 和。可以作为输入传递给 MSBuild 以处理代码的目标。MSBuild 可用于生成。基于. Net Framework 的应用程序和。基于. Net 核心的应用程序。

# 项目文件包含什么？

在本节中，让我们以 csproj 文件为例，详细了解它包含的内容以及如何在基于 C#的项目中使用它。

考虑一个基于三层架构的应用程序，其中表示层、业务层和数据库层是完整解决方案中包含的三个不同的项目。当用户创建这样的解决方案时，MSBuild 会查找 csproj 文件或项目文件来生成项目。这样，单个项目将由 MSBuild 生成。

项目文件是基于 XML 的文件，包含以下信息。

考虑解决方案中的一个业务层项目。现在，代码中的操作可以处理基于 JSON 的对象和文件、电子表格、处理 pdf 等等。并非所有这些功能都带有预定义的。Net 程序集和引用。其中一些必须在外部引用，并在代码内部使用。在编译期间，编译器必须利用它们来完成编译过程。

在这种情况下，项目中引用的包的信息是在项目文件中定义的。

考虑另一种情况。解决方案中的项目之间存在内部引用，例如，表示层与业务层交互，业务层与数据库层交互。现在需要在项目文件中跟踪的解决方案之间有引用。

除此之外，它还包括。Net，项目必须以 32 位还是 64 位模式编译，以及项目内部需要引用的类文件列表。所有这些数据都将在项目文件中提及。

# 项目文件中的节

让我们看看项目文件中可用的不同部分。

## 属性组

```
<PropertyGroup> <Configuration Condition=" '$(Configuration)' == '' ">Debug</Configuration> <Platform Condition=" '$(Platform)' == '' ">AnyCPU</Platform> <ProjectGuid>{BC644873-5471-4675-AD4C-E40D91DC7463}</ProjectGuid> <OutputType>WinExe</OutputType> <RootNamespace>WindowsFormsApp1</RootNamespace> <AssemblyName>WindowsFormsApp1</AssemblyName> <TargetFrameworkVersion>v4.7.2</TargetFrameworkVersion> <FileAlignment>512</FileAlignment> <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects> <Deterministic>true</Deterministic> </PropertyGroup>
```

属性组部分包含配置的类型、使用此配置时要生成的输出的类型、的框架。Net，它将用于构建项目、文件对齐细节等。此外，一个项目文件中可以引用多个 propertygroup。

```
<ItemGroup> <Compile Include="Form1.cs"> <SubType>Form</SubType> </Compile> <Compile Include="Form1.Designer.cs"> <DependentUpon>Form1.cs</DependentUpon> </Compile> <Compile Include="Program.cs" /> <Compile Include="Properties\AssemblyInfo.cs" /> </ItemGroup>
```

与属性组类似，项目文件中也可以有多个项组。“Compile Include”中提到的项目中的文件将由 MSBuild 编译，并生成打包的输出。类似地，项组还保存项目内部引用的程序集引用。

## 情况

条件可以作为属性包含在内，以便在运行时为属性设置值。这里，在上面的例子中，在属性组部分中，configuration 有一个名为 Condition 的属性，它有一个在运行时验证配置值的检查。

## 自定义任务

与程序集类似，文件引用、关于要使用的框架版本的详细信息、配置、自定义任务也可以添加到项目文件中。这些属于自定义任务的范畴。

## 属性函数

MSBuild 4.0 版提供了某些内置的常见函数，而不是为许多常见操作编写自定义任务。例如，打印日期时间，这可以通过

```
$([System.DateTime]::Now). $([])
```

上述内容表明，内部编写的内容是系统内置的属性，不需要寻找任何自定义任务。

# 结论

MSBuild 生成和打包。基于网络的应用程序更流畅，更容易，更省事。MSBuild 可以在命令提示符下单独调用，也可以在云服务器中的 CI/CD 管道中调用。

*原载于*[*https://www . partech . nl*](https://www.partech.nl/nl/publicaties/2021/03/ms-build---what-you-need-to-know)*。*