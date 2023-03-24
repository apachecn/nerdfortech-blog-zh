# Angular Framework 入门

> 原文：<https://medium.com/nerd-for-tech/getting-started-with-angular-framework-dc618d9b156?source=collection_archive---------4----------------------->

> 在这篇文章中，我想向刚刚入门的初学者介绍 Angular framework。这篇文章只是作为一个介绍，关于 Angular 的进一步解释和概念将在我接下来的几篇文章中发表。

Angular 是一个用于构建移动和基于 web 的客户端应用程序的框架。这对于 SPAs(单页应用程序)来说是很棒的。它使用模块化的方法，并且由于组件的缘故，对于可重用的代码来说是很棒的。

Angular 于 2010 年作为 Angular Js 首次推出。后来，Angular 2 在 2015 年左右推出，与此同时，Angular 更专注于 typescript 而不是 javascript。

## 角度与反应:

Angular 是一个成熟的软件开发框架，通常不需要任何其他库。而 React 是用于 UI 开发的库。它需要额外的库，如 Redux、React Router 等。

在推出时，React 被认为在性能上比 Angular 更好，因为 React 使用虚拟 DOM，因此浏览器上的负载更少。但是 Angular 很快就赶上了它后来的版本。Angular 使用实 DOM 和双向数据绑定。

React 使用 JS ES6 结合 JSX。它使用 babel 在浏览器中编译 jsx 代码。另一方面，Angular 可以使用 javascript 或 typescript。Typescript 比 Javascript 更紧凑，并提供了一些优势。

## 角度与角度:

AngularJs 于 2010 年由谷歌发布。但性能方面，它落后于 react 和 vue。所以他们决定使用 typescript 完全重写 angular，因此 Angular 2 被引入，它在性能上与 react 相当。

AngularJs 使用 Javascript，但后来的版本(从 Angular 2 开始)使用 typescript。TypeScript 是 JavaScript 的超集，在开发过程中提供静态类型。静态类型不仅提高了性能，还避免了许多运行时陷阱，这些陷阱使得 AngularJS 难以用于更大更复杂的应用程序。

Angular 也比 AngularJs 快得多(几乎快 5 倍)。Angular Js 使用了影响其性能的双向绑定。Angular 有一个通量架构，通过单向数据流进行变化检测，使应用程序速度更快。

但是 Angular 的学习曲线很陡。如果你有一个相当简单明了的应用程序要开发，AngularJS 可以让开发变得更快更容易。但是如果你想开发必须可伸缩的复杂应用程序，Angular 应该是你显而易见的选择。

## 开发环境:

需要以下内容:

1.  结节
2.  npm
3.  Angular 命令行界面
4.  文本编辑器(相对于代码)

首先安装节点和 npm。你可以浏览官方文档，这个过程很简单。

要检查您是否已经安装了它们，请转到命令行并运行**“node-v”**和**“NPM-v”**。如果已安装，它们会返回各自的版本。

要安装 Angular CLI，请运行:**" NPM install-g @ Angular/CLI "**

安装后，使用**“ng—version”**检查版本。在检查 angular 版本时，如果你得到一个警告，说你需要最新的节点版本，那么就去 node js 官方网站重新下载并重新安装 node js。

## 创建一个基本的 Hello world 应用程序并理解代码的工作原理:

*   在文本编辑器中，进入你想要创建应用的文件夹，运行**“ng 新应用名称”。这将创建一个以您给定的应用程序名称命名的文件夹。**
*   进入文件夹，运行**“ng serve”。**
*   打开浏览器，在 localhost:4200 你可以看到你的第一个 angular 应用。

**代码如何工作？**

*   在进入进一步的细节之前，现在要理解当你运行 **"ng serve "时，t** he 执行进入 **"main.ts"** 文件。
*   main.ts 文件中包含已引导的 app.module.ts。
*   app.module.ts 中还包含一个 app.component.ts 引导文件。

这就是指令流是如何发生的。在接下来的几篇文章中，我们将进一步解释角形建筑以及每件作品是如何工作的。