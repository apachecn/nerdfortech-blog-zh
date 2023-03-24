# LeetCode —从排序数组 II 中删除重复项

> 原文：<https://medium.com/nerd-for-tech/leetcode-remove-duplicates-from-sorted-array-ii-c6ffff9e844d?source=collection_archive---------1----------------------->

# 问题陈述

给定一个按**非降序**排序的整数数组 *nums* ，就地移除一些重复元素，使得每个唯一元素最多出现**两次**。元素的**相对顺序**应保持与**相同**。

因为在某些语言中不可能改变数组的长度，所以必须将结果放在数组 *nums* 的**第一部分**中。更正式的说法是，如果删除重复项后还有 *k* 个元素，那么 *nums* 的第一个 *k* 个元素应该保存最终结果。除了前 k 个元素之外，剩下什么并不重要。

将最终结果放入 nums 的前 k 个槽后，返回 *k。*

不要为另一个阵列分配额外空间。你必须通过用 O(1)个额外的内存修改输入数组来做到这一点。

**自定义判断:**

法官将使用以下代码测试您的解决方案:

```
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```

如果所有的断言都通过了，那么你的解决方案就会被接受。

问题陈述摘自:[https://leet code . com/problems/remove-duplicates-from-sorted-array-ii](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii)

**例 1:**

```
Input: nums = [1, 1, 1, 2, 2, 3]
Output: 5, nums = [1, 1, 2, 2, 3, _]
Explanation: Your function should return k = 5, with the first five elements of nums being 1, 1, 2, 2, and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**例 2:**

```
Input: nums = [0, 0, 1, 1, 1, 1, 2, 3, 3]
Output: 7, nums = [0, 0, 1, 1, 2, 3, 3, _, _]
Explanation: Your function should return k = 7, with the first seven elements of nums being 0, 0, 1, 1, 2, 3, and 3 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**约束:**

```
- 1 <= nums.length <= 3 * 10^4 
- -10^4 <= nums[i] <= 10^4 
- nums is sorted in non-decreasing order.
```

# 说明

在我们之前的博客文章[中，我们已经看到了一个类似的问题。这个问题的唯一区别是，我们应该保持唯一元素最多出现两次。](https://alkeshghorpade.me/post/leetcode-remove-duplicates-from-sorted-array)

如果我们观察前面的 blog post 算法，我们正在比较当前的第 I 个 index 元素和第 I-1 个 index 元素。

```
int i = 0;

for(int j = 1; j < nums.size(); j++){
    if(nums[j] != nums[i]){
        i++;
        nums[i] = nums[j];
    }
}
```

条件 **if(nums[j]！= nums[i])** 比较两个相邻元素，如果(nums[i — 1]！= nums[i]) 。由于我们最多可以保留两个相似的元素，所以条件将类似于 **if(nums[i — 1]！= nums[I]| | nums[I-2]！= nums[i])** 。

让我们检查算法，以获得一个清晰的图片。

```
- set k = 2, n = nums.size()

- if n <= 2
  - return n

- loop for i = 2; i < n; i++
  - if nums[i] != nums[k - 2] || nums[i] != nums[k - 1]
    - nums[k] = nums[i]
    - k++

- return k
```

我们保留一个整数 **k** ，仅当当前元素与前两个索引都不匹配时，它才更新数组的第**个**索引。如果第 k 个索引匹配第 k-1 和第 k-2 个元素，我们继续在数组中向前移动。让我们来看看我们在 **C++** 、 **Golang** 和 **Javascript** 中的解决方案。

## C++解决方案

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int k = 2;
        int n = nums.size();

        if(n <= 2) {
            return n;
        }

        for(int i = 2; i < n; i++){
            if(nums[i] != nums[k - 2] || nums[i] != nums[k - 1]){
                nums[k] = nums[i];
                k++;
            }
        }

        return k;
    }
};
```

## 戈朗溶液

```
func removeDuplicates(nums []int) int {
    k := 2
    n := len(nums)

    if n <= 2 {
        return n
    }

    for i := 2; i < n; i++ {
        if nums[i] != nums[k - 2] || nums[i] != nums[k - 1] {
            nums[k] = nums[i]
            k++
        }
    }

    return k
}
```

## Javascript 解决方案

```
var removeDuplicates = function(nums) {
    let k = 2;
    let n = nums.length;

    if(n <= 2) {
        return n;
    }

    for(let i = 2; i < n; i++) {
        if(nums[i] != nums[k - 2] || nums[i] != nums[k - 1]) {
            nums[k] = nums[i];
            k++;
        }
    }

    return k;
};
```

对于这个输入 **nums = [1，1，1，2，2，3]** ，我们的 **C++** 方法的模拟运行如下:

```
Input: nums = [1, 1, 1, 2, 2, 3]

Step 1: k = 2
        n = nums.size()
          = 6

Step 2: if n <= 2
           6 <= 2
           false

Step 3: loop for i = 2; i < n;
          2 < 6
          true

          if nums[i] != nums[k - 2] || nums[i] != nums[k - 1]
             nums[2] != nums[0] || nums[2] != nums[1]
             1 != 1 || 1 != 1
             false

          i++
          i = 3

Step 4: loop i < n
          3 < 6
          true

          if nums[i] != nums[k - 2] || nums[i] != nums[k - 1]
             nums[3] != nums[0] || nums[3] != nums[1]
             2 != 1 || 2 != 1
             true

             nums[k] = nums[i]
             nums[2] = nums[3]
             nums[2] = 2

             k++
             k = 3

             nums = [1, 1, 2, 2, 2, 3]

          i++
          i = 4

Step 5: loop i < n
          4 < 6
          true

          if nums[i] != nums[k - 2] || nums[i] != nums[k - 1]
             nums[4] != nums[1] || nums[4] != nums[2]
             2 != 1 || 2 != 2
             true

             nums[k] = nums[i]
             nums[3] = nums[4]
             nums[3] = 2

             k++
             k = 4

             nums = [1, 1, 2, 2, 2, 3]

          i++
          i = 5

Step 6: loop i < n
          5 < 6
          true

          if nums[i] != nums[k - 2] || nums[i] != nums[k - 1]
             nums[5] != nums[2] || nums[5] != nums[3]
             3 != 2 || 3 != 2
             true

             nums[k] = nums[i]
             nums[4] = nums[5]
             nums[4] = 3

             k++
             k = 5

             nums = [1, 1, 2, 2, 3, 3]

          i++
          i = 6

Step 7: loop i < n
          6 < 6
          false

Step 8: return k

So we return the answer as 5, and the array till the 5th index is [1, 1, 2, 2, 3].
```

*原发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-remove-duplicates-from-sorted-array-ii)*。*