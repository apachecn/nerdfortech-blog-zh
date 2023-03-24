# LeetCode —唯一路径

> 原文：<https://medium.com/nerd-for-tech/leetcode-unique-paths-1050e5e6d0e7?source=collection_archive---------10----------------------->

# 问题陈述

一个机器人位于一个 **m x n** 网格的左上角(在下图中标为“开始”)。

机器人在任何时候只能向下或向右移动。机器人正试图到达网格的右下角(下图中标有“完成”)。

有多少条可能的唯一路径？

问题陈述摘自:【https://leetcode.com/problems/unique-paths】T2

**例一:**

![](img/accfe89f2baed178b7a95672321c049d.png)

```
Input: m = 3, n = 7
Output: 28
```

**例 2:**

```
Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1\. Right -> Down -> Down
2\. Down -> Down -> Right
3\. Down -> Right -> Down
```

**例 3:**

```
Input: m = 7, n = 3
Output: 28
```

**例 4:**

```
Input: m = 3, n = 3
Output: 6
```

**约束:**

```
- 1 <= m, n <= 100
- It's guaranteed that the answer will be less than or equal to 2 * 10^9
```

# 说明

## 强力方法

根据问题陈述，机器人可以向下或向右移动。我们可以用递归来求计数。设 *numberOfPaths(m，n)* 表示到达网格中第 m 行第 n 列的路径数。*c++中的 numberOfPaths(m，n)* 可以递归地写成如下。

```
int numberOfPaths(int m, int n){
    if (m == 1 || n == 1)
        return 1;

    return numberOfPaths(m - 1, n) + numberOfPaths(m, n - 1);
}
```

上述解的时间复杂度为**指数级**。存在许多重叠的子问题，因此我们可以使用动态规划方法来避免重新计算重叠的子问题。

## 动态规划方法

我们可以通过使用上述递归方法以自下而上的方式构建临时 2D 数组 count[][]，来避免重新计算重叠的子问题。

```
int numberOfPaths(int m, int n){
    // create a 2D array to store results of sub-problems
    int count[m][n];

    // count of paths to reach any cell in first column is 1
    for (int i = 0; i < m; i++)
        count[i][0] = 1;

    // count of paths to reach any cell in first row is 1
    for (int j = 0; j < n; j++)
        count[0][j] = 1;

    for (int i = 1; i < m; i++) {
        for (int j = 1; j < n; j++)
            count[i][j] = count[i - 1][j] + count[i][j - 1];
    }

    return count[m - 1][n - 1];
}
```

上述程序的时间复杂度为 **O(mn)** 。空间复杂度为 **O(mn)** 。我们可以通过 **O(n)** 进一步减少空间，其中 n 是列的大小。

## 组合学方法

我们这里要计算 *m+n-2 C n-1* 哪个会是 **(m+n-2)！/ (n-1)！(m-1)！**

让我们检查一下如何计算上述公式的算法:

```
- set paths = 1

- loop for i = n; i < m + n - 1; i++
  - set paths = paths * i
  - update paths = paths / (i - n + 1)

- return paths
```

**C++解决方案**

```
class Solution {
public:
    int uniquePaths(int m, int n) {
        long int paths = 1;

        for(int i = n; i < m + n - 1; i++){
            paths *= i;
            paths /= (i - n + 1);
        }

        return int(paths);
    }
};
```

**Golang 解决方案**

```
func uniquePaths(m int, n int) int {
    paths := 1

    for i := n; i < m + n - 1; i++{
        paths *= i
        paths /= (i - n + 1)
    }

    return paths
}
```

**Javascript 解决方案**

```
var uniquePaths = function(m, n) {
    let paths = 1;

    for(let i = n; i < m + n - 1; i++){
        paths *= i;
        paths /= (i - n + 1);
    }

    return paths;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: m = 3, n = 7

Step 1: set paths = 1

Step 2: loop for i = n; i < m + n - 1
         i = 7
         7 < 7 + 3 - 1
         7 < 9
         7 < 9
         true

         paths = paths * i
         paths = 1 * 7
               = 7

         paths = paths / (i - n + 1)
               = 7 / (7 - 7 + 1)
               = 7 / 1
               = 7

         i++
         i = 8

Step 3: loop for i < m + n - 1
        8 < 8 + 3 - 1
        8 < 9
        8 < 9
        true

        paths = paths * i
        paths = 7 * 8
              = 56

        paths = paths / (i - n + 1)
              = 56 / (8 - 7 + 1)
              = 56 / 2
              = 28

        i++
        i = 9

Step 4: loop for i < m + n - 1
        9 < 8 + 3 - 1
        9 < 9
        false

Step 5: return paths

So we return answer as 28.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-unique-paths)*。*