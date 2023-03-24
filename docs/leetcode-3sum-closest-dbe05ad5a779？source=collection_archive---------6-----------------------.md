# LeetCode — 3Sum 最接近

> 原文：<https://medium.com/nerd-for-tech/leetcode-3sum-closest-dbe05ad5a779?source=collection_archive---------6----------------------->

# 问题陈述

给定一个长度为 **n** 的整数数组 **nums** 和一个整数 **target** ，在 **nums** 中找出三个整数，使得总和最接近 **target** 。

返回三个整数之和。

你可以假设每个输入都有一个解。

问题陈述摘自:[https://leetcode.com/problems/3sum-closest](https://leetcode.com/problems/3sum-closest)

**例 1:**

```
Input: nums = [-1, 2, 1, -4], target = 1 
Output: 2 
Explanation: The sum that is closest to the target is 2\. (-1 + 2 + 1 = 2).
```

**例 2:**

```
Input: nums = [0, 0, 0], target = 1 
Output: 0
```

**约束:**

```
- 3 <= nums.length <= 1000 
- -1000 <= nums[i] <= 1000 
- -10^4 <= target <= 10^4
```

# 说明

## 强力方法

解决该问题的一个简单方法是探索所有大小为 3 的子集，并跟踪**目标**和该子集之和之间的差异。然后返回 sum 和 target 之差最小的子集。

上述方法的 C++实现如下所示:

```
for (int i = 0; i < arr.size() ; i++)
{
    for(int j =i + 1; j < arr.size(); j++)
    {
        for(int k =j + 1; k < arr.size(); k++)
        {
            //update the closestSum
            if(abs(x - closestSum) > abs(x - (arr[i] + arr[j] + arr[k])))
                closestSum = (arr[i] + arr[j] + arr[k]);
        }
    }
}

return closestSum
```

由于我们使用了三个嵌套循环，上述方法的时间复杂度为 **O(N )** 并且不需要额外的空间，空间复杂度为 **O(1)** 。

## 双指针技术

通过对数组进行排序，可以提高算法的效率。一旦数组排序完毕，我们就可以使用双指针技术。

我们遍历数组并修复三元组的第一个元素。我们使用两点算法来找到最接近**目标的数字— nums[i]** 。更新最接近的总和。因为双指针占用线性时间，所以它比嵌套循环好。

**算法**

```
- if nums.size < 3
  - return 0

- sort(nums.begin(), nums.end())

- set i = 0, minDiff = INT_MAX
- initialize sum

- loop while i < nums.size() - 2
  - set left = i + 1
  - set right = nums.size() - 1

  - loop while right > left
    - set temp = nums[i] + nums[left] + nums[right]
    - set diff = abs(temp - target)

    - if diff == 0
      - return target

    - if diff < minDiff
      - minDiff = diff
      - sum = temp

    - if temp < target
      - left++
    - else
      - right++

  - loop end

  - i++
- loop end

- return sum
```

**C++解决方案**

```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        if(nums.size() < 3){
            return 0;
        }

        sort(nums.begin(), nums.end());

        int i = 0;
        int sum, minDiff = INT_MAX;

        while(i < nums.size() - 2){
            int left = i + 1;
            int right = nums.size() - 1;
            while(right > left){
                int temp = nums[i] + nums[left] + nums[right];
                int diff = abs(temp - target);

                if(diff == 0){
                    return target;
                }

                if(diff < minDiff){
                    minDiff = diff;
                    sum = temp;
                }

                if(temp < target){
                    left++;
                }
                else{
                    right--;
                }
            }
            i++;
        }

    return sum;
  }
};
```

**Golang 解决方案**

```
const MaxUint = ^uint(0)
const MaxInt = int(MaxUint >> 1)

func absInt(x int) int{
    if x < 0 {
        return x*-1
    }

    return x
}

func threeSumClosest(nums []int, target int) int {
    numsLength := len(nums)
    if numsLength < 3 {
        return 0
    }

    sort.Ints(nums)

    i, sum := 0, 0
    minDiff := MaxInt

    for i < numsLength - 2 {
        left := i + 1
        right := numsLength - 1

        for right > left {
            temp := nums[i] + nums[left] + nums[right]
            diff := absInt(temp - target)

            if diff == 0 {
                return target
            }

            if diff < minDiff {
                minDiff = diff
                sum = temp
            }

            if temp < target {
                left++
            } else {
                right--
            }
        }

        i++
    }

    return sum
}
```

**Javascript 解决方案**

```
var threeSumClosest = function(nums, target) {
    if(nums.length < 3) {
        return 0;
    }

    nums.sort();

    let i = 0, minDiff = Number.MAX_VALUE;
    let sum;

    while( i < nums.length - 2 ){
        let left = i + 1;
        let right = nums.length - 1

        while( right > left ){
            let temp = nums[i] + nums[left] + nums[right];
            let diff = Math.abs(temp - target);

            if( diff == 0 ){
                return target;
            }

            if( diff < minDiff ){
                minDiff = diff;
                sum = temp;
            }

            if( temp < target ){
                left++;
            } else {
                right--;
            }
        }
        i++;
    }

    return sum;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [-1, 2, 1, -4], target = 1

Step 1: if nums.size() < 3
           4 < 3
           false

Step 2: sort(nums.start(), nums.end())
          - [-4, -1, 1, 2]

Step 3: int i = 0;
        int sum, minDiff = INT_MAX;

Step 4: loop while i < nums.size() - 2
        0 < 4 - 2
        0 < 2
        true

        left = i + 1
             = 0 + 1
             = 1
        right = nums.size() - 1
              = 4 - 1
              = 3

        loop while right > left
        3 > 1
        true

        temp = nums[i] + nums[left] + nums[right]
             = nums[0] + nums[1] + nums[3]
             = -4 + -1 + 2
             = -3

        diff = abs(temp - target)
             = abs(-3 - 1)
             = abs(-4)
             = 4

        if diff == 0
           4 == 0
           false

        if diff < minDiff
           4 < INT_MAX
           true

           minDiff = diff
                   = 4
           sum = temp
               = -3

        if temp < target
           -3 < 4
           true

           left++
           left = 2

        loop while right > left
        3 > 2
        true

        temp = nums[i] + nums[left] + nums[right]
             = nums[0] + nums[2] + nums[3]
             = -4 + 1 + 2
             = -1

        diff = abs(temp - target)
             = abs(-1 - 1)
             = abs(-2)
             = 2

        if diff == 0
           2 == 0
           false

        if diff < minDiff
           2 < 4
           true

           minDiff = diff
                   = 2
           sum = temp
               = -1

        if temp < target
           -1 < 4
           true

           left++
           left = 3

        loop while right > left
        3 > 3
        false

        i++
        i = 1

Step 5: loop while i < nums.size() - 2
        1 < 4 - 2
        1 < 2
        true

        left = i + 1
             = 1 + 1
             = 2
        right = nums.size() - 1
              = 4 - 1
              = 3

        loop while right > left
        3 > 2
        true

        temp = nums[i] + nums[left] + nums[right]
             = nums[1] + nums[2] + nums[3]
             = -1 + 1 + 2
             = 2

        diff = abs(temp - target)
             = abs(2 - 1)
             = abs(1)
             = 1

        if diff == 0
           1 == 0
           false

        if diff < minDiff
           1 < 2
           true

           minDiff = diff
                   = 1
           sum = temp
               = 2

        if temp < target
           2 < 4
           true

           left++
           left = 3

        loop while right > left
        3 > 3
        false

        i++
        i = 2

Step 6: loop while i < nums.size() - 2
        2 < 4 - 2
        2 < 2
        false

Step 7: return sum
        return 2

So the answer is 2.
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-3sum-closest)*。*