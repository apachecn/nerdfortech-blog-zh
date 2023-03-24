# 如何使用 Typescript 和 Docker 设置开发服务器

> 原文：<https://medium.com/nerd-for-tech/how-to-set-up-development-server-using-typescript-and-docker-1e63735b5ca2?source=collection_archive---------0----------------------->

通过这些简单的步骤，摆脱设置您的第一个开发服务器的艰巨任务

![](img/65ce028c15fed48f855da7b8b927856e.png)

克里斯托弗·伯恩斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

设置您的第一个开发服务器不是一件容易的事情。有时，您会对需要遵循的众多步骤和其他未指定的需求感到困惑。

我不久前就面临这个问题，事实是我需要将几种资源结合起来才能让它发挥作用。那么，如果我提供另一种资源来进一步帮助社区呢？

# 背景

假设您想使用 TypeScript 构建一个应用程序。这意味着您需要编译刚刚编写的 JavaScript 代码，然后运行它。好吧，听起来很简单。

您知道您有一个远程数据库，可以与项目的所有团队成员一起使用。但是在开发应用程序一段时间后，您的开发数据库中堆满了垃圾数据。既然你不喜欢这种乱七八糟。您想知道如何设置服务器和数据库来自由地开发您的应用程序，包括运行您的测试。

谢天谢地，你来对地方了。让我们停止这种甜言蜜语，跳到主要内容。

# 安装 TypeScript 项目

首先，为您的项目创建一个目录。你可以给它起一个你想要的名字，比如 typescript-server。之后，通过运行以下命令创建一个`package.json`文件:

```
npm init// Without any questions
npm init -y
```

现在，您的项目根目录中有了一个`package.json`。

接下来，您需要安装 TypeScript 作为开发依赖项。你可以通过跑步做到这一点

```
npm i typescript --save-dev
```

既然已经安装了 TypeScript，现在可以通过运行命令创建一个`tsconfig.json`文件

```
npx tsc --init
```

瞧，您的项目根目录中有一个`tsconfig.json`。`tsconfig.json`表示您的项目是一个 TypeScript 项目，它包含编译该项目所需的编译器选项。因为基本上，你是用 TypeScript 写代码，但是你的服务器是用 JavaScript 运行的。

在`tsconfig.json`中有很多选项，但是在这种情况下，我只使用这些选项来简化文件。

对于这样一个常见的目录结构，我们需要在项目的根目录下有一个`src`文件夹。所以，我们来做一个吧！

创建`src`文件夹和里面的`index.ts`。为了进行健全性检查，您可以将它写在`index.ts`中。

现在，通过运行命令编译 TypeScript 代码

```
npx tsc
```

此时，您会看到有一个名为`build`的文件夹。那是你的 JavaScript 文件被放置的地方。因为我们已经指定编译的文件夹在`build`文件夹中(在`tsconfig.json`中寻找`outDir`选项)。

现在，通过运行来运行 JavaScript 代码

```
node ./build index.js
```

不错，我们可以看到“你好世界！”打印在终端上的信息。

最好简化我们以前在`package.json`文件的脚本中使用的命令。将`package.json`中的脚本部分重写为:

正如脚本所建议的，您可以使用`npm run build`编译代码，并使用`npm run dev`运行代码。

# 运行我们的开发服务器

之前，我们已经编写了一个健全性检查来确保我们的代码可以被编译和执行。现在，我们将使用 Express 设置我们的服务器。

运行`npm install express @types/express`安装 Express

将`src`文件夹内的`index.ts`改为

和以前一样，您可以使用`npm run build`编译代码，然后使用`npm run dev`运行它。现在，您可以看到我们的服务器运行在端口 5000 上。

# 用 eslint 定义编码规则(可选)

编写代码时拥有预定义的规则是非常必要的。它可以使您的代码保持一致，尤其是当您在团队中工作时。但是对于初学者来说，用规则写作会成为一个减速因素。不用怕，你可以跳过这一节，足够自信后再回来。

安装 eslint 及其扩展:

```
npm install eslint eslint-config-airbnb-base eslint-plugin-import @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

显然，不同的团队或项目可能有不同的编码规则。但是您可以根据自己的需要进行配置。

# 使用 Nodemon 实时重新加载

好吧，我们取得了一些进展。但是每次我们做了一些改变，终止，编译，然后运行代码是不是很累？让我们通过使用 nodemon 来简化我们的开发体验。

运行`npm install nodemon`

创建`nodemon.json`，然后写这个

将之前的`package.json`脚本改为

注意:如果不使用 eslint，请移除`eslint --fix --ext .ts ./src`

现在，您可以通过运行`npm run dev`来运行您的开发服务器。尝试改变你的代码(例如`index.ts`，你会看到代码正在重新编译和运行。

# Docker 和 docker-撰写

此时，您已经有了一个工作的开发服务器。为了实现我们之前在简介中的关注点，让我们把它和 Docker 结合起来。

在项目的根目录下创建一个 Dockerfile 文件，并编写如下代码。

然后创建一个`docker-compose.yaml`

如果您需要 Postgres 数据库服务，那么使用这个 docker-compose 文件。

运行命令`docker-compose up`来运行 docker-compose 文件中的所有服务。

# Makefile 命令

为了使我们的开发周期更简单，让我们有一个 Makefile。

我们这里的命令几乎是不言自明的。您可以使用`make build`构建映像，使用`make up`(不重建映像)和`make run`运行服务，如果您想先构建服务的话。然后`make down`删除所有服务的容器。

# 结论

我们已经经历了设置开发服务器的几个步骤。就像名字说的那样，只是为了开发的目的。如果您对如何设置一个生产就绪的服务器感兴趣，您可以查看参考资料中的详细信息。

希望这篇文章可以帮助你理解建立一个开发服务器，不是一个令人生畏的任务，而是一步一步的顺序，你可以遵循。

感谢您的阅读和快乐编码！

# 资源:

*   [https://www . digital ocean . com/community/tutorials/typescript-new-project](https://www.digitalocean.com/community/tutorials/typescript-new-project)
*   [https://cloud nweb . dev/2019/09/building-a-production-ready-node-js-app-with-typescript-and-docker/](https://cloudnweb.dev/2019/09/building-a-production-ready-node-js-app-with-typescript-and-docker/)
*   [https://dev . to/Rubin/docker-pipeline-with type script-express-for-production-20kc](https://dev.to/rubiin/docker-pipeline-with-typescript-express-for-production-20kc)