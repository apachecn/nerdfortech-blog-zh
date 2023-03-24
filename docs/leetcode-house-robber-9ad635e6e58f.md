# leet code——入室抢劫犯

> 原文：<https://medium.com/nerd-for-tech/leetcode-house-robber-9ad635e6e58f?source=collection_archive---------1----------------------->

# 问题陈述

你是一个专业的强盗，计划沿街抢劫房屋。每栋房子都藏了一定数量的钱，阻止你抢劫每栋房子的唯一限制是相邻的房子都连接了安全系统，如果两栋相邻的房子在同一个晚上被闯入，它会自动联系警察。

给定一个整数数组 *nums* 代表每家的钱数，返回*你今晚可以抢劫的最大金额* ***而不惊动警察*** 。

**例 1:**

```
Input: nums = [1, 2, 3, 1] 
Output: 4 
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3). Total amount you can rob = 1 + 3 = 4.
```

**例 2:**

```
Input: nums = [2, 7, 9, 3, 1] 
Output: 12 
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1). Total amount you can rob = 2 + 9 + 1 = 12.
```

**约束:**

```
- 1 <= nums.length <= 100 
- <= nums[i] <= 400
```

# 说明

## 动态规划

我们可以把这个问题简化为寻找没有两个选定元素相邻的最大和子序列。解决这个问题的方法是使用动态规划。所以有两种情况。

1.  如果选择了该元素，则无法选择下一个相邻元素。
2.  如果没有选择一个元素，那么可以选择下一个元素。

上述方法的一个 C++片段如下:

```
int rob(vector<int>& nums ){
    int n = nums.size();

    if (n == 0)
        return 0;
    if (n == 1)
        return nums[0];
    if (n == 2)
        return max(nums[0], nums[1]);

    int dp[n];

    dp[0] = nums[0];
    dp[1] = max(nums[0], nums[1]);

    for (int i = 2; i<n; i++)
        dp[i] = max(nums[i]+dp[i-2], dp[i-1]);

    return dp[n-1];
}
```

上述方法的时间和空间复杂度为 **O(N)** 。

## 有效的方法:使用两个变量

如果我们仔细观察动态编程方法，我们会发现在计算一个指数的值时，前面两个指数的值很重要。我们可以用两个变量代替 DP 数组。

我们先检查一下算法。

```
- set evenSum, oddSum = 0, 0

- loop for i = 0; i < nums.size(); i++
  - if i % 2 == 0 // even index
    - evenSum += nums[i]
    - evenSum = evenSum > oddSum ? evenSum : oddSum
  - else
    - oddSum += nums[i]
    - oddSum = evenSum > oddSum ? evenSum : oddSum

- return evenSum > oddSum ? evenSum: oddSum
```

上述方法的时间复杂度为 **O(N)** 而空间复杂度则降为 **O(1)** 。

## C++解决方案

```
class Solution {
public:
    int rob(vector<int>& nums) {
        int evenSum = 0, oddSum = 0;

        for(int i = 0; i < nums.size(); i++){
            if(i % 2 == 0){
                evenSum += nums[i];
                evenSum = evenSum > oddSum ? evenSum : oddSum;
            } else {
                oddSum += nums[i];
                oddSum = evenSum > oddSum ? evenSum : oddSum;
            }
        }

        return evenSum > oddSum ? evenSum: oddSum;
    }
};
```

## 戈朗溶液

```
func rob(nums []int) int {
    evenSum, oddSum := 0, 0

    for i := 0; i < len(nums); i++ {
        if i % 2 == 0 {
            evenSum += nums[i]

            if evenSum < oddSum {
                evenSum = oddSum
            }
        } else {
            oddSum += nums[i]

            if oddSum < evenSum {
                oddSum = evenSum
            }
        }
    }

    if evenSum > oddSum {
        return evenSum
    }

    return oddSum
}
```

## Javascript 解决方案

```
var rob = function(nums) {
    let evenSum = 0, oddSum = 0;

    for(let i = 0; i < nums.length; i++) {
        if( i % 2 == 0 ) {
            evenSum += nums[i];
            evenSum = evenSum > oddSum ? evenSum : oddSum;
        } else {
            oddSum += nums[i];
            oddSum = evenSum > oddSum ? evenSum : oddSum;
        }
    }

    return evenSum > oddSum ? evenSum : oddSum;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [2, 7, 9, 3, 1]

Step 1: evenSum = 0
        oddSum = 0

Step 2: loop for i = 0; i < nums.size()
        0 < 5
        true

        i % 2 == 0
        0 % 2 == 0
        true

        evenSum = evenSum + nums[i]
                = 0 + nums[0]
                = 2

        evenSum = evenSum > oddSum ? evenSum : oddSum
                = 2 > 0
                = true
                = 2

        i++
        i = 1

Step 3: loop for i < nums.size()
        1 < 5
        true

        i % 2 == 0
        1 % 2 == 0
        false

        oddSum = oddSum + nums[i]
                = 0 + nums[1]
                = 7

        oddSum = evenSum > oddSum ? evenSum : oddSum
               = 2 > 7
               = false
               = 7

        i++
        i = 2

Step 4: loop for i < nums.size()
        2 < 5
        true

        i % 2 == 0
        2 % 2 == 0
        true

        evenSum = evenSum + nums[i]
                = 2 + nums[2]
                = 2 + 9
                = 11

        evenSum = evenSum > oddSum ? evenSum : oddSum
                = 11 > 7
                = true
                = 11

        i++
        i = 3

Step 5: loop for i < nums.size()
        3 < 5
        true

        i % 2 == 0
        3 % 2 == 0
        false

        oddSum = oddSum + nums[i]
                = 7 + nums[3]
                = 7 + 3
                = 10

        oddSum = evenSum > oddSum ? evenSum : oddSum
               = 11 > 10
               = true
               = 11

        i++
        i = 4

Step 6: loop for i < nums.size()
        4 < 5
        true

        i % 2 == 0
        4 % 2 == 0
        true

        evenSum = evenSum + nums[i]
                = 11 + nums[4]
                = 11 + 1
                = 12

        evenSum = evenSum > oddSum ? evenSum : oddSum
                = 12 > 11
                = true
                = 12

        i++
        i = 5

Step 7: loop for i < nums.size()
        5 < 5
        false

Step 8: return evenSum > oddSum ? evenSum : oddSum
        12 > 11
        true

So we return the answer as 12.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-house-robber)*。*