# LeetCode —删除元素

> 原文：<https://medium.com/nerd-for-tech/leetcode-remove-element-cb3ef4f77d96?source=collection_archive---------5----------------------->

# 问题陈述

给定一个整数数组 **nums** 和一个整数 **val** ，就地删除 **nums** *中所有出现的 **val** 。元素的相对顺序可以改变。*

因为在某些语言中不可能改变数组的长度，所以必须将结果放在数组 **nums** 的**第一部分**中。更正式的说法是，如果删除重复项后还有 *k* 个元素，那么 **nums** 的前 k 个元素应该保存最终结果。除了第一个 *k* 元素之外，您留下什么并不重要。

将最终结果放入 **nums** 的第一个 *k* 槽后，返回 *k* 。

不要为另一个数组分配额外的空间。你必须通过用 O(1)个额外的内存修改输入数组来做到这一点。

**自定义判断:**

法官将使用以下代码测试您的解决方案:

```
int[] nums = [...]; // Input array
int val = ...; // Value to remove
int[] expectedNums = [...]; // The expected answer with correct length.
                            // It is sorted with no values equaling val.

int k = removeElement(nums, val); // Calls your implementation

assert k == expectedNums.length;
sort(nums, 0, k); // Sort the first k elements of nums
for (int i = 0; i < actualLength; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有断言都通过，那么您的解决方案将被**接受**。

问题陈述摘自:[https://leetcode.com/problems/remove-element](https://leetcode.com/problems/remove-element)

**例 1:**

```
Input: nums = [3, 2, 2, 3], val = 3
Output: 2, nums = [2, 2, _, _]
Explanation: Your function should return k = 2, with the first two elements of nums being 2.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**例 2:**

```
Input: nums = [0, 1, 2, 2, 3, 0, 4, 2], val = 2
Output: 5, nums = [0, 1, 4, 0, 3, _, _, _]
Explanation: Your function should return k = 5, with the first five elements of nums containing 0, 0, 1, 3, and 4.
Note that the five elements can be returned in any order.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**约束:**

```
- 0 <= nums.length <= 100
- 0 <= nums[i] <= 50
- 0 <= val <= 100
```

# 说明

## 强力方法

首先出现的强力方法是创建一个新数组，并将除 **val** 之外的所有元素复制到这个新数组中。

然后将这个新数组复制到原始数组。但是，由于问题陈述已经提到我们必须就地执行此操作，因此我们不能创建新的数组。

上述方法的时间复杂度为 **O(N)** ，但空间复杂度也为 **O(N)** 。

## 使用两个指针

我们可以降低空间复杂度，并使用两个指针就地修改数组。

我们来检查一下算法。

```
- if nums.size() == 0
  - return 0

- set i, j = 0

- loop for i = 0; i < nums.size() - 1; i++
  - if nums[i] != val
    - nums[j++] = nums[i] // assign nums[i] to nums[j] and then increment j.

- if nums[i] != val
  - nums[j++] = nums[i] // assign nums[i] to nums[j] and then increment j.

- return j
```

## C++解决方案

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        if(nums.size() == 0){
            return 0;
        }

        int i, j = 0;

        for(i = 0; i < nums.size() - 1; i++){
            if(nums[i] != val){
                nums[j++] = nums[i];
            }
        }

        if(nums[i] != val){
            nums[j++] = nums[i];
        }

        return j;
    }
};
```

## 戈朗溶液

```
func removeElement(nums []int, val int) int {
    if len(nums) == 0 {
        return 0
    }

    i, j := 0, 0

    for ; i < len(nums) - 1; i++ {
        if nums[i] != val {
            nums[j] = nums[i]
            j++
        }
    }

    if nums[i] != val {
        nums[j] = nums[i]
        j++
    }

    return j
}
```

## Javascript 解决方案

```
var removeElement = function(nums, val) {
    if( nums.length == 0 ){
        return 0;
    }

    let i = 0, j = 0;

    for(; i < nums.length - 1; i++){
        if( nums[i] != val ){
            nums[j++] = nums[i];
        }
    }

    if( nums[i] != val ){
        nums[j++] = nums[i];
    }

    return j;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [3, 2, 2, 3], val = 3

Step 1: if nums.size() == 0
        4 == 0
        false

Step 2: set i, j = 0, 0

Step 3: loop for i = 0; i < nums.size() - 1
        i < 3
        0 < 3
        true

        nums[i] != val
        nums[0] != 3
        3 != 3
        false

        i++
        i = 1

Step 4: loop for i < nums.size() - 1
        i < 3
        1 < 3
        true

        nums[i] != val
        nums[1] != 3
        2 != 3
        true

        nums[j++] = nums[i]
        nums[j] = nums[1]
        nums[0] = 2
        j++
        j = 1

        i++
        i = 2

        nums = [2, 2, 2, 3]

Step 4: loop for i < nums.size() - 1
        i < 3
        2 < 3
        true

        nums[i] != val
        nums[1] != 3
        2 != 3
        true

        nums[j++] = nums[i]
        nums[j] = nums[1]
        nums[1] = 2
        j++
        j = 2

        i++
        i = 3

        nums = [2, 2, 2, 3]

Step 4: loop for i < nums.size() - 1
        i < 3
        3 < 3
        false

Step 5: if nums[i] != val
        nums[3] != 3
        3 != 3
        false

Step 6: return j

So we return the answer as 2.
```

*原发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-remove-element)*。*