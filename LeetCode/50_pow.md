# <center>50 - Pow(x, n) (M)</center> 



<br></br>

* Tag: Math, Binary Search
* Difficulty: Medium
* Company: Uber, Facebook, Bloomberg, LinkedIn
* Link: https://leetcode.com/problems/powx-n/

<br></br>



## Description
----
Implement pow(x, n). (n is an integer.)

<br></br>



## Example
----
Input: x = 2.1, n = 3
Output: 9.261

<br></br>



## Solution
----
算法：O(logn)

Intuition:
```
2^7
= 2 * 2^6
= 2 * 4^3 <- divide power number by 2, since we have O(lgn) algorithm
= 8 * 4^2
= 8 * 16^1
= 128 * 16^0
= 128
```

<br>


### Go
```go
func GetPow(x float64, n int) float64 {
    var m int64 = int64(n)
    if m < 0 {
        m = -m
        x = 1/x
    }
    var res float64 = 1.0
    for m != 0 {
        if m % 2 == 1 {
            res *= x
        }
        m /= 2
        x *= x
    }
    
    return res
}
```
