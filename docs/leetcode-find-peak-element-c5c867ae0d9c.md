# LeetCode —查找峰值元素

> 原文：<https://medium.com/nerd-for-tech/leetcode-find-peak-element-c5c867ae0d9c?source=collection_archive---------3----------------------->

# 问题陈述

峰元素是严格大于其相邻元素的元素。

给定一个整数数组 *nums* ，找到一个 peak 元素，并返回其索引。如果数组包含多个峰值，将索引返回给**任何峰值**。

你可能会想象 *nums[-1] = nums[n] = -∞* 。

你必须写一个在 *O(log n)* 时间内运行的算法。

问题陈述摘自:[https://leetcode.com/problems/find-peak-element](https://leetcode.com/problems/find-peak-element)

**例 1:**

```
Input: nums = [1, 2, 3, 1]
Output: 2
Explanation: 3 is a peak element, and your function should return the index number 2.
```

**例 2:**

```
Input: nums = [1, 2, 1, 3, 5, 6, 4]
Output: 5
Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
```

**约束:**

```
- 1 <= nums.length <= 1000
- -2^31 <= nums[i] <= 2^31 - 1
- nums[i] != nums[i + 1] for all valid i
```

# 说明

## 强力方法

一种简单的方法是扫描数组元素并检查它们的邻居是否严格地更小。对于数组的第一个和最后一个元素，我们分别验证第一个索引和倒数第二个索引。对于其余的元素，我们验证邻居。

由于我们正在扫描数组的所有元素，代码的时间复杂度将是 **O(N)** 。

上述方法的 C++代码片段如下所示:

```
int findPeak(int array[]){
    int n = array.size();

    if (n == 1)
      return 0;
    if (arr[0] >= arr[1])
        return 0;
    if (arr[n - 1] >= arr[n - 2])
        return n - 1;

    for (int i = 1; i < n - 1; i++) {
        if (arr[i] >= arr[i - 1] && arr[i] >= arr[i + 1])
            return i;
    }
}
```

## 二分搜索法方法

我们可以使用二分搜索法将上述程序的时间复杂度降低到 **O(log(N))** 。

在二分搜索法的例子中，我们在一个排序的数组上工作，并试图通过在每次迭代中将数组大小减半来找到目标元素。我们可以修改这个问题的二分搜索法方法来寻找所需的元素。如果中间的元素不是峰值，我们检查右侧的元素是否大于中间的元素。如果是，右边总有一个峰元素。类似地，如果左侧元素更大，峰值将在左侧。

让我们先检查一下算法，以理解修正的二分搜索法方法。

```
- set low = 0, high = nums.size() - 1
  initialize mid

- loop while low < high
  - set mid = low + (high - low / 2)

  - if nums[mid] > nums[mid + 1]
    - set high = mid
  - else if nums[mid] <= nums[mid + 1]
    - set low = mid + 1

- return low
```

## C++解决方案

```
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int low = 0, high = nums.size() - 1;
        int mid;

        while(low < high) {
            mid = low + (high - low)/2;
            if(nums[mid] > nums[mid + 1]){
                high = mid;
            } else if(nums[mid] <= nums[mid + 1]){
                low = mid + 1;
            }
        }

        return low;
    }
};
```

## 戈朗溶液

```
func findPeakElement(nums []int) int {
    low, high := 0, len(nums) - 1
    var mid int

    for low < high {
        mid = low + (high - low)/2

        if nums[mid] > nums[mid + 1] {
            high = mid
        } else if nums[mid] <= nums[mid + 1] {
            low = mid + 1
        }
    }

    return low
}
```

## Javascript 解决方案

```
var findPeakElement = function(nums) {
    let low = 0, high = nums.length - 1;
    let mid;

    while(low < high) {
        mid = low + Math.floor((high - low) / 2);

        if(nums[mid] > nums[mid + 1]){
            high = mid;
        } else if(nums[mid] <= nums[mid + 1]){
            low = mid + 1;
        }
    }

    return low;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [1, 2, 1, 3, 5, 6, 4]

Step 1: set low = 0
            high = nums.size() - 1
                 = 7 - 1
                 = 6
        initialize mid

Step 2: loop while low < high
        0 < 6
        true

        mid = low + (high - low) / 2
            = 0 + (6 - 0) / 2
            = 6 / 2
            = 3

        if nums[mid] > nums[mid + 1]
           nums[3] > nums[4]
           3 > 5
           false

        else if nums[mid] <= nums[mid + 1]
                nums[3] <= nums[4]
                3 <= 5
                true

               low = mid + 1
                   = 3 + 1
                   = 4

Step 3: loop while low < high
        4 < 6
        true

        mid = low + (high - low) / 2
            = 4 + (6 - 4) / 2
            = 4 + 2 / 2
            = 4 + 1
            = 5

        if nums[mid] > nums[mid + 1]
           nums[5] > nums[6]
           6 > 4
           true

           high = mid
                = 5

Step 4: loop while low < high
        4 < 5
        true

        mid = low + (high - low) / 2
            = 4 + (5 - 4) / 2
            = 4 + 1 / 2
            = 4 + 0
            = 4

        if nums[mid] > nums[mid + 1]
           nums[4] > nums[5]
           5 > 6
           false

        else if nums[mid] <= nums[mid + 1]
                nums[4] <= nums[5]
                5 < 6
                true

                low = mid + 1
                    = 4 + 1
                    = 5

Step 5: loop while low < high
        5 < 5
        false

Step 6: return low

So we return the answer as 5.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-find-peak-element)*。*