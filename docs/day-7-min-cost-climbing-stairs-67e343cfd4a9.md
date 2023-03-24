# 第七天:爬楼梯的最小花费

> 原文：<https://medium.com/nerd-for-tech/day-7-min-cost-climbing-stairs-67e343cfd4a9?source=collection_archive---------15----------------------->

***问题链接:***

[](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3770/) [## 探索- LeetCode

### LeetCode Explore 是每个人开始在 LeetCode 上练习和学习的最好地方。不管你是不是一个…

leetcode.com](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3770/) 

***问题陈述:***

给你一个整数数组`cost`，其中`cost[i]`是楼梯上`ith`一步的成本。一旦你付了费用，你可以爬一两级台阶。

您可以从带索引`0`的步骤开始，也可以从带索引`1`的步骤开始。

返回*到达楼层顶部的最小成本*。

***例 1:***

```
**Input:** cost = [10,15,20]
**Output:** 15
**Explanation:** Cheapest is: start on cost[1], pay that cost, and go to the top.
```

***例题 2:***

```
**Input:** cost = [1,100,1,1,1,100,1,1,100,1]
**Output:** 6
**Explanation:** Cheapest is: start on cost[0], and only step on 1s, skipping cost[3].
```

***约束:***

```
- 2 <= cost.length <= 1000
- 0 <= cost[i] <= 999
```

***我的解决方案:***

```
class Solution:
    def minCostClimbingStairs(self, cost: List[int]) -> int:
        for i in range(len(cost) - 3, -1, -1):
            cost[i] += min(cost[i+1], cost[i+2])
        return min(cost[0], cost[1])
```

***解释:***

这就是**自上而下的动态编程** ( **DP** )方法解决方案。我们可以认为这是从末端开始的许多更小的子问题的累积。

在每一步，我们可以认为答案是当前步骤的总**成本**，加上下两步开始的每个解决方案的总**成本**的较小结果。这意味着通过逆向思考，我们可以先解决最小的问题，然后从那里开始构建。

对于最后两步，答案显然是它们各自的**成本**。对于倒数第三个步骤，是该步骤的**成本**，加上最后两个步骤中较低的一个。既然我们知道了这一点，我们就可以存储这些数据，供以后在较低的步骤中使用。通常，这将调用一个 DP 数组，但是在这种情况下，我们可以简单地将值**存储在**中。

*(* ***注*** *:如果我们选择不修改输入，我们可以创建一个 DP 数组来存储这些信息，代价是* ***O(N)额外的空间*** *。)*

所以我们应该从末端向下迭代，从末端的第三步开始，用从 **cost[i]** 到末端的最佳总 **cost** 更新 **cost[i]** 中的值。然后，一旦我们到达步骤的底部，我们可以选择**成本【0】**和**成本【1】****的最佳结果返回**我们的答案。

*   ***时间复杂度:O(N)*** *其中* ***N*** *是* ***的长度***
*   ***空间复杂度:O(1)*** *或****O(N)****如果我们使用单独的 DP 数组*

就是这样！

如果你有兴趣解决更多的问题，请跟随我，和我一起踏上这段旅程。

明天见！

干杯！