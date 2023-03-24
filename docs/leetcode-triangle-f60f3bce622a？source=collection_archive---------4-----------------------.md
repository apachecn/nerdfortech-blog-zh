# LeetCode —三角形

> 原文：<https://medium.com/nerd-for-tech/leetcode-triangle-f60f3bce622a?source=collection_archive---------4----------------------->

# 问题陈述

给定一个三角形数组，返回*从上到下的最小路径和*。

对于每一步，您可以移动到下一行的相邻数字。更正式地说，如果您在当前行的索引 I 上，您可以移动到下一行的索引 I 或索引 i + 1。

问题陈述摘自:【https://leetcode.com/problems/triangle[。](https://leetcode.com/problems/triangle)

**例一:**

```
Input: triangle = [[2], [3, 4], [6, 5, 7], [4, 1, 8, 3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
```

**例 2:**

```
Input: triangle = [[-10]] 
Output: -10
```

**约束:**

```
- 1 <= triangle.length <= 200 
- triangle[0].length == 1 
- triangle[i].length == triangle[i - 1].length + 1 
- -10^4 <= triangle[i][j] <= 10^4
```

# 说明

## 动态规划

乍一看，我们可能首先想到 DFS 遍历。但是如果我们仔细观察，我们可以用动态规划来解决这个问题。我们只能在下一个向量集中选取索引 **i** 或 **i + 1** 。这使得很容易存储子问题的解，并使用这些重叠的子问题来获得最小和路径。

我们可以遵循自顶向下的方法或自底向上的方法来获得我们需要的解决方案。自底向上的方法非常简单。我们将最下面一行的节点存储在一个数组中。我们逐行移动到最上面的节点，以及它下面的两个数字中较小的一个。

我们先检查一下算法。

```
- set n = triangle.size()

- set pathSums array size to bottom-most row size
  vector<int> pathSums(triangle.back())

- loop for layer = n - 2; layer >=0; layer--
  - loop for i = 0; i <= layer; i++
    - set pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i]

- return pathSums[0]
```

让我们看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    int minimumTotal(vector<vector<int>>& triangle) {
        int n = triangle.size();
        vector<int> pathSums(triangle.back());

        for (int layer = n - 2; layer >= 0; layer--) {
            for (int i = 0; i <= layer; i++) {
                pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i];
            }
        }

        return pathSums[0];
    }
};
```

## 戈朗溶液

```
func min(a, b int) int {
    if a < b {
        return a
    }

    return b
}

func minimumTotal(triangle [][]int) int {
    n := len(triangle)
    pathSums := triangle[n - 1]

    for layer := n - 2; layer >= 0; layer-- {
        for i := 0; i <= layer; i++ {
            pathSums[i] = min(pathSums[i], pathSums[i+1]) + triangle[layer][i]
        }
    }

    return pathSums[0]
}
```

## Javascript 解决方案

```
var minimumTotal = function(triangle) {
    let n = triangle.length;
    let pathSums = triangle[n - 1];

    for (let layer = n - 2; layer >= 0; layer--) {
        for (let i = 0; i <= layer; i++) {
            pathSums[i] = Math.min(pathSums[i], pathSums[i + 1]) + triangle[layer][i];
        }
    }

    return pathSums[0];
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: triangle = [[2], [3, 4], [6, 5, 7], [4, 1, 8, 3]]

Step 1: n = triangle.size()
          = 4

        vector<int> pathSums(triangle.back())
        pathSums = [4, 1, 8, 3]

Step 2: loop for layer = n - 2; layer >= 0
          layer = 4 - 2
                = 2

          layer >= 0
          2 >= 0
          true

          loop i = 0; i <= layer
            0 <= 2
            true

            pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i]
            pathSums[0] = min(pathSums[0], pathSums[1]) + triangle[2][0]
                        = min(4, 1) + 6
                        = 1 + 6
                        = 7

            i++
            1 <= 2
            true

            pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i]
            pathSums[1] = min(pathSums[1], pathSums[2]) + triangle[2][1]
                        = min(1, 8) + 5
                        = 1 + 5
                        = 6

            i++
            i = 2
            2 <= 2
            true

            pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i]
            pathSums[2] = min(pathSums[2], pathSums[3]) + triangle[2][2]
                        = min(8, 3) + 7
                        = 3 + 7
                        = 10

            i++
            i = 3
            3 <= 2
            false

          layer--
          layer = 1

          pathSums = [7, 6, 10, 3]

Step 3: loop for layer >= 0
          1 >= 0
          true

          loop i = 0; i <= layer
            0 <= 1
            true

            pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i]
            pathSums[0] = min(pathSums[0], pathSums[1]) + triangle[1][0]
                        = min(7, 6) + 3
                        = 6 + 3
                        = 9

            i++
            i = 1
            i <= layer
            1 <= 1
            true

            pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i]
            pathSums[1] = min(pathSums[1], pathSums[2]) + triangle[1][1]
                        = min(6, 10) + 4
                        = 6 + 4
                        = 10

            i++
            i = 2
            i <= layer
            2 <= 1
            false

          layer--
          layer = 0

          pathSums = [9, 10, 10, 3]

Step 4: loop for layer >= 0
          0 >= 0
          true

          loop i = 0; i <= layer
            0 <= 0
            true

            pathSums[i] = min(pathSums[i], pathSums[i + 1]) + triangle[layer][i]
            pathSums[0] = min(pathSums[0], pathSums[1]) + triangle[0][0]
                        = min(9, 10) + 2
                        = 9 + 2
                        = 11

            i++
            i = 1
            i <= layer
            1 <= 0
            false

        layer--
        layer = -1

Step 5: loop for layer >= 0
          -1 >= 0
          false

Step 6: return pathSums[0]

So we return the answer as 11.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-triangle)*。*