# Bot 救公主-1(解)-HackerRank -(AI)

> 原文：<https://medium.com/nerd-for-tech/bot-saves-princess-1-solution-hackerrank-ai-305f556e3e86?source=collection_archive---------4----------------------->

嗨，大家都希望你很好。这是代码 Shaan，我试图在 HackerRank 上解决所有 150 个人工智能问题。

![](img/24559a06a8a6f565e597ee570e8919ba.png)

这是第一个[机器人救公主](https://www.hackerrank.com/challenges/saveprincess?hr_b=1)的问题，如果你想自己动手，去试着做吧。万一你被困在里面。这是一个可以帮助你的解决方案。我相信还有很多不同的解决方案，所以如果你找到了更好的，请告诉我。我很高兴得知这一点。

# 这就是问题所在

桃子公主被困在正方形格子的四个角中的一个。您位于网格的中心，可以在四个方向中的任何一个方向上一次移动一步。你能救出公主吗？

**输入格式**

第一行包含一个奇数 N (3 <= N < 100) denoting the size of the grid. This is followed by an NxN grid. Each cell is denoted by ‘ - ’ (ASCII value: 45). The bot position is denoted by ‘m’ and the princess position is denoted by ‘ p ’.

**输出格式**

打印出你要一次救出公主的步骤。移动必须用换行符' \n '分隔。

(有效的移动是向左或向右或向上或向下。)

**样本输入**

```
3
---
-m-
p--
```

**样本输出**

```
DOWN
LEFT
```

## **任务**

完成 displayPathtoPrincess 函数，它接受两个参数—整数 N 和字符数组网格。网格的格式将与您在输入中看到的完全一样，因此对于示例输入，公主位于 grid[2][0]。该功能应连续输出移动(向左、向右、向上或向下)以营救/到达公主。目标是尽可能少的移动到达公主。

上面的示例输入只是为了帮助您理解格式。公主(' p ')可以在四个角的任何一个。

**评分**
你的分数是这样计算的:(NxN —解救公主的移动次数)/10，其中 N 是网格的大小(样本测试用例中为 3×3)。

# 解决方案:-

# 结论:-

所以这是我对这个问题的解决方案，如果你喜欢，请为这篇文章鼓掌。除了让我知道我能做得更好。如果解决方案片段有问题，您可以[查看此处](https://gist.github.com/Sushant-Kumar-Rajoriya/f5eb374f92753257aaa99fecef71ff6d)。最后，保持安全，继续编码。