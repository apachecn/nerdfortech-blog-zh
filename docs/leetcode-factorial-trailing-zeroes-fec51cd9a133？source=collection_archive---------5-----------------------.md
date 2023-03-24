# LeetCode —阶乘尾随零

> 原文：<https://medium.com/nerd-for-tech/leetcode-factorial-trailing-zeroes-fec51cd9a133?source=collection_archive---------5----------------------->

# 问题陈述

给定一个整数 *n* ，返回 *n 中尾随零的个数！*。

注意， *n！= n * (n — 1) * (n — 2) * … * 3 * 2 * 1* 。

问题陈述摘自:[https://leetcode.com/problems/factorial-trailing-zeroes](https://leetcode.com/problems/factorial-trailing-zeroes)

**例 1:**

```
Input: n = 3 
Output: 0 
Explanation: 3! = 6, no trailing zero.
```

**例 2:**

```
Input: n = 5 
Output: 1 
Explanation: 5! = 120, one trailing zero.
```

**例 3:**

```
Input: n = 0
Output: 0
```

**约束:**

```
- 0 <= n <= 10^4
```

# 说明

一个简单的方法是首先计算数字的阶乘，然后计算尾随零的数量。上述方法会导致较大数字的溢出。

想法是考虑阶乘 n 的质因数。尾随零是质因数 2 和 5 的结果。我们只需要数 2 和 5 的个数。

考虑例子 **n = 5** 。5 的质因数里有一个 5 和三个 2！。

```
5! = 5 * 4 * 3 * 2 * 1
   = 5 * 2^2 * 3 * 2
   = 2^3 * 3 * 5
```

而对于 **n = 11** ，我们有两个 5 和八个 2。

```
11! = 11 * 10 * 9 * 8 * 7 * 6 * 5 * 4 * 3 * 2 * 1
    = 2^8 * 3^4 * 5^2 * 7 * 11
```

我们很容易说 2 的数量大于 5 的数量。我们只需要计算质因数中 5 的个数，就完成了。

## 计算 n 的质因数中 5 的个数！

最简单的方法是计算楼层(n/5)。比如 7！有一个 5，10！有两个 5。但是对于 n 是 25，125 的情况，我们有不止一个 5。当我们考虑 29！我们得到一个额外的 5，尾随零的数目变成 6。为了处理这种情况，我们首先将 n 除以 5 并删除所有单个的 5，然后除以 25 以删除多余的 5，以此类推。

```
Trailing 0s in n! = floor(n/5) + floor(n/25) + floor(n/125) + ....
```

## C++解决方案

```
class Solution {
public:
    int trailingZeroes(int n) {
        int count = 0;

        for(long int i = 5; n / i >= 1; i *= 5){
            count += n/i;
        }

        return count;
    }
};
```

## 戈朗溶液

```
func trailingZeroes(n int) int {
    count := 0

    for i := 5; n / i >= 1; i *= 5 {
        count += n/i
    }

    return count
}
```

## Javascript 解决方案

```
var trailingZeroes = function(n) {
    let count = 0;

    for( let i = 5; n / i >= 1; i *= 5 ) {
        count += Math.floor(n / i);
    }

    return count;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: n = 29

Step 1: count = 0

Step 2: loop for i = 5; n / i >= 1
        28 / 5 >= 1
        5 >= 1
        true

        count = count + n / i
              = 0 + 29 / 5
              = 0 + 5
              = 5

        i *= 5
        i = 5 * 5
          = 25

Step 3: n / i >= 1
        28 / 25 >= 1
        1 >= 1
        true

        count = count + n / i
              = 5 + 29 / 25
              = 5 + 1
              = 6

        i *= 5
           = 25 * 5
           = 125

Step 4: n / i >= 1
        28 / 125 >= 1
        0 >= 1
        false

Step 5: return count

So we return the answer as 6.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-factorial-trailing-zeroes)*。*