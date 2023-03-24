# LeetCode —最长的连续子序列

> 原文：<https://medium.com/nerd-for-tech/leetcode-longest-consecutive-subsequence-1dd3edd0fbd2?source=collection_archive---------2----------------------->

给定一个未排序的整数数组 *nums* ，返回*最长连续元素序列*的长度。

你必须写一个在 *O(n)* 时间内运行的算法。

问题陈述摘自:[https://leetcode.com/problems/longest-consecutive-sequence](https://leetcode.com/problems/longest-consecutive-sequence)

**例 1:**

```
Input: nums = [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```

**例 2:**

```
Input: nums = [0, 3, 7, 2, 5, 8, 4, 6, 0, 1]
Output: 9
```

**约束:**

```
- 0 <= nums.length <= 10^5
- -10^9 <= nums[i] <= 10^9
```

# 说明

## 整理

最简单的解决方案是对数组进行排序，找出最长的连续元素。对数组排序后，我们删除重复的元素。我们运行一个循环，计算连续元素的当前数量和最大长度。如果当前数字不是前一个数字的连续元素，我们将计数器重置为 1，并更新最大长度。

上述方法的一个 C++片段如下:

```
int ans = 0, count = 0;

sort(array, array + n);

vector<int> v;
v.push_back(array[0]);

for (int i = 1; i < n; i++) {
    if (array[i] != array[i - 1])
        v.push_back(array[i]);
}

for (int i = 0; i < v.size(); i++) {
    if (i > 0 && v[i] == v[i - 1] + 1)
        count++;
    else
        count = 1;

    ans = max(ans, count);
}

return ans;
```

上述方法的时间复杂度为 **O(n(logn))，**，空间复杂度为 **O(n)** 。

## 散列表

我们可以通过使用 HashMap 将时间复杂度降低到 **O(n)** 。直接查算法吧。

```
- set n = nums.size()

- if n == 0
    return 0

- initialize map: map<int, int> m

- loop for i = 0; i < n; i++
  - m[nums[i]]++

- set prev = m.begin()
      maxLength = 1
      result = 0

- loop for i = m.begin(); i < m.end(); i++
  - if prev->first + 1 == i->first
    - maxLength++
  - else
    - result = max(result, maxLength)
    - maxLength = 1

  - prev = i

- return max(result, maxLength)
```

上述方法的时间复杂度为 **O(n)** ，空间复杂度为 **O(n)** 。

让我们在 **C++** 、 **Golang** 、 **Javascript** 中检查一下我们的算法。

## C++解决方案

```
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        int n = nums.size();
        if(n == 0) return 0;

        map<int, int> m;

        for(int i = 0; i < n; i++) {
            m[nums[i]];
        }

        auto prev = m.begin();
        int maxLength = 1, result = 0;

        for(auto i = m.begin(); i != m.end(); i++) {
            if(prev->first + 1 == i->first) {
                maxLength++;
            } else {
                result = max(result, maxLength);
                maxLength = 1;
            }

            prev = i;
        }

        return max(result, maxLength);
    }
};
```

## 戈朗溶液

```
func max(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func longestConsecutive(nums []int) int {
    n := len(nums)
    if n == 0 {
        return 0
    }

    m := make(map[int]int)

    for i := 0; i < n; i++ {
        m[nums[i]]++
    }

    maxLength, result := 1, 0

    for num := range m {
        if _, ok := m[num-1]; !ok {
            currNum := num
            maxLength = 1

            for {
                if _, ok := m[currNum+1]; ok {
                    currNum += 1
                    maxLength += 1
                } else {
                    break
                }
            }

            result = max(result, maxLength)
        }
    }

    return result
}
```

## Javascript 解决方案

```
var longestConsecutive = function(nums) {
    const n = nums.length;
    if(n === 0) {
        return 0
    }

    let set = new Set(nums)

    let maxLength = 1, result = 0

    for(let num of set) {
        if(set.has(num - 1)) {
            continue;
        }

        let current = num
        maxLength = 1

        while(set.has(current + 1)){
            current++
            maxLength++
        }

        result = Math.max(result, maxLength)
    }

    return result
};
```

让我们为**示例 1** 预演我们的算法。

```
Input: nums = [100, 4, 200, 1, 3, 2]

Step 1: int n = nums.size()
            n = 6

Step 2: n == 0
        6 == 0
        false

Step 3: map<int, int> m
        for(int i = 0; i < n; i++) {
            m[nums[i]];
        }

        m = {
            100: 0,
            4: 0,
            200: 0,
            1: 0,
            3: 0,
            2: 0,
        }

Step 4: prev = m.begin()
             = 1
        maxLength = 1
        result = 0

Step 5: loop for i = m.begin(); i != m.end()
            i = 1
            1 != m.end()
            true

            if prev->first + 1 == i->first
               1 + 1 == 1
               2 == 1
               false

            else
                result = max(result, maxLength)
                       = max(0, 1)
                       = 1

                maxLength = 1

            prev = i
                 = 1

            i++
            i -> 2

Step 6: loop for i != m.end()
            i = 2
            2 != m.end()
            true

            if prev->first + 1 == i->first
               1 + 1 == 2
               2 == 2
               true

               maxLength++
               maxLength = 2

            prev = i
                 = 2

            i++
            i -> 3

Step 7: loop for i != m.end()
            i = 3
            3 != m.end()
            true

            if prev->first + 1 == i->first
               2 + 1 == 3
               3 == 3
               true

               maxLength++
               maxLength = 3

            prev = i
                 = 3

            i++
            i -> 4

Step 8: loop for i != m.end()
            i = 4
            4 != m.end()
            true

            if prev->first + 1 == i->first
               3 + 1 == 4
               4 == 4
               true

               maxLength++
               maxLength = 4

            prev = i
                 = 4

            i++
            i -> 100

Step 9: loop for i != m.end()
            i = 100
            100 != m.end()
            true

            if prev->first + 1 == i->first
               4 + 1 == 100
               5 == 100
               false

            else
                result = max(result, maxLength)
                       = max(1, 4)
                       = 4

                maxLength = 1

            prev = i
                 = 100

            i++
            i -> 200

Step 10: loop for i != m.end()
            i = 200
            200 != m.end()
            true

            if prev->first + 1 == i->first
               100 + 1 == 200
               101 == 200
               false

            else
                result = max(result, maxLength)
                       = max(4, 4)
                       = 4

                maxLength = 4

            prev = i
                 = 200

            i++
            i -> end

Step 11: loop for i != m.end()
            false

Step 12: return max(result, maxLength)

We return the answer as 4.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-longest-consecutive-sequence)*。*