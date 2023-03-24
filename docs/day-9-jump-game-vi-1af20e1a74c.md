# 第九天:跳跃游戏六

> 原文：<https://medium.com/nerd-for-tech/day-9-jump-game-vi-1af20e1a74c?source=collection_archive---------11----------------------->

***问题链接:***

[](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/604/week-2-june-8th-june-14th/3773/) [## 探索- LeetCode

### 这个挑战是初学者友好的，对高级和非高级用户都可用。它由 30 个每日…

leetcode.com](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/604/week-2-june-8th-june-14th/3773/) 

***问题陈述:***

给你一个 **0 索引的**整数数组`nums`和一个整数`k`。

你最初站在指数`0`。在一次移动中，您最多可以向前跳`k`步，而不会超出阵列的边界。也就是说，您可以从索引`i`跳到范围`[i + 1, min(n - 1, i + k)]`**内的任何索引。**

**你想到达数组的最后一个索引(index `n - 1`)。您的**分数**是您在数组中访问的每个索引`j`的所有`nums[j]`的**总和**。**

**返回****最高分*** *即可获得*。***

******例 1:******

```
***Input:** nums = [1,-1,-2,4,-7,3], k = 2
**Output:** 7
**Explanation:** You can choose your jumps forming the subsequence [1,-1,4,3] (underlined above). The sum is 7.*
```

******例题 2:******

```
***Input:** nums = [10,-5,-2,4,0,3], k = 3
**Output:** 17
**Explanation:** You can choose your jumps forming the subsequence [10,4,3] (underlined above). The sum is 17.*
```

******例题 3:******

```
***Input:** nums = [1,-5,-20,4,-1,3,-6,-3], k = 2
**Output:** 0*
```

******约束:******

```
*- 1 <= nums.length, k <= 10^5
- -10^4 <= nums[i] <= 10^4*
```

******天真解:******

```
*def maxResult(self, nums: List[int], k: int) -> int:				
        n = len(nums)
        dp = [float('-inf') for i in range(n)]
        dp[n - 1] = nums[n - 1]
        for j in range(n - 2, -1, -1):
            for i in range(1, k + 1):
                if j + i < n:
                    dp[j] = max(dp[j], dp[j + i])
            dp[j] += nums[j]
        return dp[0]*
```

******解释:******

***从位置 I 开始，下一个位置最多有 k 个选择(即 i + 1，i + 2，…，i + k)。假设从位置 I 可以得到的最高分是 dp[i]，那么***

***dp[i] = max(dp[i + 1]，dp[i + 2]，dp[i + 3]，…，dp[i + k]) + nums[i]***

***需要注意边界情况的要求(i + k 需要小于数组大小 n)***

*   ******时间复杂度:O(nk)******

******优化方案:******

```
*def maxResult(self, nums: List[int], k: int) -> int:
        n = len(nums)
        dp = [float('-inf') for i in range(n)]
        dp[n - 1] = nums[n - 1]
        hq = []
        heapq.heappush(hq, (-dp[n - 1], n - 1))
        for j in range(n - 2, -1, -1):
            sc, idx = hq[0]
            while idx > j + k:
                heapq.heappop(hq)
                if len(hq) > 0:
                    sc, idx = hq[0]
            dp[j] = -sc + nums[j]
            heapq.heappush(hq, (-dp[j], j))
        return dp[0]*
```

******解释:******

***我们注意到，对于内部循环，我们不需要每次都检查所有 k 个元素来获得最大值。我们可以把所有的 dp[i]和它们的索引放在一个堆里。然后我们可以用 O(1)来获取最大值，同时，每次我们得到一个新的 dp[i]时，我们需要把它添加到堆中，并维护堆结构。***

*   ******时间复杂度:O(n logn)******

***就是这样！***

***如果你有兴趣解决更多的问题，请跟随我，和我一起踏上这段旅程。***

***明天见！***

***干杯！***