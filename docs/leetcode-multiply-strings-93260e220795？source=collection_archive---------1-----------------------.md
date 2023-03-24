# LeetCode —乘字符串

> 原文：<https://medium.com/nerd-for-tech/leetcode-multiply-strings-93260e220795?source=collection_archive---------1----------------------->

# 问题陈述

给定两个表示为字符串的非负整数 *num1* 和 *num2* ，返回同样表示为字符串的 *num1* 和 *num2* 的乘积。

**注意**:不能使用任何内置的 BigInteger 库，也不能直接将输入转换成整数。

问题陈述摘自:[https://leetcode.com/problems/multiply-strings](https://leetcode.com/problems/multiply-strings)。

**例 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**例 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

**约束:**

```
- 1 <= num1.length, num2.length <= 200
- num1 and num2 consist of digits only.
- Both num1 and num2 do not contain any leading zero, except the number 0 itself.
```

# 说明

## 使用标准的初等数学方法

简单的方法是用我们标准的小学数学方法将这两个数字相乘。但是当涉及到编程时，当我们移动到乘数的下一位时，我们需要调整尾随零。为了解决这个问题，我们将在 num1 中的每一个数字相乘后将结果数组索引向左移动。

让我们检查算法:

```
- set m = num1.size() and n = num2.size()
  // create a string of length m + n with all chars as '0'  
  set result = string(m + n, '0') 
  set indexCounter = 0
  initialize index, carry, currentNumber, sum- loop for i = m - 1; i >= 0; i--
  - set carry = 0
  - set index = m + n - 1 - indexCounter
  - set currentNumber = num1[i] - '0' - loop for j = n - 1; j >= 0; j--
    - set sum = (num2[j] - '0') * currentNumber + carry + result[index] - '0'
    - update carry = sum / 10
    - set result[index] = (char)(sum % 10 + '0')
    - update index = index - 1 - loop while carry > 0
    - set sum = result[index] - '0' + carry
    - update carry = sum / 10
    - set result[index] = (char)(sum % 10 + '0')
    - update index = index - 1 - update indexCounter = indexCounter + 1- set lastZeroIndex = 0- loop while lastZeroIndex < m + n && result[lastZeroIndex] == '0'
  - lastZeroIndex++- if lastZeroIndex == m + n
  - return "0"- return string(result.begin() + lastZeroIndex, result.end())
```

# C++解决方案

```
class Solution {
public:
    string multiply(string num1, string num2) {
        int m = num1.size();
        int n = num2.size();
        string result(m + n, '0');
        int indexCounter = 0;
        int index, carry, currentNumber, sum; for(int i = m - 1; i >= 0; i--){
            carry = 0;
            index = m + n - 1 - indexCounter;
            currentNumber = num1[i] - '0'; for(int j = n - 1; j >= 0; j--){
                sum = (num2[j] - '0')*currentNumber + carry + result[index] - '0';
                carry = sum / 10;
                result[index] = (char)(sum % 10 + '0');
                index--;
            } while(carry > 0){
                sum = result[index] - '0' + carry;
                carry = sum / 10;
                result[index] = (char)(sum % 10 + '0');
                index--;
            } indexCounter++;
        } int lastZeroIndex = 0;
        while(lastZeroIndex < m + n && result[lastZeroIndex] == '0'){
            lastZeroIndex++;
        } if(lastZeroIndex == m + n){
            return "0";
        } return string(result.begin() + lastZeroIndex, result.end());
    }
};
```

# 戈朗溶液

```
import "strings"func multiply(num1 string, num2 string) string {
    m, n := len(num1), len(num2)
    result := make([]byte, m + n)
    carry, indexCounter, sum, index := 0, 0, 0, 0 for i := m - 1; i >= 0; i-- {
        carry = 0
        currentNumber := int(num1[i] - '0')
        index = m + n - 1 - indexCounter for j := n - 1; j >= 0; j-- {
            sum = int(num2[j] - '0') * currentNumber + carry + int(result[index])
            carry = sum / 10
            result[index] = byte(sum % 10)
            index--
        } for carry > 0 {
            sum = int(result[index]) + carry
            carry = sum / 10
            result[index] = byte(sum % 10)
            index--
        } indexCounter++
    } lastZeroIndex := 0
    for lastZeroIndex < m + n && result[lastZeroIndex] == 0 {
        lastZeroIndex++
    } if lastZeroIndex == m + n {
        return "0"
    } return string(result[lastZeroIndex:])
}
```

# Javascript 解决方案

```
var multiply = function(num1, num2) {
    let m = num1.length, n = num2.length;
    let result = [];
    let carry, currentNumber, index, sum;
    let indexCounter = 0; for(let i = m - 1; i >= 0; i--){
        carry = 0;
        currentNumber = num1[i] - '0';
        index = m + n - 1 - indexCounter; for(let j = n - 1; j >= 0; j--){
            sum = (num2[j] - '0') * currentNumber + carry + (result[index] ? result[index] : 0);
            carry = Math.floor(sum / 10);
            result[index] = sum % 10;
            index--;
        } while(carry > 0){
            sum = (result[index] ? result[index] : 0) + carry;
            carry = Math.floor(sum / 10);
            result[index] = sum % 10;
            index--;
        } indexCounter++;
    } let lastZeroIndex = 0; for(;lastZeroIndex < m + n && result[lastZeroIndex] == 0;){
        lastZeroIndex++
    } if(lastZeroIndex == m + n){
        return "0";
    } return result.join("").replace(/^0+/, '')
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: num1 = "23", num2 = "46"Step 1: m = num1.size()
          = 2
        n = num2.size()
          = 2 string result(m + n, '0')
        result = "0000"
        indexCounter = 0
        int index, carry, currentNumber, sumStep 2: loop for i = m - 1; i >= 0
        i = 2 - 1
          = 1 i >= 0
        1 >= 0
        true carry = 0
        index = m + n - 1 - indexCounter
              = 2 + 2 - 1 - 0
              = 3
        currentNumber = num1[i] - '0'
                      = num1[1] - '0'
                      = '3' - '0'
                      = 3 loop for j = n - 1; j >= 0
        j = n - 1
          = 2 - 1
          = 1 sum = (num2[j] - '0')*currentNumber + carry + result[index] - '0'
              = (num2[1] - '0') * 3 + 0 + result[3] - '0'
              = ('6' - '0') * 3 + 0 + '0' - '0'
              = 6*3 + 0 + 0
              = 18 carry = sum / 10
                = 18/ 10
                = 1 result[3] = (char)(sum % 10 + '0')
                        = (char)(18 % 10 + '0')
                        = (char)(8 + '0')
                        = '8' index--
          index = index - 1
                = 3 - 1
                = 2 j--
          j = 0 loop for j >= 0
        0 >= 0
        true sum = (num2[j] - '0')*currentNumber + carry + result[index] - '0'
              = (num2[0] - '0') * 3 + 1 + result[2] - '0'
              = ('4' - '0') * 3 + 1 + '0' - '0'
              = 4*3 + 1 + 0
              = 13 carry = sum / 10
                = 13 / 10
                = 1 result[2] = (char)(sum % 10 + '0')
                        = (char)(13 % 10 + '0')
                        = (char)(3 + '0')
                        = '3' index--
          index = index - 1
                = 2 - 1
                = 1 j--
          j = -1 loop for j >= 0
        -1 >= 0
        false loop while carry > 0
        1 > 0
        true sum = result[index] - '0' + carry
              = result[1] - '0' + 1
              = '0' - '0' + 1
              = 1 carry = sum / 10
                = 1 / 10
                = 0 result[1] = (char)(sum % 10 + '0')
                    = (char)(1 % 10 + '0')
                    = (char)(1 + '0')
                    = '1' index--
          index = index - 1
                = 1 - 1
                = 0 indexCounter++
        indexCounter = indexCounter + 1
                     = 1 i--
        i = i - 1
          = 1 - 1
          = 0 result = ['0', '1', '3', '8']Step 3: loop for i >= 0
        0 >= 0
        true carry = 0
        index = m + n - 1 - indexCounter
              = 2 + 2 - 1 - 1
              = 2 currentNumber = num1[i] - '0'
                      = num1[0] - '0'
                      = '2' - '0'
                      = 2 loop for j = n - 1; j >= 0
        j = n - 1
          = 2 - 1
          = 1 sum = (num2[j] - '0')*currentNumber + carry + result[index] - '0'
            = (num2[1] - '0') * 2 + 0 + result[2] - '0'
            = ('6' - '0') * 2 + 0 + '3' - '0'
            = 6 * 2 + 3
            = 15 carry = sum / 10
              = 15 / 10
              = 1 result[2] = (char)(sum % 10 + '0')
                  = (char)(15 % 10 + '0')
                  = (char)(5 + '0')
                  = '5' index--
        index = index - 1
              = 2 - 1
              = 1 j--
        j = j - 1
          = 1 - 1
          = 0 loop for j >= 0
        0 >= 0
        true sum = (num2[j] - '0') * currentNumber + carry + result[index] - '0'
            = ('4' - '0') * 2 + 1 + result[1] - '0'
            = 4 * 2 + 1 + '1' - '0'
            = 8 + 1 + 1
            = 10 carry = sum / 10
              = 10 / 10
              = 1 result[1] = (char)(sum % 10 + '0')
                  = (char)(10 % 10 + '0')
                  = (char)(0 + '0')
                  = '0' index--
        index = index - 1
              = 1 - 1
              = 0 j--
        j = j - 1
          = 0 - 1
          = -1 loop for j >= 0
        -1 >= 0
        false loop while carry > 0
        1 > 0
        true sum = result[index] - '0' + carry
              = result[0] - '0' + 1
              = '0' - '0' + 1
              = 1 carry = sum / 10
                = 1 / 10
                = 0 result[0] = (char)(sum % 10 + '0')
                    = (char)(1 % 10 + '0')
                    = (char)(1 + '0')
                    = '1' index--
          index = index - 1
                = 0 - 1
                = -1 loop while carry > 0
        0 > 0
        false indexCounter++
        indexCounter = indexCounter + 1
                     = 2 i--
        i = i - 1
          = 0 - 1
          = -1 result = ['1', '0', '5', '8']Step 4: loop for i >= 0
        -1 >= 0
        falseStep 5: lastZeroIndex = 0Step 6: loop while lastZeroIndex < m + n && result[lastZeroIndex] == '0'
          0 < 2 + 2 && result[0] == '0'
          0 < 4 && '1' == '0'
          true && false
          falseStep 7: if lastZeroIndex == m + n
          0 == 2 + 2
          0 == 4
          falseStep 8: return string(result.begin() + lastZeroIndex, result.end())
          string(result.begin() + 0, result.end())
          string(['1', '0', '5', '8'])
          "1058"So we return the result as "1058".
```

*原载于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-multiply-strings)*。*