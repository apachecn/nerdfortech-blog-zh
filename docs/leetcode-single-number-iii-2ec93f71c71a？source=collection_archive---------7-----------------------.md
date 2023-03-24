# LeetCode —单个数字 III

> 原文：<https://medium.com/nerd-for-tech/leetcode-single-number-iii-2ec93f71c71a?source=collection_archive---------7----------------------->

# 问题陈述

给定一个整数数组 *nums* ，其中恰好两个元素只出现一次，所有其他元素恰好出现两次。找出只出现一次的两个元素。可以任意顺序返回答案。

您必须编写一个算法，该算法以线性运行时复杂性运行，并且只使用恒定的额外空间。

问题陈述摘自:【https://leetcode.com/problems/single-number-iii】T2

**例一:**

```
Input: nums = [1, 2, 1, 3, 2, 5]
Output: [3, 5]
Explanation:  [5, 3] is also a valid answer.
```

**例 2:**

```
Input: nums = [-1, 0]
Output: [-1, 0]
```

**例 3:**

```
Input: nums = [0, 1]
Output: [0, 1]
```

**约束:**

```
- 2 <= nums.length <= 3 * 10^4
- 2^31 <= nums[i] <= 2^31 - 1
- Each integer in nums will appear twice, only two integers will appear once.
```

# 说明

## 整理

我们对数组元素进行排序，并比较相邻的元素。使用上面的方法，我们可以很容易地得到非重复元素。

上述方法的一个 C++片段如下:

```
sort(nums, nums + n);
vector<int> result;
for (int i = 0; i < n - 1; i = i + 2) {
    if (nums[i] != nums[i + 1]) {
        result.push_back(nums[i]);
        i = i - 1;
    }
}
if (result.size() == 1)
    result.push_back(nums[n - 1]);
return result;
```

程序的时间复杂度为 **O(nlog(n))** ，空间复杂度为 **O(1)** 。

## 散列表

这个问题可以用 HashMap 在 **O(n)** 中解决。我们在数组上运行一个循环，并计算数组中每个元素出现的次数。

我们迭代散列并打印出只出现过一次的两个数字。

上述方法的一个 C++片段如下:

```
int n = nums.size();
vector<int> result;

if(n == 0) {
    return result;
}

map<int, int> m;

for(int i = 0; i < n; i++) {
    m[nums[i]]++;
}

vector<int> result;

for(auto i = m.begin(); i != m.end(); i++) {
    if(i->second == 1) {
        result.push_back(i->first);
    }
}

return result;
```

程序的时间复杂度为 **O(n)** ，空间复杂度为 **O(n)** 。

## 异或运算符

使用异或运算，可以在时间复杂度为 **O(n)** 和空间复杂度为 **O(1)** 条件下求解该程序。

设 a 和 b 是在 nums 数组中恰好出现一次的元素。我们首先计算所有数组元素的异或。

```
xor = nums[0]^nums[1]^nums[2]....nums[n - 1]
```

在上述 *xor* 变量中设置的所有位将被设置在一个非重复元素 a 或 b 中。

我们取 *xor* 的任意设置位，并将数组的元素分成两组。一组元素设置了相同的位，而另一组元素没有设置相同的位。

我们对第一个集合中的所有元素进行 XOR 运算，将返回第一个非重复元素，对另一个集合进行同样的运算，将返回第二个非重复元素。

我们先检查一下算法。

```
- set xorResult = nums[0]
  a = 0, b = 0, i = 0
  vector<int> result- loop for i = 1; i < nums.size(); i++
  - xorResult ^= nums[i]- set setBitNo = xorResult == INT_MIN ? 0 : xorResult & ~(xorResult - 1)- loop for i = 0; i < nums.size(); i+=
  - if nums[i] & setBitNo
    - a ^= nums[i]
  - else
    - b ^= nums[i]- result.push_back(a)
- result.push_back(b)- return result
```

上述方法的时间复杂度为 **O(n)** ，空间复杂度为 **O(1)** 。

让我们在 **C++** 、 **Golang** 、 **Javascript** 中检查一下我们的算法。

## C++解决方案

```
class Solution {
public:
    vector<int> singleNumber(vector<int>& nums) {
        int xorResult = nums[0];
        int a = 0, b = 0, i;
        vector<int> result;

        for(i = 1; i < nums.size(); i++) {
            xorResult ^= nums[i];
        }

        int setBitNo = xorResult == INT_MIN ? 0 : xorResult & ~(xorResult - 1);
        for(i = 0; i < nums.size(); i++) {
            if(nums[i] & setBitNo) {
                a ^= nums[i];
            } else {
                b ^= nums[i];
            }
        }
        result.push_back(a);
        result.push_back(b);
        return result;
    }
};
```

## 戈朗溶液

```
func singleNumber(nums []int) []int {
    xorResult := 0

    for _, num := range nums {
        xorResult = xorResult ^ num
    }
    setBitNo := xorResult & (-xorResult)
    result := make([]int, 2)
    for _, num := range nums {
        if num & setBitNo == 0 {
            result[0] ^= num
        } else {
            result[1] ^= num
        }
    }
    return result
}
```

## Javascript 解决方案

```
var singleNumber = function(nums) {
    let xorResult = 0;
    let a = 0, b = 0, i = 0;

    for(i = 0; i < nums.length; i++){
        xorResult ^= nums[i];
    }

    let setBitNo = xorResult & ~(xorResult - 1);

    for(i = 0; i < nums.length; i++) {
        if((nums[i] & setBitNo) === 0)
            a ^= nums[i];
        else
            b ^= nums[i];
    }

    return [a, b];
};
```

让我们为**示例 1** 预演我们的算法。

```
Input: nums = [1, 2, 1, 3, 2, 5]

Step 1: xorResult = nums[0]
                  = 1

Step 2: int a = 0, b = 0, i
        vector<int> result

Step 3: loop for i = 1; i < nums.size(); i++
            xorResult ^= nums[i]
        The xorResult is 6 (0110)

Step 4: setBitNo = xorResult == INT_MIN ? 0 : xorResult & ~(xorResult - 1)
                 = 6 == INT_MIN ? 0 : xorResult & ~(xorResult - 1)
                 = false ? 0 : xorResult & ~(xorResult - 1)
                 = xorResult & ~(xorResult - 1)
                 = 6 & ~(6 - 1)
                 = 6 & ~5
                 = 6 & -6
                 = 2

Step 5: loop for i = 0; i < nums.size()
            0 < 6
            true
            if nums[i] & setBitNo
               nums[0] & 2
               1 & 2
               0001 & 0010
               0
               false
            else
               b ^= nums[i]
                  = b ^ nums[i]
                  = 0 ^ nums[0]
                  = 0 ^ 1
                  = 1
        i++
        i = 1

Step 6: loop i < nums.size()
            1 < 6
            true
            if nums[i] & setBitNo
               nums[1] & 2
               2 & 2
               0010 & 0010
               2
               true
               a ^= nums[i]
                  = a ^ nums[1]
                  = 0 ^ 2
                  = 2
        i++
        i = 2

Step 7: loop i < nums.size()
            2 < 6
            true
            if nums[i] & setBitNo
               nums[2] & 2
               1 & 2
               0001 & 0010
               0
               false
            else
               b ^= nums[i]
                  = b ^ nums[i]
                  = 1 ^ nums[2]
                  = 1 ^ 1
                  = 0
        i++
        i = 3

Step 8: loop i < nums.size()
            3 < 6
            true
            if nums[i] & setBitNo
               nums[3] & 2
               3 & 2
               0011 & 0010
               2
               true
                a ^= nums[i]
                  = a ^ nums[3]
                  = 2 ^ 3
                  = 1
        i++
        i = 4

Step 9: loop i < nums.size()
            4 < 6
            true
            if nums[i] & setBitNo
               nums[4] & 2
               2 & 2
               0010 & 0010
               2
               true
               a ^= nums[i]
                  = a ^ nums[4]
                  = 1 ^ 2
                  = 3
        i++
        i = 5

Step 10: loop i < nums.size()
            5 < 6
            true
            if nums[i] & setBitNo
               nums[5] & 2
               5 & 2
               0101 & 0010
               0
               false
            else
              b ^= nums[i]
                  = b ^ nums[i]
                  = 0 ^ nums[5]
                  = 0 ^ 5
                  = 5
        i++
        i = 6

Step 11: loop i < nums.size()
            6 < 6
            false

Step 12: result.push_back(a)
         result.push_back(3)
         result = [3]

         result.push_back(b)
         result.push_back(5)
         result = [3, 5]

         return result
We return the result as [3, 5].
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-single-number-iii)*。*