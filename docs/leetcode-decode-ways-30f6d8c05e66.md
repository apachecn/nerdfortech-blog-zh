# LeetCode —解码方式

> 原文：<https://medium.com/nerd-for-tech/leetcode-decode-ways-30f6d8c05e66?source=collection_archive---------0----------------------->

# 问题陈述

包含从 *A-Z* 的字母的消息可以使用以下映射被**编码**成数字:

```
'A' -> "1"
'B' -> "2"
...
'Z' -> "26"
```

为了**解码**一个编码的信息，所有的数字必须被分组，然后使用上面映射的逆过程映射回字母(可能有多种方法)。例如，*“11106”*可以映射成:

```
"AAJF" with the grouping (1 1 10 6)"KJF" with the grouping (11 10 6)
```

请注意，分组(1 11 06)无效，因为“6”不同于“06”，因此“06”不能映射到“F”。

给定一个只包含数字的字符串 *s* ，*返回数字***的方式为* ***解码*** *it* 。*

*答案保证适合 32 位整数。*

*问题陈述摘自:[https://leetcode.com/problems/decode-ways](https://leetcode.com/problems/decode-ways)*

***例 1:***

```
*Input: s = "12"
Output: 2
Explanation: "12" could be decoded as "AB" (1 2) or "L" (12).*
```

***例 2:***

```
*Input: s = "226"
Output: 3
Explanation: "226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).*
```

***例 3:***

```
*Input: s = "0"
Output: 0
Explanation: There is no character that is mapped to a number starting with 0.
The only valid mappings with 0 are 'J' -> "10" and 'T' -> "20", neither of which start with 0.
Hence, there are no valid ways to decode this since all digits need to be mapped.*
```

***例 4:***

```
*Input: s = "06"
Output: 0
Explanation: "06" cannot be mapped to "F" because of the leading zero ("6" is different from "06").*
```

***约束:***

```
*- 1 <= s.length <= 100
- s contains only digits and may contain leading zero(s).*
```

# *说明*

## *强力解决方案*

*一种简单的方法是生成所有可能的组合，并计算正确序列的数量。*

*这种方法易于实现，但是具有 **O(2^N)** 的时间复杂度。*

## *动态规划*

*这个问题可以用动态规划方法来解决。*

*我们拿字符串**“12”**来说吧。我们可以用两种方式解码字符串**【1，2】**或 **12** 。现在让我们在字符串的末尾追加 *6* 。对于新的字符串，解码方式是 2 + 1 = 3。对于**【1，2，3】**或**【12，3】**为 2，对于**【1，23】**为 1。*

*我们先解决子问题，然后用它的解来解决更大的问题。那只不过是一种动态编程方法。*

*我们来检查一下算法。*

```
*- initialize count array: count[n + 1]
- set count[0] = count[1] = 1- if s[0] == 0 // first character of string is 0
  - return 0- loop for i = 2; i <= n; i++
  - set count[i] = 0 // if string is "02" we should not count "02" as a valid case.
  // But if the previous char is greater than 0 we set the current index count same
  // as the previous index count.
  - if s[i - 1] > '0'
    - count[i] = count[i - 1] // if string is "32" it is not possible to map to any character.
  // hence we have (i - 2)th index for 1 or 2 and
  // if s[i - 2] is 2 we additionally check for (i - 1)th index to
  // be less than 7.
  - if s[i - 2] == '1' || (s[i - 2] == '2' && s[i - 1] < '7')
    - count[i] += count[i - 2]- return count[n]*
```

## *C++解决方案*

```
*class Solution {
public:
    int countWays(string s, int n){
        int count[n + 1];
        count[0] = 1;
        count[1] = 1; if(s[0] == '0')
            return 0; for(int i = 2; i <= n; i++){
            count[i] = 0; if(s[i - 1] > '0')
                count[i] = count[i - 1]; if(s[i - 2] == '1' || (s[i - 2] == '2' && s[i - 1] < '7')){
                count[i] += count[i - 2];
            }
        } return count[n];
    }public:
    int numDecodings(string s) {
        return countWays(s, s.size());
    }
};*
```

## *戈朗溶液*

```
*func numDecodings(s string) int {
    count := make([]int, len(s) + 1)
    count[0], count[1] = 1, 1 if s[0] == '0' {
        return 0
    } for i := 2; i <= len(s); i++ {
        if s[i - 1] > '0' {
            count[i] = count[i - 1]
        } if s[i - 2] == '1' || (s[i - 2] == '2' && s[i - 1] < '7') {
            count[i] += count[i - 2]
        }
    } return count[len(s)]
}*
```

## *Javascript 解决方案*

```
*var numDecodings = function(s) {
    let count = [];
    count[0] = 1;
    count[1] = 1; for(let i = 2; i <= s.length; i++){
        count[i] = 0; if(s[i - 1] > '0'){
            count[i] = count[i - 1];
        } if(s[i - 2] == '1' || (s[i - 2]) == '2' && s[i - 1] < '7'){
            count[i] += count[i - 2];
        }
    } return count[s.length];
};*
```

*让我们试运行一下我们的算法，看看解决方案是如何工作的。*

```
*Input: s = "226"

Step 1: int count[n + 1]
        count[0] = count[1] = 1

Step 2: if s[0] == '0'
        '2' == '0'
        false

Step 3: loop for i = 2; i <= n;
        2 <= 3
        true

        if s[i - 1] > '0'
        s[2 - 1] > '0'
        s[1] > '0'
        '2' > '0'
        true

        count[i] = count[i - 1]
        count[2] = count[2 - 1]
                 = count[1]
                 = 1

        if s[i - 2] == '1' || (s[i - 2] == '2' && s[i - 1] < '7'))
        s[2 - 2] == '1'
        s[0] == '1'
        '2' == '1'
        false

        s[i - 2] == '2' && s[i - 1] < '7'
        s[2 - 2] == '2' && s[2 - 1] < '7'
        s[0] == '2' && s[1] < '7'
        '2' == '2' && '2' < '7'
        true

        count[2] = count[i] + count[i - 2]
                 = count[2] + count[2 - 2]
                 = 1 + 1
                 = 2

        i++
        i = 3

Step 4: loop for i <= n;
        3 <= 3
        true

        if s[i - 1] > '0'
        s[3 - 1] > '0'
        s[2] > '0'
        '6' > '0'
        true

        count[i] = count[i - 1]
        count[3] = count[3 - 1]
                 = count[2]
                 = 2

        if s[i - 2] == '1' || (s[i - 2] == '2' && s[i - 1] < '7'))
        s[3 - 2] == '1'
        s[1] == '1'
        '2' == '1'
        false

        s[i - 2] == '2' && s[i - 1] < '7'
        s[3 - 2] == '2' && s[3 - 1] < '7'
        s[1] == '2' && s[2] < '7'
        '2' == '2' && '6' < '7'
        true

        count[3] = count[i] + count[i - 2]
                 = count[3] + count[3 - 2]
                 = 2 + 1
                 = 3

        i++
        i = 4

Step 5: loop for i <= n;
        4 <= 3
        false

Step 6: return count[n]
        count[3] = 3

So the answer we return is 3.*
```

**原发表于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-decode-ways)*。**