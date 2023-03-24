# 在 Node.js 中上传图像，不使用表单

> 原文：<https://medium.com/nerd-for-tech/upload-images-in-node-js-without-a-form-be15b2ab0745?source=collection_archive---------3----------------------->

![](img/a4f137f440c762e13845388c8e76b67b.png)

曾经想上传文件，但没有一个形式，但发现它太复杂？我抓住你了。
这篇文章给出了一个简单的例子，告诉你如何在没有表格的情况下上传文件。
为了简单起见，在这个例子中，您只能上传`jpg`和`png`文件。

> 本教程需要关于 Express.js 和基本 JavaScript 的基本知识

顺便说一下，这是应用程序目录

```
index.js
  public
     index.html
     script.js
```

因此，首先让我们建立一个快速服务器来服务于`public`文件夹。

我们将使用 [Multer](https://github.com/expressjs/multer) 上传文件，Multer 是用于 [Express.js](https://expressjs.com/) 的中间件，用于轻松上传文件。所以现在我们添加了一些配置的中间件。对于文件名，我使用的是上传时间，你可以很容易地把它改成你想要的任何东西。**只要确保文件名是唯一的**(例如可以使用 [uuid](https://www.npmjs.com/package/uuid) )。

> 我们在公共文件夹中指定了文件的目标位置，以便可以轻松访问这些文件。

现在我们必须输入一个文件来发送文件，当然还有一些 JavaScript。通常我喜欢为一个应用程序设计 UI，但我会让这个例子尽可能简单，所以我没有添加 CSS，也没有使用任何 UI 框架。

主 html 页面

这是我们应用程序的主页，请注意，我使用文件输入标签的 [accept 属性](https://www.w3schools.com/tags/att_input_accept.asp)指定了文件类型。

最后但同样重要的是，代码！

文件上传脚本

基本上，当你选择一个文件时，上传按钮被激活(通过`enableUpload()`，当点击它时，一个包含文件的 POST 请求被发送到服务器(`uploadImage()`)。文件保存到服务器后，它返回文件数据作为响应。然后，通过将基本 URL 与文件名相加来获取图像的 URL，并将其显示为图像。同样**文件变量可以是任何** [**斑点**](https://javascript.info/blob) **文件**，例如音频斑点。这里，文件变量包含文件输入中的第一个文件。

瞧啊。你有一个可以保存图片的服务器，有点像 Google Drive😅。

这是一个工作示例

[](https://replit.com/@AbaanShanid/FileUploadExample) [## 文件上传示例

### AbaanShanid 的 Node.js repl

replit.com](https://replit.com/@AbaanShanid/FileUploadExample) 

如果你想叉这个，还有一个 GitHub 回购

[](https://github.com/tomatopickle/File-Upload-Example) [## tomatopickle/文件-上传-示例

### 向 Nodejs 上传文件的简单方法。为 tomatopickle/File-Upload-Example 开发做贡献，创建一个…

github.com](https://github.com/tomatopickle/File-Upload-Example) 

另外，我对文章写作很陌生，如果你发现这篇文章有什么问题，请告诉我！