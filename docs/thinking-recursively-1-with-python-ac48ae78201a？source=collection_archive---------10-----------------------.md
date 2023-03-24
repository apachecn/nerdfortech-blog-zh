# 递归思考#1

> 原文：<https://medium.com/nerd-for-tech/thinking-recursively-1-with-python-ac48ae78201a?source=collection_archive---------10----------------------->

递归是编程中很难理解的概念之一。尽管我们都是通过编写递归阶乘函数开始我们的编程之旅，但在此之后，我们大多数人都采用迭代方法(循环)来解决问题。连我也做过同样的事。但是在学习了 Haskell 课程后，我发现递归函数是多么迷人。所以我想和你分享一下。

我决定用 python 复制 Haskell 递归函数。我还将提供伪代码，以便任何人都可以理解其中的逻辑。

本文将向您介绍一些基本的递归函数，这些函数对于掌握递归概念非常重要，我将在以后的文章中介绍更高级的函数。

![](img/088cd71f6dd67ccf888875868a820f26.png)

在 [Unsplash](https://unsplash.com/s/photos/infinite-spiral?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上由 [Ludde Lorentz](https://unsplash.com/@luddelorentz?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄的照片

## 基础案例

递归只不过是一个调用自身的函数。但是一个函数怎么知道是时候停止调用自己了呢？那就是所谓的**基地案例**。每个递归函数都有**至少一个基础用例**。

## 我们如何知道什么是基本情况？

它们是递归函数的最小单元，我们预先知道递归函数的输出。例如

> 我们知道 0 的阶乘是 1，所以它是阶乘函数的基本情况。
> 
> 我们知道第一个斐波那契数列是 1，第二个斐波那契数列也是 1。所以它们是求第 n 个斐波那契数递归函数的基础条件。

在本文中，我们将看到如何递归地解决下面给出的问题

> 1.阶乘
> 
> 2.数字的 GCD
> 
> 3.反转一个数字
> 
> 4.以 K 为底的整数对数
> 
> 5.一个数的最大除数
> 
> 6.列表的长度

## 1.阶乘

考虑下面的递归阶乘函数，

```
>> factorial(5)
**120**
```

如果您查看函数调用是如何发生的，它将类似于

```
factorial(5)
5*factorial(4)
5*(**4*factorial(3)**)
5*(4***(3*factorial(2))**)
5*(4*(3***(2*factorial(1))**))
5*(4*(3*(2***(1*factorial(0))**))) //base-case
5*(4*(3*(2*(1***1**))))   //factorial(0) returns 1
5*(4*(3*(2*1)))
5*(4*(3*2))
5*(4*6)
5* 24
120
```

如果我不指定基本情况，函数会继续调用自己，这将导致堆栈溢出。

```
#if base case isn't specified
5*(4*(3*(2*(1***(0*factorial(-1))**))))
5*(4*(3*(2*(1*(0*(-1***factorial(-2)))**))))
...
...
```

## **2。计算 GCD**

你知道我们如何递归计算一个数的 gcd 吗？这很简单

```
FUNCTION INTEGER GCD (INTEGER NUM1, INTEGER NUM2)
        if NUM1==0 
            RETURN NUM2
        else
            RETURN GCD(NUM2, NUM2 MOD NUM1)
```

gcd 的 python 代码

```
gcd(20,25)  
gcd(25%20,20) => gcd(5,20)  
gcd(20%5,5)  =>  gcd(0,5)
**5 is returned  since basecase is met (num1=0)**
```

## 3.反转一个数字

给定一个数字“1234 ”,我们的目标是将它反转为“4321 ”,我们如何递归地做到这一点？

```
//PSEUDOCODE
FUNCTION INTEGER **REVERSENUMBER**(INTEGER N)
     return REVERSENUMBER_UTIL(N,0)FUNCTION INTEGER **REVERSENUMBER_UTIL**(INTEGER N, INTEGER RES)
     IF N==0
         RETURN RES 
     ELSE
         INTEGER REM = N MOD 10
         INTEGER QUOTIENT = N DIV 10
         RETURN REVERSENUMBER_UTIL(QUOTIENT, RES*10 + REM ))
```

如果你看伪代码，我把 reverse()函数分成了`**reverse(n)**`和`**reverseUtil(n,res)**` 这在递归函数中很常见。在`reverse()`函数(用户调用的函数)中，我们可以进行配置更改，比如设置默认值，做一些验证检查等，然后从函数中调用实用函数，这是真正的递归函数。

*   我将默认值作为 **0 传递给参数‘RES’。**以便用户可以用 `reverse(n)`而不是`reverse(n,0)`调用该函数(我们在这里隐藏了实现细节)
*   **验证检查** —如果我们传递负值，我们的函数将失败。所以在调用实际的递归函数之前。如果传递的值是一个负数，我们可以抛出一个错误，或者我们可以把它变成一个正数，然后把它传递给实用函数。

```
FUNCTION INTEGER **REVERSENUMBER**(INTEGER N)
    IF N>=0
        return REVERSENUMBER_UTIL(N,0)
    ELSE
        return **-1 * REVERSENUMBER(-N)**
```

Python 代码

在 python 中，我们有一个叫做默认参数的概念。如果没有为该参数指定值，将使用其默认值。所以你可以用

```
>>> reverse(1234)   
4321
```

让我们看看这些功能是如何在幕后分解的，

```
1.**reverse(1234)**2.reverse(1234/10, 0*10+ 1234%10) 
  reverse(123, 0+4)
  **reverse(123, 4)**3.reverse(123/10, 4*10 + 123%10)
  reverse(12, 4*10 + 3)
  **reverse(12,43)**4.reverse(12/10, 43*10 + 12%10)
  reverse(1, 430+2)
  **reverse(1,432)**5.reverse(1/10,432*10 + 1%10)
  reverse(0, 4320 + 1)
  reverse(0,4321)6\. Base case hit (n=0)
   so, return 4321 
```

## 4.计算以 k 为底的整数对数

> *一个数的对数是该数的底数的幂。*

例如，Logₖ(N) = x。该表达式表示“ **k”的 x 次方等于 n。**

```
log₂(8) = 3
2³ will give 8
```

例如，在整数 Log 中，我们将对结果取整

> log₂(9) = **3.169925**

我们填充 floor 并返回结果 3

```
FUNCTION INTEGER_LOG(INTEGER N, INTEGER BASE) IF N<BASE OR BASE<=1:
              RETURN 0 ELSE
            RETURN 1 + INTEGER_LOG(N DIV 10, BASE)
```

基本条件是

1.  `N<Base`这意味着如果数字小于`base` ，则返回 0，因为`base` 的任意次幂(≥1)将总是产生大于 n 的值。例如 logₙ(1),logₙ(2),…logₙ(n-1)将总是产生 0。
2.  `Base≤1`这是非常明显的，对于任何 k≤1，logₖ(n) =未定义。

```
intLog(20,2)
= **1+intLog(10,2)**
= 1+**(1+intLog(5,2))**
= 1+(1+**(1+(intLog(2,2))**))
= 1+(1+(1+**(1+(intLog(1,2))**))
= 1+(1+(1+(1+**0**))))  #base-case hit, n<base, 1<2
= 4
```

## 5.最大除数

给定一个数，求该数的最大除数。

> 例如
> 
> 50 的最大除数是 25
> 
> 25 的最大除数是 5
> 
> 17 的最大除数是 1

伪代码是

```
FUNCTION INTEGER **LARGESTDIVISOR**(INTEGER N)
     IF N>=0 
         RETURN LARGESTDIVISOR_UTIL(N, N DIV 2)
     ELSE
        RETURN LARGESTDIVISOR(-N)
-----------------------------------------------------
FUNCTION INTEGER **LARGESTDIVISOR_UTIL**(INTEGER num, INTEGER divisor)
     IF (divisor==1) OR ((num MOD divisor)==0)
         RETURN B  
     ELSE
         RETURN **LARGESTDIVISOR_UTIL**(num,divisor-1)
```

1.  如果 N≥0，我们调用 N 和 N/2 的效用函数。N/2 的原因是，它是最大除数的上界，意味着任何数的最大除数是该数的一半`n`例如

```
Divisor of 30 is  2,3,5,6,10,15.
Divisor of 15 is  3,5\. 
```

因为对于 N/2 之后的任意一个数 k，`k*2>N`所以在 N/2 之后搜索只是浪费计算，因为总是会失败。

2.如果 N<0，我们将负数转换为正数，并再次传递给同一个函数。例如

```
LARGESTDIVISOR(**-5**) WILL CALL **LARGESTDIVISOR(5)**
```

Util 函数背后的逻辑是我们正在尝试从`N/2 … 1`开始的所有数字，所以第一个除数应该是最大的除数。

在 Util 函数中，基本条件是

1.  如果`divisor`参数达到 1，这意味着它不能被任何其他数整除，所以它是一个质数。任何素数的约数都是 **1 &本身**。所以最大的除数是 1。(我们不能把数字本身作为它的最大除数)
2.  当' **num '对' divisor '取模为 0** 时，这意味着`divisor`变量是该数字的一个除数。然后我们就可以返回`divisor`了。

python 代码

```
largestDivisor(15) 
= largest_div_util(15,15/2)
largest_div_util(15,7)
largest_div_util(15,6)
**largest_div_util(15,5)**  #base-condition met: a%b==0 so return 5
5
```

## 6.列表的长度

你有没有想过我们如何递归地找到一个列表的长度？让我们看看

```
FUNCTION INTEGER LENGTH(LIST L)
     IF L.ISEMPTY() 
           RETURN 0
     ELSE
          RETURN 1 + LENGTH[1..n] 
```

基本条件:我们知道空列表的长度是 0

逻辑:非空列表的长度是，1 加上除第一个元素之外的其余元素的长度**。我们称之为列表的**尾部**。**

```
tail of [1,2,3,4] is [2,3,4] 
```

```
length([1,2,3,4,5])
=1+length([2,3,4,5])
=1+(**1+ length([3,4,5]))**
=1+(1+**(1+length([4,5]))**)
=1+(1+(1+**(1+length([5]))**))
=1+(1+(1+(1+**(1+length([]))**)))
=1+(1+(1+(1+(1+**0**))))  #base condition: length([]) = 0
**output: 5**
```

迭代方法比递归方法更优化，但是作为一个程序员，我们都需要体验递归的魔力，因为这个世界正在慢慢地从面向对象转向函数式编程，学习递归地解决一个程序肯定会使你成为一个更好的函数式程序员。

感谢您阅读这篇文章，我们已经看到了一些初级递归函数，在我的下一篇文章中，我们将探索一些更复杂的递归函数，我觉得它们非常有用和优雅。拍手声👏如果你喜欢这篇文章。