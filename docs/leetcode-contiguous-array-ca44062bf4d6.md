# LeetCode —连续数组

> 原文：<https://medium.com/nerd-for-tech/leetcode-contiguous-array-ca44062bf4d6?source=collection_archive---------1----------------------->

# 问题陈述

给定一个二进制数组 nums，返回 0 和 1 个数相等的连续子数组的最大长度。

问题陈述摘自:[https://leetcode.com/problems/contiguous-array](https://leetcode.com/problems/contiguous-array)。

**例一:**

```
Input: nums = [0, 1] 
Output: 2 
Explanation: [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.
```

**例二:**

```
Input: nums = [0, 1, 0] 
Output: 2 
Explanation: [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.
```

**约束:**

```
- 1 <= nums.length <= 10^5 
- nums[i] is either 0 or 1
```

# 说明

## 强力方法

简单的方法是考虑数组的每个子集，并验证它是否有相等数量的 0 和 1。然后，我们找出具有相等数量的 0 和 1 的最大子数组。

这种方法的一个 C++片段如下所示:

```
int maxLength = 0;

for (int i = 0; i < nums.size(); i++) {
    int zeroes = 0, ones = 0;
    for (int j = i; j < nums.length; j++) {
        if (nums[j] == 0) {
            zeroes++;
        } else {
            ones++;
        }
        if (zeroes == ones) {
            maxLength = Math.max(maxLength, j - i + 1);
        }
    }
}

return maxLength;
```

上述方法的时间复杂度为 **O(N )** ，对于大数组将超时。

## 使用附加阵列

在这种方法中，我们使用一个额外的大小为 2n + 1 的数组。我们使用一个额外的 **sum** 变量，它将在遍历时跟踪数组元素的总和。当特定索引处的元素为 1 时，我们将使总和增加 1，如果该元素为 0，则将总和减少-1。

所以我们能达到的最大和最小和是 n 和-n，其中 n 是数组的大小。因此，我们创建一个大小为 2n + 1 的数组来跟踪目前为止遇到的各种总和。每当我们在遍历数组时遇到相同的和值，我们通过从当前索引中减去该索引处的值来计算子数组的长度。我们将上面的值与我们以前可能遇到的最大子阵列进行比较。

这种优化方法的 C++代码片段如下所示:

```
int n = nums.size();
int array[2 * n + 1];
array[n] = -1;
int maxLength = 0, count = 0;

for (int i = 0; i < n; i++) {
    count = count + (nums[i] == 0 ? -1 : 1);

    if (array[count + n] >= -1) {
        maxLength = max(maxLength, i - array[count + n]);
    } else {
        array[count + n] = i;
    }
}

return maxLength;
```

对于大小为 2n + 1 的数组，上述方法的时间复杂度为 **O(N)** ，空间复杂度为 **O(N)** 。

## 使用哈希映射

我们可以通过使用哈希映射而不是数组来将空间优化为 n。哈希映射将以 index-sum 的形式存储键值对。

每当我们第一次遇到一个和时，我们就在哈希表中为这个和创建一个条目，并将它的索引存储为值。如果我们再次遇到 sum，我们从当前索引中减去现有的索引(hash map 的值)。

我们来检查一下算法。

```
- set unordered_map[int, int] = {0 , -1}
  set maxLength = 0, sum = 0

- loop for i = 0; i < nums.size(); i++
  - sum = sum + (nums[i] == 1 ? 1 : -1)

  // the sum exists in the hash map update the maxLength
  // else set the current index for that sum
  - if m.count(sum)
    - maxLength = max(maxLength, i - m[sum])
  - else
    - m[sum] = i

- return maxLength
```

让我们看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    int findMaxLength(vector<int>& nums) {
        unordered_map<int, int> m{{0, -1}};
        int maxLength = 0, sum = 0;

        for(int i = 0; i < nums.size(); i++) {
            sum = sum + (nums[i] == 1 ? 1 : -1);

            if(m.count(sum)) {
                maxLength = max(maxLength, i - m[sum]);
            } else {
                m[sum] = i;
            }
        }

        return maxLength;
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

func findMaxLength(nums []int) int {
    m := make(map[int]int)
    maxLength, sum := 0, 0
    m[0] = -1

    for i := 0; i < len(nums); i++ {
        if nums[i] == 1 {
            sum = sum + 1
        } else {
            sum = sum - 1
        }

        if index, ok := m[sum]; ok  {
            maxLength = max(maxLength, i - index)
        } else {
            m[sum] = i
        }
    }

    return maxLength
}
```

## Javascript 解决方案

```
var findMaxLength = function(nums) {
    let m = {0: -1};
    let maxLength = 0, sum = 0;

    for(let i = 0; i < nums.length; i++) {
        sum = sum + (nums[i] == 1 ? 1 : -1);

        if(m[sum] === undefined) {
            m[sum] = i;
        } else {
            maxLength = Math.max(maxLength, i - m[sum]);
        }
    }

    return maxLength;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: [0, 1, 1, 0, 1, 1, 1, 0]

Step 1: unordered_map<int, int> m{{0, -1}}
        maxLength = 0, sum = 0

Step 2: loop for i = 0; i < nums.size()
        0 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = 0 + (nums[0] == 1 ? 1 : -1)
            = 0 + (0 == 1 ? 1 : -1)
            = 0 + -1
            = -1

        if m.count(sum)
           m.count(-1) // no key with -1
           false
        else
           m[sum] = i
           m[-1] = 0

        i++
        i = 1

Step 3: i < nums.size()
        1 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = -1 + (num[1] == 1 ? 1 : -1)
            = -1 + (1 == 1 ? 1 : -1)
            = -1 + 1
            = 0

        if m.count(sum)
           m.count(0) // has key with 0
           true

           maxLength = max(maxLength, i - m[sum])
                     = max(0, 1 - (-1))
                     = max(0, 2)
                     = 2

        i++
        i = 2

Step 4: i < nums.size()
        2 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = 0 + (num[2] == 1 ? 1 : -1)
            = 0 + (1 == 1 ? 1 : -1)
            = 0 + 1
            = 1

        if m.count(sum)
           m.count(1) // no key with -1
           false
        else
           m[sum] = i
           m[1] = 2

        i++
        i = 3

Step 5: i < nums.size()
        3 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = 1 + (num[3] == 1 ? 1 : -1)
            = 1 + (0 == 1 ? 1 : -1)
            = 1 + -1
            = 0

        if m.count(sum)
           m.count(0) // has key with 0
           true

           maxLength = max(maxLength, i - m[sum])
                     = max(2, 3 - (-1))
                     = max(2, 4)
                     = 4

        i++
        i = 4

Step 6: i < nums.size()
        4 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = 0 + (num[4] == 1 ? 1 : -1)
            = 0 + (1 == 1 ? 1 : -1)
            = 0 + 1
            = 1

        if m.count(sum)
           m.count(1) // has key with 1
           true

           maxLength = max(maxLength, i - m[sum])
                     = max(4, 4 - 2)
                     = max(4, 2)
                     = 2

        i++
        i = 5

Step 7: i < nums.size()
        5 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = 1 + (num[5] == 1 ? 1 : -1)
            = 1 + (1 == 1 ? 1 : -1)
            = 1 + 1
            = 2

        if m.count(sum)
           m.count(2) // no key with 2
           false
        else
           m[sum] = i
           m[2] = 5

        i++
        i = 6

Step 8: i < nums.size()
        6 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = 2 + (num[6] == 1 ? 1 : -1)
            = 2 + (1 == 1 ? 1 : -1)
            = 2 + 1
            = 3

        if m.count(sum)
           m.count(3) // no key with 3
           false
        else
           m[sum] = i
           m[3] = 6

        i++
        i = 7

Step 9: i < nums.size()
        7 < 8
        true

        sum = sum + (nums[i] == 1 ? 1 : -1)
            = 3 + (num[7] == 1 ? 1 : -1)
            = 3 + (0 == 1 ? 1 : -1)
            = 3 + -1
            = 2

        if m.count(sum)
           m.count(2) // has key with 0
           true

           maxLength = max(maxLength, i - m[sum])
                     = max(4, 7 - 5)
                     = max(4, 2)
                     = 4

        i++
        i = 8

Step 10: i < nums.size()
         8 < 8
         false

Step 11: return maxLength

So we return the answer as 4.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-contiguous-array)*。*