# LeetCode —合并间隔

> 原文：<https://medium.com/nerd-for-tech/leetcode-merge-intervals-80444a6b2d4b?source=collection_archive---------7----------------------->

# 问题陈述

给定一个*区间*数组，其中*区间[I]=【starti，迪恩】*，合并所有重叠区间，*返回一个非重叠区间数组，覆盖输入*中的所有区间。

问题陈述摘自:[https://leetcode.com/problems/merge-intervals](https://leetcode.com/problems/merge-intervals)

**例 1:**

```
Input: intervals = [[1, 3], [2, 6], [8, 10], [15, 18]]
Output: [[1, 6], [8, 10], [15, 18]]
Explanation: Since intervals [1, 3] and [2, 6] overlaps, merge them into [1, 6].
```

**例 2:**

```
Input: intervals = [[1, 4], [4, 5]]
Output: [[1, 5]]
Explanation: Intervals [1, 4] and [4, 5] are considered overlapping.
```

**约束:**

```
- 1 <= intervals.length <= 10^4 
- intervals[i].length == 2 
- 0 <= starti <= endi <= 10^4
```

# 说明

## 强力

蛮力法是从第一个区间开始，每隔一个区间比较一次。如果它与任何其他间隔重叠，请删除该间隔，并将其他间隔合并到第一个间隔中。

我们在第一次之后的剩余间隔重复这些相同的步骤。该方法的时间复杂度为 O(N ) 。

## 高效的解决方案:排序

一种有效的方法是首先按开始时间对时间间隔进行排序。一旦区间排序，我们就线性时间合并所有区间。如果区间[i]与区间[i — 1]重叠，那么我们合并这两个区间。如果没有，我们将这个区间加到最终答案中。

让我们检查下面的算法:

```
- sort the intervals array sort(intervals.begin(), intervals.end())

- initialize vector result

- loop for interval in intervals
  - if result.empty() || result.back()[1] < interval[0]
    - result.push_back({interval[0], interval[1]})
  - else
    - result.back()[1] = max(result.back()[1], interval[1])

- return result
```

## C++解决方案

```
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());

        vector<vector<int>> result;

        for(auto interval: intervals){
            if(result.empty() || (result.back()[1] < interval[0])){
                result.push_back({interval[0], interval[1]});
            } else {
                result.back()[1] = max(result.back()[1], interval[1]);
            }
        }

        return result;
    }
};
```

## 戈朗溶液

```
func merge(intervals [][]int) [][]int {
    result := [][]int{}

    sort.Slice(intervals, func(i, j int) bool {
        return intervals[i][0] < intervals[j][0]
    })

    for i, interval := range intervals {
        if i == 0 {
            result = append(result, interval)
            continue
        }

        lastInterval := result[len(result) - 1]

        if lastInterval[1] < interval[0] {
            result = append(result, interval)
        } else if interval[1] > lastInterval[1] {
            lastInterval[1] = interval[1]
        }
    }

    return result
}
```

## Javascript 解决方案

```
var merge = function(intervals) {
    intervals.sort((i, j) => {
        return i[0] - j[0];
    })

    let result = [];

    for(let i = 0; i < intervals.length; i++) {
        if(i == 0) {
            result.push(intervals[i]);
            continue
        }

        let lastInterval = result[result.length - 1];
        if(lastInterval[1] < intervals[i][0]) {
            result.push(intervals[i]);
        } else if (lastInterval[1] > intervals[i][0]) {
            lastInterval[1] = intervals[i][1];
        }
    }

    return result;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: intervals = [[1, 3], [2, 6], [8, 10], [15, 18]]

Step 1: sort(intervals.begin(), intervals.end())
        - intervals = [[1, 3], [2, 6], [8, 10], [15, 18]]

Step 2: vector<vector<int>> result

Step 3: loop for(auto interval: intervals)
        interval = [1, 3]

        - if result.empty() || (result.back()[1] < interval[0])
             true // as result is empty array
          - result.push_back({interval[0], interval[1]})
            result = [[1, 3]]

Step 4: for(auto interval: intervals)
        interval = [2, 6]

        - if result.empty() || (result.back()[1] < interval[0])
             false || (3 < 2)
             false || false
             false

        - else
          - result.back()[1] = max(result.back()[1], interval[1])
            result.back()[1] = max(3, 6)
            result.back()[1] = 6

            result = [[1, 6]]

Step 5: for(auto interval: intervals)
        interval = [8, 10]

        - if result.empty() || (result.back()[1] < interval[0])
             false || (6 < 8)
             false || true
             true
          - result.push_back({interval[0], interval[1]})
            result.push_back({8, 10})

            result = [[1, 6], [8, 10]]

Step 6: for(auto interval: intervals)
        interval = [15, 18]

        - if result.empty() || (result.back()[1] < interval[0])
             false || (10 < 15)
             false || true
             true

          - result.push_back({interval[0], interval[1]})
            result.push_back({15, 18})

            result = [[1, 6], [8, 10], [15, 18]]

Step 7: loop ends

Step 9: return result

So we return the result as [[1, 6], [8, 10], [15, 18]].
```

*最初发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-merge-intervals)*。*