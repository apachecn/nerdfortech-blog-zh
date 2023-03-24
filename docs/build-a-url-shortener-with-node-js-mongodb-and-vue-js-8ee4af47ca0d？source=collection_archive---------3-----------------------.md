# 用 Node.js，MongoDB 和 Vue.js 构建一个 URL Shortener

> 原文：<https://medium.com/nerd-for-tech/build-a-url-shortener-with-node-js-mongodb-and-vue-js-8ee4af47ca0d?source=collection_archive---------3----------------------->

![](img/6b7f8c0af39c97862bebfe69ebf93382.png)

你可能以前用过网址缩写，比如 [shorturl](https://www.shorturl.at/) 或者 [bitly](https://bitly.com/) 。它们有助于将长的(难看的)网址缩短为短的(更吸引人的)链接，你可以很容易地与你的朋友、家人或同事分享。一个 URL 缩短器可以用不同的框架来构建。很难选择一个 JavaScript 框架——因为有太多这样的框架，而且它们之间的差异并不明显。在为一个项目选择框架时，我关注的是生产率("*我能以多快的速度完成任务("*")和性能("*我的 web 应用程序能以多快的速度完成任务("T10)，它们对我来说是最重要的标准。

因此对于这个项目，我们将在前端使用 Vue.js 框架，在后端使用 Node.js 和 MongoDB 数据库。*

# 为什么我们需要一个网址缩写

1.  有时，社交平台、位置等的链接变得如此之大，以至于很难管理它们。
2.  较短的链接占用较少的空间，更容易共享和输入。
3.  大多数网址缩写提供的功能可以让你追踪你的网址被点击的次数。

为了理解这是如何工作的，我们需要更仔细地看看 URL 缩短器——所以我们将构建一个简单的 URL 缩短器！在本文中，我们将使用 Node.js、MongoDB 和 Vue.js 构建一个简单的 URL 缩短器。

# 使用的技术

如前所述，我们将在前端使用 Vue.js 框架，在后端和 MongoDB 数据库上使用 Node.js。

# 为什么要用 Vue.js

我之所以选择 Vue.js 而不是这个项目的其他选项，是因为它:

1.  学习容易:Vue 的学习曲线相对容易，你只需要有开发 web 应用的 HTML 或 JavaScript 的基础知识。不像 React 或 Angular 等其他框架需要了解 JSX 和 typescript。
2.  易于理解的文档:Vue.js 框架[文档](https://v3.vuejs.org/guide/introduction.html#what-is-vue-js)定义良好，结构清晰，这使得开发人员编写和执行他们的应用程序更加简单。
3.  更高的性能:与其他框架相比，这些框架提供了更好的性能，比如 React。
4.  简单易懂:简单是 Vue DNA 的一部分，我选择用 Vue 做这个项目的原因之一是因为它简单易懂，结构简单。
5.  微小的尺寸:Vue.js 可以重约 20 KB 或更少，用户下载和使用它不需要时间。它击败了所有笨重的框架，如 React.js、Angular.js 和 Ember.js。

Vue.js 与其他框架的经典对比可以在这里看到[。](http://A classic comparison of Vue.js)

# 为什么对后端和 MongoDB 数据库使用 Node.js

我选择 Node.js 作为后端和 MongoDB 数据库有几个原因。尽管 Node.js 与 MySQL 数据库配合得很好，但完美的组合是像 MongoDB 这样的 NoSQL，其中的模式不需要结构良好。
我选择 Node.js 作为这个项目的后端是出于以下原因:

1.  一个代码库:通过为这个项目使用 [Node.js](https://nodejs.dev/) ，我在这个应用程序的前端和后端使用相同的语言 javascript。这比使用不同的语言(如 Laravel 或 Python)更容易维护。
2.  可重用代码:使用 Node.js 作为我的后端技术，在这个项目的前端和后端部分之间共享和重用代码更容易，这加快了开发过程。
3.  它的 Javascript:使用 Node.js，我只需要理解后端背后的方法论，而不是一门全新的语言。
4.  它快如闪电 **:** Node.js 主要是一个由 V8 支持的 [JavaScript 运行时，由谷歌开发用于 Chrome。这使得 Node.js 具有很高的性能。因此，对文件系统、网络连接和数据库的读/写在 Node 中执行得非常快。](https://nodejs.dev/learn)
5.  广泛的托管选项:使用 Node.js 时有更广泛的托管选项，我们可以用它来托管 URL shortener。

# 先决条件

以下是我们将在此项目中使用的先决条件和工具:

*   Vue.js(此处下载[或安装`npm install -g @vue/cli)`](https://vuejs.org/)
*   [VS 代码](https://code.visualstudio.com/)(此处下载)
*   节点(此处下载)
*   MongoDB(此处下载)

一旦你有一切，我们可以开始我们的项目！

# 创建项目

首先，我们将创建一个名为 **server 的目录。**
在你的终端键入以下内容:

```
mkdir server && cd server
npm init
```

第一个命令将创建我们的目录并移入其中，第二个命令初始化一个接受默认值的 **package.json** 。

```
{
 "name": "server",
 "version": "1.0.0",
 "main": "index.js",
 "license": "MIT"
}
```

我们还需要安装一些依赖项。

*   Axios→进行 API 调用；
*   mongose→连接我们的 MongoDB 数据库
*   Shortid →创建短的唯一字符串，我们需要
*   Validate.js →验证收到的 URL
*   Dotenv →加载环境变量
*   CORS →允许从我们的客户端应用程序和 express 访问
*   快速→后端 web 框架

安装下列依赖项。

```
npm install axios --save npm install mongoose --save npm install shortid --save npm install validate.js --save npm install dotenv --save npm install express --savenpm install cors --save
```

# 设置 Express

1.  在你最喜欢的编辑器上创建一个这样的文件夹结构(我的是 VS 代码)。
2.  为了设置我们的服务器，我们将首先向我们的**添加一些环境变量。env** 文件。复制并粘贴下列内容。

```
port = 5050
host = localhost
```

我们可以使用`process.env.` **variable_name** 在整个应用程序中访问这些变量。

1.  接下来，我们将设置我们的 express server open**index . js**并粘贴以下内容。

```
const express = require('express');
const app = express();
const cors = require('cors');
require('dotenv').config()
const port = process.env.port;
const host = process.env.host;
const bodyParser = require("body-parser"); //use to parse incoming request bodiesconst services = require("./routes/service");
const db = require("./data/db");
const urlDb = require("./data/url");const corsOptions = {
  origin: 'http://localhost:8080',
  optionsSuccessStatus: 200
}app.use(cors(corsOptions))
app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());app.listen(port, () => console.log("listening port " + port));
```

此时，我们正在设置我们的基本服务器，并需要我们之前安装的所需的软件包和文件，例如:

*   **dotenv →** 允许节点读入环境变量
*   **body-parser →** 用于解析传入服务器的请求体
*   **服务→** 将包含一些处理 URL 的逻辑(比如验证)
*   **db →** 我们的数据库
*   **urlDb →** 包含了我们存储和检索 URL 的函数
*   **cors →** 用于允许其他域(例如我们的前端)向我们的 API 发出请求

**corsOptions** 变量中的`origin: 'http://localhost:8080'`告诉我们的应用程序只接受来自该域的请求，该域将是我们的客户端。Vue.js 默认端口是 8080，我们的服务器被设置为监听我们的**中指定的端口。env** 文件。

# 设置 MongoDB

我假设 MongoDB 已经安装在您的本地机器上。使用以下命令检查 MongoDB 是否正在运行。

```
mongod
```

如果 MongoDB 正确安装在您的机器上，它将启动您的`mongod server`。

现在我们用 Mongoose 把 express app 和 MongoDB 连接起来。

1.  打开 **db.js** 并粘贴以下内容。我们将把我们的 MongoDB 连接设置到一个名为 **Url-Shortner 的本地数据库。**

```
const mongoose = require("mongoose");
mongoose.connect("mongodb://localhost/Url-Shortener", {
  useNewUrlParser: true,
  useUnifiedTopology: true
});
mongoose.set('useCreateIndex', true)
```

2.接下来，我们为我们的 URL 创建一个模型。打开 **url.js** ，粘贴以下内容。

```
const mongoose = require("mongoose");
const urlSchema = new mongoose.Schema({
    longURL: {
        type: String,
        required: true
    },
    shortURL: {
        type: String,
        required: true,
    },
    shortUrlId: {
        type: String,
        required: true,
        unique: true
    }
});module.exports = mongoose.model("URL", urlSchema);
```

1.  接下来，我们将在我们的 **dbUrl.js** 文件中添加保存和查找 URL 的逻辑。

```
const Url = require("../models/Url");const save = (longURL, shortURL, shortUrlId) => {
    Url.create({ longURL, shortURL, shortUrlId })
};const find = (shortUrlId) => Url.findOne({ shortUrlId: shortUrlId });module.exports = {
    save,
    find
};
```

# 创建端点

在这个阶段，我们将创建一个端点，该端点接受一个 URL，将它与缩短版本一起存储，并将缩短版本返回给用户。将以下内容添加到您的 **index.js.**

```
app.post("/url", async (req, res) => {
    try {
        if (!!services.validateUrl(req.body.url))
            return res.status(400).send({ msg: "Invalid URL." }); const urlKey = services.generateUrlKey();
        const shortUrl = `http://${host}:${port}/${urlKey}` await urlDb.save(req.body.url, shortUrl, urlKey)
        return res.status(200).send({ shortUrl }); } catch (error) {
        return res.status(500).send({ msg: "Error. Please try again." });
    }
    app.get("/:shortUrlId", async (req, res) => {
    try {
        const url = await urlDb.find(req.params.shortUrlId);
        return !url ? res.status(404).send("Not found") : res.redirect(301, url.longURL) } catch (error) {
        return res.status(500).send("Error. Please try again.")
    }
}); 
});
```

现在正在进行许多工作，下面是这些步骤的细目分类:

*   这里，我们接收一个 URL 作为请求体的一部分，然后使用 **service.js** 中的 **validateUrl()** 函数对其进行验证。
*   我们还使用 **generateUrlKey()** 函数为给定的 URL 生成一个 URL ID ( **shortUrlId** )。
*   然后，我们使用我们的服务器主机名和 **shortUrlId** 为 URL 创建一个短链接。
*   然后，我们将 URL、短链接和 **shortUrlId** 保存到数据库中。然后我们返回这个短链接。如果有错误，我们返回一个适当的错误消息。
*   接下来，我们创建一个端点，它接受一个 **shortUrlId** ，在我们的数据库中找到 **shortUrlId** ，并将浏览器重定向到与之相关联的长 URL。

# 路线

我们在 **validateUrl()** 和 **generateUrlKey()** 上面使用了两个还没有创建的函数。我们将在 **services.js.** 中创建这些函数，将以下内容复制并粘贴到 **services.js** 中。

```
const validate = require("validate.js");
const shortId = require("shortid");const validateUrl = (url = "") => {
    return validate({ website: url }, {
        website: {
            url: {
                allowLocal: true
            }
        }
    });
}const generateUrlKey = () => shortId.generate();module.exports = { validateUrl, generateUrlKey: generateUrlKey };
```

我们的服务器现在准备好了。我们可以通过在终端上运行`node app.js`来启动您的服务器来进行测试。可以按`Ctrl+C`停止服务器。或者，你可以使用[邮递员](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en)进行测试

# 开始前端

我们现在开始创建应用程序的前端/客户端。我假设 Vue.js 已经安装在您的本地机器上。如果没有，您可以通过在终端上运行以下命令来安装 Vue.js。

```
npm install -g @vue/cli
```

运行以下命令创建一个 vue 应用程序。

```
vue create client
```

如果您愿意，您可以选择**默认**预设或**手动**功能选择。接下来，在你最喜欢的代码编辑器中打开客户端文件夹。删除 **App 的内容。Vue** 并删除组件文件夹中的 **HelloWorld.vue** 文件。

我们将使用下面的文件夹结构。

1.  我们将使用 bootstrap 进行造型。在**index.html**文件的公共文件夹中，在开始和结束`<head>`标签之间添加以下链接。

```
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
```

1.  我们将添加一个简单的导航条，并导入到 **Home.vue** 组件中，并在我们的`container div.`中渲染它，复制并粘贴以下内容到 **App.vue.**

```
<template>
  <div>
    <nav class="navbar navbar-dark bg-dark">
      <a class="navbar-brand" href="#">Shortly></a>
    </nav>
    <div class="container">
     <home />
    </div>
  </div>
</template><script>
import Home from "./components/Home.vue";
export default {
  name: "App",
  components: {
    Home,
  },
};
</script>
```

在终端运行命令`npm run serve`来启动你的 vue 应用。Vue 使用热重新加载，所以你只需要运行这个命令一次，每次你做了更改并保存后，你的应用就会**更新**。您应该会看到与此类似的输出:

然后，访问**本地**链接查看您的应用。您应该会看到一个带有简单导航条的屏幕。

1.  组件将包含用户将与之交互的表单以及我们的应用程序逻辑。让我们给它一个简单的设计。

```
<template>
  <div>
    <div class="row">
     <div class="col col-12 offset-0 mt-2">
        <h1 class="jumbotron text-center text-white bg-primary">Create Click-Worthy Links</h1>
      </div>
    </div> <div class="col col-8 align-middle mt-5 offset-2">
      <div class="card">
        <div class="card-body">
          <form @submit.prevent="submit(url)">
            <div class="form-group">
              <label for="url">Enter Url</label>
              <textarea type="url" class="form-control" v-model="url" style="height:150px" />
            </div>
            <div class="for-group" v-show="shortUrl">
              <p>
                Short URL: :
                <a :href="shortUrl" class="text-primary">{{shortUrl}}</a>
              </p>
            </div>
            <div class="form-group">
              <button class="btn btn-primary" type="submit">Shorten URl</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
```

我们在这里创建了一个简单的设计。注意在我们的标签上使用 Vue 的 **v-model** 进行双向数据绑定。这将自动将用户输入存储在数据属性调用 **url** 中。

1.  现在，我们将添加逻辑，向我们的服务器提交一个 URL，并接收一个缩短的 URL，我们还将使用 axios 进行 API 调用。

```
<script>
import axios from "axios";
export default {
  data: () => {
    return {
      url: "",
      shortUrl: "",
    };
  },
  methods: {
    submit: async function (url) {
      try {
        const api = "http://localhost:5050/url";
        const response = await axios.post(api, {
          url,
        });
        this.shortUrl = response.data.shortUrl;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
```

*   当用户提交一个 URL 时，方法 **submit** 被调用。我们使用 Axios 向服务器发出请求。
*   我们用从服务器返回的 Url 更新 **shortUrl** 数据属性。对于错误，我们将它们记录到控制台。

客户端应用程序完成后，我们现在可以全面测试我们的 URL shortener 应用程序了。我们需要我们的服务器和客户端应用程序都在运行。如果您的服务器不再运行，为您的服务器目录打开另一个终端并运行`node app.js`。
您的浏览器中会有以下内容。

选择一个长 URL，通过表单提交，然后单击返回给您的 URL。您应该被重定向到原始站点。

冤家路窄！您已经使用 javascript 构建了自己的简单 URL 缩短器。

*如果你喜欢读这篇文章，别忘了分享。👏*