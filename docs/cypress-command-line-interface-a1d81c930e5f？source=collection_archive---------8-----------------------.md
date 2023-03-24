# Cypress 命令行界面

> 原文：<https://medium.com/nerd-for-tech/cypress-command-line-interface-a1d81c930e5f?source=collection_archive---------8----------------------->

![](img/8a9c2c757644bc58e90e7f1a400cdfa8.png)

在开始之前，我将脚本修改如下:

![](img/1c5b38af97a9c140fcf0186b29c200af.png)![](img/83e8eed9b8c11707ffab3493b349dfb6.png)![](img/410df794d189a4681aeb59f96bd2c171.png)

让我们打开一个终端，键入以下命令

```
npx cypress run
```

开始了。赛普拉斯正在进行测试。并以文件方式给出记录的结果。它在默认的浏览器中执行。

![](img/56b38dc879e6bc5faf3cb50d4fd0848c.png)![](img/d37cf28e703504212e9e827a95709d94.png)

如果要在 Chrome 浏览器中执行，那么输入以下不带 headless 的命令:

```
npx cypress run --browser chrome --headless
```

![](img/6956661544a1547407573a64fe3446f2.png)![](img/6f240ae6e6f57780ee27cb269062eaf8.png)

如果要执行单个测试文件，请键入以下命令:

```
npx cypress run --spec cypress/integration/orangehrm-login.spec.js
```

![](img/6cb92e65410fca4aed4c29d761767867.png)

最重要和最适用的惯例之一是 npm 测试惯例。这是生态系统的一个巨大部分，对团队成长非常好。如果您将这个工具包提交到您的源代码控制，并将其推送到您的源代码控制，则不同的开发人员将通过使用 npm install 来获取您的源代码。所有软件包都是本地的，因此“npm 安装”可以更新所有必要的信息来执行测试。然而，这是惯例，他们假设如果他们运行 npm 测试，所有的包检查都会被执行。我们必须使这成为可能。让我们打开 package.json 并将下面的命令插入脚本-> test

```
cypress run --browser chrome --headless
```

![](img/26fbcd5c1017667ddc3b3f31ff23db19.png)

现在执行以下命令，使用 npm 运行测试

```
npm run test
```

![](img/f0bad9f35ff80cbd6ef522dc18575083.png)![](img/6172ab7fb939a0428e933538280975c0.png)