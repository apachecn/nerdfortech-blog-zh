# 希望让您的角度应用性能更佳？🏋🏼‍♂️

> 原文：<https://medium.com/nerd-for-tech/looking-to-make-your-angular-application-perform-better-%EF%B8%8F-c5f2b4cf6849?source=collection_archive---------7----------------------->

## 以下是如何利用 Angular 的 trackBy 功能。并使您的 Angular 应用程序像老板一样执行。

![](img/a8479aee17dfad30a356184ad13ac578.png)

Angular 有一个很酷的东西叫做 Zone.js，它可以在 DOM 事件发生时触发变化检测。

如果你不知道的话，Zone.js 是驱动 Angular 应用程序的引擎。如果你想了解更多关于它是如何工作的，我会在本文中解释[。](https://danielk.tech/home/heres-how-you-can-improve-angular-change-detection-performance)

Angular 还有另一个很酷的功能，叫做的[。只需交给它一组信息进行渲染，并观看它呼啸而过！😏](https://angular.io/guide/built-in-directives#listing-items-with-ngfor)

我的意思是…这是一个漂亮的棱角特征…它是为了让你发出咕咕和啊啊的声音…直到你开始滥用它。然后它啪的一声，砰的一声，在你面前爆炸！🤯

[![](img/f1b6577fb74e9686aa28b76134fcd343.png)](https://school.danielk.tech/course/unleash-your-angular-testing-skills?utm_source=medium&utm_medium=banner&utm_campaign=unleash_testing_skills)

# [ngFor](https://angular.io/api/common/NgForOf) 有什么问题？

因为 Zone.js 在每次 DOM 事件发生时都会触发一次重新呈现，这意味着当一个按钮被点击时，你的列表会被重新呈现，等等…当然你永远看不到它，因为列表的数据没有改变。

如果您的组件只呈现 5 个或更少的项目，这可能没问题。但是小列表往往会变成大列表，大列表会产生性能问题，除非你是一个明智的开发人员。伙计，这就是我想要你做的——一个精明睿智的 Angular 开发者，知道如何让你的 Angular 应用程序运行起来！

> 伙计，这就是我想要你做的——一个精明睿智的 Angular 开发者，知道如何让你的 Angular 应用程序运行起来！

那么我们如何解决这个问题呢？

简单描述一下 Angular trackBy 函数，它是一个可选函数，可以与 Angular 的 ngFor 一起使用。Angular trackBy 用于定义**如何跟踪 iterable** 中某项的变化，Angular 使用指定的 trackBy 函数来跟踪函数返回值的变化。

伙计，你还在吗？

太好了！不如我们深入研究一些代码。创建我们自己的 trackBy 函数需要什么？并且避免昂贵的重新渲染操作？

下面是角度跟踪功能的一个基本示例。第一步是将 trackBy 函数添加到我们的 components Typescript 文件中，如下所示。

```
trackByItems(index: number, item: Item): number { 
    return item.id; 
}
```

然后，我们将修改我们的 NGF 来使用新的 trackBy 函数。

```
<ul>
    <li *ngFor="let item of items; index as i; trackBy: trackByFn">
     {{ item.value }}
    </li>
</ul>
```

那么我们刚才做了什么？

基本上，我们已经告诉 Angular2 框架如何通过给它一个返回 iterable 中每一项的唯一 ID 的函数，以更高效的方式处理列表数据。通过使用这个函数，Angular 将知道哪些列表项需要重新呈现，而不必拆除整个 DOM 并重新构建它。

# 结论

我的朋友，这就是你如何使用 Angular 的 trackBy 函数特性来提高 Angular 应用程序的性能。

如果你喜欢这篇文章，并发现它很有用，请一定要点击它👏按钮，关注我，获取更多类似本文的精彩文章。

**关注我:** [GitHub](https://github.com/dkreider) ，[中型](https://dkreider.medium.com/)，[个人博客](https://danielk.tech)

[![](img/f1b6577fb74e9686aa28b76134fcd343.png)](https://school.danielk.tech/course/unleash-your-angular-testing-skills?utm_source=medium&utm_medium=banner&utm_campaign=unleash_testing_skills)![](img/608657ab1474ad59ca16f674da332a53.png)

*最初发布于*[*https://danielk . tech*](https://danielk.tech/home/use-trackby-to-improve-angular-performance)*。*