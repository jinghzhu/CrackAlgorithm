# <center>258 - Add Digits (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math
* Difficulty: Easy
* Company: Adobe, Microsoft
* Link: https://www.lintcode.com/problem/add-digits/description

<br></br>



## Description
----
Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

<br></br>



## Example
----
Input: num=38
Output: 2
The process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return 2.

<br></br>



## Solution
----
算法：
1. 如果输入小于10，直接返回。
2. 反复数位分离，直到num最终小于10.

<br>


### Go
```go
func addDigits(num int) int {
    if num < 10 {
        return num
    }
    for num > 9 {
        res := 0
        for num > 0 {
            res += num % 10
            num /= 10
        }
        num = res
    }
    
    return num
}
```

<br>
