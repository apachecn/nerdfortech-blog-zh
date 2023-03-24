# LeetCode —最大乘积子阵列

> 原文：<https://medium.com/nerd-for-tech/leetcode-maximum-product-subarray-44c169d05c2b?source=collection_archive---------5----------------------->

# 问题陈述

给定一个整数数组 *nums* ，在数组中找到一个具有最大乘积的连续非空子数组，并将乘积返回给*。*

测试用例的生成是为了让答案符合一个 32 位的**整数。**

一个**子数组**是数组的一个连续子序列。

问题陈述摘自:[https://leetcode.com/problems/maximum-product-subarray](https://leetcode.com/problems/maximum-product-subarray)。

**例 1:**

```
Input: nums = [2, 3, -2, 4] 
Output: 6 
Explanation: [2, 3] has the largest product 6.
```

**例 2:**

```
Input: nums = [-2, 0, -1] 
Output: 0 
Explanation: The result cannot be 2, because [-2, -1] is not a subarray.
```

**约束:**

```
- 1 <= nums.length <= 2 * 10^4 
- -10 <= nums[i] <= 10 
- The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
```

# 说明

## 强力方法

一种简单的方法是考虑所有子阵列并返回最大乘积。

该方法的 C++代码片段如下所示:

```
int result = arr[0];

for (int i = 0; i < n; i++) {
    int mul = arr[i];

    for (int j = i + 1; j < n; j++) {
        result = max(result, mul);
        mul *= arr[j];
    }

    result = max(result, mul);
}

return result;
```

上述方法的时间复杂度为 **O(N )** ，空间复杂度为 **O(1)** 。

## 有效的方法

这种高效的方法类似于我们在之前的博客文章[最大子阵列](https://alkeshghorpade.me/post/leetcode-maximum-subarray)中使用的方法。这里需要注意的一点是，数组可以包含正数、负数和零。最大子阵列问题使用了 Kadane 的算法。我们调整了这种方法，改为使用三个变量，称为 *max_so_far* 、 *max_ending_here* 和 *min_ending_here* 。对于每个索引，结束于该索引的最大数将是 *maximum(arr[i]，max_ending_here * arr[i]，min_ending_here * arr[i])* 。同样，这里结束的最小数字将是这 3 个中的最小值。

我们先检查一下算法。

```
- set max_ending_here, min_ending_here and max_so_far to nums[0]
  initialize temp_maximum

- loop for i = 1; i < nums.size(); i++
  - temp_maximum = max(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
  - min_ending_here = min(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
  - max_ending_here = temp_maximum
  - max_so_far = max(max_so_far, max_ending_here)

- return max_so_far
```

让我们来看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int max_ending_here = nums[0];
        int min_ending_here = nums[0];
        int max_so_far = nums[0];
        int temp_maximum;

        for(int i = 1; i < nums.size(); i++) {
            temp_maximum = max({nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here});
            min_ending_here = min({nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here});
            max_ending_here = temp_maximum;
            max_so_far = max(max_so_far, max_ending_here);
        }

        return max_so_far;
    }
};
```

## 戈朗溶液

```
func max(a, b int) int {
    if a > b {
        return a
    }

    return b
}

func min(a, b int) int {
    if a < b {
        return a
    }

    return b
}

func maxProduct(nums []int) int {
    max_ending_here, min_ending_here, max_so_far := nums[0], nums[0], nums[0]
    var temp_maximum int

    for i := 1; i < len(nums); i++ {
        temp_maximum = max(nums[i], max(max_ending_here * nums[i], min_ending_here * nums[i]))
        min_ending_here = min(nums[i], min(max_ending_here * nums[i], min_ending_here * nums[i]))
        max_ending_here = temp_maximum
        max_so_far = max(max_so_far, max_ending_here)
    }

    return max_so_far
}
```

## Javascript 解决方案

```
var maxProduct = function(nums) {
    let max_ending_here = nums[0], min_ending_here = nums[0], max_so_far = nums[0];
    let temp_maximum

    for(let i = 1; i < nums.length; i++) {
        temp_maximum = Math.max(nums[i], Math.max(max_ending_here * nums[i], min_ending_here * nums[i]));

        min_ending_here = Math.min(nums[i], Math.min(max_ending_here * nums[i], min_ending_here * nums[i]));

        max_ending_here = temp_maximum;
        max_so_far = Math.max(max_so_far, max_ending_here)
    }

    return max_so_far;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [2, 3, -2, 4]

Step 1: max_ending_here, min_ending_here, max_so_far = nums[0], nums[0], nums[0]
        max_ending_here = 2
        min_ending_here = 2
        max_so_far = 2

        initialize temp_maximum

Step 2: loop for i = 1; i < nums.size()
        i < nums.size()
        1 < 4
        true

        temp_maximum = max(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
                     = max(nums[1], nums[1] * 2, nums[1] * 2)
                     = max(3, 3 * 2, 3 * 2)
                     = max(3, 6, 6)
                     = 6

        min_ending_here = min(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
                        = min(nums[1], nums[1] * 2, nums[1] * 2)
                        = min(3, 3 * 2, 3 * 2)
                        = min(3, 6, 6)
                        = 3

        max_ending_here = temp_maximum
                        = 6

        max_so_far = max(max_so_far, max_ending_here)
                   = max(2, 6)
                   = 6

        i++
        i = 2

Step 3: loop for i < nums.size()
        i < nums.size()
        2 < 4
        true

        temp_maximum = max(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
                     = max(nums[2], nums[2] * 6, nums[2] * 3)
                     = max(-2, -2 * 6, -2 * 3)
                     = max(-2, -12, -6)
                     = -2

        min_ending_here = min(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
                        = min(nums[2], nums[2] * 6, nums[2] * 3)
                        = min(-2, -2 * 6, -2 * 3)
                        = min(-2, -12, -6)
                        = -12

        max_ending_here = temp_maximum
                        = -2

        max_so_far = max(max_so_far, max_ending_here)
                   = max(6, -2)
                   = 6

        i++
        i = 3

Step 4: loop for i < nums.size()
        i < nums.size()
        3 < 4
        true

        temp_maximum = max(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
                     = max(nums[3], nums[3] * -2, nums[3] * -12)
                     = max(4, 4 * -2, 4 * -12)
                     = max(4, -8, -48)
                     = 4

        min_ending_here = min(nums[i], nums[i] * max_ending_here, nums[i] * min_ending_here)
                        = min(nums[3], nums[3] * -2, nums[3] * -12)
                        = min(4, 4 * -2, 4 * -12)
                        = min(4, -8, -48)
                        = -48

        max_ending_here = temp_maximum
                        = 4

        max_so_far = max(max_so_far, max_ending_here)
                   = max(6, 4)
                   = 6

        i++
        i = 4

Step 5: loop for i < nums.size()
        i < nums.size()
        4 < 4
        false

Step 6: return max_so_far

So we return the answer as 6.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-maximum-product-subarray)*。*