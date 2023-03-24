# 机器学习的概念|使用梯度下降法寻找系数的最佳值的优化😉

> 原文：<https://medium.com/nerd-for-tech/gradient-descend-method-d6e2a9d906be?source=collection_archive---------11----------------------->

## 在这篇文章中，你将学习最大值，最小值的概念，凹凸函数，使用爬山法寻找最大值最小值，梯度下降算法。

机器学习的概念|使用梯度下降法寻找系数的最佳值的优化😉

# 梯度下降法

梯度下降是机器/深度学习算法中使用的优化算法，使用迭代来最小化目标凸函数 f(x)。它寻找目标函数的全局最小值。

![](img/e61d87c195253fe101574ee39b699cd1.png)

用梯度下降法寻找最优解

在开始之前，你必须知道下面的数学概念。

## 凹凸函数

![](img/2f83a01623c0489faa2c3f04948e5a20.png)

**凹面功能**

在凹函数中，连接曲线任意两点的直线总是位于曲线下方。

![](img/0142e2bd601b7fd84127ac55f6a2b37a.png)

**凸函数**

在凸函数中，连接曲线任意两点的直线总是位于曲线的上方。

![](img/9476d68182032e70efa462bf5eb2b7c7.png)

**不凹不凸**

![](img/aec01da0ec2180b4e0f6cc897af00780.png)

**既不凹也不凸**

有一些曲线，其中连接曲线上任意两点的直线可能位于上方，可能位于下方，可能位于两者都有，也可能没有。

## 最大值和最小值

![](img/2814a7a4cbe6b0c42313a0f0f0282e98.png)

**千里马**

如果最大值存在于 a 点，那么对于 a>a+e 和 a>a-e，其中 e 极小。

![](img/d1fa1cfdcaa04e71a089e76e2139b3b8.png)

**最小值**

如果最小值存在于 a 点，那么对于 a 点

## Finding Max/Min by hill climbing :

![](img/83a177c0fd1dfc5c0dc448100f1b2134.png)

**通过爬山**找到最大值/最小值

## n 的选择:

![](img/da057b76fbbf398270fca57498033ea4.png)

如果 n 太小，下降速度会变慢。如果 n 太大，克服最大值或最小值。

![](img/16005bcecb201cce45e6206934d19dbf.png)

**n 的常用选择**

这是正确的选择。

## 收敛标准:

![](img/596ee95a86a04979eeb1b6b31a78ddbf.png)

**收敛标准**

其中 E(ε)是要设置的阈值。

![](img/07bcc7d3dccbe3cdfd565c6759fb5e87.png)

可视化梯度下降

## 机器学习中的梯度下降；

![](img/a1f0122e29b69c81fabea0c392f50e70.png)

B0、B1 与 J 的关系

![](img/624a433deaa28ada3d9e4c9cb0fe58e4.png)

上图的俯视图

![](img/2f72184379aed2cadc0d3dedb6b61a66.png)

对于两个不同的点 J1 和 J2 画出不同的图。

## 梯度下降算法

![](img/d4aaef8dbbd21b7789211a1e083b0fea.png)

梯度下降算法

## 哪个是正确的？

## 方法 1:

![](img/5e8bdb4e2cd2883cae10aec14449da53.png)

**方法二:**

![](img/f04c5aa223cbccdfa2285b607034d436.png)

方法 2 不对。因为当我们找到 B1 时，B0 的值已经改变了，所以 J(B0，B1)已经改变了。所以方法 1 是正确的。

## RSS 的梯度

![](img/971a364f0799760f4d06ed60f591c33c.png)

**方法 1 :**

RSS 的梯度= 0

![](img/30ddc862dd805c2f409d51a902f81e66.png)

RSS 的梯度= 0

**方法二:**

![](img/4efc8d89c6dbdd13987c979888c31817.png)

用这种方法我们计算梯度下降。

对于大多数机器学习问题，不可能计算出梯度=0。对于那些我们必须使用第二种方法。

## **重要的**

***特征比例:*** 确保特征比例相似。如果我们不缩放数据，水平曲线(等高线)会更窄更高，这意味着需要更长的时间来收敛

![](img/93c45b8f9190cc2e3604dfa15ca31f71.png)

标准化与非标准化曲线上的梯度下降

***表示归一化:*** 为了归一化数据，我们必须用(xᵢ — uᵢ)代替 xᵢ，并将其除以特征范围(最大值—最小值)，其中 uᵢ是平均值。xᵢ在训练集中的价值。

![](img/cec7e3526fb749a7ebe058769e14c025.png)

均值归一化

***确保梯度下降有效:*** J(B)在每次迭代后减少。

![](img/a71fa69d8157aba4a1a3d70cf99f05ba.png)

***确保梯度下降正在工作***

## 全文系列:

[](https://ujjwalkar.netlify.app/post/concept-of-machine-learning-tutorial-series/) [## 机器学习的概念文章系列| Ujjwal Kar

### 回归入门|使用梯度下降的简单线性回归优化…

ujjwalkar.netlify.app](https://ujjwalkar.netlify.app/post/concept-of-machine-learning-tutorial-series/)