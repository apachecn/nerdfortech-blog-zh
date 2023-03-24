# 创建一个简单的物联网项目

> 原文：<https://medium.com/nerd-for-tech/creating-a-simple-iot-project-590342f6f904?source=collection_archive---------0----------------------->

# 介绍

本教程介绍了一个简单的物联网项目。它由模拟传感器、服务、数据库、web api 和 web 应用程序组成。首先介绍一个快速入门来直接进入代码，然后是一个真实的用例场景。为了实现用例，提出了所需的系统架构和系统组件。最后，说明了潜在的改进可能性。为了有一个简短的概述，下面给出了目录。如果你的兴趣只是其中之一，你可以直接关注相关部分，但是，我建议阅读所有教程。

1.  快速启动
2.  用例
3.  系统结构
4.  所需技术
5.  系统组件

*   MQTT 代理配置
*   物联网数据模型设计
*   模拟物联网传感器实施
*   数据处理和观察者服务实现
*   数据存储和请求服务实现
*   用户 Web 界面实现
*   预测性维护服务实施(TODO)

6.未来作品

# 快速启动

所有项目组件都是 docker 化的，docker-compose 解决了它们之间的所有依赖问题。要运行该项目，只需调用以下命令:

*   假设:docker 和 docker-compose 安装在开发环境中
*   克隆项目:https://github.com/cemakpolat/simple-iot-project.git[git 克隆](https://github.com/cemakpolat/simple-iot-project.git)
*   执行项目文件夹中的命令:`docker-compose build`
*   执行项目文件夹中的命令:`docker-compose up`
*   用户 web 界面: [http://localhost:4200/](http://localhost:4200/)
*   Swagger web 界面:[http://localhost:5003/swagger/index . html](http://localhost:5003/swagger/index.html)
*   Rest 接口:[http://localhost:5003/API/Entity/](http://localhost:5003/api/Entity/)

# 用例

智能工厂环境由许多拥有不同功能的简单和复杂设备组成。所有这些功能的协调需要一个物联网平台，该平台统一从设备到人类参与者的各种不同数据模型。智能工厂的众所周知的主题和关键方面是预先预测设备何时会损坏，因为整个生产链可能会受到系统中损坏设备的严重影响，并且这可能导致整个系统停止，由于成本和用户的观点，这是不希望的。为了防止这类问题，应引入预测性维护系统，以预测设备何时会损坏，何时需要更换。为了形象化这种用例，我们假设在工厂环境中连续监控嵌入生产线的温度传感器。将从该传感器检索到的所有数据发送到后端服务，以供进一步分析。基于观察分析，预测设备损坏的可能性。在下一节中，只实现了数据聚合和可视化部分。预测维护模块稍后将集成到系统的其余部分。

# 系统结构

这个想法的实现可以通过下面介绍的一个小型物联网设置来实现。

![](img/d6e18817c974bc88b1aaaa4ada38054f.png)

# 所需技术

*   通信协议:MQTT、HTTP、Websocket
*   编程语言:C#、JavaScript、TypeScript、Python
*   数据库:MongoDB
*   集装箱技术:码头工人
*   机器学习算法:LSTM，… (TODO)
*   开发环境:麦克·OSX·卡特琳娜
*   。NET 5:从此链接安装【https://docs.microsoft.com/en-us/dotnet/core/install/macos 

# 系统组件

## MQTT 代理

所有其他组件之间的数据分发是通过 MQTT broker 进行的。在这个项目中，选择的 mqtt-broker 是基于 mosquitto-eclipse broker `toke/mosquitto`的定制映像，因为在这个映像中启用了 websocket 通信，这是用户界面所需要的。

技术视图您可以使用以下命令行测试代理:

`docker run -ti -p 127.0.0.1:1883:1883 -p 9001:9001 toke/mosquitto`

测试代理的最简单方法是运行一个简单的 python 脚本或者使用 MQTT.fx 之类的工具。对于 python 库，您可以查看这个链接:[https://github . com/emqx/MQTT-Client-Examples/tree/master/MQTT-Client-python 3](https://github.com/emqx/MQTT-Client-Examples/tree/master/mqtt-client-Python3)

## 物联网数据模型

物联网数据模型抽象了物联网传感器的功能，并允许其与其他传感器或设备进行通信。我们(我和其他同事)在[https://ieeexplore.ieee.org/document/8767276](https://ieeexplore.ieee.org/document/8767276)描述了一个物联网模型，并在[https://github.com/GT-ARC/chariot-apis](https://github.com/GT-ARC/chariot-apis)实现了它。在本教程中，使用了这个模型的一个非常简化的版本。两个类，即 IoTEntity 和属于 IoTEntity 的 Property 是用有限的特性实现的。此型号仅涵盖主要设备，对于复杂设备，建议使用整个库。

## 公共 API

使用物联网数据模型的理想方法是在一个公共 api 文件夹下描述它，并使它能够被其他项目集成。即使这种方法是可能的，并且在 common-api/README.md 文件中描述了如何做，它也不在本教程中考虑。

## 温度传感器/服务(生产商):

传感器模拟一个温度服务，该服务随机生成温度值，并通过 MQTT 通信协议将其发送给作为发布者的代理。服务模拟设备首先传输数据，然后在连续循环中停止一段时间。使用`App.config`文件可以很容易地修改模拟的行为。

***技术视图***

*   创建一个名为 iotservice 的项目:

`dotnet new console --name iotservice`

*   添加 mqtt 连接、更好的日志记录、json-to-from-object 转换器(POJO)和处理配置文件所需的以下库。

```
dotnet add package MQTTnet --version 3.1.1
dotnet add package MQTTnet.Extensions.ManagedClient --version 3.1.1
dotnet add package serilog
dotnet add package Newtonsoft.Json
dotnet add package Serilog.Sinks.Console
dotnet add package System.Configuration.ConfigurationManager --version 6.0.0
```

*   App.config:创建一个 App.config 文件，并按如下方式编辑它。如果单独测试，brokerUrl 可以替换为 localhost。前三个参数是标准的 mqttp 参数，其余的定义模拟器应该如何将数据发送到 mqtt 代理。下面，`waiting_daruratio`表示数据传输频率，而`sim_mode_changing_duration`表示代理应该暂停多长时间，即没有数据传输。最后一个参数显示了传感器 UUID，这是该传感器与其他传感器的不同之处。

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="brokerUrl" value="broker" />
        <add key="brokerPort" value="1883" />
        <add key="mqttClientId" value="sensor-1" />
        <add key="waiting_duration" value="100" />
        <add key="sim_mode_changing_duration" value="10000" />
        <!-- <add key="entityId" value="UUID" /> -->
    </appSettings>
</configuration>
```

*   类:Simulator 类描述了随机值应该多长时间生成和传输一次，然后暂停一次。它可以通过 app.config 文件从另一个脚本进行扩展和控制
*   运行程序，看看它是如何工作的。测试的最低要求是拥有一个 mqtt 代理和一个可以充当订阅者的工具。两者都在 MQTT-Broker 部分给出。

`dotnet run`

*   docker ize:docker 文件描述项目将如何被 docker 化，即复制文件、恢复应用程序、将程序构建/发布为版本。注意，应用多状态构建是为了保持 docker 容器小而高效。多态 docker 构建的工作原理很简单:对第一个图像的需求不会转移到第二个图像，因此图像会保持较小的尺寸。请到 docker-compose 部分，那里给出了这个主题的参考文献。

## 数据处理和观测服务(温度观测服务/数据消费者)

该模块消耗从温度传感器发送的数据并对其进行处理。实际的服务将所有接收到的对象发送给 MQTT 代理，如果在一段时间内接收到任何消息，还会发送错误消息。

***技术视图***

`dotnet new console --name iotservice`

*   App.config:与 iotsensor 中的结构相同。

```
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
    <appSettings>
        <add key="brokerUrl" value="broker" />
        <add key="brokerPort" value="1883" />
        <add key="mqttClientId" value="service-1" />
        <!-- <add key="entityId" value="UUID" /> -->
    </appSettings>
</configuration>
```

*   Classes: Consumer.cs 文件是主类，它使用 Servies/MQTTConnection 对象开始接收消息，并在不同的循环中单独侦听，以了解是否可以在最大时间内接收到消息，如果不能，则向代理发送一条消息来报告此问题。
*   运行程序，看看它是如何工作的。测试的最低要求是拥有一个 mqtt 代理和一个可以充当发布者的工具。两者都在 MQTT-Broker 部分给出。

`dotnet run`

*   Dockerize:与 iotsensor 中的结构相同。

## Mongo 数据库

项目目标之一是使数据在数据库中持久化。在这个项目中，选择 mongodb 来保存数据。对于上述用途，任何类型的数据库，无论是 sql 还是非 sql，都可以满足要求。选择 mongodb 的另一个原因是此链接中提供了良好的结构化和书面教程和文档[https://docs . Microsoft . com/en-us/aspnet/core/tutorials/first-mongo-app？view = aspnetcore-5.0&tabs = visual-studio](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mongo-app?view=aspnetcore-5.0&tabs=visual-studio)或许多其他链接。

***技术视图***

*   配置:所有数据库配置都写在 docker-compose 文件中。如果打算在本地测试它，可以通过 docker 使用 docker-compose 文件中的配置参数 givne 启动映像。
*   mongodb-settings/mongo-init.js:每当 mongo 数据库启动时，它都会调用这个 javascript 脚本，并创建一个名称为`iotworld`，用户为`root`，pwd 为`universe`的数据库。必须给出这些参数

## 数据存储和请求服务

所有传感器数据都直接存储在 mongo 数据库中，swagger 接口能够从数据库中提取并可视化存储的数据。swagger 界面的视图如下所示。

![](img/0f71ea7c4e6eee3e2af8d7d782e3184f.png)

***技术视图***

*   创建项目:该模块不同于 iotsensor 和 iotservice，因为它的需求涵盖了数据库及其可视化。为此，选择了不同的项目模型`webapi`来支持 MVC 概念。此链接详细介绍了这种方法:[https://docs . Microsoft . com/en-us/aspnet/core/tutorials/first-mongo-app？适用于 mac 环境的 view = aspnetcore-5.0&tabs = visual-studio](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mongo-app?view=aspnetcore-5.0&tabs=visual-studio)。除了 MVC 部分，如果消息来自 MQTT 代理，它将直接存储在数据库中。

`dotnet new webapi --name datastore`

*   添加以下库:这些库是与 mongodb 数据库交互、通过 MQTT broker 通信、更好的日志记录和支持 swagger 所必需的。

```
dotnet add package MongoDB.Driver
dotnet add package MQTTnet --version 3.1.1
dotnet add package MQTTnet.Extensions.ManagedClient --version 3.1.1
dotnet add package serilog
dotnet add package Newtonsoft.Json
dotnet add package Serilog.Sinks.Console
dotnet add package serilog --version 2.10.0
dotnet add package Swashbuckle.AspNetCore --version 5.6.3
```

*   appsettings.json:这个文件是自动创建的，它也是一个类似 App.config 的配置文件，只是符号是 json 而不是 XML。有三个配置块可用:1)日志配置，2)数据库设置，如用户名、密码和数据库名，这些可以直接从 docker-compose 传递，3)docker-compose 提供的 mqtt 设置。如果目标是只测试这个组件，请修改 brokerUrl 地址，如 localhost 或另一个 Url 地址。

```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "DatabaseSettings": {
    "ConnectionString": "mongodb://root:universe@mongodb:27017/?readPreference=primary&ssl=false",
    "Database": "iotworld"
  },
  "MQTTSettings":{
    "BrokerURL":"broker",
    "BrokerPort":"1883",
    "MQTTClientID":"datastore-service"
  },
  "AllowedHosts": "*"
}
```

*   json:这个文件也被创建，它包含启动设置。两个参数很重要，`applicationUrl`应该和 docker-compose 文件中写的相似。初始配置的值是 localhost:5000。`ASPNETCORE_ENVIRONMENT`也很重要，必须在 docker-compose 中给出。否则无法访问 swagger 接口。
*   类别:

cs 是一个典型的 rest 类，它使用户能够发送/接收请求/响应。如果需要，可以扩展这个 rest 接口。

Domain/* classes 显示了数据在 mongodb 中的存储结构。

Models/*类用于将 Json 消息转换为经典对象，这也在其他示例中显示

Services/*通过 mqtt connection 使用 IoTService 和 mqtt 连接支持类与数据库的直接交互。JSON 中的 IoT 对象位于 MQTTConnection 对象中，转换为实体对象，然后使用 IoTService 对象存储在 mongodb 中

Startup.cs 类的行为类似于 main initiater，添加所有必需的应用程序配置，附加新类(如 IoTservice)并启动 MQTT 连接。

*   Dockerfile:与 iotsensor 中的结构相同
*   一旦该组件运行，就可以通过[http://localhost:5003/swagger/index . html](http://localhost:5003/swagger/index.html)访问 swagger 接口。请注意，端口号可以不同于 5003，这是 docker-compose 文件中给定的端口。
*   REST API 可以通过[http://localhost:5003/API/Entity/](http://localhost:5003/api/Entity/)调用

## 用户 Web 界面

聚合数据的可视化对最终用户来说更令人满意，而该组件的目的正是专注于只有两个视图的 web 应用程序，这两个视图旨在描述传感器数据和显示日志消息。有了这些次要需求，就可以使用任何种类的 javascript 框架(从 jquery、angular、vue 到 reactjs)轻松创建 web 项目。在该应用中，选择了 angular，并将一个完整的仪表板示例，即`coreui`[https://coreui.io/angular/](https://coreui.io/angular/)，作为开发的基础。原因是要有一个坚实的代码结构，允许在需要时扩展特性。可以通过[https://coreui.io/angular/demo/free/2.11.1/#/dashboard](https://coreui.io/angular/demo/free/2.11.1/#/dashboard)访问免费版本的 coreui 仪表盘。

实现的用户界面如下所示:

![](img/969c56380ee372160361340886adaa01.png)![](img/b1c53db7fec71a49e58f8ffc21861898.png)

***技术视图***

如上所述，该应用程序是建立在(core ui/angular)[[https://coreui.io/angular/](https://coreui.io/angular/)]的自由版本上，通过他的 angular 框架实现。所有附加的子组件都被删除，以形成简化的代码结构。

*   coreui angular template 的使用说明已经在[https://github . com/core ui/core ui-free-angular-admin-template](https://github.com/coreui/coreui-free-angular-admin-template)中解释过了。通过应用它们，可以通过 [http://localhost:4200](http://localhost:4200/) 轻松执行和访问 web 应用程序
*   删除所有不必要的文件，对于这个项目，所有视图都被删除。
*   安装通过 MQTT 交互所需的`ngx-mqtt`模块
*   更新' app.module.ts '中的 mqtt 配置
*   如果没有自动添加`import { IMqttMessage, MqttModule, IMqttServiceOptions } from "ngx-mqtt";`
*   添加 mqtt 配置并在 ngmodule 内部传递配置

```
export const MQTT_SERVICE_OPTIONS: IMqttServiceOptions = {
  hostname: 'localhost',
  port: 9001,
  path: '/mqtt'
}@NgModule({
  declarations: [AppComponent, ...APP_CONTAINERS],
  imports: [
  ...,
   MqttModule.forRoot(MQTT_SERVICE_OPTIONS)
  ],
  providers: [
```

*   创建一个服务来观察 mqtt 主题，并在消息到达时通知 angular 组件，`../app/services/event.mqtt.service.ts`
*   创建两个组件:1)传感器/设备数据可视化，2)在视图上显示日志数据
*   视图/设备:使用 EventMQTTService，解析传入的 json 数据并更新 chartjs 折线图
*   视图/日志:使用 EventMQTTService，解析传入的 json 数据并更新日志数据表。
*   更新 app/containers/default-layout/_ nav . ts 以访问导航面板上的模块。

```
{
    name: 'Devices',
    url: '/devices',
    iconComponent: { name: 'cil-layers' }
  },
  ...
```

*   更新 app-routing.module.ts 以加载应用程序组件。`default`选择的模块是`devices`组件。

```
const routes: Routes = [
  {
    path: '',
    redirectTo: 'devices',
    pathMatch: 'full'
  },
  {
    path: '',
    component: DefaultLayoutComponent,
    data: {
      title: 'Home'
    },
    children: [
      {
        path: 'devices',
        loadChildren: () =>
          import('./views/devices/devices.module').then((m) => m.DevicesModule)
      },
    ...
```

*   上面给定的路由结构确实非常复杂，对于这个应用程序来说可能是不必要的，但是，如果您计划扩展它，很高兴看到如何在这里轻松地添加一个特性。所有路线都列在一个文件中。
*   `npm start`或`ng serve`将直接构建并运行 web 应用程序。如果出现任何问题，可通过 localhost:4200 联系该应用程序

## 预测性维护(TODO)

自主智能工厂需要检测设备的错误或故障的可能性。预测性维护服务旨在汇总来自传感器的数据，并根据历史数据预测设备是否运行正常。该组件将作为未来作品添加。

# 未来作品

*   (短期)使用 Python 和现有项目完成预测性维护应用程序，并在出现故障时通知用户 web 界面。
*   扩展物联网数据模型
*   通过 App.config 将 UUID 添加到每个传感器
*   在 App.config 中添加要订阅或发布到服务或设备的 UUID
*   找到单点故障的解决方案，比如不止一个 MQTT 代理、多个数据库等。
*   改进用户 web 界面，能够在仪表板中显示所有设备。
*   扩展数据存储中的控制器功能