# 在 Javascript 中利用 Husky 的 git 挂钩

> 原文：<https://medium.com/nerd-for-tech/leveraging-git-hooks-with-husky-in-javascript-c34cac4613ee?source=collection_archive---------2----------------------->

![](img/a0aaa1e8f2590f9b3e9f6131f121c53a.png)

由[麦克斯韦·达科姆](https://unsplash.com/@maxwelldacombe?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 快速介绍

Git 为在特定事件之前/之后触发命令提供了不同的钩子。但是开发时可能会派上用场的钩子一般是`pre-commit`、`commit-msg`和`pre-push`钩子。

我们将使用`pre-commit`(提交前运行命令，提交失败)和`commit-msg`(检查提交消息，失败)

这些钩子与`bash`命令一起使用，但是为了快速入门而无需编写`bash`代码，我们可以使用 Husky npm 包来为我们设置。

# 设置 Husky

这很容易，因为默认情况下，husky 提供了一个命令来设置和创建一个`pre-commit`钩子。

但是第一步是初始化 git 存储库(如果还没有初始化的话)。执行`git init`并初始化 git 存储库。

下一步是安装哈士奇

```
npx husky-init && npm install
```

上面的命令将安装 husky 作为一个开发依赖项，并创建一个`.husky`文件夹，并在`.husky/pre-commit`文件中设置一个演示`pre-commit`钩子。默认情况下，文件内容会有`npm test`命令，您可以根据需要进行更改。

您可以通过类似于`npx eslint && npm test`的方式来更改这个默认提交，它将首先 lint 整个代码库，然后运行测试(如果有的话)。

要手动添加`husky`，请执行以下操作

```
npm install --save-dev huskynpx husky install
```

要添加其他 git 挂钩，请使用

```
npx husky add .husky/<hook-name> "your command"
```

要在同一个挂钩中添加更多命令，请在新的一行开始添加命令

```
// .husky/pre-commit#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.shnpx eslint
npm test
```

如果出于某种原因，您想跳过 git-hooks 检查，那么在提交时添加`--no-verify`标志

```
git commit --no-verify -m "WIP"
```

# 使用 lint-staged 搭配 Husky

当林挺代码时，您只需要 lint 暂存区中将要提交的文件。

但是，`eslint`或其他 linters 对代码库中的所有文件或满足特定 glob 模式(如`src/**/*.{js,ts}`)的文件运行林挺。这不利于时间效率。相反，只对暂存区中的文件运行林挺会更好。

有一个包可以解决这个问题。`lint-staged`可让您配置要为暂存文件运行的命令。

安装`lint-staged`作为开发依赖项

```
npm install --save-dev lint-staged
```

将下面的配置添加到`package.json`，并在提交暂存的`js`或`ts`文件之前提供您想要运行的任何命令。

```
"lint-staged": {
  "*.{js,ts}": ["npx eslint --fix", "npm test"]
}
```

在`.husky/pre-commit`档中，将`npm test`替换为`npx lint-staged`。这将运行`lint-staged`，而`lint-staged`又将运行提供给`lint-staged`配置的命令，只在暂存文件上运行。

# 林挺用`Commitlint`提交消息

我们可以在 husky 中使用另一个流行的 git 挂钩`commit-msg`。我们将检查我们的提交是否写得很好并遵循了标准。

我们可以利用`commitlint`在提交之前验证我们的提交消息。

安装`@commitlint/cli`和`@commitlint/config-conventional`(带有预定义提交规则的示例配置)。

```
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

然后创建一个`commitlint.config.js`文件并扩展`config-conventional`

```
// commitlint.config.jsmodule.exports = { extends: ["@commitlint/config-conventional"] };
```

默认情况下，这个设置不会做任何事情。在 husky 中添加一个`commit-msg`挂钩，以便在提交前自动运行。

```
npx husky add .husky/commit-msg "npx commitlint --edit"
```

如果消息无效，这会抛出错误，并且不允许提交(根据 commitlint 配置)。

如果您想了解更多关于`commitlint`的信息并想定制配置，请访问[https://commitlint.js.org/#/reference-configuration](https://commitlint.js.org/#/reference-configuration)。

这是所有的乡亲。如果这有帮助，请留下👏下一集再见。

# 有用的链接

*   【https://typicode.github.io/husky/#/】
*   [https://www.npmjs.com/package/lint-staged](https://www.npmjs.com/package/lint-staged)
*   [https://commitlint.js.org/#/](https://commitlint.js.org/#/)