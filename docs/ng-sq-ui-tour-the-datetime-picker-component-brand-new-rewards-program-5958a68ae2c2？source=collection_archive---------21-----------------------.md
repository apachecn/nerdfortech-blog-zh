# ng-sq-ui 之旅:日期选择器组件(+全新的奖励计划)

> 原文：<https://medium.com/nerd-for-tech/ng-sq-ui-tour-the-datetime-picker-component-brand-new-rewards-program-5958a68ae2c2?source=collection_archive---------21----------------------->

![](img/9d5e8253d4a32077835ac0cab69d3b9e.png)

你好！欢迎来到“ng-sq-ui 之旅”系列的另一篇文章 Angular 的简单质量 ui 介绍！这一次，我们的重点将是[@ sq-ui/ng-datetime-picker](https://www.npmjs.com/package/@sq-ui/ng-datetime-picker)模块及其导出的组件。

# 概观

使用两个独立的组件来实现日期时间选择器功能:[**sq-日期时间选择器**](https://sq-ui.github.io/ng-sq-ui/#/datetime-picker-module?id=sq-datetime-picker) 和[**sq-时间选择器**](https://sq-ui.github.io/ng-sq-ui/#/datetime-picker-module?id=sq-time-picker) 。默认情况下，sq-datetime-picker 组件只显示一个日期选择器。它公开了启用/禁用其内置 sq-time-picker 组件的配置属性。这两个组件都由 NgDatetimePickerModule 导出，可以单独使用。该模块可以作为一个独立的包使用，即[@ sq-ui/ng-datetime-picker](https://www.npmjs.com/package/@sq-ui/ng-datetime-picker)，也包含在主 [ng-sq-ui npm 包](https://www.npmjs.com/package/@sq-ui/ng-sq-ui) e 中。

# 装置

```
npm install @sq-ui/ng-datetime-picker
```

或者

```
npm install @sq-ui/ng-sq-ui
```

# 综合

与 [sq-modal 组件](/@rhythmxholic/ng-sq-ui-tour-the-modal-component-f858586b566f)类似，sq-datetime-picker 依赖于[**NgSqCommonModule**](https://sq-ui.github.io/ng-sq-ui/#/common-module)获得附加逻辑。它被列为依赖项，因此应该自动安装。

如果您已经安装了独立软件包，则需要导入这两个模块:

```
import { NgSqCommonModule } from **'**@sq-ui/ng-sq-common';
import { NgDatetimePickerModule } from '@sq-ui/ng-datetime-picker';
```

然后将它们添加到我们希望它们加入的模块中:

或者，如果您选择使用主软件包:

```
import { NgSqUiModule } from '@sq-ui/ng-sq-ui';
```

日期时间选择器和独立的时间选择器都使用@fortawesome/fontawesome-free 包中的一些图标。通过在 angular.json 的构建样式数组中包含 fontawesome 图标样式表的路径，可以将它添加到您的项目中:

```
"styles": [
  "./node_modules/@fortawesome/fontawesome-free/css/fontawesome.min.css",
  "./node_modules/@fortawesome/fontawesome-free/css/solid.min.css",
  "./node_modules/@fortawesome/fontawesome-free/css/regular.min.css",
  "src/styles.css"
],
```

# 日期时间选择器:用法

## 基本配置

组件是为表单设计的，所以它们的底层逻辑基于 [Angular 的表单 API](https://angular.io/api/forms/ControlValueAccessor) 。这两个组件都可以使用[模板驱动](https://angular.io/guide/forms)，以及[反应式表单](https://angular.io/guide/reactive-forms)方法:

角形 API 集成示例

> **注意:**上面的例子使用了 sq-ui 主题(参见 angular.json 中的“styles”数组)。简单质量的 UI 组件有它们自己的主题，但是默认情况下**是禁用的**。使用它们完全是可选的。从 angular.json 中删除“sq”类和 sq-ui-theme.scss 文件将使它们变得非常简单。

## 集成时间选择器

该组件附带了一个集成的时间选择器(它也可以作为一个单独的组件获得，但是我将在本文的后面提到它)。要启用它，我们必须将 **isTimepickerEnabled** 输入属性设置为 true，并将有效的时间选择器配置传递给 **timepickerConfig** 输入属性:

集成时间选择器示例

## 输出类型

在实际开发 datetime-picker 之前，我们仔细考虑了几个日期对象操作库作为选项。我们在 date-fns 和 moment.js 之间左右为难，最后我们决定用 moment.js，因为它广为人知，使用广泛。它还提供了现成的区域设置支持，这是常规日期时间选择器所必需的。

默认的输出类型设置为常规的 JavaScript Date 对象。通过将“moment”或“unix”作为值传递给 **dateObjectType** 输入，可以将日期时间选择器配置为以 moment 对象或 unix 字符串的形式返回选定的日期时间对象:

不同日期输出示例

## 自定义输出日期格式

如果作者希望输出是特定的字符串格式，他们可以将一个字符串值传递给**格式的**输入。要求该值是 [moment.js](https://momentjs.com/docs/#/parsing/string-format/) 支持的有效格式字符串。

> **注意:**一旦你将一个格式字符串传递给**格式**输入属性，那么**日期对象类型**的值将被忽略，日期时间选择器将把输出作为一个字符串返回。

自定义字符串输出格式示例

## 多重日期选择

datetime-picker 组件可以配置为支持多个日期选择。这可以通过将 **isMultipleSelect** 输入设置为真来实现。在这种模式下，datetime-picker 将以字符串数组的形式返回选定的日期。日期将根据为**日期对象类型**或**格式输入指定的值进行格式化。**

多重日期选择示例

## 可选日期范围限制

我们可以通过设置 **minDate** 和/或 **maxDate** 输入属性来限制可供选择的日期:

日期范围限制示例

# 时间选择器:用法

## 基本配置

与 datetime-picker 类似，该组件也是为了与角度表单(反应式和模板驱动式)集成而构建的。其最显著的特点之一是，通过使用 **isMeridiem** 标志输入，其输入可以无缝地配置为 12 小时或 24 小时:

基于反应的时间选择器示例

时间选择器的输出可以是一个字符串或一个 moment 对象——这是通过 **timeObjectType** 属性配置的，如上例所示。

## 附加选项

时间选择器组件提供了几个不同的配置选项，其中包括**小时**和**分钟**输入属性。作为作者，它们允许您直接通过模板预先填写时间选择器:

基于模板的时间选择器示例

默认情况下，用户可以通过键盘编辑小时和分钟。这可以通过将 **isEditable** 属性设置为 false 来防止，这样，小时和分钟将仅通过控件来更改。此外，小时和分钟的变化步长也可以通过给**小时步长**和**分钟步长**输入属性赋值来配置。每个输入都有自己相应的变化事件，可以单独订阅。

# 包扎

NgDatetimePickerModule 之旅到此结束。请随意使用本文中的 StackBlitz 示例，并尝试使用这些组件。如果你需要快速复习一下这个库的公共 API，你可以随时查看我们的文档。

# 我们小组正在寻找赏金猎人！

我们想奖励那些努力帮助开发这个项目的人进一步开发令牌！根据您的贡献，您可以获得多达 5 个开发令牌的奖励，包括发布功能请求、完成功能或修复 bug。请务必看看我们的[奖励计划 T & C](https://sq-ui.github.io/ng-sq-ui/#/bounty-program) 。

# 最后但同样重要的是…

我们要感谢所有支持我们的人，由[主演我们的回购](https://github.com/SQ-UI/ng-sq-ui)或[参与我们的项目](https://stakes.social/0x014f98F05c0BeD44B4Cf0532a93312a2135afaB8)！如果你有迫切的问题或者只是想给我们写信，请随时加入我们的[休闲频道](https://join.slack.com/t/sq-ui-kit/shared_invite/zt-6sfsgfgm-NVIG8lgR~205VjxsuSG8Gg)！