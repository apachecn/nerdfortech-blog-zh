# 第五天:团队的最佳表现

> 原文：<https://medium.com/nerd-for-tech/day-5-maximum-performance-of-a-team-837c1e41bf52?source=collection_archive---------15----------------------->

***问题链接:***

[](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3768/) [## 探索- LeetCode

### 这个挑战是初学者友好的，对高级和非高级用户都可用。它由 30 个每日…

leetcode.com](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3768/) 

***问题陈述:***

给你两个整数`n`和`k`以及两个长度都为`n`的整数数组`speed`和`efficiency`。有从`1`到`n`编号的`n`工程师。`speed[i]`和`efficiency[i]`分别代表`ith`工程师的速度和效率。

从`n`工程师中选择**最多** `k`个不同的工程师组成一个**绩效最高的团队**。

一个团队的表现是他们的工程师的速度之和乘以他们的工程师的最低效率。

返回*这个团队的最大性能*。既然答案可以是一个巨大的数，就返回**模**。

***例 1:***

```
**Input:** n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 2
**Output:** 60
**Explanation:** 
We have the maximum performance of the team by selecting engineer 2 (with speed=10 and efficiency=4) and engineer 5 (with speed=5 and efficiency=7). That is, performance = (10 + 5) * min(4, 7) = 60.
```

***例二:***

```
**Input:** n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 3
**Output:** 68
**Explanation:** This is the same example as the first but k = 3\. We can select engineer 1, engineer 2 and engineer 5 to get the maximum performance of the team. That is, performance = (2 + 10 + 5) * min(5, 4, 7) = 68.
```

***例题 3:***

```
**Input:** n = 6, speed = [2,10,3,1,5,8], efficiency = [5,4,3,9,7,2], k = 4
**Output:** 72
```

***约束:***

```
- 1 <= <= k <= n <= 10^5
- speed.length == n
- efficiency.length == n
- 1 <= speed[i] <= 10^5
- 1 <= efficiency[i] <= 10^8
```

***我的解决方案:***

```
class Solution:
    def maxPerformance(self, n: int, speed: List[int], efficiency: List[int], k: int) -> int:
        ord = sorted(zip(efficiency, speed), reverse=True)
        print(ord)
        spheap, totalSpeed, best = [], 0, 0
        for eff, spd in ord:
            heappush(spheap, spd)
            if len(spheap) <= k: totalSpeed += spd
            else: totalSpeed += spd - heappop(spheap)
            best = max(best, totalSpeed * eff)
        return best % 1000000007
```

***解释:***

解决这个问题的诀窍是找到一种方法，按顺序遍历其中一个值，然后为每个组合计算另一个值，并取最佳结果。如果我们按照**效率**对工程师进行分类，我们可以在评估组合速度(即该组的**总速度**)时向下迭代工程师。

由于**速度**和**效率**之间的索引号是相互对应的，所以我们不应该只对**效率**进行排序，而是可以创建另一个数组即 ord，将两者合并成一个数组，然后根据效率进行排序。

当我们按照**order**的顺序遍历工程师并将他们添加到可用池中时，我们知道到目前为止所有的工程师都达到或高于 minEff，所以我们可以自由地只为我们的组选择 **k** 最快的工程师。为了跟踪我们可用池中工程师的速度排序，我们可以使用一个**最小堆**，即 **spheap** 。这样，每当我们添加一个超过 **k** 限制的工程师时，我们就可以从我们的池中淘汰最慢的工程师。在每次停车时，我们还应找到**总速度**和当前最小效率的乘积，并在必要时更新**最佳**结果。

需要注意的是，说明书上写着“最多” **k** 工程师，所以我们应该马上开始跟踪**最好的**。此外，我们应该记住在我们**返回最佳**之前**对 1e9+7** 取模。

*   ***时间复杂度:O(N * log(N))*** *其中****N****是长度* ***速度*** *或* ***效率*** *，为排序的* ***和为堆的***
*   ****空间复杂度:O(N)****for****ord*******SPH EAP*****

**就是这样！**

**如果你有兴趣解决更多的问题，请跟随我，和我一起踏上这段旅程。**

**明天见！**

**干杯！**