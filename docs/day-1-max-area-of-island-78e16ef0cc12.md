# 第一天:岛屿的最大面积

> 原文：<https://medium.com/nerd-for-tech/day-1-max-area-of-island-78e16ef0cc12?source=collection_archive---------25----------------------->

***问题链接:***

[](https://leetcode.com/explore/featured/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3764/) [## 探索- LeetCode

### 这个挑战是初学者友好的，对高级和非高级用户都可用。它由 30 个每日…

leetcode.com](https://leetcode.com/explore/featured/card/june-leetcoding-challenge-2021/603/week-1-june-1st-june-7th/3764/) 

***问题陈述:***

给你一个`m x n`二进制矩阵`grid`。岛屿是一组`1`(代表陆地)四向**(水平或垂直)相连的岛屿。)你可以假设网格的四个边都被水包围着。**

岛的**面积**是岛中具有值`1`的单元的数量。

返回*最大* ***区域*** *中的一个岛屿* `grid`。如果没有岛，返回`0`。

***例 1:***

```
**Input:** grid = [
[0,0,1,0,0,0,0,1,0,0,0,0,0],       
[0,0,0,0,0,0,0,1,1,1,0,0,0],
[0,1,1,0,1,0,0,0,0,0,0,0,0],
[0,1,0,0,1,1,0,0,1,0,1,0,0],
[0,1,0,0,1,1,0,0,1,1,1,0,0],
[0,0,0,0,0,0,0,0,0,0,1,0,0],
[0,0,0,0,0,0,0,1,1,1,0,0,0],
[0,0,0,0,0,0,0,1,1,0,0,0,0]]**Output:** 6**Explanation:** The answer is not 11, because the island must be connected 4-directionally.
```

***例 2:***

```
**Input:** grid = [[0,0,0,0,0,0,0,0]]
**Output:** 0
```

***约束:***

`1 <= m, n <= 50`

***我的解决方案:***

```
def getArea(row: int, col: int, grid: List[List[int]]):
        if(row < 0 or row >= len(grid) or col < 0 or col >= len(grid[row])):
            return 0
        if(grid[row][col] == 1):
            grid[row][col] = 0
            return 1 + (
                getArea(row + 1, col, grid) + 
                getArea(row - 1, col, grid) + 
                getArea(row, col - 1, grid) + 
                getArea(row, col + 1, grid)
            )
        return 0

def maxAreaOfIsland(grid: List[List[int]]) -> int:
    maximumArea = 0
    for row in range(len(grid)):
        for col in range(len(grid[row])):
            maximumArea = max(maximumArea, getArea(row, col, grid))
    return maximumArea
```

***解释:***

我们知道，岛屿被水包围，是由相邻的陆地水平或垂直连接而成的。为了探索岛屿，我们通过递归搜索顶部、底部、左侧和右侧的位置，找出所有与该陆地相连的陆地。每块土地占用面积值`1`。

一旦我们知道了我们的当前位置岛，为了将它标记为已访问，我们简单地通过将它标记为水，即 0(汇)，并递归地标记它周围的所有连接的陆地，从网格中移除已访问的岛。

现在我们知道如何找到一个岛，为了解决这个问题，在矩阵的每个位置，我们检查它是否是一个岛。如果我们发现了一个岛，我们把岛沉了，然后回来说我们发现了一个岛。最后，我们将发现岛屿的位置数量相加。

就是这样！

如果你有兴趣解决更多的问题，请跟随我，和我一起踏上这段旅程。

明天见！

干杯！