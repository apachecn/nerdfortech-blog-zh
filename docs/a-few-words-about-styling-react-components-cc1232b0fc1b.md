# 关于 React 组件样式的几点说明

> 原文：<https://medium.com/nerd-for-tech/a-few-words-about-styling-react-components-cc1232b0fc1b?source=collection_archive---------3----------------------->

![](img/4c84a624287bbcabca485f559d952ba4.png)

照片由[劳塔罗·安德烈亚尼](https://unsplash.com/@lautaroandreani?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这对我来说是一个非常重要的时刻。尽管使用 React 已经多年，但每当我开始一个新项目时，我都会考虑样式。我尝试了很多方法。最重要的是，我使用 CSS 模块和样式组件。今天我想邀请你考虑另一个同样有趣的选择。

首先，我想简要地浏览一下上面的方法，解释一下我到底不喜欢它们的什么地方，以及为什么我想出了另一个标准。

# CSS 模块

简单、清晰、直观。风格重叠没问题。

一切看起来都很好，但只是简单的小组件。在更严重的情况下，我们在创建复杂类名时会遇到一个意大利面条代码——ов。

当然，也有像类名这样的库是为了解决这个问题而设计的…

但是即使在这里，像 className={cn(…)}这样没完没了的[样板文件](https://dzone.com/articles/removing-boilerplate-code-with-lombok)也可能会令人讨厌，你可能想要一些更简洁的东西。

# 样式组件

嗯，不错……有那么一刻，我为这个发现感到非常高兴，以至于我立即用一种新的方式重写了我的一个大项目。

代码看起来更好，方法似乎更新鲜。[样式组件](https://dzone.com/articles/dynamic-styling-in-vuejs)可以从主组件文件移动到一个单独的文件，这样它们就不会引起你的注意。

但是！迟早，你需要摘下你的玫瑰色眼镜，同样的意大利面条代码，然而，用新的酱料，又开始激怒你。

更重要的是，组件的原始名称生成器(头骨中的那个)上有一个持续的负载，并且[以在虚构的别名后面隐藏 HTML 标签的真实名称](https://dzone.com/articles/a-reminder-abstraction)的形式进行了过度的抽象。

我使用这种方法已经一年多了，但是我得出的结论是，首先，我希望在它的原生 CSS 文件中编写 CSS 代码，其次，我希望组件具有来自常规 HTML 标签的常规 HTML [标记。](https://dzone.com/articles/understanding-web-standards-shadow-dom-and-custom)

# 糖果

让我们弄清楚——这是一个提议。纯粹从我的经验来看——重新发明一个轮子通常会变成一种空虚、无用的时间浪费。因此，这一次我草拟了一个版本，在进入生产准备阶段之前，我决定用这篇文章进行测试。

这个想法很简单。我们编写常规的 CSS、sass 等等，从而将这个过程变成了一种真正的乐趣:)

然后我们从样式文件中导入 HTML 标签。这些组件具有与 CSS 类名相关联的布尔属性。

由于网袋[装糖机](https://www.npmjs.com/package/candy-loader)，表演这样的把戏是可能的。

# 好曲折:)

我们有机会编写常规的 CSS 和同样常规的 HTML，唯一的区别是标签是大写的，并使用一组额外的属性进行扩展，这些属性来自于为解决这个问题而设计的库，如类名。

您可以导入任何标准的 HTML 标签。输出类名的格式与 CSS 模块相同，即不同来源的同名类不会重叠。

您可以通过访问 CSS 文件的样式来包含它们。

# 环境

candy-loader 基于 post CSS，所以您可以使用标准配置文件进行额外的设置

# 当然，还有智能感知！

[typescript-plugin-candy](https://github.com/iminside/typescript-plugin-candy) 就是为此而设计的。这个插件很容易安装和配置，允许你自动完成和类型检查。

感谢您的宝贵时间！我将很高兴看到您的意见和建议的发展。