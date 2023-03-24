# justill——探索 Purescript 模块#1

> 原文：<https://medium.com/nerd-for-tech/justifill-exploring-purescript-modules-1-c565e73eec1?source=collection_archive---------3----------------------->

我已经决定开始写我觉得有趣的 Purescript 模块，这是系列***# exploring-PS-modules***的第一篇文章。

我们的前端团队在 Purescript 上广泛工作，我们的应用程序做了许多 api 调用，请求负载中的大多数字段都是可选的，你可以在任何地方找到`Maybe`字段。也许字段是好的，它们通过传递`Nothing`让我们忽略非强制字段，但是假设你有 10 个字段，其中 8 个是`Maybe`字段，有时你想传递`Nothing`给所有的 8 个字段。*叹气！！*

这让我们想到，我们能不能只传递那些必要的字段，而所有没有被传递的可能字段都被默认设置为`Nothing`，以一种类型安全的方式，不给编译器抱怨的机会。

![](img/dfc1669ec24998f2080c08ed0811dcb8.png)

圣地亚哥·拉卡尔塔在 [Unsplash](https://unsplash.com/s/photos/fill?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

就在那时，我们发现了名为`[JustiFill](https://pursuit.purescript.org/packages/purescript-justifill/0.5.0/docs/Justifill#v:justifill)`的软件包，它可以让我们做我们想做的事情。

*快速举例*，

```
type Person = { name :: String
              , age :: Int
              , salary :: Maybe Int
              , children :: Maybe Int
              }student :: Person
student = justifill {name : "Mike", age : 14}--- > student 
--- > { age: 14, name: "Mike", salary: Nothing, children: Nothing }
```

你可以从上面的例子中看到，我们只将强制字段传递给函数`justifill`，它用`Nothing`填充可能字段。看，这有多有用！！

有时候，我们希望将一些可能字段 与强制字段一起传递，我们可以用这个库来实现吗？绝对是的。

```
freshGrad :: Person
freshGrad = justifill { name : "Steve"
                      , age : 21
                      , salary: Just 200
                      --  see i'm skipping the `children` field
                      }
```

它还允许我们在不使用`Just`构造函数的情况下填充字段。

```
familyMan :: Person
familyMan = justifill { name : "Jim"
                      , age : 21
                      , salary: Just 2000
                      , children : 1 -- we skipped Just 
                      }
```

这个库通过减少大量的 boiler plate 代码使你的代码更加整洁。

希望你也将开始在你的项目中使用它，如果你也觉得这个库有趣的话，和你的同事一起 share🗣这篇文章。