# 黑客马拉松实践和算法:分解一些令人生畏的编码部分

> 原文：<https://medium.com/nerd-for-tech/hackathon-practice-and-algorithms-breaking-down-some-intimidating-parts-of-coding-ae6e1bc5cdde?source=collection_archive---------13----------------------->

本周，我花了一些时间做了一次“实践”黑客马拉松，并通过算法来解决问题。现在这两个时髦的词对我来说都是暗示，因此是一个挑战，我知道我将不得不面对，但害怕它。

一如往常，在生活中，它最终在某些方面有点反气候。我已经建立了黑客马拉松和算法的想法，认为它们是不可能完成的困难任务，只有精英程序员或那些天生就能说出精彩数学语句的人才能在编码之旅的早期完成。不，那根本不是真的，好吧，那些个人可能可以而且确实可以，但是我们这些凡人和编码学生也可以！

![](img/6562c88765e00f7dca1f26675da94d67.png)

图片来自 [Pixabay](https://pixabay.com/users/iammrrob-5387828/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=2903156) 上的“我是罗布先生”

在我的黑客马拉松实践中，我与其他一些代码新手合作，在设定的时间限制内一起解决一些复杂的问题。现在，与真正的黑客马拉松不同，我们的问题对于高级开发人员来说可以很快解决，他们可能会在大约 15 分钟内单独解决这些问题，而不是我们的 1 小时时间框架。但对我们来说，它们更复杂，需要我们沟通，在这段经历中，我感到前所未有的自信。这是一个非常非常出乎意料的结果。

它巩固了我的一些“软技能”,这些技能对于我在科技领域的成功职业生涯是绝对必要的，不管职位是什么。这对于在黑客马拉松或协作编程环境中建立并说服代码规划团队成员来解决复杂问题具有重要意义。

这不是唯一一次，也不会是我最终依靠我在项目管理和学术教学方面的背景。我是一个关注大局的人，在我们开始编码之前，强迫我的团队真正地谈论和描述问题，把它分解。这并不容易，因为我们都很兴奋地开始了解决问题的部分，但是作为一个团队真正解决一个问题是不可能的，除非我们都明白这个问题意味着什么，我们想采取什么步骤来解决它。通过大声说出来，加上一些听写，我们能够确保我们都相互理解每个人在想什么。作为项目经理，我也觉得有必要确保每个人都有发言权。这使得实际的编码更加有效，我们的“分而治之”算法也不困难，因为我们有一个如何分而治之的计划。当有 bug 时，作为一个团队，我们能够更好地讨论它们，因为我们知道为什么要写代码。

![](img/2f78d11897a598d569be0e2258d6505e.png)

Peggy Marco 在 [Pixabay](https://pixabay.com/users/peggy_marco-1553824/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=1027571) 上拍摄的图片

因此，我对我未来的黑客马拉松的看法以及我对我们采取的结构的分析(为了更好地衡量，有一些对大图的提醒)。

1)每个人都必须与目标保持一致——同样的提示或问题可以有不同的解释，甚至书面步骤也可以有不同的解释，这总是令人惊讶的。但是在一个有时间限制的情况下，在一个合作的环境中，让每个人从一开始就在同一页上是很重要的。

2)代码规划甚至是首先描述目标背后的逻辑是很重要的——这是一个机会，可以确保每个使用不同短语的人真正表达相同的意思，并且每个人都相互理解。

3)然后写下所有的步骤，这样它就被分解成一个个增量——所以这就像建立一系列的提示/待办事项列表，使之成为一系列简单的问题，共同解决复杂的问题。

4)然后按照业务逻辑顺序或商定的顺序逐一处理这些步骤。

5)测试过程中的增量(单元测试或 console.log)。

6)总的来说，确保你们在持续沟通。一个人甚至整个团队都可以进行研究，然后讨论先尝试哪种解决方案。

7)使流程更加高效，确保每个人都被倾听并做出贡献，同时确保我们以每个人都受益的方式找到解决方案(将其转化为学习体验)

**现在谈谈我一直在研究的面向对象编程的一些算法和概念！**

**不变性:它是什么，有什么好处和坏处？**

不变性是函数式编程的核心原则，也为面向对象程序提供了很多东西。可变对象是在创建后其状态可以被修改的对象。不可变对象是在创建后其状态不能被修改的对象。

**优点**

*   具有不可变对象的程序考虑起来没有那么复杂，因为你不需要担心一个对象如何随时间演变。因此，从某些方面来说，在相同或不同团队的不同开发人员之间，它更容易维护，更容易编码和阅读。
*   一个对象的一个副本和另一个一样好，所以你可以缓存对象或者多次重用同一个对象。

**缺点**

*   分发许多小对象而不是修改现有对象会导致性能影响。
*   像图这样的循环数据结构很难构建。如果你有两个在初始化后不能被修改的对象，你如何让它们指向彼此？

**这很好，但是我们如何在自己的代码中实现不变性呢？**

还有像 [immutable.js](http://facebook.github.io/immutable-js/) 、 [mori](https://github.com/swannodette/mori) 或者 [immer](https://github.com/immerjs/immer) 这样的不变性库。

否则，使用结合上述技术的`const`声明进行创建。

然后，使用“扩散”操作符、`Object.assign`、`Array.concat()`等对对象进行“变异”。，以创建新对象，而不是改变原始对象。

[](https://www.invivoo.com/immutability-and-javascript/) [## 不变性和 Javascript

### 在函数式编程语言中，不变性是一个被高度利用的概念，其中……

www.invivoo.com](https://www.invivoo.com/immutability-and-javascript/) 

[https://wecodetheweb . com/2016/02/12/immutable-JavaScript-using-es6-and-beyond/](https://wecodetheweb.com/2016/02/12/immutable-javascript-using-es6-and-beyond/)

[](https://www.sitepoint.com/immutability-javascript/) [## JavaScript - SitePoint 中的不变性

### Christian Johansen 介绍了什么是不变性，如何在 JavaScript 中使用不变性，以及它为什么有用。

www.sitepoint.com](https://www.sitepoint.com/immutability-javascript/) ![](img/58026b6601f1582b98d5af1932e54645.png)

通过[像素显示的图像](https://pixabay.com/users/mmillustrates-8219771/)

**什么是分治算法？它们是如何工作的？**

嗯，我们在我们的黑客马拉松中解决了一些分而治之的问题，它们似乎在编码面试和现实世界编程中相对常见。它可以分为以下三个部分:

划分:把问题分成一些子问题

征服:通过递归调用解决子问题，直到子问题解决。

结合:一旦子问题被解决，你将找到问题的解决方案。

分治算法问题的一些快速示例包括:

*   二分搜索法。
*   快速排序。
*   合并排序。
*   整数乘法。
*   矩阵乘法(斯特拉森的**算法**
*   极大子序列。

[https://en . Wikipedia . org/wiki/Divide-and-conquer _ algorithm #:~:text = A % 20 Divide % 2d and % 2d conquer % 20 algorithm，solution % 20to % 20the % 20original %问题](https://en.wikipedia.org/wiki/Divide-and-conquer_algorithm#:~:text=A%20divide%2Dand%2Dconquer%20algorithm,solution%20to%20the%20original%20problem)。

【https://www.radford.edu/~nokie/classes/360/divcon.html】

[https://www . geeks forgeeks . org/divide-and-conquer-algorithm-introduction/](https://www.geeksforgeeks.org/divide-and-conquer-algorithm-introduction/)

**一些算法问题:插入排序、堆排序、快速排序、合并排序是如何工作的？**

首先，请注意算法是一个数学术语，但更容易把它们看作只是配方。它们是一些代码块，一起工作来创造一些东西。当你真正思考这个问题时，在你的编码之旅中遇到真正的“算法”这个词之前，你很可能已经编写、阅读和研究过算法了。因此，我之前说他们都可以被我们这些仅仅是编码的凡人征服。但是在有压力的情况下，他们可能很难回忆起来，而且他们也可能非常复杂。这里有一些简单的方法让我们开始！

**插入排序:**

插入排序是一种比较算法，它一次一个元素地构建最终排序的数组。

它遍历一个输入数组，每次迭代删除一个元素。然后，它在数组中找到该元素所属的位置，并将它放在那里。

**快速排序:**

快速排序是一种使用分治法对数组进行排序的比较算法。

该算法选取一个枢纽元素 A[x],然后将数组重新排列成两个子数组。

子阵列 1) A[p .。。x-1]，这样所有元素都小于 A[x]

子阵列 2) A[x+1。。。r]，使得所有元素都大于或等于 A[x]。

**堆排序:**

堆排序是另一种比较算法，它使用二进制堆数据结构对元素进行排序。

它将其输入划分为一个排序区域和一个未排序区域。然后，通过移除最大的元素并将其移动到已排序的区域中，迭代地缩小未排序的区域。

**合并排序:**

合并排序是一种比较算法，其核心是如何将两个预先排序的数组合并在一起，从而得到一个也已排序的数组。

[https://www.cpp.edu/~ftang/courses/CS241/notes/sorting.htm](https://www.cpp.edu/~ftang/courses/CS241/notes/sorting.htm)

[](https://sciencing.com/the-advantages-disadvantages-of-sorting-algorithms-12749529.html) [## 排序算法的优缺点

### 对列表中的一组项目进行排序是计算机编程中经常出现的任务。通常，人类可以做到这一点…

sciencing.com](https://sciencing.com/the-advantages-disadvantages-of-sorting-algorithms-12749529.html) 

[http://www . cs . Cornell . edu/courses/cs 211/2000 fa/materials/Nov16 % 20 sorting . pdf](http://www.cs.cornell.edu/courses/cs211/2000fa/materials/Nov16%20Sorting.pdf)

**算法递归的三大定律是什么？**

定律 1)递归算法必须有一个基格。

定律 2)递归算法必须改变其状态，并向基本情况移动。

法则 3)递归算法必须递归地调用自身。

在基本情况下，这意味着问题小到可以直接解决。

改变它的状态并向基本情况移动意味着一些数据被修改，以某种方式，它使我们更接近解决方案。

算法必须调用自己，本质上只是递归。

[http://pages . di . unipi . it/Marino/python/Recursion/thethreawsofrecursion . html #:~:text = Like % 20 the % 20 robots % 20 of % 20 asimov，move % 20 to % 20 the % 20 base % 20 case](http://pages.di.unipi.it/marino/python/Recursion/TheThreeLawsofRecursion.html#:~:text=Like%20the%20robots%20of%20Asimov,move%20toward%20the%20base%20case)。

我希望这有助于任何听到“黑客马拉松”和“算法”这些词的人不那么害怕，并意识到，是的，它们可能很可怕和复杂，但一旦我们将它们分解，这两个概念与我们编码旅程的其他部分是一样的。一个可怕的概念，然后我们去了解和体验(有时是推进)，然后最终变得不那么可怕，也许还很有趣？！至少在编码面试之外很有趣。

你的代码朋友，

雷切尔(女子名)