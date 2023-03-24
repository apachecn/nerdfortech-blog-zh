# 为 React/Angular 和 Node 开发准备 Mac，安装 NodeJS、npm、yarn、VS 代码，并配置 VS 代码

> 原文：<https://medium.com/nerd-for-tech/preparing-your-mac-for-react-and-node-development-install-nodejs-npm-vs-code-and-configure-vs-e4f0cf1e99a4?source=collection_archive---------1----------------------->

# 按照以下步骤/视频安装 Nodejs

1.  [https://nodejs.org/en/download](https://nodejs.org/en/download)
2.  向下滚动直到你看到你的操作系统并点击它(在我的例子中是 Mac)

![](img/6c6e55696e177d81db34f02421d4fba1.png)

3.文件下载后，点击它并按照说明操作

![](img/af61adb9b69074e793c914b0afcd424d.png)

# 安装 Brew(纱线可选)

/bin/bash-c " $(curl-fsSL https://raw . githubusercontent . com/home brew/install/master/install . sh)"

# 安装纱线

brew 安装纱线

或者

```
npm install -g yarn
```

# 安装 VS 代码

1.  转到以下网址:[https://code.visualstudio.com/download](https://code.visualstudio.com/download)

[](https://code.visualstudio.com/download) [## 下载 Visual Studio 代码- Mac、Linux、Windows

### Visual Studio 代码是免费的，可以在您喜欢的平台上获得——Linux、macOS 和 Windows。下载 Visual Studio…

code.visualstudio.com](https://code.visualstudio.com/download) 

2.向下滚动直到你看到你的操作系统并点击它(在我的例子中是 Mac)

![](img/20d63c2efd0a312a7e7adbf32c7c74c8.png)

3.文件下载完成后，点击解压并打开

![](img/da5f4e5cdd8ae04bdd1eb39d5a5a4aaf.png)

4.运行该文件并按照说明进行操作

文件解压缩后，将应用程序移到“应用程序”文件夹

5.当 IDE 启动时，打开命令面板(⇧⌘P)并键入“shell command”以找到 shell 命令:在路径命令中安装“code”命令。

![](img/e544254c22fa7bfd21ef858a7cac09ce.png)![](img/f1adac7198bc612799de8d76df60d695.png)

*   重启终端，使新的`$PATH`值生效。你可以输入“代码”开始编辑该文件夹中的文件。
*   导航到微前端 1 目录并编写代码。如下所示:

![](img/758de5720b4ea498d7a816907c9a6fb0.png)

设置 git

```
git config --global user.email "email@example.com"
```

# HTML Emmet

这些东西可以帮助你用几个字母添加模板

示例:

如果你在 html 文件和类型！+ enter，它将创建如下的模板 html 文件

![](img/92801fc871775aa247ce17593bce89dc.png)

# 代码片段

您可以使用经常使用的样板文件创建自己的代码片段。

转到设置/用户片段

![](img/ffdbfa16ffcd4abdec1b1f9eac79a176.png)

然后选择语言

写下你的片段

![](img/f2b1679beba2bac0a44825b656d3473b.png)

它会在您键入代码时出现，单击 enter/tab，它会自动完成，如下所示

![](img/721bcdf3ca9e71b9162a76f2b3c2c6cd.png)

# 添加一些扩展:

## 较美丽

更漂亮是非常重要的组织你的代码和调整空间和制表符。

![](img/bdda11ef19f2c9c0d9a4a5b3958d44ed.png)

然后转到“设置”,按如下方式搜索“更漂亮”:

![](img/2be6cacfe2fc985acaf7c947dd28158a.png)

之后，您可以选择您的选项，它会在您保存时自动强制执行。

确保选中“保存时的格式”

![](img/277a00b2f1950d4c022b1e7666868514.png)

工作空间也是如此

![](img/94551f25f4be02df771fbfa83d8aa966.png)

## 设置同步

[](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) [## 设置同步- Visual Studio 市场

### 虽然是免费和开源的，但如果你觉得它有用，请考虑通过 PayPal 或 Open……

marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync) ![](img/6e98d79b7f3f971a1d93c65231faac0e.png)![](img/59bb4b65e39b6dec9c677b68d073a9e7.png)

## 路径智能感知

[](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense) [## 路径智能感知- Visual Studio 市场

### 自动完成文件名的 Visual Studio 代码插件。消除上下文切换和昂贵的干扰。创建并…

marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense) 

这是进口的另一个重要扩展。

![](img/c2341e27b55e28ad185c6373c0e9b8dc.png)

## 反应片段

[](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets) [## ES7 React/Redux/graph QL/React-本机代码片段- Visual Studio 市场

### 这个扩展为你提供了 ES7 中的 JavaScript 和 React/Redux 代码片段，以及 VS 代码启动的 Babel 插件特性…

marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets) ![](img/2edc97bb1a4d0f7773f3756b22990f05.png)![](img/21e6ebff6b16ec14eed5f154eb7b6aba.png)![](img/562a4533b147c0b442d67039637301c4.png)![](img/3453b6db2a22c3fe13826a12cca6988e.png)![](img/989835b5df60856532cb431a5fd57354.png)![](img/e3e4aae3783a81dcc002fe0accdec1ae.png)

我使用 rafce 为 react 函数组件编写模板代码

![](img/341f4b847147a2d10611ff661033c327.png)![](img/d7628b6ded8d13a8ca4db7ffd9185339.png)

## 自动重命名标签

![](img/840fa3ed50405b51597cbeff40a95b2c.png)

当你试图重命名一个标签时，这是非常重要的，它会自动重命名结束标签

![](img/46459de126707dff4a62dd489fef9863.png)

## Polacode(给你的代码拍个快照)

如果你正在写文章或者想要分享你的代码的快照，这是很好的

![](img/4a4c8134c84e301d069f95576e20466b.png)![](img/c178ce73c566c9e2bd37c4b8af50afce.png)

下图是使用 Polacode 的截图:

![](img/7d838a9c5547a7523ce30b1b452484f9.png)

## 括号对上色器

![](img/4f3212097b3c532a1a6810ea44f439bf.png)![](img/5aa89adfc9ac66cc55a36fd07e7f9d1f.png)

## HTML 中 CSS 类名的智能感知

[](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion) [## HTML 中 CSS 类名的 IntelliSense-Visual Studio 市场

### Visual Studio 代码的扩展-基于定义的 HTML 类属性的 CSS 类名完成…

marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=Zignd.html-css-class-completion) ![](img/6557ccc58a13d09c63becff6afe59191.png)

这有助于您使用 CSS 和引导选项

## 颜色选择器

[](https://marketplace.visualstudio.com/items?itemName=anseki.vscode-color) [## 颜色选择器- Visual Studio 市场

### 帮助 GUI 生成颜色代码，如 CSS 颜色符号。还有，一个命令转换颜色来改变颜色…

marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=anseki.vscode-color) 

## 材料图标主题

![](img/b11db614f2b5c75b2d3b9cd2c1db147c.png)![](img/aba8ea4ea487e8efa6cb4d34cbf8b38c.png)

## 彩虹括号

![](img/00af22e58c13538c813da8930ecf9eb0.png)

这个扩展通过给它们不同的颜色来帮助你识别嵌套的括号。

## Lorem impsum

Lorem impsum 将帮助您为演示和教程编写随机段落

[](https://marketplace.visualstudio.com/items?itemName=Tyriar.lorem-ipsum) [## Lorem ipsum - Visual Studio 市场

### 生成 lorem ipsum 文本并将其插入 Visual Studio 代码中。打开 VS 代码按 F1 键输入“安装”选择…

marketplace.visualstudio.com](https://marketplace.visualstudio.com/items?itemName=Tyriar.lorem-ipsum) ![](img/6defc82bf1d0735751e939b6903db922.png)

# 定制您的 zsh 终端

1.  打开终端
2.  键入`touch ~/.zshrc`创建文件。
3.  打开`~/.zshrc (open ~/.zshrc)`
4.  添加以下内容

PROMPT='Rany > '

![](img/b4db297d205ccf2cb3dae70816233a8b.png)

5.重启你的终端

![](img/17dc9ae6decf511305fb901bccb7d889.png)