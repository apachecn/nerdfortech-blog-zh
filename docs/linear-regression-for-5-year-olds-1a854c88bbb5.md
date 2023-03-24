# 5 岁儿童的线性回归

> 原文：<https://medium.com/nerd-for-tech/linear-regression-for-5-year-olds-1a854c88bbb5?source=collection_archive---------16----------------------->

## 第 2/3 部分:算法背后的数学

> 第 1/3 部分的链接:[直觉](/@mohitpatil246/linear-regression-for-5-year-olds-2e8d83e9a680)
> 第 3/3 部分的链接:[从头开始用 Python 实现](https://mohitpatil246.medium.com/linear-regression-for-5-year-olds-1bdc5d0badc4)

![](img/79b8eedc37a84d3cbcacc9a4ff86259f.png)

照片由[前方什么都没有](https://www.pexels.com/@ian-panelo?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)从[像素](https://www.pexels.com/photo/monochrome-photo-of-math-formulas-3729557/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

大家好！这是面向 5 岁儿童的线性回归的第二部分，涵盖了线性回归背后的数学知识，这将带您进入第三部分，从头开始讲述用 python 实现线性回归。

*这部分会有些冗长，所以请耐心等待！*

**线性回归的公式是-**

![](img/f7ef558d3cb601f1b4345347567f4d10.png)![](img/6e5f272b17e091ce945e82f61ee94033.png)

# **现在，让我们试着找出系数(B)和截距(a)是如何计算的-**

1.如上所示，我们将最佳拟合线**定义为-**

![](img/04fc513fffda7c3df916939830928b9b.png)

2.对于线性回归，我们有我们的成本函数(无非是 **SSE** 即误差平方和)

![](img/0a8b21dbed7954c30a3527e959044df4.png)

3.让我们把最佳拟合线放入成本函数-

![](img/00e0ad067d76b3f8b027e9785b550b63.png)

4.为了最小化我们的成本函数， **S** ，我们必须找到 S 相对于 **a** 和**B**的导数等于或接近 0 的点， **a** 和 **B** 越接近 0，每个点的总误差越小。

# **5。找一个**

![](img/c1592764aa5562b0466374bd3711e42f.png)![](img/6168c385792b37e001522a144d381667.png)

6.类似地，在我们的等式中，我们从指数开始，然后是括号之间的等式。*(括号内方程的导数出来就是-1)。*

![](img/796cdc1ad972b1a73d297f1399595cf2.png)

7.现在，因为 2 是一个常数值，我们将把它从求和中取出，移到 L.H.S-

![](img/654540b2a6584bd8213ec86ec3a5e124.png)

8.简化，

![](img/ae514ef4a4d19551e2f52392af93ed6f.png)

可以说 **a** 到 **n** 之和为-

![](img/29ea3b2b363ef23c83e0819ae5402cfb.png)

9.将这个值代入等式-

![](img/0cfe890f034f2e4a8fdcad6178052a7e.png)

9.为了得到 **a** 的值，让我们重新排列-

![](img/282603dde261d7d32dbd317b611638e9.png)

如果你注意到， **Y** 的总和除以观察次数， **x** 的总和除以观察次数就是平均值，因此 **a** 的值-

![](img/2ce1297a47ab730ebf1d05e2117a75b1.png)

# **10。调查结果 B**

类似地，最小化关于**B**-
T5 的成本函数 **S** (这里，括号之间的等式的导数为-x)

![](img/e0e24b4b3a84a48164414256574f8053.png)

11.排除 2，

![](img/ade863f1c05f95c989882a1bd1029ef1.png)

12.分发 **x** ，

![](img/880665ce357bc9121cb24232e4ac344f.png)

13.还记得我们上面推导的 **a** 的值吗？让我们把它代入我们的方程-

![](img/c38147db75c8514c59c14a1baf2cea5e.png)

14.打开内部支架，

![](img/f637101f91ee20e1c246dd111cf1e0d3.png)

15.把总数分成两部分-

![](img/a44f0134bc52fcdce4096fdc257cc1b7.png)

16.从求和中取出常数 **B**

![](img/b256da95dd31df88bcb6bd26ab61cabd.png)

为了得到 **B** 的值，让我们重新排列-

![](img/e2781c164eeb340b761c125afa947b8d.png)

17.所以，最后一个等式-

![](img/833150ed4781b8454c8da27bb5471002.png)

18.上面的等式也可以写成-

![](img/b10891ab40f37dc3b56393dc7a49e7f4.png)![](img/7b276f9ec097a6a7ac2a96c077268886.png)![](img/04730e90e65a26598860461035f95f7d.png)

因此，这两个方程是相同的，

![](img/11946ca3caee9e7b7a7ed2bd49d2861e.png)

# 总而言之-

如果数据集只有一个独立变量，可以通过计算 **B、**找到最佳拟合线

![](img/637bebb109ff0286d136b8da2d38abbd.png)![](img/18429f7aae59063bab6d31ce367f5f14.png)

然后将 **B** 代入 **a** ，

![](img/4895a44c2b520fe4d270322c1fb1f611.png)

最后将 **B** 和 **a** 代入线性回归方程-

![](img/54eb4d144732e0997235ea0fa8846dfb.png)