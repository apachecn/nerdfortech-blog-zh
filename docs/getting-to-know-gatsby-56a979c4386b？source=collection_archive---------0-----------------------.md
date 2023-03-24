# 了解盖茨比

> 原文：<https://medium.com/nerd-for-tech/getting-to-know-gatsby-56a979c4386b?source=collection_archive---------0----------------------->

## 这并非没有障碍

所以，我搬进了纽约一栋神秘豪宅旁边的小屋。我以一个好价格得到它，但是我感觉有人正在从大厦监视我。我相信是盖茨比先生。呃哼..

抱歉，那是不同的故事。让我们重新开始。

本周，我开始了我的第一个盖茨比项目。Gatsby 是一个基于 react 的框架，用于创建静态生成的网站和应用。它的生态系统包含插件、主题和所谓的配方，可以自动完成某些任务。官网上说:

> *在通常制作一个原型所需的时间内制作一个完整的网站*

我很兴奋的试了一下，看看上面说的是不是真的。我有一个很好的第一印象，但遇到了一些障碍。然而，并不是所有的人都和盖茨比有关系。让我们看看进展如何。

![](img/07f1393056f2ffa145688530ec372335.png)

照片由 [CHUTTERSNAP](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

初始设置非常简单。您可以运行以下命令来运行项目:

```
npm install -g gatsby-cli
gatsby new projectname
cd projectname
gatsby develop
```

下一步是什么？我们可以创建一些 React 组件并显示标题。但是有什么我应该遵循的结构吗？让我们问问 Gatsby.js 先生，原来我们在/ *src* 里面有/ *page* s 和/ *templates* 文件夹。还有很多配置文件！

```
|-- /.cache
|-- /plugins
|-- /public
|-- /src
    |-- /pages
    |-- /templates
    |-- html.js
|-- /static
|-- gatsby-config.js
|-- gatsby-node.js
|-- gatsby-ssr.js
|-- gatsby-browser.js
```

我不需要创建很多页面，因为我的项目是一个单页页面:-)，所以我将只在那里保存 *index.js* 文件。其他文件夹呢？

*   */plugins* 文件夹存放任何特定于项目的(“本地”)插件**，这些插件没有作为** `**npm**` **包**发布。
*   */public* 文件夹是我们运行 *gatsby build* 时自动生成的
*   */static* —您可以在这里放置任何您不想被 webpack 处理的文件。它们将原封不动地复制到公共文件夹中。

静态文件夹很有意思，因为我假设我可以把图像之类的资产放在那里。你可能想缩小你的图像，所以静态文件夹只对一些不太常见的场景有用。

好的，那么我把资产放在哪里呢？我在 */src 里面创建了 */images* 文件夹。*我对**组件**做了同样的操作。现在让我们来看看插件。

# 插件和样式

插件是 npm 包，你可以使用命令行安装，因为你已经习惯了，如果你从事任何前端项目。你可以找到一个[响应图像](https://www.gatsbyjs.com/plugins/gatsby-image/)，搜索引擎优化，[打字稿支持](https://www.gatsbyjs.com/plugins/gatsby-plugin-typescript/)和数以千计的插件。

我在想如何设计发型。还有许多选项可供选择:

*   使用全局 CSS 文件[带](https://www.gatsbyjs.com/docs/recipes/styling-css/#using-global-styles-in-a-layout-component)或[不带](https://www.gatsbyjs.com/docs/recipes/styling-css/#using-global-css-files-without-a-layout-component) youu..我指的是布局组件
*   使用样式组件
*   使用 CSS 模块

我还不太熟悉 JS 方法中的 CSS，但是我知道我想使用 Sass。反正我只需要设计一页，所以我不需要任何花哨的东西。原来盖茨比里面还有一个使用 Sass 的插件叫做 [gatsby-plugin-sass](https://www.gatsbyjs.com/plugins/gatsby-plugin-sass) 。

我用下面的命令安装了插件(还要注意 node-sass)

```
npm install node-sass gatsby-plugin-sass
```

然后我们需要编辑 *gatsby-config.js* 文件并添加:

```
plugins: [`gatsby-plugin-sass`]
```

这里是第一个[问题](https://github.com/gatsbyjs/gatsby/issues/27754)出现的地方。尝试运行我的项目时，我在控制台上看到:

*节点 Sass 版本 5.0.0 与^4.0.0.不兼容*

经过一些其他的建议，我发现 [LibSass 和 Node Sass 已经被弃用](https://sass-lang.com/blog/libsass-is-deprecated)。什么？谁还记得 Ruby 中的实现？麦当娜的歌曲《挂断》中的歌词“时间过得如此之慢”在网络开发中显然并不适用。然而，有一个新的 [Dart 实现](https://sass-lang.com/dart-sass)所以我[尝试了那个](https://www.gatsbyjs.com/plugins/gatsby-plugin-sass#alternative-sass-implementations)。

```
npm install --save-dev sass
```

而在 *gatsby-config.js*

```
plugins: [
  {
    resolve: `gatsby-plugin-sass`,
    options: {
      implementation: require("sass"),
    },
  },
]
```

很好，这很好，我可以简单地通过将它导入 index.js 来设计组件的样式:

```
*import* '../styles/styles.scss'
```

他们可以更新插件并将 Dart 实现设置为默认值。

# 排印

任何东西都必须有一个插件，所以我在 Gatsby 知识库中搜索了“字体”。第一个结果是 **gatsby-plugin-webfonts** ，所以我安装了它。

```
// with npm
npm install gatsby-plugin-webfonts// with yarn
yarn add gatsby-plugin-webfonts
```

哦，又一个错误:

*错误“gatsby-plugin-webfonts”在运行 onPreBootstrap 生命周期时抛出错误:*

我不确定出了什么问题。这个包下载了 34.8k 次。

> *提示:坚持 1 包管理制度。使用 npm 或纱线*

我做了另一个搜索，这次选择了[**Gatsby-plugin-we B- font-loader**](https://www.gatsbyjs.com/plugins/gatsby-plugin-web-font-loader/?=fonts#gatsby-plugin-web-font-loader)**(106.1k)，运行得很好。**

# **如何评价 JSX**

**最后一件让我有点难以接受的事是 JSX。我像往常一样用快捷方式注释掉了代码，但它看起来像这样:**

```
// <div>
//     <span>some text</span>
// </div>
```

**这不是一个正确的语法，所以我不得不再次调查。我检查了我的扩展，发现了问题所在。我必须做以下事情:**

**由 dzannotti 卸载 [**Babel ES6/ES7 插件**](https://marketplace.visualstudio.com/items?itemName=dzannotti.vscode-babel-coloring)**

**安装迈克尔·麦克德莫特的巴贝尔 JavaScript**

**正确的语法如下所示:**

```
{/*
   <div> 
     <span>some text</span> 
   </div>
*/}
```

# **结论**

**有很多东西我没有涉及到，要学习，但是到目前为止我喜欢盖茨比。它发展得非常快，我也对食谱(新功能)感到兴奋。正如他们在最近的新闻发布会上所说**

> ***这是一个挑战当用户进入盖茨比生态系统时，他们必须将“我想做 x”翻译成“x”在盖茨比中是如何完成的***

**没错，我发现自己也在这么做。菜谱从 CLI 运行，并自动执行常见任务，如创建页面和布局、安装插件等。我是带着 Angular(使用 Angular CLI)的一些经验来到 React world 的，所以我只能欣赏这一步走向更好的工具，并给开发人员更多自以为是的方法来构建极快的 web 应用程序。我还发现 IntelliSense(代码完成)落后于 Angular，但这可能是由于设计或我缺乏知识/扩展设置，所以我将感谢您对此的想法。**

**你试过《盖茨比》吗？你最喜欢的插件是什么？**

## **附加功能— VS 代码快捷键:**

**文本转换为小写=> CTRL + K & CTRL + L
文本转换为大写= > CTRL + K & CTRL + U**

**[](https://miro.substack.com/) [## 是米罗的错！

### 关于 web 前端技术的时事通讯

miro.substack.com](https://miro.substack.com/)**