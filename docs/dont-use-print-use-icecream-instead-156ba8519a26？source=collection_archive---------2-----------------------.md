# 不要用 print()，用冰淇淋代替

> 原文：<https://medium.com/nerd-for-tech/dont-use-print-use-icecream-instead-156ba8519a26?source=collection_archive---------2----------------------->

![](img/de08d22bdd295b54366f9b1f24175050.png)

[Zan](https://unsplash.com/@zanilic?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

大家好，

> 调试一个程序需要很多时间和很长的 print()语句。

你曾经用冰淇淋调试过 python 程序吗？

*调试是程序的重要组成部分。高效地做这件事可以节省时间，提高生产率。*

> 你也可以在我的 YouTube 上观看我关于如何有效调试你的 python 程序的视频，以及我的 [GitHub](https://github.com/varchasa/YouTube-Projects/) 库上的全部代码。

现在，让我们看看它是如何工作的。

## 首先，您需要安装所需的库，即冰淇淋。

```
> pip install icecream
```

## 让我们继续讨论如何使用它。

> 我会推荐你使用 Jupyter 笔记本来查看专用结果

*   简单使用

```
a = 8 
b = ic(a)> ic| a: 8
```

*   功能实现

```
a = 8
**def** half(i):
    **return** i/2
b = ic(half(a))> ic| half(a): 4.0
```

*   列表实现

```
l = [1,2,3,4,5]
b = ic(l)> ic| l: [1, 2, 3, 4, 5]
```

*   启用和禁用变量

```
ic.disable()
ic(20)
ic.enable()
ic(30)> ic| 30
```

*   程序实现——毕达哥拉斯定理

```
**import** **math** 
n = int(input("Enter a number"))
**if**(n<=2):
    print("No triplets exist")
**else**:
    **for** i **in** range(2,n):
        **for** j **in** range(1,i):
            k = math.sqrt(i*i + j*j)
            **if**(k%1==0 **and** k<=n):
                k = int(k)
                a = ic(i,j,k)
```

我们把 n 的值取为 30。那么输出将会是—

```
ic| i: 4, j: 3, k: 5
ic| i: 8, j: 6, k: 10
ic| i: 12, j: 5, k: 13
ic| i: 12, j: 9, k: 15
ic| i: 15, j: 8, k: 17
ic| i: 16, j: 12, k: 20
ic| i: 20, j: 15, k: 25
ic| i: 21, j: 20, k: 29
ic| i: 24, j: 7, k: 25
ic| i: 24, j: 10, k: 26
ic| i: 24, j: 18, k: 30
```

忠太😊，现在您可以看到，在没有使用 print()函数的情况下，它打印变量和值。

💥我的 GitHub 库上的全部代码。

[](https://github.com/varchasa/YouTube-Projects/) [## GitHub-varchasa/YouTube-Projects:我的所有 YouTube 内容及其代码

### 我所有的 YouTube 内容及其代码。通过在…上创建一个帐户，为 varchasa/YouTube 项目开发做出贡献

github.com](https://github.com/varchasa/YouTube-Projects/) 

感谢您阅读这篇文章🙌。请鼓掌👏，如果你觉得我的文章有用。

瓦尔恰萨·阿加尔瓦尔