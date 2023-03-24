# 用标准和更漂亮的方式建立 ESlint

> 原文：<https://medium.com/nerd-for-tech/setting-up-eslint-with-standard-and-prettier-be245cb9fc64?source=collection_archive---------0----------------------->

![](img/f491a09b96a1a2c66a11ccd43930584a.png)

瑞兰德·迪恩在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 快速介绍

ESlint: 代码检查器，用于检查代码，并在开发过程中提前显示潜在的错误。ESlint 支持为您的代码库使用自定义规则。
**StandardJs:** 执行一组固定的预定义 ESlint 规则的库。如果您想使用标准的默认规则，这里不需要配置。
**更漂亮:**一个代码格式器，用于保持一致的编码风格。更漂亮的格式的东西，如空格数，括号，分号等到了一个格式。

让我们开始在项目中设置这些。

# **第一步:安装并配置 ESlint**

运行下面的命令，这将打开一个 REPL，让您在不同的配置选项之间进行选择。

```
npm init @eslint/config
```

下面是 ESLint 会问的问题:
1)你希望如何使用 ESlint？选择:`To check syntax, find problems, and enforce code style`如果你不想使用更漂亮或其他样式的格式化程序，选择:`To check syntax and find problems`
2)你的项目使用什么类型的模块？根据您的项目选择此项，即`import/export`或`require/exports`
3)您的项目使用哪个框架？根据你的项目选择
4)你的项目使用 TypeScript 吗？根据你的项目选择
5)你的代码在哪里运行？选择`Browser`还是`NodeJS`
6)你希望如何为你的项目定义一种风格？此问题允许从 Airbnb、Google 等组织中选择预定义的规则/配置集。选择:`Use a popular style guide`然后从 Google、Airbnb 或 Standard 中选择一种风格。你希望你的配置文件是什么格式？这并不重要，但为了简单起见，选择`JavaScript`
8)您想要安装依赖项吗？`Yes`
9)选择一个包管理器:选择`yarn`或`npm`

完成上述所有步骤后，将安装 eslint 和依赖项。此外，将在项目的根目录下生成一个 eslint 配置文件。

它看起来与下面的文件相似，根据你在上面选择的选项也有一点不同。

```
module.exports = { env: { browser: true, commonjs: true, es2021: true }, extends: 'standard', overrides: [ ], parserOptions: { ecmaVersion: 'latest' }, rules: { }}
```

如果有的话，您可以在`rules`对象属性中添加更多的自定义规则。

```
rules: {
  'no-unused-vars': 'off'
}
```

如果你想跳过 eslint 的问题，想手动安装 eslint。做`npm install --save-dev eslint`或者`yarn add -D eslint`。然后在您的根项目中创建一个`.eslintrc.js`文件并添加配置。

在这一点上，您应该已经开始看到红色的曲线向您大喊，要求您修复语法、样式和其他不好的做法。

要快速修复代码，运行`npx eslint "**/*.js" --fix`。这应该可以修复大多数错误。但是对于像`x is not defined`这样的错误，你必须手动修改代码。

## **与不同框架的集成:**

**Angular:** 安装 Angular 插件
`npm install --save-dev eslint-plugin-angular`

在`.eslintrc.js`
`extends: ['standard', 'plugin:angular/johnpapa']`
的`extends`属性中添加插件 Doc:[https://github.com/EmmanuelDemey/eslint-plugin-angular](https://github.com/EmmanuelDemey/eslint-plugin-angular)

**React:** 安装 React 插件`npm install eslint-plugin-react --save-dev`

在`.eslintrc.js`
`extends: ['standard', 'plugin:react/recommended']`
Doc:[https://github.com/jsx-eslint/eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react#recommended)的`extends`属性中添加插件

## 小费💡

如果您想在林挺时节省时间，请将缓存选项作为参数传递给 eslint。这确保了只检查新的代码更改，而不是整个代码库。
`npx eslint "./**/*.js" --cache --cache-strategy content --fix`
在 package.json 中把这个加到你的`scripts`中，避免每次都打这么长一行。

```
"scripts": {
  "lint": "npx eslint \"./**/*.js\" --cache --cache-strategy content --fix"
}
```

doc:[https://eslint.org/docs/latest/](https://eslint.org/docs/latest/)

# 步骤 2:安装/添加标准组件

如果您遵循了`npm init @eslint/config`步骤并选择了`standard`作为默认的编码风格，那么您已经在您的 ESlint 中添加了 StandardJs，您不需要做任何其他事情！🙌。

另一个选择是只安装`standard`包并使用它。这是使用标准的最简单的方法。这不需要配置和`.eslintrc.js`文件。这是简单的即插即用，没有配置过载。
但是这个选项也有一个缺点，就是不可配置。不能更改预定义的规则，也不能添加新规则。

要独立安装和使用`standard`，执行
`npm install — save-dev standard`并使用`npx standard --fix`运行

如果您想要一个可配置的 Standard 版本，并且想要使用 eslint 的自定义规则，请这样做

```
npm install --save-dev eslint-config-standard eslint-plugin-promise eslint-plugin-import eslint-plugin-n
```

然后在配置文件中扩展`standard`

```
extends: ["standard"]
```

如果您在项目中使用 typescript，请使用`standard-with-typescript`。安装它并将其添加到您的`.eslintrc.js`文件中

```
npm install --save-dev eslint-config-standard-with-typescript// .eslintrc.js
module.exports = {
  extends: 'standard-with-typescript',
  parserOptions: {
    project: './tsconfig.json'
  }
}
```

Doc:[https://standardjs.com/index.html](https://standardjs.com/index.html)
Doc:[https://github.com/standard/eslint-config-standard](https://github.com/standard/eslint-config-standard)
Doc:[https://github . com/standard/eslint-config-standard-with-type script](https://github.com/standard/eslint-config-standard-with-typescript)

# 第三步:安装得更漂亮

我们将在 eslint 之上使用更漂亮的，因为与 ESlint 这样的 linters 相比，它提供了良好的代码格式。注意:Prettier 只是一个代码格式化程序。它不会检查语法错误以外的错误。

ESlint 和 appearlier 相互冲突，因为它们有重叠的规则，而这些规则在这两者中的定义是不同的。我们可以通过使用`eslint-config-prettier`很容易地使这些一起工作，这样 eslint 就不会抱怨冲突的更漂亮的规则。

使用安装更漂亮

```
npm install --save-dev prettier
```

您可以创建一个`.prettierrc.js`文件来设置所需的规则，或者使用默认的格式规则。它看起来会像这样

```
module.exports = { tabWidth: 2, semi: false, singleQuote: true,}
```

然后用`npx prettier --write "**/*.js"`按照更漂亮的来格式化文件。每次运行都很痛苦，所以大多数现代编辑器都提供了扩展，允许在保存时格式化文件，这非常方便。

最后一步，将`eslint-config-prettier`添加到`.eslintrc.js`文件中，这样 ESlint 和 Prettier 就可以很好地配合了。确保在`extends`数组的末尾添加更漂亮的。

```
npm install --save-dev eslint-config-prettier // .eslintrc.js
{
  "extends": [
    "some-other-config-you-use",
    "prettier"
  ]
}
```

Doc:[https://prettier.io/docs/en/index.html](https://prettier.io/docs/en/index.html)T2 Doc:[https://github.com/jsx-eslint/eslint-plugin-react](https://github.com/jsx-eslint/eslint-plugin-react)

那都是乡亲们！如果你面临任何问题，请在评论中告诉我，并确保留下👏如果这对你有帮助。