# 将数字减为零的步骤数-第 74 天(Python)

> 原文：<https://medium.com/nerd-for-tech/number-of-steps-to-reduce-a-number-to-zero-day-74-python-df1d109c37ef?source=collection_archive---------9----------------------->

![](img/ab1fbdb8e858448e3e277bde53d32a99.png)

由[林赛·亨伍德](https://unsplash.com/@lindsayhenwood?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

今天的问题很简单。我从 Leetcode 的每日编码挑战二月版中提取了这个问题。

[**1342**](https://leetcode.com/problems/number-of-steps-to-reduce-a-number-to-zero/) **。将一个数减少到零的步骤数**

给定一个非负整数`num`，返回将其减为零的步数。如果当前数是偶数，你就要把它除以 2，否则，你就要从中减去 1。

**例 1:**

```
**Input:** num = 14
**Output:** 6
**Explanation:** 
Step 1) 14 is even; divide by 2 and obtain 7\. 
Step 2) 7 is odd; subtract 1 and obtain 6.
Step 3) 6 is even; divide by 2 and obtain 3\. 
Step 4) 3 is odd; subtract 1 and obtain 2\. 
Step 5) 2 is even; divide by 2 and obtain 1\. 
Step 6) 1 is odd; subtract 1 and obtain 0.
```

**例 2:**

```
**Input:** num = 8
**Output:** 4
**Explanation:** 
Step 1) 8 is even; divide by 2 and obtain 4\. 
Step 2) 4 is even; divide by 2 and obtain 2\. 
Step 3) 2 is even; divide by 2 and obtain 1\. 
Step 4) 1 is odd; subtract 1 and obtain 0.
```

**例 3:**

```
**Input:** num = 123
**Output:** 12
```

**约束:**

*   `0 <= num <= 10^6`

解决这个问题的方法之一是检查我们要用 2 除多少次给定的数。如果我们观察这个例子，我们可以看到每当除法运算的结果是奇数，计数器就增加 2，而对于偶数，计数器就增加 1。怎么才能检查出数字是奇数还是偶数？每次我们用 2 除这个数时，我们都需要检查余数。如果我们有余数，这个数就是奇数，否则就是偶数。

让我们来看看代码片段。

```
class StepsCounter:
    def numberOfSteps (self, num: int) -> int:
        counter = 0
        while(num > 1):
            if num%2 == 0:  
                counter += 1
            else:
                counter += 2
            num = num//2
        if num == 1:
            counter += 1
        return counter
```

**复杂性分析。**

**时间复杂度**

由于我们每次都是除以 2，所以时间复杂度为 O(logN)。

**空间复杂性。**

因为我们没有使用任何额外的数据结构来存储中间结果，所以空间复杂度是 O(1)。