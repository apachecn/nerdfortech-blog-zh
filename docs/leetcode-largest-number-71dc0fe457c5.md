# LeetCode —最大数量

> 原文：<https://medium.com/nerd-for-tech/leetcode-largest-number-71dc0fe457c5?source=collection_archive---------2----------------------->

# 问题陈述

给定一个非负整数列表*num*，排列它们使它们形成最大的数并返回它。

由于结果可能非常大，所以你需要返回一个字符串而不是一个整数。

问题陈述摘自:【https://leetcode.com/problems/largest-number】T2

**例一:**

```
Input: nums = [10, 2] 
Output: "210"
```

**例 2:**

```
Input: nums = [3, 30, 34, 5, 9] 
Output: "9534330"
```

**约束:**

```
- 1 <= nums.length <= 100 
- 0 <= nums[i] <= 10^9
```

# 说明

## 使用自定义比较器排序

为了构造最大数，我们需要将最大数放在最高有效位。

我们需要将每个整数转换成一个字符串，然后对字符串数组进行排序。**注意:**我们不能对整数的个数进行排序。例如，如果我们有数字[9，30，15]并按降序排列，将得到[30，15，9]，构造的数字是 30159。最大的数字是 93015。

让我们检查算法:

```
// largestNumber function
- if nums.size() == 0
  - return ""

- initialize vector<string> numbers
  i = 0

- loop for i < nums.size(); i++
  // convert integer to string and push in the array
  - numbers.push_back(std::to_string(nums[i]))

- sort(numbers.begin(), numbers.end(), cmp)

- set ans = ""

- loop for i = 0; i < numbers.size(); i++
  - ans = ans + numbers[i]

- return ans[0] == "0" ? "0" : ans

// cmp function
cmp(string a, string b)
- return a + b > b + a
```

## C++解决方案

```
class Solution {
static bool cmp(string a, string b){
    return a + b > b + a;
};

public:
    string largestNumber(vector<int>& nums) {
        if(nums.size() == 0){
            return "";
        }

        vector<string> numbers;
        int i = 0;

        for(; i < nums.size(); i++){
            numbers.push_back(std::to_string(nums[i]));
        }

        sort(numbers.begin(), numbers.end(), cmp);
        string ans = "";

        for(i = 0; i < numbers.size(); i++){
            ans += numbers[i];
        }

        return ans[0] == "0" ? "0" : ans;
    }
};
```

## 戈朗溶液

```
func largestNumber(nums []int) string {
    if len(nums) == 0 {
        return ""
    }

    numbers := make([]string, len(nums))
    i := 0

    for i < len(nums) {
        numbers = append(numbers, strconv.Itoa(nums[i]))
        i++
    }

    sort.Slice(numbers, func(a, b int) bool { return numbers[a] + numbers[b] > numbers[b] + numbers[a] })

    ans := ""

    for _, v := range numbers { ans += v }

    if ans[0] == '0' {
        return "0"
    }

    return ans
}
```

## Javascript 解决方案

```
var largestNumber = function(nums) {
    if( nums.length == 0 ) {
        return "";
    }

    let numbers = [];

    for( let i = 0; i < nums.length; i++ ) {
        numbers.push(nums[i].toString());
    }

    numbers.sort(function (x, y) {
        return x + y > y + x ? -1 : 1;
    });

    let ans = "";

    for( i = 0; i < numbers.length; i++ ){
        ans += numbers[i];
    }

    return ans[0] == "0" ? "0" : ans;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: nums = [3, 30, 34, 5, 9]

Step 1: nums.size() == 0
        5 == 0
        false

Step 2: vector<string> numbers;
        int i = 0;

Step 3: loop i < nums.size()
        0 < 5
        true

        numbers.push_back(std::to_string(nums[i]))

        so the loop iterates from 0 to 4, and we append the string numbers

        numbers = ["3", "30", "34", "5", "9"]

Step 4: sort(numbers.begin(), numbers.end(), cmp)

// in cmp function
Step 5: return a + b > b + a

        so for first two element we check
        "3" + "30" > "30" + "3"
        "330" > "303"
        true

        "3" + "34" > "34" + "3"
        "334" > "343"
        false

        "5" + "34" > "34" + "5"
        "534" > "345"
        true

        "9" + "5" > "5" + "9"
        "95" > "59"
        true

        The final array is ["9", "5", "34", "3", "30"].

Step 6: string ans = ""

Step 7: loop for(i = 0; i < numbers.size(); i++)
            - ans += numbers[i]
        ans is set to "9534330"

Step 8: ans[0] == "0"
        "9" == "0"
        false
        return ans

So we return the result as "9534330".
```

*原发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-largest-number)*。*