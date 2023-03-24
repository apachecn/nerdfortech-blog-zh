# 为 UUID 编写简单的函数，而不是在 ReactJS 中安装包！！！

> 原文：<https://medium.com/nerd-for-tech/write-simple-function-for-uuid-instead-of-install-package-in-reactjs-29619e46d99c?source=collection_archive---------12----------------------->

![](img/77b45c3a89d183f7af5c868d3bc2a20f.png)

保罗·埃施-洛朗在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

一般来说，当我们安装软件包到我们的项目时，我们不会三思而行，这导致了我们在构建 ReactJS 应用程序时的高成本。我们总是考虑减少构建的规模，以便我们的项目可以尽快加载到虚拟 DOM 中。

我有一个例子，可以节省一些你的应用程序的大小💯。让我们来看看这个

## 生成 UUID 而不是使用 [UUID 包](https://www.npmjs.com/package/uuid)

```
**uuidv4 = () => {**
    return "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(
    /[xy]/g,
     function (c) {
       var r = (Math.random() * 16) | 0,
        v = c == "x" ? r : (r & 0x3) | 0x8;
      return v.toString(16);
     }
  );
**};**
```

这是一个简单的程序，将为您生成一个唯一的密钥。使用时你只需要在变量前面调用这个函数。让我们举个例子:

```
Class Component :this.state = {
key : this.**uuidv4,** }***// the same is the case with functional but you don't have to write this before this function.*** 
```

试试这个，如果它能在你的项目中节省一些空间的话，留下一个掌声。更多功能评论。 **❤🙌**

***快乐编码:)***