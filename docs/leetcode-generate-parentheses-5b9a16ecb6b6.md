# LeetCode —生成括号

> 原文：<https://medium.com/nerd-for-tech/leetcode-generate-parentheses-5b9a16ecb6b6?source=collection_archive---------8----------------------->

# 问题陈述

给定 **n** 对括号，写一个函数给*生成所有格式良好的括号组合*。

问题陈述摘自:【https://leetcode.com/problems/generate-parentheses 

**例 1:**

```
Input: n = 3
Output: ["((()))", "(()())", "(())()", "()(())", "()()()"]
```

**例 2:**

```
Input: n = 1
Output: ["()"]
```

**约束:**

```
- 1 <= n <= 8
```

# 说明

## 强力

解决这个问题的一个强力方法是使用 **(** 和 **)** 生成括号的所有组合。然后验证哪些是有效的，并将有效的添加到结果中。

上述逻辑的一小段 C++代码如下所示:

```
vector<string> generateParenthesis(int n) {
    vector<string> combinations;
    generateAll("", 0, combinations);
    return combinations;
}

void generateAll(string current, int pos, vector<string> result) {
    if (pos == current.length) {
        if (valid(current))
            result.add(string(current));
    } else {
        current += '(';
        generateAll(current, pos+1, result);
        current += ')';
        generateAll(current, pos+1, result);
    }
}

bool valid(string current) {
    int balance = 0;
    for (int i = 0; i < current.length; i++) {
        if (current[i] == '(') balance++;
        else balance--;
        if (balance < 0) return false;
    }
    return balance == 0;
}
```

上述程序的时间复杂度为 **O((2 n)*n)** 。

## 追踪

使用回溯法可以避免产生所有可能的括号排列。

与上述方法中每次添加 **(** 或 **)** 不同，我们只在知道它仍然是有效序列时才添加它们。为此，我们可以跟踪到目前为止我们添加的开始和结束括号的数量。

## 算法

```
- initialize result array.

- call _generateParenthesis("", n, 0, 0, result)
  - This is a recursive function that will generate the valid parenthesis.

- return result

// _generateParenthesis(current, n, left, right, result)

- if right == n
  - result.push_back(current) and return
- else
  - if left < n
    - call _generateParenthesis(current + '(', n, left + 1, right, result)

  - if left > right
    - call _generateParenthesis(current + ')', n, left, right + 1, result)
```

**C++解决方案**

```
class Solution {
public:
    void _generateParenthesis(string current, int n, int left, int right, vector<string>& result) {
        if(right == n){
            result.push_back(current);
            return;
        } else {
            if(left < n){
                _generateParenthesis(current + '(', n, left + 1, right, result);
            }

            if(left > right){
                _generateParenthesis(current + ')', n, left, right + 1, result);
            }
        }
    }

    vector<string> generateParenthesis(int n) {
        vector<string> result;
        _generateParenthesis("", n, 0, 0, result);

        return result;
    }
};
```

**戈朗解**

```
func generateParenthesis(n int) []string {
    result := make([]string, 0)

    _generateParenthesis("", n, 0, 0, &result)
    return result
}

func _generateParenthesis(current string, n, left, right int, result *[]string) {
    if right == n {
        *result = append(*result, current)
        return
    } else {
        if left < n {
            _generateParenthesis(current + "(", n, left + 1, right, result)
        }

        if left > right {
            _generateParenthesis(current + ")", n, left, right + 1, result)
        }
    }
}
```

**Javascript 解决方案**

```
var generateParenthesis = function(n) {
    let result = [];

    _generateParenthesis("", n, 0, 0, result);

    return result;
};

var _generateParenthesis = function(current, n, left, right, result){
    if( right === n ) {
        result.push(current);
        return;
    } else {
        if( left < n ) {
           _generateParenthesis(current + '(', n, left + 1, right, result);
        }

        if( left > right) {
            _generateParenthesis(current + ')', n, left, right + 1, result);
        }
    }
}
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: n = 2

Step 1: vector<string> result;

Step 2: _generateParenthesis("", n, 0, 0, result)

// in _generateParenthesis(current, n, left, right, result)

Step 3: right == n
        0 == 2
        false

        left < n
        0 < 2
        true

        _generateParenthesis(current + '(', n, left + 1, right, result)
        _generateParenthesis('' + '(', 2, 0 + 1, 0, [])
        _generateParenthesis('(', 2, 1, 0, [])

Step 4: right == n
        0 == 2
        false

        left < n
        1 < 2
        true

        _generateParenthesis(current + '(', n, left + 1, right, result)
        _generateParenthesis('(' + '(', 2, 1 + 1, 0, [])
        _generateParenthesis('((', 2, 2, 0, [])

Step 5: right == n
        0 == 2
        false

        left < n
        2 < 2
        false

        left > right

        2 > 0
        true

        _generateParenthesis(current + ')', n, left, right + 1, result)
        _generateParenthesis('((' + ')', 2, 2, 0 + 1, [])
        _generateParenthesis('(()', 2, 2, 1, [])

Step 6: right == n
        1 == 2
        false

        left < n
        2 < 2
        false

        left > right

        2 > 1
        true

        _generateParenthesis(current + ')', n, left, right + 1, result)
        _generateParenthesis('(()' + ')', 2, 2, 1 + 1, [])
        _generateParenthesis('(())', 2, 2, 2, [])

Step 7: right == n
        2 == 2
        true

        result.push_back(current)
        [].push_back("(())")
        ["(())"]

Step 8: This step goes to the next line of Step 4, where the left is set to 1 and the right is 0.

        left = 1
        right = 0
        current = '('

        _generateParenthesis(current + ')', n, left, right + 1, result)
        _generateParenthesis('(' + ')', 2, 1, 0 + 1, ["(())"])
        _generateParenthesis('()', 2, 1, 1, ["(())"])

Step 9: right == n
        1 == 2
        false

        left < n
        1 < 2
        true

        _generateParenthesis(current + '(', n, left + 1, right, result)
        _generateParenthesis('()' + '(', 2, 1 + 1, 1, ["(())"])
        _generateParenthesis('()(', 2, 2, 1, ["(())"])

Step 10: right == n
         1 == 2
         false

         left < n
         2 < 2
         false

         left > right
         2 > 1

         _generateParenthesis(current + ')', n, left, right + 1, result)
         _generateParenthesis('()(' + ')', 2, 2, 1 + 1, ["(())"])
         _generateParenthesis('()()', 2, 2, 2, ["(())"])

Step 11: right == n
         2 == 2
         true

         result.push_back(current)
         ["(())"].push_back("()()")

Control flows back to Step 3 and then fallbacks to Step 2.

We return result as ["(())", "()()"].
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-generate-parenthesis)*。*