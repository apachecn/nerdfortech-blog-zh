# 带有 Vue.js 和 GO 的社交应用

> 原文：<https://medium.com/nerd-for-tech/social-application-with-vue-js-and-go-24a1d1e7137d?source=collection_archive---------6----------------------->

![](img/4cdfb643ef90df60dcd8aaa1429ba169.png)

标志归功于 vuejs.org 和 golang.org

## 用 vue.js 和 golang 创建和服务一个类似 twitter 的应用程序第 3 部分:组件和插槽

这是本系列的第三部分。在这里检查所有零件:

*   [第一部分:设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4e4db0cdde64)
*   [第二部分:VUE 入门](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64b3adee8dac)
*   第 3 部分:组件和插槽(本)
*   [第四部分:Vuex 首次设置](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-3a11d506fc38)
*   [第 5 部分:VUEX 终结](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-ef364b572422)
*   [第 6 部分:Vuex 中的表格和数据](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-a22a1afb76eb)
*   [第七部分:与 golang 服务器的连接](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-d9e563466b66)
*   [第 8 部分:基于令牌的认证](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-64978f7c381f)
*   [第 9 部分:存储索引为 DB 的认证令牌](https://ivano-dalmasso.medium.com/social-application-with-vue-js-and-go-4d0caa37ddac)

在这一课中，我们将添加一些前端功能，通过这样做，我们将学习更多关于使用组件以及如何用插槽扩展它们的知识。

这里的主要目标是更新前端项目，你可以在这里找到代码。

## 重用帖子的组件

在上一课中，我们创建了一个列表，从中可以看到用户的帖子。现在，这个视图不是很好，所以这里我们要添加 css 来使它好一点。

此外，我们将创建一个 card 组件，我们将在应用程序的其他地方再次使用它，这样用户的总体感觉将始终相同。

在 UI 文件夹中新建一个组件，命名为“BaseCard.vue”。在里面写下下面的代码:

样式标签实际上填充了最少的代码，让应用程序有一个更好的外观，但当然这是为了演示的目的。

让我们简单解释一下 scripts 标签:实际上，有一个称为“可扩展”的属性将从外部传递给这个组件。请注意，这一次 prop 的声明是以不同的方式进行的:事实上，props 参数接受一个数组或一个 javascript 对象，在第二种情况下，我们可以将 props 作为对象传递，这样我们可以更好地描述 prop 应该具有的属性。

在本例中，我们已经为“可扩展”属性设置了类型 Boolean 和默认值 false。

data 属性只包含一个值，该值指示卡片实际上是展开的还是未展开的，我们有一个方法“toggleDetails”来切换卡片的展开。

还有一个配置选项是我们从来没有见过的:“计算”。

这是一个包含实际上可以用作数据值的函数的对象。所以我们可以使用它们(也在模板中),就像它们是数据值一样(不需要括号),它们也有反应行为。

事实上，vue 内部知道某个计算值的依赖关系是否发生了变化，并且只在需要时才重新评估计算方法。在我们的代码中，只有当“this.expanded”更改时，messageValue 计算属性才会自动重新计算。

请注意，在方法和计算属性中，我们可以使用关键字“this”来使用任何其他数据、方法或计算属性，就像我们在一个类的对象中一样。

最后，模板:这里添加了两个新东西，v-if 和插槽。

v-if 构造根据条件的有效性创建或删除 DOM 中的元素。这必须完全按照 v-for 来编写，所以我们也可以在这里插入一点逻辑(但通常最好创建一个计算属性或方法来完成这项工作，这样我们就可以与代码更加分离)。

在我们的示例中，只有在设置了$slots.header 变量的情况下，才会呈现头部分。

最后也是更重要的一点，是插槽的使用。实际上，在组件中使用插槽可以让开发人员从外部插入一些内容，然后扩展内部组件。

它是这样工作的:父模板中插入到子组件标记值中的所有内容，实际上都是插入到子组件中“slot”标记的位置。在只有一个插槽的情况下，这就是您需要做的全部工作。

如果你需要设置不止一个插槽，就像上面的例子一样，你可以在孩子中插入“命名插槽”,这样插槽就有了一个名称参数。

在最后一种情况下，一个槽也可以不命名，这实际上称为“默认槽”。在这种情况下，在父亲中，要被替换的值应该被放置在附加标签“template”和参数“v-slot:slotName”中，其中 slotName 是孩子的槽的名称。

同样，在这个例子中，我们需要在父节点中插入一个“<template v-slot:header=""></template>

现在，在文件 SinglePost.vue 中，我们可以更改模板，以便在实际的 SinglePost 模板中使用此组件 BaseCard:

这里我们使用了一个可以扩展的卡片，如果有一些评论的话，所以实际上 post 对象将会有一个升级来包含它们。

在页眉中，我们可以看到文章的标题，作为页脚，我们插入了另一个卡片列表，其中包含了每个卡片的评论。

为了代码的简单性，我们对评论和发布对象使用了相同的结构，因此它们可以共享一些功能(比如“postTitle”)。

下一件要做的事，去岗位。Vue 组件，并更改“posts”变量以在每个帖子中包含一些评论，即

我们没有将 BaseCard 导入到我们的组件中:因为我们可能会在我们的应用程序中重用这个“Base”组件，我们可以将它导入到 main.js 中的初始 Vue 对象中，然后在 Vue 全局实例的“components”配置参数中设置它。

## 更改“关于”页面

只是为了完成更多的应用程序，还更改了 User.vue 中的“About.vue”组件，这将显示用户实际登录和他的帖子。

这实际上是不言自明的:页面是一个简单的页面，标题由用户的用户名组成，在显示他的所有帖子之前会显示一些描述。实际上，数据仍然是一些虚拟的对象块，我们稍后会解决这个问题。

在后列表中设置“title”参数，需要在子组件的 props 配置参数中也设置该值。此外，在删除了“About”组件之后，我们还必须更新路由器内部的 index.js 文件，以指向这个新组件。

在这一点上，有必要对 vue 路由器做一点解释:它实际上是一个组件，可以让您组织、维护和纠正应用程序中不同页面之间的路由。在 PWA 中，页面不是每次都从服务器加载，但是这种行为实际上是由代码模拟的，这就是路由器实际做的事情。

首先要做的是检查 main.js 文件中的路由器是否确实被导入并注册到 vue 应用程序中。

路由器允许应用程序拥有不同的页面，并且只在被调用时显示它们。此外，它还与浏览器的“后退”和“前进”按钮进行交互，这样，对于用户来说，就好像调用并显示了多个页面，而实际上，脚本只更改了页面中被路由的部分。

正如我们之前看到的，与 vue-router 交互的两个基本标签是<router-link>和<router-view>:第一个是一个实际的链接(它实际上是一个扩展了标签的组件)，需要一个“to”参数，也可以是一个 javascript 对象，指向要显示的页面。另一个是占位符，告诉路由器应该在哪里注入路由视图。</router-view></router-link>

查看路由器文件夹中的 index.js 文件，我们还可以看到路由器的配置:

```
Vue.use(VueRouter);
```

第一行指示 Vue 使用 VueRouter。在路由器文件的末尾写下:

本质上，它创建一个 VueRouter 并将其导出，使用模式“history”将应用程序的基本 url 用作基本 url，因此它将使用 history pushstate API 来实现 url 导航(更多[此处为](https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations))，然后在构造器处传递它将必须使用的路由列表。

此路由列表在此构造之前的行中定义:

这是定义可从路由器调用的实际路由的对象列表。在每个对象中都设置了 url 的路径(因此，在本例中，第一个路由将服务于根目录，而第二个路由将服务于“/user”路径)、名称以及将用于在该路由上呈现路由器视图的组件。

名称通常在路由器链接中使用，以引用链接的路由，将其与 url 解耦(我们以前做过，在应用程序栏中，router-links 标记有一个 to 参数，它指向一个带有名称参数集的对象，还记得吗？)

最后，我们有了 props 配置参数，在最后一个路由中设置为 true:通过它，我们指示 vue 路由器，路由器链接可以将一些 props 传递给组件。这很重要，因为路由器的默认行为是不传递这些属性。

将此设置为 true 后，将来我们可以将对用户页面的调用更改为不同的用户。

这样，我们在 vue 路由器上做了一点回归，如果您现在启动前端，应用程序看起来会更好，数据仍然是静态的，用户无法更改，但我们将在接下来的几节课中讨论。