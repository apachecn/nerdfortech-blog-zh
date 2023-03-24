# 排序颜色

> 原文：<https://medium.com/nerd-for-tech/sort-colors-a0f5e7354b6d?source=collection_archive---------29----------------------->

# **问题陈述**

[](https://leetcode.com/problems/sort-colors/) [## 排序颜色- LeetCode

### 给定一个数组 nums，包含 n 个红色、白色或蓝色的对象，对它们进行排序，使相同颜色的对象…

leetcode.com](https://leetcode.com/problems/sort-colors/) 

# **天真的解决方案**

解决这个问题的第一个方法是对数组进行排序。因此，如果我们使用像合并排序这样的排序技术，这种方法将在 *O(nlogn)* 中工作。这就是这个问题的天真解决方案。

```
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        nums.sort()
```

*时间复杂度:O(nlogn)*

*空间复杂度:没有多余的空间*

# **高效解决方案**

高效的方法是取 3 个指针，即 ***低*******高****和***中*** 。从第 0 个索引开始将*低位*和*中间*指针和*高位*指针指向最后一个索引。现在考虑 3 种情况:**

1.  **一旦*中*指针到达值为 0 的元素，就将其与*低*指针值交换，并将两个指针都增加 1。**
2.  **如果*中间*指针到达值为 1 的元素，那么简单地将*中间*指针增加 1**
3.  **如果*中*指针到达值为 2 的元素，则将其与*高*指针值交换，并减少*高*指针。**

**这将持续到中间≤高。**

```
**class Solution:
    def sortColors(self, nums: List[int]) -> None:
        low = 0
        high = len(nums) - 1
        mid = 0
        while (mid <= high):
            if nums[mid] == 0:
                nums[low], nums[mid] = nums[mid], nums[low]
                low += 1
                mid += 1
            elif nums[mid] == 1:
                mid += 1
            else:
                nums[high], nums[mid] = nums[mid], nums[high]
                high -= 1
        return nums**
```

**在这个解决方案中，我们确保 low 左边的所有元素都是 0，high 右边的所有元素都是 2。**

***时间复杂度:O(n)***

***空间复杂度:O(1)***

**这个问题就这么多了。如果你有任何疑问或者想让我就任何具体问题写博客，请写下来。**

**继续求解！**