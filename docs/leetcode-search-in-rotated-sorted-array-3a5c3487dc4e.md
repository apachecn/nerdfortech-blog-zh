# LeetCode —在旋转排序数组中搜索

> 原文：<https://medium.com/nerd-for-tech/leetcode-search-in-rotated-sorted-array-3a5c3487dc4e?source=collection_archive---------0----------------------->

# 问题陈述

有一个按升序排序的整数数组 *nums* (具有**不同的**值)。

在传递给你的函数之前， *nums* 被**可能在一个未知的旋转索引`k (1 <= k < nums.length)`处旋转**，这样得到的数组是* [nums[k]，nums[k + 1]，...，nums[n - 1]，nums[0]，nums[1]，...，nums[k - 1]](https://alkeshghorpade.me/post/leetcode-search-in-rotated-sorted-array) ( **0 索引**)。例如，*【0，1，2，4，5，6，7】*可能在枢轴索引 3 处旋转，变成*【4，5，6，7，0，1，2】*。

给定数组*nums*nums**后**可能的旋转和一个整数 *target* ，如果*在 nums 中则返回 *target* 的索引，如果不在 nums 中则返回-1*。

你必须写一个运行时复杂度为`O(log n)`的算法。

问题陈述摘自:[https://leet code . com/problems/search-in-rotated-sorted-array](https://leetcode.com/problems/search-in-rotated-sorted-array)

**例 1:**

```
Input: nums = [4, 5, 6, 7, 0, 1, 2], target = 0 
Output: 4
```

**例 2:**

```
Input: nums = [4, 5, 6, 7, 0, 1, 2], target = 3 
Output: -1
```

**例 3:**

```
Input: nums = [1], target = 0 
Output: -1
```

**约束:**

```
- 1 <= nums.length <= 5000 
- 10^4 <= nums[i] <= 10^4 
- All values of nums are unique. 
- nums is an ascending array that is possibly rotated. 
- -10^4 <= target <= 10^4
```

# 说明

## 二分搜索法双通

最简单的解决方案是找到 pivot 元素。元素小于前一个元素的索引。然后，我们在两个子数组之一上调用二分搜索法。如果我们找到目标元素，我们返回索引或返回-1。

该方法的一个 C++代码片段如下所示:

```
int rotatedBinarySearch(int arr[], int n, int key) {
    int pivot = findPivot(arr, 0, n - 1);

    if (pivot == -1)
        return binarySearch(arr, 0, n - 1, key);

    if (arr[pivot] == key)
        return pivot;

    if (arr[0] <= key)
        return binarySearch(arr, 0, pivot - 1, key);

    return binarySearch(arr, pivot + 1, n - 1, key);
}

int findPivot(int arr[], int low, int high) {
    if (high < low)
        return -1;

    if (high == low)
        return low;

    int mid = (low + high) / 2;
    if (mid < high && arr[mid] > arr[mid + 1])
        return mid;

    if (mid > low && arr[mid] < arr[mid - 1])
        return (mid - 1);

    if (arr[low] >= arr[mid])
        return findPivot(arr, low, mid - 1);

    return findPivot(arr, mid + 1, high);
}

int binarySearch(int arr[], int low, int high, int key) {
    if (high < low)
        return -1;

    int mid = (low + high) / 2;
    if (key == arr[mid])
        return mid;

    if (key > arr[mid])
        return binarySearch(arr, (mid + 1), high, key);

    return binarySearch(arr, low, (mid - 1), key);
}
```

该方法的时间复杂度为 **O(logN)** ，空间复杂度为 **O(1)** 。

## 二分搜索法一张通行证

我们可以在一次循环中找到目标元素，而不是迭代数组两次，一次是为了找到支点，然后在其中一个子数组中找到目标数字。

标准的二分搜索法方法需要改变。我们需要将左右索引传递给我们的搜索函数，并根据中间的元素考虑数组的左半部分或右半部分。

我们先检查一下算法。

```
// searchIndex function
- set mid = low + high / 2

- if low > high
  - return -1

- if nums[mid] == target
  - return mid

- if nums[low] <= nums[mid]
  - if nums[low] <= target && nums[mid] >= target
    - return searchIndex(nums, low, mid - 1, target)
  - else
    - return searchIndex(nums, mid + 1, high, target)
- else
  - if nums[high] >= target && nums[mid] <= target
    - return searchIndex(nums, mid + 1, high, target)
  - else
    - return searchIndex(nums, low, mid - 1, target)

// search function
- searchIndex(nums, 0, nums.size() - 1, target)
```

## C++解决方案

```
class Solution {
static int searchIndex(vector<int>& nums, int left, int right, int target){
    int mid = (left + right) / 2;
    if(left > right){
        return -1;
    }

    if(nums[mid] == target){
        return mid;
    }

    if(nums[left] <= nums[mid]){
        if(nums[left] <= target && nums[mid] >= target){
            return searchIndex(nums, left, mid - 1, target);
        } else {
            return searchIndex(nums, mid + 1, right, target);
        }
    } else {
        if(nums[right] >= target && nums[mid] <= target){
            return searchIndex(nums, mid + 1, right, target);
        } else {
            return searchIndex(nums, left, mid - 1, target);
        }
    }
};

public:
    int search(vector<int>& nums, int target) {
        return searchIndex(nums, 0, nums.size() - 1, target);
    }
};
```

## 戈朗溶液

```
func searchIndex(nums []int, left, right, target int) int {
    mid := (left + right) / 2

    if left > right {
        return -1
    }

    if nums[mid] == target {
        return mid
    }

    if nums[left] <= nums[mid] {
        if nums[left] <= target && nums[mid] >= target {
            return searchIndex(nums, left, mid - 1, target)
        } else {
            return searchIndex(nums, mid + 1, right, target)
        }
    } else {
        if nums[right] >= target && nums[mid] <= target {
            return searchIndex(nums, mid + 1, right, target)
        } else {
            return searchIndex(nums, left, mid - 1, target)
        }
    }
}

func search(nums []int, target int) int {
    return searchIndex(nums, 0, len(nums) - 1, target)
}
```

## Javascript 解决方案

```
var searchIndex = function(nums, left, right, target) {
    let mid = (left + right) / 2;

    if(left > mid) {
        return -1;
    }

    if(nums[mid] == target) {
        return mid;
    }

    if (nums[left] <= nums[mid]) {
        if(nums[left] <= target && nums[mid] >= target) {
            return searchIndex(nums, left, mid - 1, target);
        } else {
            return searchIndex(nums, mid + 1, right, target);
        }
    } else {
        if(nums[right] >= target && nums[mid] <= target) {
            return searchIndex(nums, mid + 1, right, target);
        } else {
            return searchIndex(nums, left, mid - 1, target);
        }
    }
};

var search = function(nums, target) {
  return searchIndex(nums, 0, nums.length - 1, target);
};
```

让我们试着解决这个问题。

```
Input: nums = [4, 5, 6, 7, 0, 1, 2], target = 0

Step 1: // search function
        searchIndex(nums, 0, nums.size() - 1, target)

// searchIndex function
Step 2: int mid = (left + right) / 2
        mid = (0 + 6) / 2
            = 6 / 2
            = 3

        if nums[mid] == target
           nums[3] == 0
           7 == 0
           false

        if nums[left] <= nums[mid]
           nums[0] <= nums[3]
           4 <= 7
           true

           if nums[left] <= target && nums[mid] >= target
              nums[0] <= 0 && nums[3] >= 0
              4 <= 0 && 7 >= 0
              false

              return searchIndex(nums, mid + 1, right, target)
                     searchIndex(nums, 4, 6, 0)

// searchIndex(nums, 4, 6, target)
Step 3: int mid = (left + right) / 2
        mid = (4 + 6) / 2
            = 10 / 2
            = 5

        if nums[mid] == target
           nums[5] == 0
           1 == 0
           false

        if nums[left] <= nums[mid]
           nums[4] <= nums[5]
           0 <= 1
           true

           if nums[left] <= target && nums[mid] >= target
              nums[4] <= 0 && nums[5] >= 0
              0 <= 0 && 1 >= 0
              true

              return searchIndex(nums, left, mid - 1, target)
                     searchIndex(nums, 4, 4, 0)

// searchIndex(nums, 4, 4, 0)
Step 4: int mid = (left + right) / 2
        mid = (4 + 4) / 2
            = 8 / 2
            = 4

        if nums[mid] == target
           nums[4] == 0
           0 == 0
           return mid
           return 4

The flow backtracks from step 4 to step 1.

We return the answer as 4.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-search-in-rotated-sorted-array)*。*