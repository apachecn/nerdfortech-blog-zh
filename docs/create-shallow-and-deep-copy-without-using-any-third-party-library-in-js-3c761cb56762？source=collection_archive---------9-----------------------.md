# 不使用 JS 中的任何第三方库创建浅层和深层副本

> 原文：<https://medium.com/nerd-for-tech/create-shallow-and-deep-copy-without-using-any-third-party-library-in-js-3c761cb56762?source=collection_archive---------9----------------------->

![](img/b209acc4fc01230bafa964662234a2ba.png)

浅层和深层复制

> 复制品只是看起来像旧的东西，但不是。当您更改副本时，您希望原始内容保持不变，而副本却发生了变化。

JS 中的副本有两种**浅层副本**和**深层副本。**让我们先了解一下它是如何工作的

1.  **浅拷贝** 它将创建一个新对象，其值与原对象中的值完全相同。如果原始对象中的任何字段是引用，它将复制该字段的引用地址或内存地址，并且不会创建新对象。浅层副本仅复制对象的顶级属性。
2.  **深度复制** 它将创建一个新对象，其值与原对象中的值完全相同，但与原对象完全断开。对于引用属性，它将分配一个新的内存位置。

# **创建浅层拷贝的方法**

*   **传播算子**

```
let copiedObject = {...originalObject};
```

*   **Object.assign()**

```
let copiedObject = Object.assign({}, originalObject);
```

# 创建深层副本的方法

*   **用 JSON**

```
let copiedObject = JSON.parse(JSON.stringify(originalObject));
```

创建深度副本似乎非常容易，对吗？但是这种方法不靠谱。
如果源对象不是 *JSON 安全的*例如，如果它包含日期、未定义、无穷大、函数、正则表达式、映射、集合或其他复杂类型，那么结果会丢失一些数据。让我们来看看

那有什么靠谱的办法呢？我们可以创建自己的 javascript 方法来深度克隆对象。

这个方法几乎涵盖了你需要的一切。许多第三方库将帮助您创建浅层和深层副本，例如`lodash, underscore.js, rfdc`等。要了解不同第三方库的更多细节，请阅读德里克·奥斯汀博士的文章。

> 在评论中分享你的想法