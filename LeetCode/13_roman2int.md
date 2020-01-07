# <center>13 - Roman to Integer (E)</center> 



<br></br>

* Tag: Math
* Company: Microsoft, Uber, Facebook, Bloomberg, Yahoo
* Difficulty: Easy
* Link: https://leetcode.com/problems/roman-to-integer/

<br></br>



## Description
----
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

```
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, two is written as II in Roman numeral, just two one's added together. Twelve is written as, XII, which is simply X + II. The number twenty seven is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

- I can be placed before V (5) and X (10) to make 4 and 9. 
- X can be placed before L (50) and C (100) to make 40 and 90. 
- C can be placed before D (500) and M (1000) to make 400 and 900.

Given a roman numeral, convert it to an integer. Input is guaranteed to be within the range from 1 to 3999.

<br></br>



## Example
----
```
Input: "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

```
Input: "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

<br></br>



## Solution
----
算法：
1. 异常检查。
2. 建立速查字典。
3. 结果初始化为string最后一个字符对应的罗马数字。
4. 从倒数第二位往前开始遍历：
    1. 如果当前数字大于后一位，则是正常顺序，加。
    2. 否则，减。

<br>


### Python
```python
class Solution:
    def romanToInt(self, s: str) -> int:
        if not s:
            return 0
        m = {
            'I': 1,
            'V': 5,
            'X': 10,
            'L': 50,
            'C': 100,
            'D': 500,
            'M': 1000
        }
        l = len(s)
        res = m[s[l - 1]]
        for i in range(l - 2, -1, -1):
            if m[s[i]] >= m[s[i + 1]]:
                res += m[s[i]]
            else:
                res -= m[s[i]]
        
        return res
```

<br>


### Go
```go
func romanToInt(s string) int {
    if s == "" {
        return 0
    }
    m := map[string]int{
        "I": 1,
        "V": 5, 
        "X": 10,
        "L": 50,
            "C": 100,
            "D": 500,
            "M": 1000,
    }
    l := len(s)
    res := m[string(s[l - 1])]
    for i := l - 2; i >= 0; i-- {
        a, b := string(s[i]), string(s[i + 1])
        if m[a] >= m[b] {
            res += m[a]
        } else {
            res -= m[a]
        }
    }
    
    return res
}
```