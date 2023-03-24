# 反应属性类型检查:属性类型

> 原文：<https://medium.com/nerd-for-tech/react-prop-type-checking-proptypes-a7033dd1939e?source=collection_archive---------8----------------------->

![](img/8933e4fa3854e49902a096a7f2866351.png)

虽然许多 react 应用程序可能会使用 TypeScript 进行内置类型检查，但如果您在普通 React 应用程序中工作，您可以使用 *prop-types* 库来做一些相同的工作。特别是这个库允许你检查道具的类型是否正确以及它们是否存在。

# 装置

如果您使用的是 React 版本 16 或更高版本，您可以安装包含以下内容的库:

```
npm install — save prop-types
```

如果你正在使用一个旧版本的 React，没有必要安装，prop-types 已经内置在 React 中了！

# 进口

要开始使用 prop-types，请将它导入到一个组件中，并在文件顶部添加以下代码行:

```
import PropTypes from ‘prop-types’
```

如果你在旧版本的 React 中工作，你可以用`React.PropTypes`访问道具类型。

# 使用

两个版本的道具类型的用法相似。让我们从较新的库版本开始。首先，您需要通过在组件上使用`.propTypes`方法来定义组件的属性类型，如下所示:

```
Example.propTypes = {}
```

在对象内部，我们将通过一个键值对来声明道具的名称和预期类型。为了定义类型，我们使用`PropTypes.` ，然后是我们期望的任何类型。

```
Example.propTypes = { name: PropTypes.string};
```

对于老版本的 React，初始设置是一样的，但是要设置道具的类型，你需要通过`React.PropTypes`来访问`PropTypes`

可以接受`strings`、`arrays`、`Booleans`、`functions`、`numbers`、`objects`等一整串类型。关于更详细的列表和用法，请查阅官方[文档](https://www.npmjs.com/package/prop-types)。

# 检查是否存在

为了确保一个属性总是被传递，而不管它的类型如何，你可以将`.isRequired`方法添加到它的类型定义的末尾，如下所示:

```
Example.propTypes = { name: PropTypes.string.isRequired};
```

# 案例研究|尝试一下！

在下面的例子中，我设置了一个组件，它接受一个属性、一个名称，我没有传递一个字符串，而是传递了一个数字(哎呀！).控制台显示一条警告，提示有一个类型为`number`的无效属性`name`，它应该是一个`string`。

请随意使用我设置的这个例子。如果你根本不传入一个值会怎么样？