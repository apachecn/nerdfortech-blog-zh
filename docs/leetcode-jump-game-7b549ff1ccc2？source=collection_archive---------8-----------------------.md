# LeetCode —跳跃游戏

> 原文：<https://medium.com/nerd-for-tech/leetcode-jump-game-7b549ff1ccc2?source=collection_archive---------8----------------------->

# 问题陈述

给你一个整数数组 **nums** 。您最初位于数组的**第一个索引**，数组中的每个元素代表您在该位置的最大跳跃长度。

如果可以到达最后一个索引，则返回 **true** ，否则返回 **false** 。

问题陈述摘自:[https://leetcode.com/problems/jump-game](https://leetcode.com/problems/jump-game)

**例 1:**

```
Input: nums = [2, 3, 1, 1, 4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**例 2:**

```
Input: nums = [3, 2, 1, 0, 4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

**约束:**

```
- 1 <= nums.length <= 10^4 
- 0 <= nums[i] <= 10^5
```

# 说明

## 强力方法

一种简单的方法是从第一个元素开始，递归调用从第一个元素可以到达的所有元素。我们可以用下面的方法来解决这个问题。

```
minJumps(start, end) = Min ( minJumps(k, end) ) for all k reachable from start
```

上述方法的一小段 C++代码如下所示:

```
int minJumps(int arr[], int n){
    if (n == 1)
        return 0;

    int res = INT_MAX;
    for (int i = n - 2; i >= 0; i--) {
        if (i + arr[i] >= n - 1) {
            int sub_res = minJumps(arr, i + 1);
            if (sub_res != INT_MAX)
                res = min(res, sub_res + 1);
        }
    }

    return res;
}
```

由于从一个元素移动有 N 种最大可能的方式，上述方法的时间复杂度为 **O(N )** 。

## 优化解决方案

这个问题可以在线性时间内解决。我们需要确定我们可以从当前索引 **i** 中获得的最大跳转。只有当当前跳转大于最大跳转时，我们才使用该索引并递增计数。

让我们检查下面的算法:

```
- set max = nums[0] the first element of the array.

- if nums.size() == 1 && nums[0] == 0
  - return true

- loop for i = 0; i < nums.size(); i++
  - if max <= i && nums[i] == 0
    - return false

  - if i + nums[i] > max
    - max = i + nums[i]

  - if max >= nums.length - 1
    - return true

- return false
```

## C++解决方案

```
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int max = nums[0];

        if(nums.size() == 1 && nums[0] == 0)
            return true;

        for(int i = 0; i < nums.size(); i++){
            if(max <= i && nums[i] == 0)
                return false;

            if(i + nums[i] > max)
                max = i + nums[i];

            if(max >= nums.size() - 1)
                return true;
        }

        return false;
    }
};
```

## 戈朗溶液

```
func canJump(nums []int) bool {
    max := nums[0]
    length := len(nums)

    if length == 1 && nums[0] == 0 {
        return true
    }

    for i := 0; i < length; i++ {
        if max <= i && nums[i] == 0 {
            return false
        }

        if i + nums[i] > max {
            max = i + nums[i]
        }

        if max >= length - 1 {
            return true
        }
    }

    return false
}
```

## Javascript 解决方案

```
var canJump = function(nums) {
    let max = nums[0];
    const size = nums.length;

    if( size == 1 && nums[0] == 0 ){
        return true;
    }

    for(let i = 0; i < size; i++){
        if( max <= i && nums[i] == 0 ){
            return false;
        }

        if( i + nums[i] > max ){
            max = i + nums[i];
        }

        if( max >= size - 1 ){
            return size;
        }
    }

    return false;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [2, 3, 1, 1, 4]

Step 1: max = nums[0]
            = 2

Step 2: if nums.size() == 1 && nums[0] == 0
           5 == 1 && 2 == 0
           false

Step 3: loop for i = 0; i < nums.size()
        0 < 5
        true

        max <= i && nums[i] == 0
        2 <= 0 && nums[0] == 0
        2 <= 0 && 2 == 0
        false

        i + nums[i] > max
        0 + nums[0] > 2
        0 + 2 > 2
        false

        max >= nums.size() - 1
        2 >= 5 - 1
        2 >= 4
        false

        i++
        i = 1

Step 4: i < nums.size()
        1 < 5
        true

        max <= i && nums[i] == 0
        2 <= 1 && nums[1] == 0
        2 <= 1 && 3 == 0
        false

        i + nums[i] > max
        1 + nums[1] > 2
        1 + 3 > 2
        4 > 2
        true

        max = i + nums[i]
            = 1 + nums[1]
            = 1 + 3
            = 4

        max >= nums.size() - 1
        4 >= 5 - 1
        4 >= 4
        true

        return true

So the answer we return is true.
```

让我们试运行一下负面测试用例。

```
Input: nums = [3, 2, 1, 0, 4]

Step 1: max = nums[0]
            = 3

Step 2: if nums.size() == 1 && nums[0] == 0
           5 == 1 && 3 == 0
           false

Step 3: loop for i = 0; i < nums.size()
        0 < 5
        true

        max <= i && nums[i] == 0
        3 <= 0 && nums[0] == 0
        3 <= 0 && 3 == 0
        false

        i + nums[i] > max
        0 + nums[3] > 3
        0 + 3 > 3
        false

        max >= nums.size() - 1
        3 >= 5 - 1
        3 >= 4
        false

        i++
        i = 1

Step 4: i < nums.size()
        1 < 5
        true

        max <= i && nums[i] == 0
        3 <= 1 && nums[2] == 0
        3 <= 1 && 2 == 0
        false

        i + nums[i] > max
        1 + nums[2] > 3
        1 + 2 > 3
        3 > 3
        false

        max >= nums.size() - 1
        3 >= 5 - 1
        3 >= 4
        false

        i++
        i = 2

Step 5: i < nums.size()
        2 < 5
        true

        max <= i && nums[i] == 0
        3 <= 2 && nums[2] == 0
        3 <= 2 && 1 == 0
        false

        i + nums[i] > max
        2 + nums[2] > 3
        2 + 1 > 3
        3 > 3
        false

        max >= nums.size() - 1
        3 >= 5 - 1
        3 >= 4
        false

        i++
        i = 3

Step 6: i < nums.size()
        3 < 5
        true

        max <= i && nums[i] == 0
        3 <= 3 && nums[3] == 0
        3 <= 3 && 0 == 0
        true

        return false

So the answer we return is false.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-jump-game)*。*