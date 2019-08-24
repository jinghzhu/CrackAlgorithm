# <center>17 - Letter Combinations of a Phone Number (M)</center> 



<br></br>

* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Google, Amazon, Facebook, Uber
* Link: https://leetcode.com/problems/letter-combinations-of-a-phone-number/

<br></br>



## Description
----
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![](./Images/letter_combination_phone_num.png)

<br></br>



## Example
----
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

<br></br>



## Solution
----
## Python
```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        result = []
        for i in range(len(digits)):
            if not self.legal(digits[i]):
                return []
            result = self.append(digits[i], result)
        return result
    
    def legal(self, char):
        return char >= '2' and char <= '9'
    
    def append(self, digit, cache):
        tmp = self.get_chars(digit)
        l = len(cache)
        if l == 0:
            return tmp
        result = []
        for i in range(l):
            for j in range(len(tmp)):
                result.append(cache[i] + tmp[j])
        return result
    
    def get_chars(self, digit):
        if digit == '2':
            return ['a', 'b', 'c']
        if digit == '3':
            return ['d', 'e', 'f']
        if digit == '4':
            return ['g', 'h', 'i']
        if digit == '5':
            return ['j', 'k', 'l']
        if digit == '6':
            return ['m', 'n', 'o']
        if digit == '7':
            return ['p', 'q', 'r', 's']
        if digit == '8':
            return ['t', 'u', 'v']
        return ['w', 'x', 'y', 'z']
```
