# 带有 Go 的 web 服务—带有 JSON/HTTP 的 ReST 服务器

> 原文：<https://medium.com/nerd-for-tech/webservices-with-go-rest-server-with-json-http-1c9c66e2c156?source=collection_archive---------1----------------------->

![](img/c96990948c81ee4940f47c06d4af10cc.png)

我已经开始在 go 中构建 web 服务。这是一次很棒的学习经历，我将在这个系列中分享我的学习成果，我称之为 **Webservices with Go** 。这将是一个 4 篇文章的系列，其中两篇文章涉及在 Go 中设置 webservice 和使用 JSON-over-HTTP 创建 ReST APIs。后两篇文章介绍了使用 protobuf 和 g rpc 的基于 RPC 的服务器，以及如何使用网关支持 RPC 和 ReST。

为什么选择 webservice？因为 API 是访问系统资源的最佳方法之一。任何设备都可以使用简单的 API 与另一个设备进行交互。

本文假设您已经了解了使用 JSON 作为消息交换格式的 HTTP 协议上的 ReST API 开发。虽然这篇文章使用了陈述的技术，但是你可以理解其中的原理并根据你自己的技术选择进行调整。

我们将在 Go 中使用 **gin-gonic 库**来添加 ReST 支持。让我们开始…

**设置基本回购** 让项目目录为 example.com。在示例中，我们创建了应用程序 hello。因此，我们的基本目录结构是 example.com/hello.，我选择的包结构是

```
config -- 
   config.go
constants -- 
   constants.go
controller -- 
   router.go
   health_controller.go
dao -- 
   db.go
   datastore.go
server -- 
   primary_server.go
main.go
```

首先，我们需要一个依赖管理系统。这里我们使用 **Go 模块**。模块是存储在文件树中的 [Go 包](https://golang.org/ref/spec#Packages)的集合，文件树的根是`go.mod`文件。

```
go mod init example.com/hello
```

这将为依赖管理设置 go 模块，并创建一个 go.mod 文件。为了理解模块的魔力，无论何时你想使用任何 go 库，你需要做的就是运行命令

```
go get <dependency>go get github.com/go-sql-driver/mysql
```

多田…并且你的应用已经为 go 的 mysql db 的 sql 驱动添加了 go 库。

从这里开始，这篇文章将假设无论何时你需要一个新的库，你都可以 google 并**去获得**你的项目中的依赖项。不过，我会尽量强调一下。

**设置应用程序配置** 让我们从设置数据库的最低配置开始，在我的例子中，数据库是 mysql、环境变量和服务器配置。

让我们了解一下我们对这 3 个文件做了什么:

1.  在 **constants.g** o 中，我们定义了可以在整个应用中使用的集中常数。这里，我们定义了映射到环境变量的常量，这些变量将用于为我们的应用程序提供配置。我们已经定义了**服务器端口**和**数据库属性**。
2.  在 **config.go** 中，我们定义了 getEnv()函数，该函数试图使用 go 的 **os** 包来获取环境变量属性。在没有单独提供这些值的情况下，我们为它们定义了一个默认值。
3.  在 **db.go**
4.  我已经定义了用于通过 **DatabaseConfig** struct 建立数据库连接的基础结构
5.  我已经定义了 **DBStruct** struct，它保存了一个指向 **sql 的指针。DB** 和对 DatabaseConfig 结构的引用。 **sql。DB** 来自 Go 的**数据库/sql** 包。还要注意导入的**github.com/go-sql-driver/mysql**，因为这是**数据库/sql** 包内部需要的。

这些都是应用程序需要的基本配置。下一个工作是移动初始化我们的**连接到我们的数据库**、**服务器**和**主**。

**连接到数据库**

**设置基础服务器**

**设置 main 以启动服务器**

您现在可以尝试构建并运行您的服务器。我建议在根文件夹中维护一个 makefile，并在那里创建一个可运行的任务。

**makefile**

只要在你的终端上运行 **make app** 命令，服务器就应该设置好了。如果服务器启动并运行，那么**恭喜！！！**

**注意**:观察控制器。路线已被注释。这是因为我们还没有在项目中添加控制器/路由。

尽管它现在没有多大用处，因为我们还没有添加任何 API。在我的下一篇文章中，我们将学习创建一些 API，然后我们的服务器就变得有用了。

我将在下一篇文章中尝试在 Go 中完成 webservice，主要是通过**学习使用**控制器、路由和处理程序**添加 API**。在那之前继续学习。