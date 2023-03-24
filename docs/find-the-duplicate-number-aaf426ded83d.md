# 找到重复的号码

> 原文：<https://medium.com/nerd-for-tech/find-the-duplicate-number-aaf426ded83d?source=collection_archive---------1----------------------->

(LeetCode:中)

![](img/a75dd0194b5f10251ef74071c487b40c.png)

给定一个包含`n + 1`个整数的整数数组`nums`，其中每个整数都在`[1, n]`范围内。

`nums`中只有**一个重复数**，返回*这个重复数*。

你必须在不修改数组`nums`的情况下解决**问题，并且只使用恒定的额外空间。**

**例 1:**

```
**Input:** nums = [1,3,4,2,2]
**Output:** 2
```

**例 2:**

```
**Input:** nums = [3,1,3,4,2]
**Output:** 3
```

**例 3:**

```
**Input:** nums = [1,1]
**Output:** 1
```

**例 4:**

```
**Input:** nums = [1,1,2]
**Output:** 1
```

**约束:**

*   `1 <= n <= 10^5`
*   `nums.length == n + 1`
*   `1 <= nums[i] <= n`
*   除了**恰好有一个整数**出现**两次或更多次**外，`nums`中的所有整数都只出现**一次**。

我将在这里讨论四种类型的方法，从蛮力到优化方法。让我们开始吧:

# 方法 1

运行两个循环，一个在另一个里面，看看它是否满足条件 arr[i]==arr[j]，是否满足。

***时间复杂度-O(n*n)，空间复杂度-O(1)***

```
PS : Try to code this by yourself, I have given the logic above.
```

# **方法 2**

步骤 1:对数组进行排序，时间复杂度为 O(nlogn)

第二步:运行一个循环，查找相邻元素是否相同？

TC-O(n)，所以总 ***时间复杂度-O(nlogn)+O(n)***

***空间复杂度-O(1)***

```
PS : Try to code this by yourself, I have given the logic above.
```

# 方法 3

利用鸽子原理/二分搜索法的概念。

```
**class Solution {
public:
int findDuplicate(vector& nums) {
int n=nums.size();** **int left=1,right=n-1,c,mid;

    while(left<right)
    {
        mid=(left+right)/2;
        c=0;

        for(int i=0;i<n;i++)
            if(nums[i]<=mid)c++;

        if(c>mid)right=mid;
        else left=mid+1;
    }
    return left;
   }** **};****Time Complexity - O(nlogn)
Space Complexity - O(1)**PS : I urge you to do a dry run for the above code for a clear understanding. If you face any issues feel free to comment below and ask. 
```

# 方法 4

使用散列的方法

步骤 1:使用另一个数组来存储元素的频率。频率大于 1 的元素，即重复/重复元素。

***时间复杂度-O(n)，空间复杂度-O(n)***

# 方法 5

(链表循环法)

```
**class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow = nums[0];
        int fast = nums[0];

        do{
            slow = nums[slow];
            fast = nums[nums[fast]];
        }while(slow!=fast);

        fast = nums[0];

        while(slow!=fast)
        {
            slow = nums[slow];
            fast = nums[fast];
        }
        return slow;
    }
};**Let us do a dry run to understand better the approach behind the code :
nums = [1,3,4,2,2]slow = nums[0] = 1
fast = nums[0] = 1slow = nums[1] = 3
fast = nums[nums[1]] = nums[3] = 2
since these two are not equal we update the values ,
slow = nums[3] = 2
fast = nums[nums[2]] = nums[4] = 2
Now since these are equal , we exit the loop.Now fast = nums[0] = 1
slow!=fast , 
slow = nums[2] = 4
fast = nums[1] = 3slow!=fast so ,
slow = nums[4] = 2
fast = nums[3] = 2Now slow and fast are equal so , the repeating element is 2.Intution : The basic thinking is that we keep two counter variables, slow is incremented by +1 and fast by +2 and at one point they will be pointing to the repeating elements, which is returned by the slow pointer. 
```

希望这篇文章有所帮助！敬请关注更多内容！！！继续编码💻🙌

既然你喜欢看我的博客，为什么不请我喝杯咖啡，支持我的工作呢！！[https://www.buymeacoffee.com/sukanyabharati](https://www.buymeacoffee.com/sukanyabharati)☕