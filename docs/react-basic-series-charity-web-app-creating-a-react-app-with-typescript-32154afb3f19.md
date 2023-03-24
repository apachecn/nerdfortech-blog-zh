# React with Typescript 系列(慈善网站应用程序)-使用 Typescript 创建 React 应用程序

> 原文：<https://medium.com/nerd-for-tech/react-basic-series-charity-web-app-creating-a-react-app-with-typescript-32154afb3f19?source=collection_archive---------0----------------------->

大家好，我最近开始了一个简单的项目，为一个慈善组织创建一个 web 应用程序。他们所做的是，接受需要经济支持的大学生的申请，然后选择学生提供奖学金。作为这个应用程序的第一阶段，我将为这个慈善组织的管理员和赞助商创建一个仪表板类的应用程序，以查看有关学生的更新和详细信息。为此，我将为 UI 创建一个 React 应用程序，并作为后端创建一个 Node JS 应用程序。关于数据库和其他后端相关的细节将在我们启动后端应用程序后的下一个教程中介绍。

在我的开发环境中，我使用的是 Node v15.3.0。当使用 JS 相关技术时，我建议你们使用 [nvm](https://npm.github.io/installation-setup-docs/installing/using-a-node-version-manager.html) 来安装 Node JS。通过这种方式，您可以在开发环境中安装多个节点版本。所以我们 React 项目的第一步是安装 Node。请使用上面的 nvm 链接安装 Node JS，如果有任何问题，请留下评论，这样我可以帮助你们。

![](img/cc4b54e45599849a1c0cc2a8f234a9a1.png)

安装 Node 后，打开终端(在 windows a 命令提示符下)并运行此命令。

```
npx create-react-app serendib-scholarship-ui --template typescript
```

如果您有以前的节点版本或以前的 React 应用程序，可能会出现一个错误，告诉您首先删除全局 create-react-app 安装。即使卸载后，如果此错误仍然存在，请运行以下命令，然后运行上述命令。

```
npx clear-npx-cache
```

现在我们已经成功地用 typescript 创建了一个基本的 React 应用程序。现在运行以下命令来检查应用程序。

```
npm start
```

现在您将在您的[本地主机端口 3000](http://localhost:3000/) 中看到该应用程序。现在让我们看看这个初始应用程序中有哪些文件。

作为这个应用程序的 IDE，我推荐 VS 代码。为了支持调试，VS 代码中有一个扩展。对于 chrome 浏览器，它是 Chrome 的调试器。[请先安装](https://create-react-app.dev/docs/setting-up-your-editor)。现在创建一个名为。然后在. vscode 中创建一个名为 launch.json 的文件。

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Chrome",
            "type": "chrome",
            "request": "launch",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceFolder}/src",
            "sourceMapPathOverrides": {
                "webpack:///src/*": "${webRoot}/*"
            }
        }
    ]
}
```

现在再次使用 npm start 运行应用程序，然后转到 vscode 并按 f5。现在，您将获得一个针对 React 代码启用的调试选项。

所以我认为你有了一个用 Typescript 创建 React 应用程序的好主意。在下一个教程中，我们将讨论创建与应用程序相关的组件和容器。快乐编码:)

[应用](https://github.com/deBilla/serendib-scholarship-ui)的 Github 链接。