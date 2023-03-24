# 数组串联

> 原文：<https://medium.com/nerd-for-tech/concatenation-of-array-ffaab0833f2?source=collection_archive---------4----------------------->

(Leetcode 易题)

给定一个长度为`n`的整数数组`nums`，你想创建一个长度为`2n`的数组`ans`，其中`ans[i] == nums[i]`和`ans[i + n] == nums[i]`为`0 <= i < n`(**0-索引**)。

具体来说，`ans`是两个`nums`数组的**串联**。

返回*数组* `ans`。

**例 1:**

```
**Input:** nums = [1,2,1]
**Output:** [1,2,1,1,2,1]
**Explanation:** The array ans is formed as follows:
- ans = [nums[0],nums[1],nums[2],nums[0],nums[1],nums[2]]
- ans = [1,2,1,1,2,1]
```

**例二:**

```
**Input:** nums = [1,3,2,1]
**Output:** [1,3,2,1,1,3,2,1]
**Explanation:** The array ans is formed as follows:
- ans = [nums[0],nums[1],nums[2],nums[3],nums[0],nums[1],nums[2],nums[3]]
- ans = [1,3,2,1,1,3,2,1]
```

**约束:**

*   `n == nums.length`
*   `1 <= n <= 1000`
*   `1 <= nums[i] <= 1000`

**方法:**

这种方法只是再次插入 nums 中已经存在的数组元素，所以基本上是将数组元素连接两次。

时间复杂度— O(n)

空间复杂性— O(1)

```
**class Solution {
public:
    vector<int> getConcatenation(vector<int>& nums) {
        int n = nums.size();
        for(int i=0;i<n;i++)
        {
            nums.push_back(nums[i]);
        }
        return nums;
    }
};**So for eg: nums = [1,2,1] is initially 
**Afterwards it becomes nums = [1,2,1,1,2,1]**We are just resizing the initial array to twice its length thats all the logic that is used in this question.
```

![](img/ad2aed271ce058a46859cc367254eedf.png)

希望这有所帮助！坚持编码，坚持学习，因为一致性是关键！！！💻🙌

既然你喜欢看我的博客，为什么不请我喝杯咖啡，支持我的工作呢！！[https://www.buymeacoffee.com/sukanyabharati](https://www.buymeacoffee.com/sukanyabharati)☕