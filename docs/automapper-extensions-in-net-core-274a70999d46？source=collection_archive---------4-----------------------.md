# 中的自动映射扩展。网络核心

> 原文：<https://medium.com/nerd-for-tech/automapper-extensions-in-net-core-274a70999d46?source=collection_archive---------4----------------------->

![](img/e70b3f2bde51f2ff24d19d4d83298f05.png)

在 c#中，一个 POCO 类对象到另一个 POCO 类对象的映射可以通过将源属性逐个分配给目标属性来实现。

源 DTO 类:

![](img/05c9827a247b6ae11782df4146531bd0.png)

OrderDetails.cs

目的地 DTO 舱位等级:

![](img/59ea6e612404510c13bc4bf52af61a6a.png)

客户详细信息. cs

![](img/d101aa9bb36e8a84d39cefde0f813307.png)

OrderDetailResponse 方法

在上面的代码中，第 16 到 19 行是我们将数据从源对象属性逐一映射到目的对象属性的地方。

为了避免逐个映射每个属性，我们可以使用 AutoMapper 包。使用这个包，我们可以定义源对象到目的对象，这避免了开发人员的责任。代码可以是可读的格式，代码看起来很干净。

**步骤 1** :从 nuget 安装 Automapper 包，如下图

![](img/2e682d645ffabed2966b66d984dbe8f9.png)

**步骤 2** :在 OrderResponse 类中，我们将使用 Automapper 包声明 IMapper 接口。

![](img/2030f2a8d241f3d01ae1b75659b64d86.png)

**步骤 3** :在 OrderDetailResponse 方法内部，我们可以使用来自自动映射器引用的 MapperConfiguration 方法来定义源和目标 DTO 类。

![](img/d879fdcef62c54d5d1ec1245df6b9d3e.png)

**步骤 4** :使用 Create mapper()创建映射器，如下所示

![](img/1858bc2db162f258aade58c603ebfc4a.png)

**步骤 5** :现在我们使用 Map 将源数据映射到目标数据

![](img/48202bd2516e0e79bea6b881ef153782.png)

在上面第 27 行代码中，我们将数据映射到目标对象

通过使用上面的 Automapper，我们避免了逐个映射每个属性。

使用 Automapper 包后，OrderResponse.cs 代码会像下面这样

![](img/2c83656ece8d33ab24620c1e93c81bc4.png)

订单响应. cs

**步骤 6** :添加以下代码，在 Startup 类的 ConfigureServices 方法中注册 AutoMapper。

![](img/fd4518eabf2f8c76161e4be12225e2ad.png)

这就完成了中的自动映射器实现。Net 核心应用程序:)