# 我如何构建我的项目

> 原文：<https://medium.com/nerd-for-tech/how-i-structure-my-projects-6ec4fddc667a?source=collection_archive---------19----------------------->

![](img/bdaf942472266db710741f90455abc6a.png)

照片由[屋大维丹](https://unsplash.com/@octadan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最终，结构是由项目需求决定的。如果你正在为一个现有的项目做贡献，保持现有的结构是很重要的。但是如果你是从零开始一个项目，你会有一些自由空间。这也取决于项目是否真的是你自己的，或者你的公司是否有新项目的指导方针。

但是，如果你是这个领域的新手，还在学习过程中，那么我希望这能帮助你开发你的个人项目。

首先，结构最终取决于你选择的框架。

# 反应/打字稿

有几种方法可以设置 React 项目。对于小型和一些中型项目(取决于复杂性)，我使用 monorepo。

```
MyProject
  |_.idea (IDE generated folder for IDE project settings)
  |_Backend
    |_config
    |_controllers
    |_middleware
    |_models
    |_routes
    |_tests
    server.js
    (and other global files)
  |_Frontend
    |_docs
    |_node_modules
    |_public
    |_src
      |_assets
        |_images (and so on)
      |_config
      |_components
        |_subcomponents
      |_containers (for different layouts a view/page might have)
      |_redux (or other state management stuff)
      |_types (if using Typescript)
      |_models
      |_pages (or views folder)
      |_css
      |_sass (if I'm using sass) index.tsx
    |_tests
    .gitignore
    .env
    Dockerfile
    package.json
    yarn.lock
```

对于分割回购方法，设置总体上是相同的，只是两个不同的项目目录。

如果你采用的是 SSR(服务器端渲染)方法，只要把我上面提到的前端和后端连接起来就行了(你不需要前端模型文件夹)。我至少会将您的服务器内容放在名为 server root 的目录中。这种串联结构也是我使用 EJS 这样的模板语言的方式。

# 反应自然

我使用 Expo 的设置，除了添加一个 src 文件夹

# 动态超文本链接标记语言

DHTML(动态 HTML)只是一种只使用 HTML/CSS/JS 和 DOM 构建的网页的花哨说法。从技术上来说，你可以把 React 和类似的框架扔进去，但是我没有。当您不需要或不想要 node/npm 带来的复杂性时，这种设置非常有用。

随着你在这个领域的进步，你很可能不会经常使用这种设置，但如果你是在课程之外创建你的第一个项目，我绝对建议你这样做。这是检验你的普通 HTML/CSS/JS 技能的好方法。试着创造一些对 JS 很重要的东西，比如游戏，无论如何，回到正轨。

这就是这类项目的结构。

```
MyProject
  |_assets
    |_images
    |_videos
    |_files
  |_stylesheets (or css/styles, depending on my mood)
  |_sass (if using sass...you can use sass without node.)
  |_scripts (or js/javascript, depending on my mood)
  |_pages (if there's more than one page)
  index.html
  README.md
  (other miscellaneous files that are not assets)
```

澄清一下，资源是您可能想要从计算机复制到项目中的文件。这是为了直接放在你的网站上(而不是从另一个网站嵌入)。

一如既往，我希望你觉得这很有用！