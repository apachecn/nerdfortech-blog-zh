# LeetCode —搜索插入位置

> 原文：<https://medium.com/nerd-for-tech/leetcode-search-insert-position-5e830af7dd2?source=collection_archive---------10----------------------->

# 问题陈述

给定一个不同整数的排序数组和一个目标值，如果找到目标，则返回索引。如果不是，则返回索引，如果它是按顺序插入的话。

你必须写一个运行时复杂度为 **O(log n)** 的算法。

问题陈述摘自:【https://leetcode.com/problems/search-insert-position】T2

**例一:**

```
Input: nums = [1, 3, 5, 6], target = 5
Output: 2
```

**例 2:**

```
Input: nums = [1, 3, 5, 6], target = 2
Output: 1
```

**例 3:**

```
Input: nums = [1, 3, 5, 6], target = 7
Output: 4
```

**例 4:**

```
Input: nums = [1, 3, 5, 6], target = 0
Output: 0
```

**例 5:**

```
Input: nums = [1], target = 0
Output: 0
```

**约束:**

```
- 1 <= nums.length <= 10^4
- -10^4 <= nums[i] <= 10^4
- nums contains distinct values sorted in ascending order.
- -10^4 <= target <= 10^4
```

# 说明

## 强力方法

强力方法是线性迭代数组，找到可以插入目标的索引。

该解决方案实施起来简单快捷，但需要花费时间。

由于元素已经排序，我们可以使用二分搜索法算法找到正确的索引。

## 二分搜索法方法

**算法**

```
- set start = 0 and end = N - 1.
- loop while (start <= end)
  - mid = (start + end)/2

  - if target > nums[mid]
    - start = mid + 1
  - else if target < nums[mid]
    - end = mid - 1
  - else
    - return mid

- return start
```

**C++解决方案**

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int start = 0;
        int end = nums.size()-1;

        while(start <= end){
            int mid = (start + end)/2;

            if(target > nums[mid]){
                start = mid + 1;
            }else if(target < nums[mid]){
                end = mid - 1;
            }else{
                return mid;
            }
        }

        return start;
    }
};
```

**戈朗解**

```
func searchInsert(nums []int, target int) int {
    start := 0
    end := len(nums) - 1

    for start <= end {
        mid := (start + end) / 2

        if target < nums[mid] {
            end = mid - 1
        } else if target > nums[mid] {
            start = mid + 1
        } else {
            return mid
        }
    }

    return start
}
```

**Javascript 解决方案**

```
var searchInsert = function(nums, target) {
    let start = 0, end = nums.length - 1;
    let mid;

    while( start < end ){
        mid = (start + end) / 2;

        if( target < nums[mid] ){
            end = mid - 1;
        } else if( target > nums[mid] ){
            start = mid + 1;
        } else {
            return mid;
        }
    }

    return start;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [1, 3, 5, 6], target = 5

Step 1: start = 0
        end = nums.size() - 1
            = 4 - 1
            = 3

Step 2: loop while( start < end )
        0 < 3
        true

        mid = (start + end)/2
            = (0 + 3)/2
            = 3/2
            = 1

        if target < nums[mid]
           5 < nums[1]
           5 < 3
           false
        else if target > nums[mid]
           5 > nums[1]
           5 > 3
           true

           start = mid + 1
                 = 1 + 1
                 = 2

Step 3: loop while( start < end )
        2 < 3
        true

        mid = (start + end)/2
            = (2 + 3)/2
            = 5/2
            = 2

        if target < nums[mid]
           5 < 5
           false
        else if target > nums[mid]
           5 > nums[1]
           5 > 5
           false
        else
          return mid
          return 2

So the answer returned is 2.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-search-insert-position)*。*