# LeetCode —子集 II

> 原文：<https://medium.com/nerd-for-tech/leetcode-subsets-ii-80dc4c8edf64?source=collection_archive---------1----------------------->

# 问题陈述

给定一个可能包含重复项的整数数组 *nums* ，返回*所有可能的子集(幂集)*。

解集**不能**包含重复的子集。以**任意顺序**返回解决方案。

问题陈述摘自:[https://leetcode.com/problems/subsets-ii](https://leetcode.com/problems/subsets-ii)。

**例 1:**

```
Input: nums = [1, 2, 2]
Output: [[], [1], [1, 2], [1, 2, 2], [2], [2, 2]]
```

**例 2:**

```
Input: nums = [0]
Output: [[], [0]]
```

**约束:**

```
- 1 <= nums.length <= 10
- -10 <= nums[i] <= 10
```

# 说明

## 追踪

解决这个问题的方法类似于我们之前的博客 [LeetCode 子集](https://alkeshghorpade.me/post/leetcode-generate-subsets)。唯一的区别是，在生成子集时，我们需要排除重复的元素。

首先，我们将对 nums 数组进行排序。我们可以在递归调用子集生成器函数时排除重复的元素，也可以将子集标记为集合(集合是一种抽象数据类型，可以存储唯一的值)。

我们先检查一下算法。

```
// subsetsWithDup(nums) function
- sort nums array sort(nums.begin(),nums.end())- initialize vector<int> subset
             set<vector<int>> result
             vector<vector<int>> answer- call util function subsetsUtil(nums, result, subset, 0)- push set result in vector array
  loop for(auto it:result)
         answer.push_back(it)- return answer// subsetsUtil(nums, result, subset, index) function
- insert subset in result
  result.insert(subset)- loop for i = index; i < nums.size(); i++
  - subset.push_back(nums[i]) - subsetsUtil(nums, result, subset, i + 1) - subset.pop_back()
```

让我们看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。**注意:**在 C++解决方案中，子集是一个集合，而在 Golang 和 Javascript 中，它是一个普通的数组，我们忽略了重复的数组。

## C++解决方案

```
class Solution {
public:
    vector<vector<int>> subsetsWithDup(vector<int>& nums) {
        sort(nums.begin(),nums.end());
        vector<int> subset;
        set<vector<int>> result; subsetsUtil(nums, result, subset, 0); vector<vector<int>> answer; for(auto it:result){
            answer.push_back(it);
        } return answer;
    }public:
    void subsetsUtil(vector<int>& nums, set<vector<int>>& result, vector<int>& subset, int index) {
        result.insert(subset); for(int i = index; i < nums.size(); i++){
            subset.push_back(nums[i]); subsetsUtil(nums, result, subset, i + 1); subset.pop_back();
        } return;
    }
};
```

## 戈朗溶液

```
func subsetsUtils(nums, subset []int, result *[][]int) {
    cp := make([]int, len(subset))
    copy(cp, subset) *result = append(*result, cp) for i := 0; i < len(nums); i++ {
        subsetsUtils(nums[i+1:], append(subset, nums[i]), result) for ; i < len(nums)-1 && nums[i] == nums[i+1]; i++ {
        }
    }
}func subsetsWithDup(nums []int) [][]int {
    sort.Ints(nums) var result [][]int
    subset := make([]int, 0, len(nums)) subsetsUtils(nums, subset, &result) return result
}
```

## Javascript 解决方案

```
var subsetsWithDup = function(nums) {
    nums.sort((a, b) => a - b); const result = []; subsetsUtils(0, []); return result; function subsetsUtils (index, array) {
        result.push([...array]); for (let i = index; i < nums.length; i++) {
            if (i > index && nums[i] == nums[i - 1]) {
                continue;
            } array.push(nums[i]);
            subsetsUtils(i + 1, array);
            array.pop();
        }
    }
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [1, 2, 2]Step 1: sort(nums.begin(),nums.end())
        nums = [1, 2, 3]Step 2: initialize vector<int> subset
                   set<vector<int>> resultStep 3: subsetsUtil(nums, result, subset, 0)// in subsetsUtils function
Step 4: result.push_back(subset)
        result.push_back([]) result = [[]] loop for i = index, i < nums.size()
        i = 0
        0 < 3
        true subset.push_back(nums[i])
        subset.push_back(nums[0])
        subset.push_back(1) subset = [1] subsetsUtil(nums, res, subset, i + 1)
        subsetsUtil([1, 2, 2], [[]], [1], 0 + 1)
        subsetsUtil([1, 2, 2], [[]], [1], 1)Step 5: result.push_back(subset)
        result.push_back([1]) result = [[], [1]] loop for i = index, i < nums.size()
        i = 1
        1 < 3
        true subset.push_back(nums[i])
        subset.push_back(nums[1])
        subset.push_back(2) subset = [1, 2] subsetsUtil(nums, res, subset, i + 1)
        subsetsUtil([1, 2, 2], [[], [1]], [1, 2], 1 + 1)
        subsetsUtil([1, 2, 2], [[], [1]], [1, 2], 2)Step 6: result.push_back(subset)
        result.push_back([1, 2]) result = [[], [1], [1, 2]] loop for i = index, i < nums.size()
        i = 2
        2 < 3
        true subset.push_back(nums[i])
        subset.push_back(nums[2])
        subset.push_back(2) subset = [1, 2, 2] subsetsUtil(nums, res, subset, i + 1)
        subsetsUtil([1, 2, 2], [[], [1], [1, 2]], [1, 2, 2], 2 + 1)
        subsetsUtil([1, 2, 2], [[], [1], [1, 2]], [1, 2, 2], 3)Step 7: result.push_back(subset)
        result.push_back([1, 2, 3]) result = [[], [1], [1, 2], [1, 2, 3]] loop for i = index, i < nums.size()
        i = 3
        3 < 3
        falseStep 8: Here we backtrack to last line of Step 6 where
        i = 2
        subset = [1, 2, 2] We execute the next line
        subset.pop() subset = [1, 2]Step 9: We backtrack to last line of Step 5 where
        i = 1
        subset = [1, 2] We execute the next line
        subset.pop() subset = [1]Step 10: For loop continues where we execute
        loop for i = index, i < nums.size()
        i = 2
        i < nums.size()
        2 < 3
        true subset.push_back(nums[i])
        subset.push_back(nums[2])
        subset.push_back(2) subset = [1, 2] subsetsUtil(nums, res, subset, i + 1)
        subsetsUtil([1, 2, 2], [[], [1], [1, 2]], [1, 2], 2 + 1)
        subsetsUtil([1, 2, 2], [[], [1], [1, 2]], [1, 2], 3)Step 11: result.push_back(subset)
         result.push_back([1, 2]) result = [[], [1], [1, 2], [1, 2, 2]] loop for i = index, i < nums.size()
         i = 3
         3 < 3
         falseStep 12: Here we backtrack to last line of Step 3 where
         i = 0
         subset = [1] We execute the next line
         subset.pop() subset = []Step 13: For loop continues where we execute
         loop for i = index, i < nums.size()
         i = 1
         i < nums.size()
         1 < 3
         true subset.push_back(nums[i])
         subset.push_back(nums[1])
         subset.push_back(2) subset = [2] subsetsUtil(nums, res, subset, i + 1)
         subsetsUtil([1, 2, 2], [[], [1], [1, 2]], [2], 1 + 1)
         subsetsUtil([1, 2, 2], [[], [1], [1, 2]], [2], 2)Step 14: result.push_back(subset)
         result.push_back([2]) result = [[], [1], [1, 2], [1, 2, 2], [1, 2], [2]] loop for i = index, i < nums.size()
         i = 2
         2 < 3
         true subset.push_back(nums[i])
         subset.push_back(nums[2])
         subset.push_back(2) subset = [2, 2] subsetsUtil(nums, res, subset, i + 1)
         subsetsUtil([1, 2, 2], [[], [1], [1, 2], [2]], [2, 2], 2 + 1)
         subsetsUtil([1, 2, 2], [[], [1], [1, 2], [2]], [2, 2], 3)Step 15: result.push_back(subset)
         result.push_back([2, 2]) result = [[], [1], [1, 2], [1, 2, 2], [2], [2, 2]] loop for i = index, i < nums.size()
         i = 3
         3 < 3
         falseStep 16: Here we backtrack to last line of Step 14 where
         i = 2
         subset = [2, 2] We execute the next line
         subset.pop() subset = [2]Step 17: Here we backtrack to last line of Step 13 where
         i = 1
         subset = [2] We execute the next line
         subset.pop() subset = []Step 18: For loop continues where we execute
         loop for i = index, i < nums.size()
         i = 2
         i < nums.size()
         2 < 3
         true subset.push_back(nums[i])
         subset.push_back(nums[2])
         subset.push_back(2) subset = [2] subsetsUtil(nums, res, subset, i + 1)
         subsetsUtil([1, 2, 2], [[], [1], [1, 2], [2], [2, 2]], [2], 2 + 1)
         subsetsUtil([1, 2, 2], [[], [1], [1, 2], [2], [2, 2]], [2], 3)Step 19: result.push_back(subset)
         result.push_back([2]) result = [[], [1], [1, 2], [1, 2, 2], [2], [2, 2]] loop for i = index, i < nums.size()
         i = 3
         3 < 3
         falseStep 20: We have no more stack entries left. We return to the main function.Step 21: for(auto it:result){
            answer.push_back(it);
        } We push result Set to answer Vector.Step 22: return answerSo we return the answer as [[], [1], [1, 2], [1, 2, 2], [2], [2, 2]].
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-subsets-ii)*。*