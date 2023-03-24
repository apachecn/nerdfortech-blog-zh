# 使用对象析构和扩展语法替换 lodash 省略

> 原文：<https://medium.com/nerd-for-tech/replacing-lodash-omit-using-object-restructuring-and-the-spread-syntax-d7af1607a390?source=collection_archive---------25----------------------->

快速替代您的方便省略

![](img/41d4f66ddb7d396fc79e3e8bd1f28e61.png)

照片由[昆吉·帕瑞克](https://unsplash.com/@kunjparekh?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/throw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

是一个非常方便的函数，可以让你创建一个从对象中“排除”属性的对象，而不是显式地“包含”所有其他属性。但是它现在在 Lodash 5 中被弃用了，而且可能会变得更好。要深入探究它被删除的原因，我建议看看邓普西的文章。

[](https://levelup.gitconnected.com/omit-is-being-removed-in-lodash-5-c1db1de61eaf) [## Lodash 5 中删除了省略

### 现在你可以使用一些替代方案来保持领先一步

levelup.gitconnected.com](https://levelup.gitconnected.com/omit-is-being-removed-in-lodash-5-c1db1de61eaf) 

这里总结一下:性能，性能，性能。例如，语义 UI React 团队注意到渲染时间提高了 12000 倍(是的，你数对了，12000 倍)。

由于对象析构和 spread 语法，我在这里列出了一个快速的替代方法。假设我们想从列表中排除`apple`，使用`omit`，我们可能会有:

但是即使不添加库，我们也可以用这个来代替:

我强烈建议您检查使用 lodash 中的 omit 的代码。这可能会省去升级依赖项的麻烦，甚至会提高代码性能。