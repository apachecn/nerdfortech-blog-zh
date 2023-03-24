# 调查指南反应 JS[DOM，虚拟 DOM 和 JSX]第五部分

> 原文：<https://medium.com/nerd-for-tech/an-investigative-guide-to-react-js-dom-virtual-dom-and-jsx-part-v-ca095cabe41c?source=collection_archive---------1----------------------->

部分亮点— [四](https://www.pansofarjun.com/post/an-investigative-guide-to-react-js-dom-virtual-dom-and-jsx-part-iv)

*   每次在真实的 DOM 树中发生新的重画，都会带来巨大的性能代价。这是因为涉及到更多的计算。
*   浏览器会识别回流和重绘，以优化流程
*   React 在浏览器前增加了一个额外的层。这个额外的层被称为虚拟 DOM。
*   当任何东西被更新时，react 都会创建新的虚拟 DOM。这反过来与旧的虚拟 DOM 相比。这个过程叫做**差**。
*   DOM 中的所有更改都被发送到队列中，然后对这个队列进行批处理。
*   这些批量更改作为对原始 DOM 的单个更新发送。这使得它成为**批量更新**。正是这种批量更新使它变得更容易并优化了性能。

![](img/9f75e0715b15919ad234d3d4f6e68a48.png)

# 从 JS 角度理解虚拟 DOM

我们知道 DOM 有一个树形数据结构。在 Javascript 中，树由 **javascript 对象表示。由于 JS 是一个真实的 DOM 操纵器，我们可以将它用于虚拟 DOM 操纵器。就像我们通过 DOM 查询操纵真实 DOM 一样，我们通过 **React DOM 查询操纵虚拟 DOM。**就像 DOM 是由 DOM 元素组成的一样，React/Virtual DOM 也是由 React 元素组成的。**

*注:以上解释是为了更容易理解。事实上，DOM 元素和 React 元素是不一样的。实际上，react 元素是创建浏览器/真实 DOM 的指令。*

## 创建 React 元素

> *React 使用 React.createElement 创建元素*

如果你想在浏览器 DOM 中有一个 h1 元素，我们应该首先在 react DOM 中创建。

因为 react 负责浏览器 DOM，所以我们不需要负责浏览器 DOM。我们将只关心 **React 元素。**

React.createElement

(

**h1**’，

{

**id:“博客**”

},

**潘索法君**’，

)

上面的方法(createElement)在实际的 DOM 中创建了*<h1 id = ' blog '>PansofArjun</h1>*

如果你观察，在上面的 createElement 方法中有三个参数。

第一个——我们想要创建的元素类型，即 **h1**

第二个—元素属性，即 **id:“博客”**

第三个——元素子元素——在开始和结束标记之间插入的任何节点，即 **PansofArjun**

假设如果您想要控制台记录这个 react 元素，我们需要创建一个变量来存储这个元素，因为我们不能直接控制台记录 React.createElement。

因此，

const element = react . createelement

(

**h1** '，

{

**id:“博客**”

},

**PansofArjun**’，

)

如果你是控制台日志，你会得到一个 JS 对象

{

**类型**:‘h1’，

**键:**空、

**ref** :空，

**道具** :{

**id** :“博客”

**子女**:‘PansofArjun’

},

**_ 所有者**:空，

**_ 商店** :{}

}

对象关键字是类型、关键字、引用、属性、_ 所有者和 _ 商店。我们现在将学习'**类型'**和'**道具'**。

> *类型——我们想要在真实的 DOM 中创建什么类型的元素*
> 
> *props——构建真实 DOM 所需的数据和子对象*

## 道具魔术，孩子们！

在上面的例子中， **props.children** 被设置为' **PansofArjun** '这是一个文本节点。我们也可以将它设置为另一个元素节点，如列表。

React.createElement

(

*ul*’，

空，

*React.createElement('li '，null，' one ')，*

*React.createElement('li '，null，' two ')，*

*React.createElement('李'，null，'三')*，

)

如果我们像前面一样控制台记录这个

{

类型:“ul”

道具:{

**孩子:[{**

***类型:【李】，道具:【儿童:】【一】等，***

***类型:【李】，道具:【儿童:【二等】，*，**

***类型:【李】，道具:【儿童:【三】等】***

**}]**

等等

}

}

注意:它包括密钥、ref、所有者和商店。

我们说‘道具’包括数据和子对象来构造真正的 DOM。但是等等！这里的数据在哪里？就是**道具.儿童**键——*一*、*二*和*三的值。*

我们可以像 *const arr = [one，two，three]一样将数据存储为数组。*

React.createElement(

ul '，

空，

***arr . map(data =>react . createelement(' Li '，null，data))***

)

我们上面所做的是将数据从 UI 元素中分离出来，即 ***我们存储数据分离变量(arr)并使用 javascript 逻辑(映射函数)*** 将它们 ***集成到 UI 中***

将数据与 UI 分离是使用 React 的主要优势之一。

从上面我们可以很舒服的说' **React 是由 React 元素**组成的，这一点从 props.children 就不言而喻了！

*原载于 2022 年 3 月 15 日 https://www.pansofarjun.com**的* [*。*](https://www.pansofarjun.com/post/an-investigative-guide-to-react-js-dom-virtual-dom-and-jsx-part-v)