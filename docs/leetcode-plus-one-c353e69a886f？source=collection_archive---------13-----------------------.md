# LeetCode —加一

> 原文：<https://medium.com/nerd-for-tech/leetcode-plus-one-c353e69a886f?source=collection_archive---------13----------------------->

# 问题陈述

给定一个代表非负整数的非空的十进制数字数组，该整数加 1。

存储数字时，最高有效数字位于列表的开头，数组中的每个元素都包含一个数字。

你可以假设整数不包含任何前导零，除了数字 0 本身。

问题陈述摘自:【https://leetcode.com/problems/plus-one】T2

**例一:**

```
Input: digits = [1, 2, 3]
Output: [1, 2, 4]
Explanation: The array represents the integer 123.
```

**例 2:**

```
Input: digits = [4, 3, 2, 1]
Output: [4, 3, 2, 2]
Explanation: The array represents the integer 4321.
```

**例 3:**

```
Input: digits = [0]
Output: [1]
```

**约束:**

```
- <= digits.length <= 100 
- 0 <= digits[i] <= 9
```

# 说明

## 强力方法

我们想到的基本方法是将数组转换成数字，递增 1，然后以数组形式返回结果。

但是对于无法适应给定数据类型的非常大的数组，这种方法可能会失败。

## 从右向左递增

我们可以使用下面的算法来解决这个问题:

```
- start from the last index of the array and process each digit till the first index.

- if the current digit is smaller than 9, add one to the current digit,
and return the array else assign zero to the current digit.

- if the first element is 9, then it means all the digits are 9.
  - increase the array size by 1, and set the first digit as 1.

- return array
```

**C++解决方案**

```
class Solution {
public:
    vector<int> plusOne(vector<int> &digits)
    {
        int i, size = digits.size(), carry = 1;

        for (i = size - 1; i >= 0 && carry != 0; i--){
            carry += digits[i];
            digits[i] = carry % 10;
            carry = carry / 10;
        }

        if( carry != 0 ){
            digits.emplace(digits.begin(), 1);
        }

        return digits;
    }
};
```

**Golang 解决方案**

```
func plusOne(digits []int) []int {
    carry := 1

    for i:= len(digits) - 1; i >= 0; i-- {
        digits[i] += carry

        if digits[i] < 10 {
            return digits
        } else {
            digits[i] = 0
            carry = 1
        }
    }

    if carry != 0 {
        return append([]int{1}, digits...)
    } else {
        return digits
    }
}
```

**Javascript 解决方案**

```
var plusOne = function(digits) {
    for(let i = digits.length - 1; i >= 0; i--) {
        digits[i]++;

        if( digits[i] > 9 ){
            digits[i] = 0;
        } else{
            return digits;
        }
    }

    digits.unshift(1);
    return digits;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: digits = [8, 9, 9, 9]

Step 1: size = digits.size()
             = 4

        carry = 1

Step 2: i = size - 1
          = 4 - 1
          = 3

        i >= 0 && carry != 0
        3 >= 0 && 1 != 0
        true

        carry += digits[i]
              += digits[3]
              += 9

        carry = 10

        digits[3] = 10 % 10;
                  = 0

        carry = 10 / 10
              = 1

        i--
        i = 2

Step 3: i >= 0 && carry != 0
        2 >= 0 && 1 != 0
        true

        carry += digits[i]
              += digits[2]
              += 9

        carry = 10

        digits[2] = 10 % 10;
                  = 0

        carry = 10 / 10
              = 1

        i--
        i = 1

Step 4: i >= 0 && carry != 0
        1 >= 0 && 1 != 0
        true

        carry += digits[i]
              += digits[1]
              += 9

        carry = 10

        digits[1] = 10 % 10;
                  = 0

        carry = 10 / 10
              = 1

        i--
        i = 0

Step 4: i >= 0 && carry != 0
        0 >= 0 && 1 != 0
        true

        carry += digits[i]
              += digits[0]
              += 8

        carry = 9

        digits[1] = 9 % 10;
                  = 9

        carry = 9 / 10
              = 0

        i--
        i = -1

Step 5: carry != 0
        0 != 0
        false

Step 6: return digits

So the answer is [9, 0, 0, 0].
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-plus-one)*。*