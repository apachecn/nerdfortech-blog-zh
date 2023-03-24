# LeetCode —除自身以外的数组乘积

> 原文：<https://medium.com/nerd-for-tech/leetcode-product-of-array-except-self-fb40b9fcafdb?source=collection_archive---------4----------------------->

# 问题陈述

给定一个整数数组 **nums** ，返回*一个数组* **answer** 使得 answer【I】*等于 nums 中除 nums【I】*以外的所有元素的乘积。

nums 的任何前缀或后缀的乘积**保证**适合 32 位整数。

您必须编写一个在不使用除法运算的情况下运行时间为 O(n) 的算法。

问题陈述摘自:[https://leetcode.com/problems/product-of-array-except-self](https://leetcode.com/problems/product-of-array-except-self)

**例 1:**

```
Input: nums = [1, 2, 3, 4] 
Output: [24, 12, 8, 6]
```

**例 2:**

```
Input: nums = [-1, 1, 0, -3, 3] 
Output: [0, 0, 9, 0, 0]
```

**约束:**

```
- 2 <= nums.length <= 10^5 - 
-30 <= nums[i] <= 30 
- The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
```

**跟进**:你能解决 O(1)额外空间复杂度的问题吗？(输出数组不作为空间复杂度分析的额外空间。)

# 说明

## 强力方法

根据问题陈述，我们不能使用除法运算符。我们可以想到的第一种方法是使用两个嵌套的 for 循环，并在索引不匹配时将这两个数字相乘。

上述解决方案的一小段 C++代码如下所示:

```
vector<int> answer(nums.size(), 0);

for(int i = 0; i < nums.size(); i++){
    product = 1;

    for(int j = 0; j < nums.size(); j++){
        if(i != j){
            product *= nums[j];
        }
    }

    answer[i] = product;
}
```

上述方法的问题是时间复杂性。上述方法的时间复杂度为 **O(N )** 。

## 划一减税办法

我们可以通过从左到右和从右到左计算元素的乘积，将上面的解优化为 **O(N)** 。

让我们检查一下算法

```
- initialize vector<int>answer, i
- set product = 1

- loop for i = 0; i < nums.size(); i++
  - append answer.push_back(product)
  - set product = product * nums[i]

- reset product = 1

- loop for i = nums.size() - 1; i >= 0; i--
  - set answer[i] = answer[i]*product
  - product *= nums[i]

- return answer
```

## C++解决方案

```
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> answer;
        int product = 1, i;

        for(i = 0; i < nums.size(); i++){
            answer.push_back(product);
            product *= nums[i];
        }

        product = 1;
        for(i = nums.size() - 1; i >= 0; i--){
            answer[i] *= product;
            product *= nums[i];
        }

        return answer;
    }
};
```

## 戈朗溶液

```
func productExceptSelf(nums []int) []int {
    answer := make([]int, len(nums))
    product := 1

    for i := 0; i < len(nums); i++ {
        answer[i] = product
        product *= nums[i]
    }

    product = 1

    for i := len(nums) - 1; i >= 0; i-- {
        answer[i] *= product
        product *= nums[i]
    }

    return answer
}
```

## Javascript 解决方案

```
var productExceptSelf = function(nums) {
    var answer = [];
    let product = 1;

    for(let i = 0; i < nums.length; i++){
        answer[i] = product;
        product *= nums[i];
    }

    product = 1;

    for(let i = nums.length - 1; i >= 0; i--){
        answer[i] *= product;
        product *= nums[i];
    }

    return answer;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [1, 2, 3, 4]

Step 1: vector<int> answer
        int product = 1, i

Step 2: loop for i = 0; i < nums.size()
        0 < 4
        true

        answer.push_back(product)
        answer.push_back(1)
        answer = [1]

        product *= nums[i]
        product = product * nums[0]
                = 1 * 1
                = 1

        i++
        i = 1

Step 3: loop for i < nums.size()
        1 < 4
        true

        answer.push_back(product)
        answer.push_back(1)
        answer = [1, 1]

        product *= nums[i]
        product = product * nums[1]
                = 1 * 2
                = 2

        i++
        i = 2

Step 4: loop for i < nums.size()
        2 < 4
        true

        answer.push_back(product)
        answer.push_back(2)
        answer = [1, 1, 2]

        product *= nums[i]
        product = product * nums[2]
                = 2 * 3
                = 6

        i++
        i = 3

Step 5: loop for i < nums.size()
        3 < 4
        true

        answer.push_back(product)
        answer.push_back(6)
        answer = [1, 1, 2, 6]

        product *= nums[i]
        product = product * nums[3]
                = 6 * 4
                = 24

        i++
        i = 4

Step 6: loop for i < nums.size()
        4 < 4
        false

Step 7: product = 1

Step 8: loop for i = nums.size() - 1; i >= 0
        i = 4 - 1 = 3
        i >= 0
        3 >= 0
        true

        answer[i] *= product
                  = answer[3] * product
                  = 6 * 1
                  = 6

        product *= nums[i]
                 = product * nums[3]
                 = 1 * 4
                 = 4

        i--
        i = 2

Step 9: loop for i >= 0
        2 >= 0
        true

        answer[i] *= product
                  = answer[2] * product
                  = 2 * 4
                  = 8

        product *= nums[i]
                 = product * nums[2]
                 = 4 * 3
                 = 12

        i--
        i = 1

Step 10: loop for i >= 0
        1 >= 0
        true

        answer[i] *= product
                  = answer[1] * product
                  = 1 * 12
                  = 12

        product *= nums[i]
                 = product * nums[1]
                 = 12 * 2
                 = 24

        i--
        i = 0

Step 11: loop for i >= 0
        0 >= 0
        true

        answer[i] *= product
                  = answer[0] * product
                  = 1 * 24
                  = 24

        product *= nums[i]
                 = product * nums[0]
                 = 24 * 1
                 = 24

        i--
        i = -1

Step 12: loop for i >= 0
         -1 >= 0
         false

Step 13: return answer

So the answer is [24, 12, 8, 6]
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-product-of-array-except-self)*。*