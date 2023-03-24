# 第 6 天:最长的连续序列

> 原文：<https://medium.com/nerd-for-tech/day-6-longest-consecutive-sequence-bc540f176dc2?source=collection_archive---------9----------------------->

**问题链接:**

[](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3769/) [## 探索- LeetCode

### 这个挑战是初学者友好的，对高级和非高级用户都可用。它由 30 个每日…

leetcode.com](https://leetcode.com/explore/challenge/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3769/) 

***问题陈述:***

给定一个未排序的整数数组`nums`，返回*最长连续元素序列的长度。*

你必须写一个在`O(n)`时间内运行的算法。

***例 1:***

```
**Input:** nums = [100,4,200,1,3,2]
**Output:** 4
**Explanation:** The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

***例二:***

```
**Input:** nums = [0,3,7,2,5,8,4,6,0,1]
**Output:** 9
```

***约束:***

```
- 0 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9
```

***蛮力:***

```
def longestConsecutiveSequence(A: List[int], n: int) -> int:
    longestStreak = 0
    for i in range (n):
        currentNum = A[i] - 1
        currentStreak = 1
        while(currentNum in A):
            currentStreak += 1
            currentNum -= 1
        currentNum = A[i] + 1
        while(currentNum in A):
            currentStreak += 1
            currentNum += 1
        longestStreak = max(longestStreak, currentStreak)
    return longestStreak
```

***解释:***

对于中的每个元素，线性搜索大于和小于当前元素的连续元素。跟踪数组中连续元素的当前条纹和最长条纹。

*   **时间复杂度:** **O(n )** :内循环线性搜索连续元素。该搜索将在条纹的长度内进行，并且条纹的最长长度可以是 n。因此，在外环内，最坏情况下的时间复杂度是 O(n)。同样，对数组的每个元素重复这个过程。所以时间复杂度= n * O(n ) = O(n)
*   **空间复杂度:O(1)**

***使用排序:***

```
def longestConsecutiveSequence(A: List[int], n: int) -> int:
    longestStreak = 0
    A.sort()
    i = 0
    while i < n:
        currentStreak = 1
        while(i < n and A[i + 1] == A[i] + 1):
            currentStreak += 1
            i += 1
        if currentStreak == 1:
            i += 1
        longestStreak = max(longestStreak, currentStreak)
    return longestStreak
```

***解释:***

对整个数组 a 进行排序。现在，连续的元素将一个接一个地线性排列。现在线性检查最长的连续序列。

*   ***时间复杂度:*** 对一个数组排序+数组的线性遍历= O(nlogn)+O(n)=**O(nlogn)**
*   **空间复杂度:O(1)**

***使用哈希表:***

```
def longestConsecutiveSequence(A: List[int], n: int) -> int:
    longestStreak = 0
    hash = {i for i in A}
    for i in range (n):
        if A[i] - 1 not in hash:
            currentStreak = 1
            currentNum = A[i] + 1
            while currentNum in hash:
                currentStreak += 1
                currentNum += 1
            longestStreak = max(longestStreak, currentStreak)
    return longestStreak
```

***解释:***

我们可以通过使用哈希表来改进搜索连续元素的时间复杂度，该哈希表可以检查 O(1)平均值中连续元素的存在。

*   ***时间复杂度:* O(n)** 搜索操作的时间复杂度现在是 O(1)，我们可以看到每个元素最多搜索两次，即第一次在 if 条件下，第二次在 while 循环条件下。因此，由此产生的复杂性本质上是线性的(O(n))。由于每个元素只被访问一次，时间复杂度:O(n)
*   **空间复杂度:O(n)** ，对于哈希表

就是这样！

如果你有兴趣解决更多的问题，请跟随我，和我一起踏上这段旅程。

明天见！

干杯！