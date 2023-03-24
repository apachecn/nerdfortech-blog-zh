# 通过一个使用 React、Webpack 5 和模块联合的实际例子来理解微前端

> 原文：<https://medium.com/nerd-for-tech/micro-front-ends-hands-on-project-63bd3327e162?source=collection_archive---------0----------------------->

![](img/92fdb3047182f0e25765d944667be956.png)

最近，微前端一直在增长，因为需要打破前端整体结构，并且在微服务架构中取得了成功。然而，很少有人理解这个概念或知道它的存在。阅读理论概念使事情变得比实际困难。因此，我决定用一个动手项目来解释这些概念。我喜欢看到事物工作，以理解挑战和事物如何工作。我将使用 ***Webpack 5*** 和 ***模块联盟插件*** 。希望这对你有用。像往常一样，请将您的反馈发送给我。

”*好的前端开发很难。扩展前端开发，让许多团队可以同时开发一个大型复杂的产品就更难了。”马丁·福勒*

# 设置开发环境

准备好您的机器并安装 nodejs，如果需要，请按照下面的文章/视频操作:

[](https://ranyel.medium.com/preparing-your-mac-for-react-and-node-development-install-nodejs-npm-vs-code-and-configure-vs-e4f0cf1e99a4) [## 为 React 和节点开发准备您的 Mac，安装 NodeJS、npm、VS 代码，并配置 VS…

### 按照以下步骤/视频安装 Nodejs

ranyel.medium.com](https://ranyel.medium.com/preparing-your-mac-for-react-and-node-development-install-nodejs-npm-vs-code-and-configure-vs-e4f0cf1e99a4) 

[可选]你可以创建一个 Github 项目并克隆它(我使用的是 https://github.com/ranyelhousieny/micro-front-ends 的

git 克隆[https://github.com/ranyelhousieny/micro-front-ends.git](https://github.com/ranyelhousieny/micro-front-ends.git)

![](img/07cf4f38cec35351c56c7c9aec5d4415.png)

我们将创建以下 3 个前端容器，微前端 1 和微前端 2

我们将使用 create-react-app 创建 3 个基础项目，如下面的视频中所解释的，但我们将把它们称为容器、微前端 1 和微前端 2。

容器将在多个微前端之间协调

![](img/92fdb3047182f0e25765d944667be956.png)

npx 创建-反应-应用容器

![](img/588215c8b823e5bf4d285dea06c1efb5.png)

npx 创建-反应-应用微前端-1

npx 创建-反应-应用微前端-2

# 安装微前端 1 的依赖项

cd 微前端-1

npm 安装 web pack web pack-CLI web pack-服务器 html-web pack-插件

![](img/a1a987372b556aef7b5d455caaf54c6c.png)

npm 安装 web pack web pack-CLI web pack-server html-web pack-plugin web pack-dev-server

# 实施微前端-1

进入 micro-front-end-1 目录，使用(代码。)

在 src/index.js 中导航

删除 index.js 中的所有代码，替换为

console.log("微前端-1 ")；

![](img/56acc41ab065ec0943faba97739b69ec.png)![](img/13cd1c75e71b6a2c979d011b1d0bdc63.png)

# 使用 Webpack 捆绑代码

Webpack 配置的详细解释可以在下面的文章中找到

[](https://www.linkedin.com/pulse/understanding-micro-frontends-webpack5-configurations-rany/) [## 逐步了解微前端 Webpack5 配置

### 在前两篇文章中，我演示了如何构建微前端并将它们部署到 AWS。在这个过程中，我…

www.linkedin.com](https://www.linkedin.com/pulse/understanding-micro-frontends-webpack5-configurations-rany/) 

让我们使用 Webpack 创建一个包

![](img/9d69ce62e7e8bf96035dae15543ae4b5.png)

在微前端-1 的根目录下创建 webpack.config.js

触摸 webpack.config.js

![](img/484c4b0a93c485bfa0366c1d328af2d7.png)

现在，让我们试着构建一下，看看会发生什么。运行以下命令:

```
npm webpack
```

你会得到很少的错误。我们一个一个来。

![](img/1b01eb2b0e80ea5f059031c507e39909.png)

第一个错误是询问模式。Webpack 需要知道使用哪种模式来运行，以便能够相应地捆绑依赖项。让我们使用开发模式。在 webpack.config.js 中添加以下内容

```
module.exports = {
  mode: 'development',
};
```

![](img/246eaa72664ac3b8ad603b6d065a1501.png)![](img/2a84fbaa5ef8a16fa21dc49ed4672c6e.png)![](img/790d014b725f35f0cec636f697f72549.png)![](img/a2476fc90b200c44c32e6c2c39e90393.png)

这将处理第一个错误，并告诉 Weboack 在开发模式下构建。现在，再次构建并观察左侧的文件夹。您会注意到一个名为“dist”的目标文件夹将被创建。

![](img/549d763d01b373c78a6d1cf86f75bfcc.png)![](img/8b35db63aa565fdcef7e8b72d5bf1a50.png)

这是将由 Webpack 的构建和捆绑流程生成的文件夹，也是我们将部署到 S3 存储区的文件夹。这里发生的事情是，Webpack 将我们拥有的所有代码 index.js、bootstrap.js、App.js …以及它们的依赖项打包在 main.js 中，如您所见。

现在我们看不到模式错误，但是我们仍然对加载器错误做出反应。这些错误是因为 Webpack 需要加载器来理解 React。如你所知，浏览器只理解 CSS、HTML、ES5 和 JavaScript(目前)。JSX 和 ES6+需要一个编译器。我们用巴别塔来做这个。我将向您展示如何将这些加载器添加到配置中，但首先让我们创建一个本地服务器来查看我们的演示网站(这将是 S3 在最后配置为网站)

向 package.json 添加脚本

![](img/e504063528ccf6c8e165ec95d7dba870.png)![](img/4eba4f52e7d139bbd62e40cfa91f9c90.png)

将创建一个新文件 dist/main.js

![](img/25a47313696f8f26283824d0c1205df5.png)

# 添加 Webpack 服务器

要在某个端口(这里，我使用 8001)上添加本地服务器，请在 webpack.config.js 中添加以下几行

```
devServer: {
    port: 8081,
    },
```

![](img/99ef965e21052f7919a7ad8261772f87.png)

现在，运行以下命令

```
yarn webpack serve
```

这将在端口 8081 上启动一个本地 web 服务器。但是，如果您导航到 [https://localhost:8081，](https://localhost:8001,)您将只能找到文件列表。这是因为我们还没有配置 index.html。我们将在下一步中这样做。但是，尝试浏览 main.js，您将能够看到其内容如下:

![](img/b4ed005d67c0fd3e140ba1a40b4d3bc4.png)

到目前为止，Webpack 按照我们在 Webpack.config.js 中的配置，在端口 8081 上创建了一个本地 web 服务器。但是，它仍然缺少浏览器的一个最小组件:index.html

# HTML Webpack 插件

我们需要为 webpack 导入 html-webpack-plugin 来生成 index.html，并将捆绑的 js 文件添加到其中。

```
const HtmlWebpackPlugin = require('html-webpack-plugin');
```

另外，添加 html-webpack-plugin 来填充 index.html，如下所示

![](img/d7d7e0e626305eca87db041a17dbd490.png)

webpack.config.js 到这一步可以在[这里找到](https://github.com/ranyelhousieny/micro-front-ends/blob/503e162181fb66882c6dbe5bb64480a3e84f0753/micro-front-end-1/webpack.config.js)

将 serve 添加到 package.json 中的脚本

![](img/a1554b07f6c6e6fb5e4cb01f6465ccfa.png)

删除 public/index.html 中的所有内容，并编写以下代码

![](img/799feb08df3c2aa72a8650c062e578c0.png)

从终端的微前端-1 目录下，启动脚本

npm 运行 webpack

![](img/526f466fca8fa8f825f7dc8d1cf9446a.png)

进入浏览器，浏览 [HTTP://localhost:8081](http://HTTP://localhost:8081)

右键单击并检查以查看 console.log 的输出

![](img/617be424694a2e38c756ed37f052f792.png)![](img/1985d668bbf65d59b7c70078fa7eac29.png)

这是主要概念。

现在，让我们显示在页面上，而不是控制台上

将以下内容添加到 index.html

![](img/17c21dd61746d904a58138296fd46941.png)

添加以下 querySelector

document.querySelector('#root ')。innerHTML = `

# 微前端-1

![](img/cfed23a3ab97738029836e906dc5a232.png)

刷新浏览器

![](img/7f9a0e29a2a956c6b5a43f9c21cb25a1.png)

到目前为止，我们有一个组件，它没有连接到任何东西

![](img/10fc57f49c8714e50cf5bd0c8466f28f.png)![](img/c53aaccddc60280cefe5e1b6f104c780.png)

=====================================

# 现在，让我们构建容器

![](img/3a402a636e99a8f646ca226992067761.png)

打开另一个终端，导航到我们有三个应用程序的根目录

![](img/f4bab819d275e049cc7ef77ec73972bb.png)

现在，在容器目录中导航

![](img/fb1f46276b4aab90eb0827d58c8e8c46.png)

像以前一样安装依赖项。

安装 web pack web pack-CLI web pack-server html-web pack-插件

![](img/08a1357a0f0c0dbbff7824b77ef36631.png)

现在，让我们回到这三个服务的根，打开带有这三个组件的 VS 代码，同时对它们进行操作

![](img/9cdc028e0d4ce2e7ca4d79ef52b33b02.png)![](img/e9cf2b3a758ddc162115376aa4a56fae.png)

将 webpack.config.js 从 micro-front-end-1 复制到容器，并将端口更改为 8080，如下所示

![](img/a7caa65f4e037ef2ffc1b7d3478ebe43.png)

按照我们之前的方式修改 package.json 脚本(您可以从 micro-front-end-1 复制它

“网络包”:“网络包服务”，

![](img/32a9f65dda8192a61aef8c630c0ac8ab.png)

像以前一样更改 index.js 和 index.html，但是写入控制台“容器”

![](img/1a01b9c5a6dd02fc4d1ae4d4431e77e0.png)![](img/e6db6ea15df7879dd52f5a7ab59d241a.png)

运行脚本(npm run webpack)并检查浏览器 [http://localhost:8080/](http://localhost:8080/)

![](img/8515173d78b2ef7bc64c590c1d8b11df.png)

========================

# 将两者连接在一起

![](img/9c0bcde922ba51fc0d5ea74b9aea75c7.png)

# 1-添加模块联合插件

在 micro-front-end-1/web pack . config . js 中添加以下内容

![](img/001101752c79dcbdeb24f64426b999fb.png)

这样会从微前端-1 暴露 index.js。(注意，我们使用了 camelCase)

## 现在，让我们把它从容器中取出来

转到[container/web pack . config . js](https://github.com/ranyelhousieny/micro-front-ends/blob/ad48c75fd13b04a43614d8bb650d0978b801bccc/container/webpack.config.js)并添加以下内容

![](img/3a09d621b833fecdaa39aba3c1655cfa.png)

[container/webpck . config . js](https://github.com/ranyelhousieny/micro-front-ends/blob/ad48c75fd13b04a43614d8bb650d0978b801bccc/container/webpack.config.js)至此可以在[这里找到](https://github.com/ranyelhousieny/micro-front-ends/blob/ad48c75fd13b04a43614d8bb650d0978b801bccc/container/webpack.config.js)

现在，将***container/src/index . js***重命名为***container/src/bootstrap . js***并导入 micro frontend 1/micro frontend 1 index(这是为了异步运行文件，直到它从 micro-front-end-1 获取数据到容器)

![](img/22906bde17c125a8fa32c8fd3c37cd73.png)

新建一个 ***容器/src/index.js 和*** 导入('。/bootstrap’)；

![](img/658259117f73c1af56fa5201403c3841.png)

现在重新启动 npm，为两个服务运行 webpack

## 首先，运行微前端-1

ctrl + c

npm 运行 webpack

![](img/99e3a85138f947f822b48870fcaefe22.png)

## 其次，运行容器

ctrl + c

npm 运行 webpack

![](img/2764f9ad2b28eac07e6a27177fca6be7.png)

## 将 id 添加到 container/public/index.html

![](img/8017d35b9fc2cea03120dce05e76364a.png)

浏览容器端口 8080[http://localhost:8080/](http://localhost:8080/)，您会发现来自 micro-front-end-1 的文本出现在那里

![](img/c4aa5b4036128a72bf47e511359aecda.png)

现在，让我们明白我们做了什么。

在页面上，选择网络、禁用缓存和 Java，如下所示:

![](img/80745ad08343e8f1772d147c4bc5f3a7.png)

刷新页面以向服务器发送呼叫，并注意输出

![](img/a041c9a2731b814c38236b533aa0745f.png)

右键单击名称旁边的标题，并从菜单中选择 URL 以显示已调用的 Url。

![](img/0c6a4cd6185491fc0413bd26c88640a2.png)

你会看到以下网址被调用

![](img/7df341e02d7f61f07099d8ab89dc3063.png)

1.  它首先在[http://localhost:8080/](http://localhost:8080/)main.js 上调用 main . js。这是集装箱
2.  调用了[http://localhost:808](http://localhost:8080/)1/remoteEntry.js 上的 remote entry . js。这是微前端-1
3.  再次返回到调用引导的容器
4.  最后从 micro-front-end-1 (index.js)调用 src_index_js.js 将其输出呈现在屏幕上

所有这些都来自 ModuleFederationPlugin

![](img/133f2f1fea039e6c69b841dbf4608408.png)

webpack 使用了来自 micro-front-end-1/web pack . config . js 的这个配置创建了[***http://localhost:808***](http://localhost:8080/)***1/remote entry . js***和[***http://localhost:808******1/src _ index _ js . js***](http://localhost:8080/)

然后 container/webpack.config.js 上的配置告诉服务器如何获取[***http://localhost:808***](http://localhost:8080/)***1/remote entry . js***

![](img/f8b61ec01b1b505e2b50593f10ff7375.png)

在[***http://localhost:808***](http://localhost:8080/)***1/remote entry . js***获取 src_index_js.js 所需的信息

![](img/edd890ff2b7545891b58520ce790f2b2.png)

模块联合插件允许 JavaScript 在运行时将代码从 micro-front-end-1 动态导入容器。

==============

# 添加第二个微前端

在添加第二个微前端之前，我们先给第一个添加更多的段落。我喜欢使用一个叫做 Lorem ipsum 的工具

[](https://marketplace.visualstudio.com/items?itemName=Tyriar.lorem-ipsum) [## Lorem ipsum - Visual Studio 市场

### 生成 lorem ipsum 文本并将其插入 Visual Studio 代码中。打开 VS 代码按 F1 键输入“安装”选择…

marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=Tyriar.lorem-ipsum) ![](img/8868e6d9a09f069601405e6fdaf17485.png)

你只需按 F1，写出 Lorem ipsum，然后选择有多少段落

![](img/1c55267368949b7c7c2d15c88e83acd2.png)

这一步的新 index.js 可以在[这里](https://github.com/ranyelhousieny/micro-front-ends/blob/bda1a820fb54d230e3fc1244c44dae8acddf6601/micro-front-end-1/src/index.js)找到

现在，在微前端-2 中重复相同的过程

1.  将 webpack.config.js 从微前端-1 复制到微前端-2
2.  更新其中的名称，如下所示:

![](img/5d761636100f56db43c02d81ab0027e7.png)

## 安装微前端 2 的依赖项

cd 微前端-2

npm 安装 web pack web pack-CLI web pack-服务器 html-web pack-插件

![](img/2faed20cbd9949a4d88c3e06ffbb239e.png)

将 webpack 脚本添加到 package.json

![](img/a34ec5ee6edc84b2272d1044fdf7f21f.png)

从 micro-front-end-2 文件夹中运行 npm 安装

![](img/daaaf0127826555818eb56070f5a1bc0.png)

擦除微前端- **2** /public/index.html 的内容，复制到其中微前端- **1** /public/index.html 内容。用新 id 替换该 id，如下所示

![](img/4de7f7925b31b70adb80b3bc73313ee5.png)

擦除微前端- **2** /src/index.js 的内容，复制到其中微前端- **1** /public/index.html 内容。用上面的 id 替换 id，并在< h1 >标签中替换服务名，如下所示

![](img/0d2ea23f5f9b7dc4f4eec9ef267e17ac.png)

现在，运行 npm 运行 webpack

![](img/76958a4821f1d96685a898b9848c3a0f.png)

现在导航到 [http://localhost:8082/](http://localhost:8082/)

![](img/192f7c2db24c82b75972985cf49bfe9c.png)

## 现在，把它连接到容器上

在 container/webpack.config.js 中添加另一个遥控器，如下所示

微前端 2:'微前端 2 @ http://localhost:8081/remote entry . js '，

![](img/0199364c95a46b7d83465285e57feed3.png)

在 container/src/bootstrab.js 中添加以下导入

导入“micro frontend 2/micro frontend 2 index”；

![](img/e766901e8d2b46bb3387b79b0b61545d.png)

在 container/public/index.html 中，在您想要的空间中添加微前端-2 的 id。我补充如下

![](img/6ab8931dfd0d5548d7c018755ad8105a.png)

重启容器的 webpack 并导航到 [http://localhost:8080/](http://localhost:8080/)

![](img/d34ee4159b6da592a863eaf5c6878308.png)

不，如果您检查网络流量，您会发现对两台服务器的呼叫

![](img/a5d5b6a7c7d8107351f502d343c0ab37.png)

=================================

到目前为止，我们还没有使用 React。在下面的文章中，我们将添加第三个微前端 react-microfrontend-3

[](https://ranyel.medium.com/micro-frontends-hands-on-example-using-react-webpack-5-and-module-federation-adding-a-third-2fe8c61a73f) [## 使用 React、Webpack 5 和模块联合的微前端实践示例:添加第三个…

### 这篇文章是上一篇文章的延续

ranyel.medium.com](https://ranyel.medium.com/micro-frontends-hands-on-example-using-react-webpack-5-and-module-federation-adding-a-third-2fe8c61a73f) ![](img/a441dcdf8a4259b0e5e16e10aa44ba1d.png)

# 了解更多信息

[](https://www.linkedin.com/pulse/micro-frontends-what-why-how-rany-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0) [## 微前端:什么、为什么和如何

### 在我以前的文章(本文末尾和这里的链接)中，我亲自展示了什么是微前端以及如何…

www.linkedin.com](https://www.linkedin.com/pulse/micro-frontends-what-why-how-rany-elhousieny-phd%E1%B4%AC%E1%B4%AE%E1%B4%B0)