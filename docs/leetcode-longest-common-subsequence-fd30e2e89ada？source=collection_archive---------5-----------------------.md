# LeetCode —最长公共子序列

> 原文：<https://medium.com/nerd-for-tech/leetcode-longest-common-subsequence-fd30e2e89ada?source=collection_archive---------5----------------------->

# 问题陈述

给定两个字符串 *text1* 和 *text2* ，返回它们最长的**公共子序列**的长度。如果没有**公共子序列**，返回 0。

字符串的**子序列**是从原始字符串生成的新字符串，其中删除了一些字符(可以没有)，而不改变剩余字符的相对顺序。

*   比如‘ace’就是‘abcde’的子序列。

两个字符串的**公共子序列**是两个字符串公共的子序列。

问题陈述摘自:[https://leetcode.com/problems/longest-common-subsequence](https://leetcode.com/problems/longest-common-subsequence)

**例 1:**

```
Input: text1 = 'abcde', text2 = 'ace'
Output: 3
Explanation: The longest common subsequence is 'ace' and its length is 3.
```

**例 2:**

```
Input: text1 = 'abc', text2 = 'abc'
Output: 3
Explanation: The longest common subsequence is 'abc' and its length is 3.
```

**例 3:**

```
Input: text1 = 'abc', text2 = 'def'
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

**约束:**

```
- 1 <= text1.length, text2.length <= 1000
- text1 and text2 consist of only lowercase English characters.
```

# 说明

## 递归

一个简单的解决方案是检查文本 1[1]的每个子序列..n] 查看它是否也是 *text2[1..m]* 。为了实现这一点，我们可以将问题分解成更小的子问题，直到解决方案变得微不足道。我们通过删除最后一个元素来缩短每个序列，将问题分解成子问题。我们递归地在缩短的字符串上执行相同的操作，并继续提取最后一部分，直到到达字符串的第一个字符。

删除字符串的最后一个字符取决于两种情况。

*   *文本 1* 和*文本 2* 都在同一个元素中结束

```
LCS(text1[1..n], text2[1..m]) = LCS(text1[1..n - 1], text2[1..m - 1]) if text1[n] == text2[m]
```

*   *文本 1* 和*文本 2* 不在同一元素中结束

我们举个例子来理解这一点。

```
text1 = 'abc'
text2 = 'abcde'
```

*文本 1* 以 *c* 结尾，而*文本 2* 以 *e* 结尾。我们可以从字符串 *text2* 中删除最后一个字符 *e* ，并将我们的问题简化为 *LCS(text1[1..n]，文本 2[1..m — 1])*

类似地，我们从字符串 *text1* 中删除最后一个字符 *c* ，并将我们的问题简化为 *LCS(text1[1..n — 1]，文本 2[1..m])* 。我们想计算最长公共子序列的长度。我们使用下面的方法来处理这种情况。

```
LCS(text1[1..n], text2[1..m]) = max(LCS(text1[1..n], text2[1..m - 1]), LCS(text1[1..n - 1], text2[1..m]))
```

上述方法的一个 C++片段如下:

```
int lcsLength(string text1, string text2, int n, int m) {
    if(n == 0 || m == 0) {
        return 0;
    } if(text1[n - 1] == text2[m - 1]) {
        return lcsLength(text1, text2, n - 1, m - 1) + 1;
    } return max(lcsLength(X, Y, m, n - 1), lcsLength(X, Y, m - 1, n));
}
```

上述方法的时间复杂度为**o(2 ^(n+m)**，空间复杂度为 **O(1)** 。

## 动态规划

上述方法有重叠的子问题。让我们为上面的例子构造一个部分来验证重叠的子问题。

```
text1 = 'abc'
text2 = 'abcde'n = text1.length
  = 3m = text2.length
  = 5 (3, 5)
                   ________|_________
                  |                  |
                (2, 5)             (3, 4)
                  |                  |
     _____________|                  |_____________
     |            |                  |             |
    (1, 5)      (2, 4)              (2, 4)        (3, 3) (2, 4) is computed twice.
```

子问题 *(2，4)* 计算两次。我们知道，具有**最优子结构**和**重叠子问题**的问题可以通过动态规划来解决。

我们先检查一下算法。

```
- set n = text1.size()
      m = text2.size()- initialize 2D array int dp[n + 1][m + 1]- loop for i = 1; i <= n; i++
    loop for j = 1; j <= m; j++
      if text1[i - 1] == text2[j - 1]
        - update dp[i][j] = dp[i - 1][j - 1] + 1
      else
        - update dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])- return dp[n][m]
```

上述方法的时间复杂度为 **O(N * M)** ，空间复杂度为 **O(N * M)** 。

让我们在 **C++** 、 **Golang** 和 **Javascript** 中检查一下我们的算法。

## C++解决方案

```
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int n = text1.size();
        int m = text2.size();
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0)); for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                if(text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = max(dp[i][j - 1], dp[i - 1][j]);
                }
            }
        } return dp[n][m];
    }
};
```

## 戈朗溶液

```
func longestCommonSubsequence(text1 string, text2 string) int {
    n := len(text1)
    m := len(text2)
    dp := make([][]int, n + 1) for i := range dp {
        dp[i] = make([]int, m + 1)
    } for i := 1; i <= n; i++ {
        for j := 1; j <= m; j++ {
            if text1[i - 1] == text2[j - 1] {
                dp[i][j] = dp[i - 1][j - 1] + 1
            } else {
                dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
            }
        }
    } return dp[n][m]
}func max(a, b int) int {
    if a > b {
        return a
    } return b
}
```

## Javascript 解决方案

```
var longestCommonSubsequence = function(text1, text2) {
    let n = text1.length;
    let m = text2.length;
    let dp = new Array(n + 1).fill(0); for (let i = 0; i < n + 1; i++) {
        dp[i] = new Array(m + 1).fill(0);
    } for(let i = 1; i <= n; i++) {
        for(let j = 1; j <= m; j++) {
            if(text1[i - 1] == text2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i- 1][j], dp[i][j - 1]);
            }
        }
    } return dp[n][m];
};
```

让我们为**示例 1** 预演我们的算法。

```
Input: text1 = 'abcde'
       text2 = 'ace'Step 1: n = text1.size()
          = 5
        m = text2.size()
          = 3Step 2: dp(n + 1, vector<int>(m + 1, 0))
        dp = [
               [0, 0, 0, 0],
               [0, 0, 0, 0],
               [0, 0, 0, 0],
               [0, 0, 0, 0],
               [0, 0, 0, 0],
               [0, 0, 0, 0]
             ]Step 3: loop for i = 1; i <= n
          1 <= 5
          true loop for j = 1; j <= m
            1 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[0] == text2[0]
               'a' == 'a'
               true dp[i][j] = dp[i - 1][j - 1] + 1
               dp[1][1] = dp[0][0] + 1
                        = 1 j++
            j = 2 loop for j <= m
            2 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[0] == text2[1]
               'a' == 'c'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[1][2] = max(dp[1][1], dp[0][2])
                        = max(1, 0)
                        = 1 j++
            j = 3 loop for j <= m
            3 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[0] == text2[2]
               'a' == 'e'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[1][3] = max(dp[1][2], dp[0][3])
                        = max(1, 0)
                        = 1 j++
            j = 4 loop for j <= m
            4 <= 3
            false i++
          i = 2 dp = [
               [0, 0, 0, 0],
               [0, 1, 1, 1],
               [0, 0, 0, 0],
               [0, 0, 0, 0],
               [0, 0, 0, 0],
               [0, 0, 0, 0]
             ]Step 4: loop for i = 1; i <= n
          2 <= 5
          true loop for j = 1; j <= m
            1 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[1] == text2[0]
               'b' == 'a'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[2][1] = max(dp[2][0], dp[1][1])
                        = max(0, 1)
                        = 1 j++
            j = 2 loop for j = 1; j <= m
            2 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[1] == text2[1]
               'b' == 'c'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[2][2] = max(dp[2][1], dp[1][2])
                        = max(1, 1)
                        = 1 j++
            j = 3 loop for j = 1; j <= m
            3 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[1] == text2[2]
               'b' == 'e'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[2][3] = max(dp[2][2], dp[1][3])
                        = max(1, 1)
                        = 1 j++
            j = 4 loop for j <= m
              4 <= 3
              false i++
          i = 3 dp = [
               [0, 0, 0, 0],
               [0, 1, 1, 1],
               [0, 1, 1, 1],
               [0, 0, 0, 0],
               [0, 0, 0, 0],
               [0, 0, 0, 0]
             ]Step 5: loop for i = 1; i <= n
          3 <= 5
          true loop for j = 1; j <= m
            1 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[3] == text2[0]
               'c' == 'a'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[3][1] = max(dp[3][0], dp[2][1])
                        = max(0, 1)
                        = 1 j++
            j = 2 loop for j = 1; j <= m
              2 <= 3
              true if text1[i - 1] == text2[j - 1]
                 text1[2] == text2[1]
                 'c' == 'c'
                 true dp[i][j] = dp[i - 1][j - 1] + 1
                 dp[3][2] = dp[2][1] + 1
                          = 1 + 1
                          = 2 j++
              j = 3 loop for j = 1; j <= m
              3 <= 3
              true if text1[i - 1] == text2[j - 1]
                 text1[2] == text2[2]
                 'c' == 'e'
                 false else
                 dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
                 dp[3][3] = max(dp[3][2], dp[2][3])
                          = max(2, 1)
                          = 2 j++
              j = 4 loop for j <= m
                4 <= 3
                false i++
            i = 4 dp = [
               [0, 0, 0, 0],
               [0, 1, 1, 1],
               [0, 1, 1, 1],
               [0, 1, 2, 2],
               [0, 0, 0, 0],
               [0, 0, 0, 0]
            ]Step 6: loop for i = 1; i <= n
          4 <= 5
          true loop for j = 1; j <= m
            1 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[3] == text2[0]
               'd' == 'a'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[4][1] = max(dp[4][0], dp[3][1])
                        = max(0, 1)
                        = 1 j++
            j = 2 loop for j = 1; j <= m
            2 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[3] == text2[1]
               'd' == 'c'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[4][2] = max(dp[4][1], dp[3][2])
                        = max(1, 2)
                        = 2 j++
            j = 3 loop for j = 1; j <= m
            3 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[3] == text2[2]
               'd' == 'e'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[4][3] = max(dp[4][2], dp[3][3])
                        = max(2, 2)
                        = 2 j++
            j = 4 loop for j <= m
              4 <= 3
              false i++
          i = 5 dp = [
               [0, 0, 0, 0],
               [0, 1, 1, 1],
               [0, 1, 1, 1],
               [0, 1, 2, 2],
               [0, 1, 2, 2],
               [0, 0, 0, 0]
             ]Step 7: loop for i = 1; i <= n
          5 <= 5
          true loop for j = 1; j <= m
            1 <= 3
            true if text1[i - 1] == text2[j - 1]
               text1[4] == text2[0]
               'e' == 'a'
               false else
               dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
               dp[5][1] = max(dp[5][0], dp[4][1])
                        = max(0, 1)
                        = 1 j++
            j = 2 loop for j = 1; j <= m
              2 <= 3
              true if text1[i - 1] == text2[j - 1]
                 text1[4] == text2[1]
                 'e' == 'c'
                 false else
                 dp[i][j] = max(dp[i][j - 1], dp[i - 1][j])
                 dp[5][2] = max(dp[5][1], dp[4][2])
                        = max(1, 2)
                        = 2 j++
              j = 3 loop for j = 1; j <= m
              3 <= 3
              true if text1[i - 1] == text2[j - 1]
                 text1[4] == text2[3]
                 'e' == 'e'
                 true dp[i][j] = dp[i - 1][j - 1] + 1
                 dp[5][3] = dp[4][2] + 1
                          = 2 + 1
                          = 3 j++
              j = 4 loop for j <= m
                4 <= 3
                false i++
            i = 6 dp = [
               [0, 0, 0, 0],
               [0, 1, 1, 1],
               [0, 1, 1, 1],
               [0, 1, 2, 2],
               [0, 1, 2, 2],
               [0, 1, 2, 3]
            ]Step 8: loop for i = 1; i <= n
          6 <= 5
          falseStep 9: return dp[n][m]
               dp[5][3] = 3We return the answer as 3.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-longest-common-subsequence)*。*