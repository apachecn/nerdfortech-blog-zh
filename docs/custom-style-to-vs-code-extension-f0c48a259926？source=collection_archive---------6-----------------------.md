# VS 代码扩展的自定义样式

> 原文：<https://medium.com/nerd-for-tech/custom-style-to-vs-code-extension-f0c48a259926?source=collection_archive---------6----------------------->

## 如何给 VS 代码 API 扩展添加自定义样式`Webview?`

假设您想注册一个命令，用您自己的 CSS 设计在编辑器中打开一个基本的 Webview 面板。Visual Studio 代码有自己的风格，但您也可以使用自己的风格。面板使用 HTML 语法来显示内容。我们需要记住，Webview 是沙箱化的，所以所有的通信都是通过消息传递来处理的。在我们的例子中，我们不需要与外部 JavaScript 函数进行任何消息通信，因为我们所做的只是将一个. css 资源链接到我们的 Webview。

注册了执行面板的命令后，我们需要编写 HTML 代码，并将其传递给**webview.html、**，它只接受一个普通字符串作为输入**。我建议使用一个函数，在这个函数中，我们构造面板，向所用的元素添加样式和逻辑，然后将构造的字符串赋给 webview.html。**

这不是必需的，但它为设计和构建面板提供了更多的灵活性。

下一步是在您的目录中创建一个. css 文件，我将在 media 本地目录文件夹中创建我的文件。

通过使用 **asWebviewUri** 我们可以转换我们的。css 文件转换为可以在 Webview 中使用的文件，从而为我们的元素提供自定义样式。

```
const myStyle = webview.asWebviewUri(
    vscode.Uri.joinPath(
        context.extensionUri, 'media', 'my-custom-style.css'
    )
);
```

Uri 现在可以链接到我们的 HTML 文件，这样，我们就成功地将 CSS 内容导入到 Webview 面板中。

```
<link href="${myStyle}" rel="stylesheet" />
```

有关更多信息，请考虑阅读 VS code API 的官方文档:

[](https://code.visualstudio.com/api/extension-guides/webview) [## Webview API

### webview API 允许扩展在 Visual Studio 代码中创建完全可自定义的视图。例如……

code.visualstudio.com](https://code.visualstudio.com/api/extension-guides/webview) 

开发 VS 代码扩展并不难，但是有一些困难需要克服。我希望这个简短的教程是有用的。

祝您愉快！