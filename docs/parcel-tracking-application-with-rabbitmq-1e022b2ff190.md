# 使用 RabbitMQ 的包裹跟踪应用程序

> 原文：<https://medium.com/nerd-for-tech/parcel-tracking-application-with-rabbitmq-1e022b2ff190?source=collection_archive---------9----------------------->

![](img/fa724cc485a4836b49fd387643e22aab.png)

在开发过程中，除了指定将要使用的技术之外，确定项目架构将会如何也是很重要的。如今，最常见的项目架构是微服务架构。微服务架构的一个基本特性是每个服务都可以单独部署。由于这个特性，每个服务都可以在项目中使用不同的协议。这意味着一个项目可以包含 HTTP、AMQP 和 WS 连接协议。

在本文中，我们将构建一个同时使用 HTTP、AMQP 和 WS 的服务。在这项服务中，我们将使用这些技术:

*   快递. js
*   MongoDB
*   [插座。木卫一](http://socket.io/)
*   兔子 q
*   反应堆

我们将使用 Express.js 库来处理 HTTP 请求。当收到 HTTP 请求时，我们将执行 *publishers* ，它向 RabbitMQ 消息代理发布关于事件的消息。我们还将创建消费者来监听发布者发送给 RabbitMQ 消息代理的消息，并将它们保存到 MongoDB 数据库中。最后，我们将使用[插座。IO](http://socket.io/) 为了跟踪 MongoDB 中的每一个变化，并在使用 React 构建的前端中呈现这些变化，而无需刷新页面。

下面是[项目](https://github.com/ardaorkin/rabbitmq-parcel-tracking.git)的最终代码

# 要求

*   NodeJS 12v+

# 建筑开发环境

# 计算机网络服务器

*   首先，让我们创建一个名为包裹追踪系统的文件夹。在该文件夹中，我们再创建一个名为 server 的文件夹。然后打开一个终端，转到您刚刚创建的服务器文件夹所在的目录，并运行 npm init -y。

然后再次返回到终端，要安装项目依赖项，请在名为 server 的文件夹所在的目录中运行以下命令:

```
npm i express dotenv tortoise mongoose socket.io nodemon
```

为了确保命令被成功执行并且包被安装，查看终端中返回的消息，打开 package.json 文件并查看依赖项部分。

此外，让我们安装开发环境依赖项:

`npm i --save-dev @babel/core @babel/preset-env babel-loader`

稍后，创建一个名为。babelrc 并在其中编写以下代码:

```
{                                        
    "presets": ["@babel/env"]                                        
}
```

现在，让我们再创建一个名为 server.js 的文件，并在其中编写以下代码:

```
//server.jsimport express from "express" const app = express() app.use("/", (req, res) => { res.send("Welcome to parcel tracking system") }) app.listen(8000, () => console.log(`Server listening on 8000`))
```

使用以下命令执行 server.js:

```
nodemon ./server --exec babel -e js
```

执行后，在浏览器中打开 [localhost:3000](http://localhost:3000/) 。如果页面上出现“欢迎使用包裹追踪系统”消息，则意味着 Express.js 安装已成功结束。

# 出版商和消费者创作

在这些步骤中，我们将创建我们的第一个发布者和消费者。发布者将发布一条消息，将其发送到 RabbitMQ 消息代理中的*交换站*,*交换站*将消息发送到消息队列，*消费者*监听队列中他们关心的消息，接收消息并做我们想做的事情。

现在，我们将通过消费者获得的消息记录到控制台。为此，让我们在名为 server 的文件夹下创建两个名为 publisher 和 consumer 的文件夹。在 publisher 文件夹中，创建一个名为 shippingPublihser.js 的文件，在 consumer 文件夹中，创建一个名为 shippingConsumer.js 的文件。

将此代码写入 shippingPublihser.js 文件:

```
//shippingPublisher.jsimport Tortoise from "tortoise"          
import dotenv from "dotenv";          
dotenv.config() const tortoise = new Tortoise(process.env.AMQP_URL)    
        tortoise      
            .exchange("parcel-tracking", "topic", { durable: false })      
            .publish("parcel.shipping", { name: "test", status: "shipping" });
```

现在，让我们编写消费者，它将接收由发布者发送给消息代理的消息。为此，请将此代码写入 shippingConsumer.js 文件:

```
//shippingConsumer.jsimport Tortoise from "tortoise"                            
import dotenv from "dotenv";                            
dotenv.config() const tortoise = new Tortoise(process.env.AMQP_URL) tortoise  
    .queue("", { durable: false })  
    .exchange("parcel-tracking", "topic", "*.shipping", { durable: false })  
    .prefetch(1)  
    .json()  
    .subscribe((msg, ack, nack) => {    
        console.log(msg)    
        ack();  
    });
```

我们可以选择在本地机器上安装 RabbitMQ，但是在这种情况下，不同操作系统的安装步骤会有所不同，我们需要修改一些网络设置。因此，我们用[cloudamqp.com](http://cloudamqp.com/)做这一步。为此，让我们创建[cloudamqp.com](https://cloudamqp.com/)。稍后，单击按钮 Create New Instance 并创建一个新的 message broker 实例。我们可以随意命名实例。要自由计划，选择小狐猴选项。然后分别按下名为选择区域>的按钮审核>创建实例。转到列出了消息代理的[页面](https://customer.cloudamqp.com/instance),单击我们刚刚创建的消息代理实例的名称。在页面中出现的详细信息部分复制 AMQP URL 的值。在应用这些步骤之后，返回到服务器文件夹，创建一个名为. env 的文件。环境文件:

```
AMQP_URL="<copied_amqp_url>"
```

通过这个过程，我们创建了一个 AQMP 服务。现在是运行我们的发布者和消费者的时候了。为此，在两个独立的终端中，分别运行以下命令:

```
nodemon ./consumers/shippingConsumer --exec babel-node -e js          
nodemon ./publishers/shippingPublisher --exec babel-node -e js
```

应用这些步骤后，我们应该在运行 shippingConsumer.js 的终端中看到这条消息:

```
{ name: 'test', status: 'shipping' }
```

此时我们有一个工作服务！该服务使用 AMQP 作为通信协议。服务有两个目的；一个是发行商，一个是消费者。在这个流程发布者的工作完成之后，发布者向消息代理发送消息，并且不等待任何回复。消费者只关心消息头包括什么。在这种情况下，消费者关心标题中有 *shipping* 声明的消息。除了消息头之外，消费者不需要任何东西。

下一步，我们将在收到 HTTP 请求时发布消息。

# 处理 HTTP 请求

让我们为每个发布者和消费者再创建两个。再次到服务器文件夹。在 publishers 文件夹中创建这些文件:

*   onroadPublisher.js
*   deliveredPublisher.js

并在消费者文件夹中创建这些文件:

*   onroadConsumer.js
*   deliveredConsumer.js

现在我们将按照*的承诺*编写所有 publisher 文件:

```
//shippingPublisher.jsimport Tortoise from "tortoise";            
import dotenv from "dotenv";            
dotenv.config(); const tortoise = new Tortoise(process.env.AMQP_SERVER); const shippingPublisher = (name) =>              
    new Promise((resolve, reject) => {                
        tortoise                  
            .exchange("parcel-tracking", "topic", { durable: false })                  
            .publish("parcel.shipping", { name, status: "shipping" });                  
        resolve({ name, status: "shipping" });              
}); export default shippingPublisher;//onroadPublisher.jsimport Tortoise from "tortoise";            
import dotenv from "dotenv";            
dotenv.config(); const tortoise = new Tortoise(process.env.AMQP_SERVER); const onroadPublisher = (name) =>              
    new Promise((resolve, reject) => {                
        tortoise                  
            .exchange("parcel-tracking", "topic", { durable: false })                  
            .publish("parcel.onroad", { name, status: "onroad" });                  
        resolve({ name, status: "onroad" });              
}); export default onroadPublisher;//deliveredPublisher.jsimport Tortoise from "tortoise";            
import dotenv from "dotenv";            
dotenv.config(); const tortoise = new Tortoise(process.env.AMQP_SERVER); const deliveredPublisher = (name) =>              
    new Promise((resolve, reject) => {                
        tortoise                  
            .exchange("parcel-tracking", "topic", { durable: false })                  
            .publish("parcel.delivered", { name, status: "delivered" });                  
        resolve({ name, status: "delivered" });              
}); export default deliveredPublisher;
```

稍后，让我们对消费者进行编码:

```
//shippingConsumer.jsimport Tortoise from "tortoise";            
import dotenv from "dotenv";            
dotenv.config(); const tortoise = new Tortoise(process.env.AMQP_SERVER);            
tortoise              
    .queue("", { durable: false })              
    .exchange("parcel-tracking", "topic", "*.shipping", { durable: false })              
    .prefetch(1)              
    .json()              
    .subscribe((msg, ack, nack) => {                
        console.log(msg)                
    ack();              
    });//onroadConsumer.jsimport Tortoise from "tortoise";            
import dotenv from "dotenv";            
dotenv.config(); const tortoise = new Tortoise(process.env.AMQP_SERVER);            
tortoise              
    .queue("", { durable: false })              
    .exchange("parcel-tracking", "topic", "*.onroad", { durable: false })              
    .prefetch(1)              
    .json()              
    .subscribe((msg, ack, nack) => {                
        console.log(msg)                
    ack();              
    });//deliveredConsumer.jsimport Tortoise from "tortoise";            
import dotenv from "dotenv";            
dotenv.config(); const tortoise = new Tortoise(process.env.AMQP_SERVER);            
tortoise              
    .queue("", { durable: false })              
    .exchange("parcel-tracking", "topic", "*.delivered", { durable: false })              
    .prefetch(1)              
    .json()              
    .subscribe((msg, ack, nack) => {                
        console.log(msg)                
    ack();              
    });
```

在这些配置之后，当服务器接收 HTTP 请求时，发布者将被执行，当这些发布者向消息代理发送消息时，我们的消费者将会监听这些消息。

现在让我们创建 HTTP 路由。出于最佳实践和干净代码的考虑，在服务器文件夹下创建一个名为 routes 的文件夹，并在其中创建一个名为 index.js 的文件。稍后将这几行写到 index.js 文件中:

```
//index.jsimport { Router } from "express";            
import shippingPublishers from "../publishers/shippingPublisher";            
import onroadPublisher from "../publishers/onroadPublisher";            
import deliveredPublisher from "../publishers/deliveredPublisher"; const router = Router(); router.get("/", (req, res) => {              
    res.send("Welcome to parcel-tracking system");            
}); router.get("/shipping/:name", async (req, res, next) => {              
    const name = req.params.name;              
    await shippingPublishers(name).then((message) => res.json(message));            
}); router.get("/onroad/:name", async (req, res, next) => {              
    const name = req.params.name;              
    await onroadPublisher(name).then((message) => res.json(message));            
}); router.get("/delivered/:name", async (req, res, next) => {              
    const name = req.params.name;              
    await deliveredPublisher(name).then((message) => res.json(message));            
}); export default router;
```

创建 HTTP 路由后，让我们像这样修改所有 server.js 文件:

```
//server.jsimport express from "express";
import router from "./routes"; const app = express();
const port = process.env.PORT || 8000;app.use(router);app.listen(port, () => console.log(`Server listening on port ${port}`));
```

稍后，转到服务器文件夹下，运行以下命令:

```
nodemon ./server --exec babel-node -e js            
nodemon ./consumers/shippingConsumer --exec babel-node -e js            
nodemon ./consumers/onroadConsumer --exec babel-node -e js            
nodemon ./consumers/deliveredConsumer --exec babel-node -e js
```

命令运行后，在浏览器中分别打开这些链接:

*   [本地主机:8000/出货/测试](http://localhost:8000/shipping/test)
*   [本地主机:8000/在途/测试](http://localhost:8000/onroad/test)
*   [本地主机:8000/交付/测试](http://localhost:8000/delivered/test)

当我们打开链接时，我们应该在终端中以 JSON 格式看到发送到消息代理的消息。

在看到消费者运行的终端和在浏览器中返回的 JSON 数据之后，我们可以确定当服务器接收 HTTP 请求时，发布者和消费者都在运行。

下一步，我们将把消息保存到 MongoDB。

# MongoDB 配置

要配置 MongoDB，让我们在[mongodb.com/cloud](https://www.mongodb.com/cloud)中创建一个帐户。稍后，创建一个组织、组织中的一个项目和项目中的一个群。创建集群后，在集群所在的页面中，单击数据库访问。在那里单击添加新数据库用户选项，并创建一个数据库用户。稍后，单击左侧栏上的集群选项。在打开的页面上，单击连接按钮。在“设置连接安全性”步骤中，选择“允许任何地方”选项，然后单击“选择连接方法”。然后，让我们单击“连接您的应用程序”选项，并将“添加您的连接字符串”下的连接信息复制到您的应用程序代码中。让我们回到文本编辑器，在。env 文件，并将 MongoDB 连接信息赋给该变量。使用刚才在数据库访问步骤中创建的密码进行更改，并将 myFirstDatabase 更改为 parceltracking:

```
MONGODB_URL="mongodb+srv://username:12345@cluster0.mjh9d.mongodb.net/parceltracking?retryWrites=true&w=majority"
```

完成这些步骤后，转到 server.js 文件，在导入 routes 文件夹的行之后写下这些行:

```
//server.jsimport mongoose from "mongoose"
import dotenv from "dotenv"
dotenv.config()              
mongoose.connect(process.env.MONGODB_URL, {              
    useNewUrlParser: true,              
    useUnifiedTopology: true,            
}); const db = mongoose.connection;            
db.on("error", console.error.bind(console, "connection error:"));            
db.once("open", () => console.log("Connected to database"));
```

通过这些线路，我们打开了一个 MongoDB 连接。为了确保一切正常，让我们用以下命令运行 server.js:

```
nodemon ./server --exec babel-node -e js
```

如果我们在终端的控制台中看到连接到数据库的消息，这意味着我们正确地进行了数据库配置。下一步，我们将创建一个 MongoDB 模式和模型。为此，让我们在服务器文件夹中创建一个名为 model 的文件夹。在模型文件夹中，创建一个名为 Tracking.js 的文件，并写下以下代码:

```
//Tracking.jsimport mongoose from "mongoose";const trackingSchema = new mongoose.Schema({              
    name: String,              
    status: String,            
}); const Track = mongoose.model("Track", trackingSchema); export default Track;
```

在创建模型之后，我们将为消费者使用它。但是当我们使用 mongoose 库中的 save()方法时，updateOne()方法将用于其他消费者。首先，让我们像这样修改 shippinConsumer.js 文件:

```
//shippingConsumer.jsimport Tortoise from "tortoise";            
import mongoose from "mongoose"            
import Track from "../model/Tracking";            
import dotenv from "dotenv"            
dotenv.config()                          
mongoose.connect(process.env.MONGODB_URL, {                          
    useNewUrlParser: true,                          
    useUnifiedTopology: true,                        
}); const db = mongoose.connection;                        
db.on("error", console.error.bind(console, "connection error:"));                        
db.once("open", () => console.log("Connected to database")); const tortoise = new Tortoise(process.env.AMQP_SERVER);            
tortoise              
    .queue("", { durable: false })              
    .exchange("parcel-tracking", "topic", "*.shipping", { durable: false })              
    .prefetch(1)              
    .json()              
    .subscribe((msg, ack, nack) => {                
        const newParcel = new Track(msg);                
        newParcel.save((err, parcel) => {                  
            if (err) throw err;                  
            console.log("shipped parcel:", parcel);                  
            return parcel;                  
        });                
        ack();              
    });
```

这样，shippinConsumer.js 将在 MongoDB 中创建新记录。现在，是时候更新这个记录了。为此，让我们像这样修改 onroadConsumer 和 deliveredConsumer:

```
//onroadConsumer.jsimport Tortoise from "tortoise";              
import mongoose from "mongoose"              
import Track from "../model/Tracking";              
import dotenv from "dotenv"              
dotenv.config()                            
mongoose.connect(process.env.MONGODB_URL, {                            
    useNewUrlParser: true,                            
    useUnifiedTopology: true,                          
}); const db = mongoose.connection;                          
db.on("error", console.error.bind(console, "connection error:"));                          
db.once("open", () => console.log("Connected to database")); const tortoise = new Tortoise(process.env.AMQP_SERVER);            
tortoise              
    .queue("", { durable: false })              
    .exchange("parcel-tracking", "topic", "*.onroad", { durable: false })              
    .prefetch(1)              
    .json()              
    .subscribe(async (msg, ack, nack) => {                
        const onroadParcel = await Track.updateOne(                  
            { name: msg.name },                  
            { status: msg.status },                  
            (err, parcel) => {                    
                if (err) throw err;                    
                else return parcel;                  
            }                
        );                
        console.log("parcel is on road:", onroadParcel);                
        ack();              
    });//deliveredConsumer.jsimport Tortoise from "tortoise";              
import mongoose from "mongoose"              
import Track from "../model/Tracking";              
import dotenv from "dotenv"              
dotenv.config()                            
mongoose.connect(process.env.MONGODB_URL, {                            
    useNewUrlParser: true,                            
    useUnifiedTopology: true,                          
}); const db = mongoose.connection;                          
db.on("error", console.error.bind(console, "connection error:"));                          
db.once("open", () => console.log("Connected to database")); const tortoise = new Tortoise(process.env.AMQP_SERVER);            
tortoise              
    .queue("", { durable: false })              
    .exchange("parcel-tracking", "topic", "*.delivered", { durable: false })              
    .prefetch(1)              
    .json()              
    .subscribe(async (msg, ack, nack) => {                
        const deliveredParcel = await Track.updateOne(                  
            { name: msg.name },                  
            { status: msg.status },                  
            (err, parcel) => {                    
                if (err) throw err;                    
                else return parcel;                  
            }                
        );                
        console.log("parcel was delivered:", deliveredParcel);                
        ack();              
    });
```

完成这些配置后，运行以下命令:

```
nodemon ./server --exec babel-node -e js            
nodemon ./consumers/shippingConsumer --exec babel-node -e js            
nodemon ./consumers/onroadConsumer --exec babel-node -e js            
nodemon ./consumers/deliveredConsumer --exec babel-node -e js
```

如果您在所有终端中都看到连接到数据库，这意味着您成功地跳过了所有步骤。现在在浏览器中打开下面的链接，以便测试所有的数据库查询，并且每次都检查 MongoDB 数据库:

*   [本地主机:8000/发货/测试](http://localhost:8000/shipping/test)
*   [本地主机:8000/在途/测试](http://localhost:8000/onroad/test)
*   [本地主机:8000/交付/测试](http://localhost:8000/delivered/test)

单击 MongoDB 云中集群页面中的 Collections 选项卡。在出现的页面中，单击 parceltracking 下名为 tracks 的集合。

如果我们的查询是正确的，每次当 shippingConsumer 处理消息时，一条新记录将被添加到 MongoDB，并且每次其他消费者处理消息时，该记录将被更新。要显示记录中的更改，您需要单击集合页面上的刷新按钮

现在是时候使用 Web Socket 来呈现记录中的更改了。

# 用 WebSocket 渲染实时数据

为了用 WebSocket 渲染实时数据，我们将使用[套接字。IO](http://socket.io/) 库。每次用户在数据库中进行更改时，新的记录将呈现在应用程序的首页，而无需刷新页面。为此，让我们在服务器文件夹中创建名为 socket 的文件夹，并添加名为 trackerSocket 的文件。js 在里面。稍后在文件中写下这几行:

```
//trackerSocket.jsimport socketIo from "socket.io";            
import express from "express";            
import http from "http";            
import mongoose from "mongoose";            
import Track from "../model/Tracking";            
import dotenv from "dotenv"; dotenv.config(); const port = process.env.WS_PORT || 8001; mongoose.connect(process.env.MONGODB_URL, {            
      useNewUrlParser: true,              
    useUnifiedTopology: true,            
}); const db = mongoose.connection;            
db.on("error", console.error.bind(console, "connection error:"));            
db.once("open", () => console.log("Connected to database")); const app = express(); const server = http.createServer(app); const io = socketIo(server, {              
    cors: {                
    origin: "*",                
    methods: ["GET", "POST"],              
    },            
}); let interval; const findParcel = async (socket) => {              
    const parcel = await Track.find({}, (err, parcel) => {                
        if (err) throw err;                
            console.log(parcel);                
            return parcel;              
    });              
    socket.emit("parcel", parcel);            
}; io.on("connection", (socket) => {              
    console.log("New client connected");              
    if (interval) {                
        clearInterval(interval);              
    }              
    interval = setInterval(() => findParcel(socket), 1000);              
    socket.on("disconnect", () => {                
        console.log("Client disconnected");                
        clearInterval(interval);                
    });                
}); server.listen(port, () => console.log(`Listening on port ${port}`));
```

写完这些行之后，运行以下命令:

```
nodemon ./socket/trackerSocket --exec babel-node -e js
```

如果您看到消息侦听端口 8001 并连接到命令的数据库输出，这意味着命令正在成功运行。

现在，为了进行前端安装，打开一个终端，转到我们最初创建的名为 parcel-tracking-system 的文件夹下:

```
npx create-react-app client
```

稍后，在客户端文件夹下，运行此命令以安装[socket . io](http://socket.io/)-客户端库:

```
yarn add socket.io-client
```

安装过程结束后，转到客户端文件夹中 src 文件夹下的。在这里，创建一个名为 socket.js 的文件，并在其中写下以下几行:

```
//socket.jsimport socketIOClient from "socket.io-client";            
const ENDPOINT = "http://127.0.0.1:8001"; const socket = socketIOClient(ENDPOINT); export default socket;
```

创建 sokcet.js 后，在同一个目录中打开 App.js，并对其进行如下修改:

```
//App.jsimport React from "react";            
import socket from "./socket"; function App() {              
    const [parcels, setParcel] = React.useState([{}]);              
    React.useEffect(() => {                
        socket.on("parcel", (data) => setParcel(data));                
    });                
    return (                  
        <div>                  
            {parcels.map((parcel) => (                    
            <>                    
                <div>ID: {parcel._id}</div>                    
                <div>Name: {parcel.name}</div>                    
                <div>Status: {parcel.status}</div>                    
                <br></br>                    
            </>                    
        ))}                    
        </div>                    
    );                    
} export default App;
```

要运行 React 应用程序，请在终端的客户端目录下运行以下命令:

```
yarn start
```

在自动打开的页面中，您应该会看到 MongoDB 中的记录。要添加新记录并进行更新，请分别打开以下链接:

*   [localhost:8000/shipping/my cargo](http://localhost:8000/shipping/mycargo)
*   [localhost:8000/on road/my cargo](http://localhost:8000/onroad/mycargo)
*   [localhost:8000/delivered/my cargo](http://localhost:8000/delivered/mycargo)

如果你每次点击都能看到 MongoDB 的变化，那么恭喜你，你有一点服务了！

# 结论

微服务结构在新的应用中越来越成为首选结构。软件团队正在努力将现有的整体结构迁移到微服务架构。此外，AMQP 作为一种非常受欢迎的通信协议脱颖而出，并为 HTTP 遇到的一些问题提供了解决方案。在最终用户方面，对实时应用的兴趣和需求也在增加，web sockets 为软件开发人员提供了极大的便利。掌握这些工具是一种乐趣，它们用于响应新的需求和新的技术，并且对开发者有很大的帮助。在整篇文章中，我编写了一个小服务，并试图通过总结使用这些结构和技术来提供一个大致的理解。希望对那些阅读、跟随、应用的人有所贡献。