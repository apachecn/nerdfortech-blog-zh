# LeetCode —下一个排列

> 原文：<https://medium.com/nerd-for-tech/leetcode-next-permutation-6b21b9519ebf?source=collection_archive---------3----------------------->

# 问题陈述

实现 **next permutation** ，将数字重新排列成按字典顺序排列的下一个更大的数字。

如果这样的安排是不可能的，它必须重新安排它作为最低可能的顺序(即，按升序排序)。

替换必须到位，并且只使用恒定的额外内存。

问题陈述摘自:【https://leetcode.com/problems/next-permutation】T2

**例一:**

```
Input: nums = [1, 2, 3]
Output: [1, 3, 2]
```

**例 2:**

```
Input: nums = [3, 2, 1]
Output: [1, 2, 3]
```

**例 3:**

```
Input: nums = [1, 1, 5]
Output: [1, 5, 1]
```

**例 4:**

```
Input: nums = [1]
Output: [1]
```

**约束:**

```
- 1 <= nums.length <= 100
- 0 <= nums[i] <= 100
```

# 说明

## 强力方法

强力方法是找到数组元素的所有可能排列，并找出下一个最大的排列。

这里的问题是，我们要生成数组元素的所有排列，这需要很多时间。

这种方法的时间复杂度是 O(N！)而空间复杂度是 **O(N)** 。

## 单程方法

对于按如下降序排列的给定序列

[8, 5, 3, 2, 1]

不可能有下一个更大的排列了。这给了我们一个识别下一个更大排列的提示。

我们需要从右边找到第一对两个连续的数字 **nums[i]** 和**nums[i1]**，它们满足**nums[I]>nums[i1]**。

一旦我们找到索引**I-1**，我们需要用位于其右边部分 **nums[i]的数字中刚好大于自身的数字来替换数字**nums[I-1]**..nums[nums.size() — 1]** ，说 **nums[j]** 。

我们交换数字 **nums[i — 1]** 和 **nums[j]** 。我们从索引 **i** 和 **nums.size() — 1** 中反转所有的数字。

**算法**

```
- return if nums.size() <= 1
- set n = nums.size(), i = n - 1
- loop while i > 0
  - if nums[i] > nums[i - 1]
    - break

- if i <= 0
  - i = 0

- set x = ( i == 0 ) ? nums[i] : nums[i - 1]
- smallest = i

- loop for j = i + 1; j < n; j++
  - nums[j] > x && nums[j] < nums[smallest]
    - smallest = j

- swap(&nums[smallest], (i == 0 ? &nums[i] : &nums[i - 1]));

- sort(nums.begin() + i, nums.end());
```

**C++解决方案**

```
class Solution {
public: void swap(int *a, int *b)
    {
        int temp = *a;
        *a = *b;
        *b = temp;
    }
public:
    void nextPermutation(vector<int>& nums) {
        if(nums.size() <= 1){
            return;
        }

        int n = nums.size();
        int i = n - 1;
        for(;i > 0; i--){
            if(nums[i] > nums[i-1])
                break;
        }

        if(i <= 0){
            i = 0;
        }

        int x = (i == 0 ? nums[i] : nums[i - 1]);
        int smallest = i;

        for(int j = i + 1; j < n; j++){
            if(nums[j] > x && nums[j] < nums[smallest])
                smallest = j;
        }

        swap(&nums[smallest], (i == 0 ? &nums[i] : &nums[i - 1]));

        // we can also use reverse
        sort(nums.begin() + i, nums.end());
    }
};
```

**戈朗解决方案**

```
func reverse(nums []int) {
    for i := 0; i < len(nums); i++ {
        j := len(nums) - 1 - i
        if i >= j {
            break
        }

        nums[i], nums[j] = nums[j], nums[i]
    }
}

func nextPermutation(nums []int)  {
    i := 0
    for i = len(nums) - 2; i >= 0; i-- {
        if nums[i] < nums[i + 1] {
            break
        }
    }

    if i == -1 {
        reverse(nums)
        return
    }

    var j int
    for j = len(nums)-1; j > i; j-- {
        if nums[j] > nums[i] {
            break
        }
    }

    nums[i], nums[j] = nums[j], nums[i]
    reverse(nums[i + 1:])
}
```

**Javascript 解决方案**

```
var nextPermutation = function(nums) {
    if (nums === null || nums.length === 0) {
        return nums;
    }

    let index = -1;
    for (let i = nums.length - 2; i >= 0; i--) {
        if (nums[i] < nums[i + 1]) {
            index = i;
            break;
        }
    }

    if (index >= 0) {
        for (let i = nums.length - 1; i > index; i--) {
            if (nums[i] > nums[index]) {
                let temp = nums[i];
                nums[i] = nums[index];
                nums[index] = temp;
                break;
            }
        }
    }

    let start = index + 1;
    let end = nums.length - 1;
    while (start < end) {
        let temp = nums[start];
        nums[start] = nums[end];
        nums[end] = temp;
        start++;
        end--;
    }
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [1, 2, 3, 6, 5, 4]
Output: [1, 2, 4, 3, 5, 6]

Step 1: nums.size() <= 1
        6 <= 1
        false

Step 2: n = nums.size()
        n = 6

        i = n - 1
          = 6 - 1
          = 5

Step 3: loop for i > 0
        5 > 0
        true

        if nums[i] > nums[i - 1]
           nums[5] > nums[4]
           4 > 5
           false

        i--
        i = 4

Step 4: loop for i > 0
        4 > 0
        true

        if nums[i] > nums[i - 1]
           nums[4] > nums[3]
           5 > 6
           false

        i--
        i = 3

Step 5: loop for i > 0
        3 > 0
        true

        if nums[i] > nums[i - 1]
           nums[3] > nums[2]
           6 > 3
           true

           break

Step 6: i <= 0
        3 <= 0
        false

Step 7: x = (i == 0 ? nums[i] : nums[i - 1])
          = (3 == 0 ? nums[3] : nums[2])
          = (false ? nums[3] : nums[2])
          = nums[2]
          = 3

        smallest = i
                 = 3

Step 8: loop for(j = i + 1; j < n; j++)
        j = 3 + 1
          = 4

        j < n
        4 < 6
        true

        nums[j] > x && nums[j] < nums[smallest]
        nums[4] > 3 && nums[4] < nums[3]
        5 > 3 && 5 < 6
        true

        smallest = j
                 = 4

        j++
        j = 5

Step 9: loop for(j = i + 1; j < n; j++)
        j < n
        5 < 6
        true

        nums[j] > x && nums[j] < nums[smallest]
        nums[5] > 3 && nums[5] < nums[4]
        4 > 3 && 4 < 6
        true

        smallest = j
                 = 5

        j++
        j = 6

Step 10: loop for(j = i + 1; j < n; j++)
         j < 6
         6 < 6
         false

Step 11: swap(&nums[smallest], (i == 0 ? &nums[i] : &nums[i - 1]));
         swap(&nums[5], 3 == 0 ? &nums[3] : &nums[2])
         swap(&nums[5], &nums[2])
         swap(3, 4)

         [1, 2, 4, 6, 5, 3]

Step 12: reverse(nums[i], nums[n - 1])
         reverse(nums[3], nums[5])

         [1, 2, 4, 3, 5, 6]
```

*原发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-next-permutation)*。*