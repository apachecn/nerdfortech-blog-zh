# LeetCode —单一数字

> 原文：<https://medium.com/nerd-for-tech/leetcode-single-number-308915bcc307?source=collection_archive---------3----------------------->

# 问题陈述

给定一个**非空的**整数数组*num*，每个元素出现*两次*，除了一个。找到那个单身的。

您必须实现一个具有线性运行时复杂性的解决方案，并且只使用恒定的额外空间。

问题陈述摘自:[https://leetcode.com/problems/single-number](https://leetcode.com/problems/single-number)。

**例 1:**

```
Input: nums = [2, 2, 1]
Output: 1
```

**例 2:**

```
Input: nums = [4, 1, 2, 1, 2]
Output: 4
```

**例 3:**

```
Input: nums = [1]
Output: 1
```

**约束:**

```
- 1 <= nums.length <= 3 * 10^4
- -3 * 10^4 <= nums[i] <= 3 * 10^4
- Each element in the array appears twice except for one element which appears only once.
```

# 说明

## 强力解决方案

强力解决方案是检查每个元素是否出现一次。一旦找到只出现一次的元素，我们就返回该元素。上述方法的时间复杂度为 **O(N )** 。

通过使用散列，时间复杂度可以降低到 **O(N)** 。我们遍历数组中的所有元素，并将它们放入哈希表中。数组元素将是哈希表中的键，它的值将是该元素在数组中出现的次数。

这种方法的 C++代码片段如下:

```
int singleNumber(vector<int>& nums) {
    map<int, int> m;

    for(int i = 0; i < nums.size(); i++) {
        m[nums[i]]++;
    }

    for(auto const & [key, value]: m) {
        if(value == 1) {
            return key;
        }
    }

    return -1;
}
```

时间复杂度降低到了 **O(N)** ，但是空间复杂度增加到了 **O(N)** 。

## 优化解决方案

通过使用单个 int 变量，我们可以将空间复杂度降低到 **O(1)** 。我们可以用算术异或运算符 **^** 。当操作数相似时，XOR 运算符返回 0。

```
3 ^ 1
=> 2

3 ^ 2
=> 0

3 ^ 0
=> 3
```

由于数组中的每个元素除了一个以外都出现了两次，所以对所有重复元素的 XOR 运算将返回 0。并且对任何非零数字与零进行 XOR 运算将返回相同的数字。我们需要迭代数组，并对所有元素执行 XOR 运算。

现在我们来检查一下算法。

```
- initialize singleNum = 0

- loop for i = 0; i < nums.size(); i++
  - singleNum ^= nums[i]

- return singleNum
```

让我们来看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int singleNum = 0;

        for(int i = 0; i < nums.size(); i++) {
            singleNum ^= nums[i];
        }

        return singleNum;
    }
};
```

## 戈朗溶液

```
func singleNumber(nums []int) int {
    singleNum := 0

    for i := 0; i < len(nums); i++ {
        singleNum ^= nums[i]
    }

    return singleNum
}
```

## Javascript 解决方案

```
var singleNumber = function(nums) {
    let singleNum = 0;

    for(let i = 0; i < nums.length; i++) {
        singleNum ^= nums[i];
    }

    return singleNum;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [4, 1, 2, 1, 2]

Step 1: singleNum = 0

Step 2: loop for i = 0; i < nums.size()
          0 < 5
          true

          singleNum ^= nums[i]
                     = singleNum ^ nums[0]
                     = 0 ^ 4
                     = 4

          i++
          i = 1

Step 3: i < nums.size()
          1 < 5
          true

          singleNum ^= nums[i]
                     = singleNum ^ nums[1]
                     = 4 ^ 1
                     = 5

          i++
          i = 2

Step 4: i < nums.size()
          2 < 5
          true

          singleNum ^= nums[i]
                     = singleNum ^ nums[2]
                     = 5 ^ 2
                     = 7

          i++
          i = 3

Step 5: i < nums.size()
          3 < 5
          true

          singleNum ^= nums[i]
                     = singleNum ^ nums[3]
                     = 7 ^ 1
                     = 6

          i++
          i = 4

Step 6: i < nums.size()
          4 < 5
          true

          singleNum ^= nums[i]
                     = singleNum ^ nums[4]
                     = 6 ^ 2
                     = 4

          i++
          i = 5

Step 7: i < nums.size()
          5 < 5
          false

Step 8: return singleNum

So we return the answer as 4.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-single-number)*。*