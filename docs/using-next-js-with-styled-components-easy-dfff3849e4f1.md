# 将 Next.js ▲用于样式化组件💅容易的

> 原文：<https://medium.com/nerd-for-tech/using-next-js-with-styled-components-easy-dfff3849e4f1?source=collection_archive---------0----------------------->

需要服务器端渲染的时候 React app。 [Next.js](https://nextjs.org/) 是最好的选择。当谈到样式 react 组件时，我首先想到的是[样式组件](https://www.styled-components.com/)。虽然这取决于你的喜好。

假设您想要将 [Next.js](https://nextjs.org/) 与[样式化组件](https://www.styled-components.com/)配对。但是在 Next.js 中使用样式化组件并不简单。这有点棘手。让我们看看如何实现这一点。

1.  [Setup Next.js](https://app.contentful.com/spaces/x2hu75nfw600/entries/2VoqEfclBehJH5eZK6OrnW#1-setup-nextjs-project)
2.  [创建我们的第一个 Next.js 页面](https://app.contentful.com/spaces/x2hu75nfw600/entries/2VoqEfclBehJH5eZK6OrnW#2-create-our-first-nextjs-page)
3.  [安装样式组件](https://app.contentful.com/spaces/x2hu75nfw600/entries/2VoqEfclBehJH5eZK6OrnW#3-install-styled-components)
4.  [安装并设置样式组件巴别插件](https://app.contentful.com/spaces/x2hu75nfw600/entries/2VoqEfclBehJH5eZK6OrnW#4-install-and-setup-styled-components-babel-plugin)
5.  [Create _document.js 文件](https://app.contentful.com/spaces/x2hu75nfw600/entries/2VoqEfclBehJH5eZK6OrnW#5-create-custom-_documentjs-file)
6.  [结果](https://app.contentful.com/spaces/x2hu75nfw600/entries/2VoqEfclBehJH5eZK6OrnW#6-result)

# 1.设置 Next.js 项目

我们首先创建一个新目录。和 cd 放入该目录。

```
mkdir next-with-styled-component &amp;&amp; cd next-with-styled-component
```

现在我们需要创建我们的`package.json`文件。然后在您的终端中运行以下命令。

```
npm init
```

它将创建一个`package.json`文件。现在我们需要为我们的项目安装所有与 Next.js 相关的依赖项。在 cmd/终端中运行以下命令。

```
npm install --save next react react-dom
```

它将创建一个`node_module`文件夹，并在其中安装所有 Next.js 相关的依赖项。

现在，在您喜欢的编辑器中打开 package.json 文件。添加以下内容。

```
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```

# 2.创建我们的第一个 Next.js 页面

创建 pages 目录，并在 pages 目录下创建文件名`index.js`。现在打开`index.js`文件，将以下内容粘贴进去。

```
function Home() {
  return <div>Hello world!</div>;
}export default Home;
```

现在使用下面的命令在浏览器中运行我们的第一个页面。

```
npm run dev
```

它在 [http://localhost:3000](http://localhost:3000/) 启动开发服务器。

# 3.安装样式组件

现在是时候将样式组件添加到我们的项目中了。您可以使用以下命令添加样式化的组件。

```
npm install --save styled-components
```

# 4.安装和设置样式组件巴别塔插件

我们还需要`babel-plugin-styled-components`。它有助于在不同的环境之间保持一致的组件类名散列(这是服务器端呈现所必须的)。

您可以使用下面的命令将`babel-plugin-styled-components`安装为一个开发依赖项。

```
npm install --save-dev babel-plugin-styled-components
```

然后在项目的根目录下创建一个`.babelrc`文件。

打开`.babelrc`文件添加以下内容。

1.  添加“下一个/巴别塔”作为预设
2.  添加一个样式化组件插件，并将 SSR 选项设置为 true

现在你的`.babelrc`文件看起来是这样的。

```
{ 
  "presets": ["next/babel"], 
  "plugins": [
      ["styled-components", { "ssr": true }]
   ]
}
```

# 5.创建 custom _document.js 文件

为了在服务器端呈现我们的样式化组件，我们需要覆盖`_document.js`。为此，在 pages 文件夹下创建一个`_document.js`文件。并在其中添加以下内容。

```
import Document from "next/document";
//highlight-next-line
import { ServerStyleSheet } from "styled-components";
export default class MyDocument extends Document {
  static async getInitialProps(ctx) {
    //highlight-next-line
    const sheet = new ServerStyleSheet();
    const originalRenderPage = ctx.renderPage;
    try {
      ctx.renderPage = () =&gt;
        originalRenderPage({
          //highlight-next-line
          enhanceApp: App =&gt; props =&gt; sheet.collectStyles()
        });
      const initialProps = await Document.getInitialProps(ctx); return {
        ...initialProps,
        styles: (
          &lt;&gt;
            {initialProps.styles}
            //highlight-next-line               
            {sheet.getStyleElement()}

        )
      };
    } finally {
      sheet.seal();
    }
  }
}
```

这里的基本思想是，每次在服务器端加载页面时，我们都需要将样式表插入到文档中。

# 6.结果