# LeetCode —锯齿形转换

> 原文：<https://medium.com/nerd-for-tech/leetcode-zigzag-conversion-4bc08d0832e1?source=collection_archive---------2----------------------->

# **问题陈述**

字符串*“PAYPALISHIRING”*以之字形模式写在给定数量的行上，如下所示:(为了更好的可读性，您可能希望以固定的字体显示该模式)

```
P   A   H   N
A P L S I I G
Y   I   R
```

然后逐行朗读:*《pahnaplsigyir》*

根据给定的行数，编写接受字符串并进行转换的代码:

```
string convert(string s, int numRows);
```

问题陈述摘自:[https://leetcode.com/problems/zigzag-conversion](https://leetcode.com/problems/zigzag-conversion)

**例 1:**

```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```

**例 2:**

```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```

**例 3:**

```
Input: s = "A", numRows = 1
Output: "A"
```

**约束:**

```
- 1 <= s.length <= 1000
- s consists of English letters (lower-case and upper-case), ',' and '.'
- 1 <= numRows <= 1000
```

# 说明

## 逐行处理

字符串应该之字形传递到的行数。我们可以创建一个字符串数组。基于当前索引，将该字符追加到相应的字符串数组索引中。

我们来检查一下算法。

```
- if numRows <= 1
  - return s

- initialize i, set row = 0, down = true

- initialize array of strings: string array[numRows]

- loop for i = 0; i < numRows; i++
  - set array[i] = "" (empty string)

- loop for i = 0; i < s.size(); i++
  - append character to string
    array[row] += s[i];

  - if row == 0
    - set down = true

  - if row == numRows - 1
    - set down = false

  - increment or decrement row based on down boolean
    down ? row++ : row--

- set string answer = ""

- loop for i = 0; i < numRows; i++
  - answer += array[i]

- return answer
```

## C++解决方案

```
class Solution {
public:
    string convert(string s, int numRows) {
        if(numRows <= 1) {
            return s;
        } int i, row = 0;
        bool down = true;
        string array[numRows]; for(i = 0; i < numRows; i++){
            array[i] = "";
        } for(i = 0; i < s.size(); i++){
            array[row] += s[i]; if(row == 0){
                down = true;
            } if(row == numRows - 1){
                down = false;
            } down ? row++ : row--;
        } string answer = ""; for(i = 0; i < numRows; i++){
            answer += array[i];
        } return answer;
    }
};
```

## 戈朗溶液

```
func convert(s string, numRows int) string {
    if numRows <= 1 {
        return s
    } i, row := 0, 0
    down := true
    array := make([]string, numRows) for i = 0; i < len(s); i++ {
        array[row] += string(s[i]) if row == 0 {
            down = true
        } if row == numRows - 1 {
            down = false
        } if down {
            row++
        } else {
            row--
        }
    } answer := "" for i = 0; i < numRows; i++ {
        answer += array[i]
    } return answer
}
```

## Javascript 解决方案

```
var convert = function(s, numRows) {
    if( numRows <= 1 ){
        return s;
    } let i, row = 0;
    let down = true;
    let array = []; for( i = 0; i < numRows; i++ ){
        array[i] = "";
    } for( i = 0; i < s.length; i++ ){
        array[row] += s[i]; if( row == 0 ){
            down = true;
        } if( row == numRows - 1 ){
            down = false;
        } down ? row++ : row--;
    } var answer = ""; for( i = 0; i < numRows; i++ ){
        answer += array[i];
    } return answer;
};
```

让我们试运行一下我们的算法，看看解决方案是如何工作的。

```
Input: s = "ABCDEFGH", numRows = 3Step 1: if numRows <= 1
          3 <= 1
          falseStep 2: int i, row = 0
        bool down = true
        string array[numRows]
        string array[3];Step 3: loop for i = 0; i < numRows; i++
          - set array[i] = ""; numRows = 3
        so array[0] = array[1] = array[2] = ""Step 4: loop for i = 0; i < s.size()
        i < 8
        0 < 8
        true array[row] += s[i]
        array[row] = array[0] + s[0]
                   = "" + "A"
                   = "A"
        array[0] = "A" row == 0
        true
        down = true down ? row++ : row--
        row++
        row = 1 i++
        i = 1Step 5: loop for i < s.size()
        i < 8
        1 < 8
        true array[row] += s[i]
        array[row] = array[1] + s[1]
                   = "" + "B"
                   = "B"
        array[1] = "B" row == 0
        false row == numRows - 1
        1 == 2
        false down ? row++ : row--
        row++
        row = 2 i++
        i = 2Step 6: loop for i < s.size()
        i < 8
        2 < 8
        true array[row] += s[i]
        array[row] = array[2] + s[2]
                   = "" + "C"
                   = "C"
        array[2] = "C" row == 0
        false row == numRows - 1
        2 == 2
        true
        down = false down ? row++ : row--
        row--
        row = 1 i++
        i = 3Step 7: loop for i < s.size()
        i < 8
        3 < 8
        true array[row] += s[i]
        array[row] = array[1] + s[3]
                   = "B" + "D"
                   = "BD"
        array[1] = "BD" row == 0
        false row == numRows - 1
        1 == 2
        false down ? row++ : row--
        row--
        row = 0 i++
        i = 4Step 8: loop for i < s.size()
        i < 8
        4 < 8
        true array[row] += s[i]
        array[row] = array[0] + s[4]
                   = "A" + "E"
                   = "AE"
        array[1] = "AR" row == 0
        true
        down = true row == numRows - 1
        1 == 2
        false down ? row++ : row--
        row++
        row = 1 i++
        i = 5Step 9: loop for i < s.size()
        i < 8
        5 < 8
        true array[row] += s[i]
        array[row] = array[1] + s[5]
                   = "BD" + "F"
                   = "BDF"
        array[1] = "BDF" row == 0
        false row == numRows - 1
        1 == 2
        false down ? row++ : row--
        row++
        row = 2 i++
        i = 6Step 10: loop for i < s.size()
        i < 8
        6 < 8
        true array[row] += s[i]
        array[row] = array[2] + s[6]
                   = "C" + "G"
                   = "CG"
        array[2] = "CG" row == 0
        false row == numRows - 1
        2 == 2
        true
        down = false down ? row++ : row--
        row--
        row = 1 i++
        i = 7Step 11: loop for i < s.size()
        i < 8
        7 < 8
        true array[row] += s[i]
        array[row] = array[1] + s[7]
                   = "BDF" + "H"
                   = "BDFH"
        array[1] = "BDFH" row == 0
        false row == numRows - 1
        1 == 2
        false down ? row++ : row--
        row--
        row = 0 i++
        i = 8Step 11: loop for i < s.size()
        i < 8
        8 < 8
        falseStep 12: string answer = "";Step 13: loop for( i = 0; i < numRows; i++ )
           answer += array[i]; array[0] = "AE"
        array[1] = "BDFH"
        array[2] = "CG" so answer is "AEBDFHCG"Step 14: return answerSo the answer is "AEBDFHCG"
```

*最初发布于*[*https://alkeshghorpade . me*](https://alkeshghorpade.me/post/leetcode-zigzag-conversion)*。*