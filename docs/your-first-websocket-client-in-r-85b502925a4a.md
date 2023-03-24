# R 中的第一个 Websocket 客户端

> 原文：<https://medium.com/nerd-for-tech/your-first-websocket-client-in-r-85b502925a4a?source=collection_archive---------1----------------------->

![](img/d4530038d137d9b02d7752db90a29ab0.png)

在本教程中，您将学习如何设置“R”和“RStudio ”,以及如何用 R 编写程序。您还将学习如何连接到 WebSocket 服务(在本例中为 Forex data feed ),获取流式 Forex、CFD 和加密货币数据，然后将 JSON 数据解析为可用的格式。您可以从我们网站上的文档部分下载此[代码的版本，并使用您的 API 密钥预先填充](https://tradermade.com/docs/streaming-data-api#wsR)，请确保您已登录。您可以根据自己的选择兑换货币，我们的 WebSockets 可以提供超过 1500 种货币对的数据。

**让我们设置环境**

访问[官方 R 网站](https://cran.rstudio.com/)并下载 R，同时安装 [RStudio](https://www.rstudio.com/products/rstudio/) IDE，这是一个 R 的集成开发环境，可以轻松编写、导入依赖项和运行 R 代码。

**设置研发项目**

现在你有了新的工作空间，我们可以创建一个文件来添加我们的代码，点击**File->New File->R Script**。这将创建一个标题为“Untitled1”的新窗口，单击保存图标，并将该文件命名为“WebSocketClient.r”

**获取您的 API 密钥**

现在环境已经设置好了，让我们来获取您的 WebSocket API 密钥如果您没有帐户，您可以[在此注册](https://tradermade.com/signup)，只需几秒钟，然后您就可以开始 WebSocket 试用，并从您的仪表板复制您的密钥。

**导入库**

我们需要做的第一件事是导入库，我们将下面的代码添加到源文件的顶部。

```
library(websocket)
```

现在我们将需要导入库，从菜单中单击工具->安装包，然后在显示的对话框中的包框中输入“websocket”。

接下来，我们将创建一个 WebSocket 实例

```
ws <- WebSocket$new("wss://marketdata.tradermade.com/feedadv")
```

对于 WebSocket 实现，我们需要创建一些回调函数，当我们的客户端收到消息时，这些函数将被触发。我们将创建一个 onOpen 函数，当 WebSocket 成功建立连接时将调用该函数。我们需要用一个包含用户键和“符号”的连接字符串来响应这个消息。

```
ws$onOpen(function(event){
  ws$send("{\"userKey\":\"userKey\", \"symbol\":\"GBPUSD,EURUSD\"}")
}
```

我们还需要实现一个 onMessage 函数，当收到新消息时，

```
ws$onMessage(function(event) {
  cat( " Symbol ", d, "
")
}
```

完成代码后，我们可以通过单击 RStudio 中的 run 图标来运行它。运行程序时，您需要单击程序的顶部来设置光标位置，然后单击 run 按钮来单步执行代码。

```
library(websocket){
  ws <- WebSocket$new("wss://marketdata.tradermade.com/feedadv")
  ws$onMessage(function(event) {
    d <- event$data   
    cat(" Message ", d, "\n") })
  ws$onOpen(function(event) {
    ws$send("{\"userKey\":\"userKey\", \"symbol\":\"GBPUSD,EURUSD\"}")
  })
}
```

程序运行后，您应该会看到类似如下的输出。

```
Message  Connected 
Message  {"symbol":"EURUSD","ts":"1651070094743","bid":1.05339,"ask":1.05341,"mid":1.0534} 
Message  {"symbol":"EURGBP","ts":"1651070094760","bid":0.84075,"ask":0.84079,"mid":0.84077} 
Message  {"symbol":"GBPUSD","ts":"1651070094765","bid":1.25288,"ask":1.25292,"mid":1.2529} 
Message  {"symbol":"EURUSD","ts":"1651070094768","bid":1.0534,"ask":1.05341,"mid":1.053405} 
Message  {"symbol":"EURUSD","ts":"1651070094771","bid":1.0534,"ask":1.05342,"mid":1.05341} 
Message  {"symbol":"GBPUSD","ts":"1651070094814","bid":1.25289,"ask":1.25292,"mid":1.252905} 
Message  {"symbol":"GBPUSD","ts":"1651070094815","bid":1.25289,"ask":1.25293,"mid":1.25291} 
Message  {"symbol":"EURGBP","ts":"1651070094971","bid":0.84076,"ask":0.84078,"mid":0.84077}
```

现在我们得到了 JSON 格式的实时数据，让我们看看如何将它分解成各个组成部分。为此，我们将使用 jsonlite 库。我们需要使用以下命令导入这个库。

```
library(jsonlite)
```

我们还需要将其导入到 RStudio 开发环境中，从菜单中单击**工具- >安装包**，然后在显示的对话框中的包框中输入 jsonlite。

当程序连接到 API 时，它会发送一个“Connected”消息，我们需要在解析数据之前捕捉到这个消息。

```
d <- event$data
if (d != "Connected"){}
```

我们现在可以解析 JSON 字符串，这将为我们提供一个可以用来访问数据项的 JSON 对象。

```
json = fromJSON(d)
cat(" Symbol ", json$symbol, json$ts, json$bid, json$ask, json$mid)
```

下面是完整的程序代码。

```
library(websocket)
library(jsonlite){
  ws <- WebSocket$new("wss://marketdata.tradermade.com/feedadv")
  ws$onMessage(function(event) {
    d <- event$data
    if (d != "Connected"){
        json = fromJSON(d)
        cat(" Symbol ", json$symbol, json$ts, json$bid, json$ask, json$mid)
    }
   }) ws$onOpen(function(event) {
       ws$send("{\"userKey\":\"userKey\", \"symbol\":\"GBPUSD,EURUSD\"}")
   }) }
```

现在，您应该有一个工作的 WebSocket 客户端，它将连接到实时外汇、CFD 和加密货币数据服务，并将返回的 JSON 代码解析为可用的格式。

如果你有任何问题，让我知道。随时欢迎对未来教程的想法和建议。请为我们的工作鼓掌，我们非常感激。此外，请关注以获取最新更新。