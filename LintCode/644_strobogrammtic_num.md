# <center>644 - Strobogrammatic Number (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math
* Difficulty: Easy
* Company: Google
* Link: https://www.lintcode.com/problem/strobogrammatic-number

<br></br>



## Description
----
A mirror number is a number that looks the same when rotated 180 degrees (looked at upside down).For example, the numbers "69", "88", and "818" are all mirror numbers.

Write a function to determine if a number is mirror. The number is represented as a string.

<br></br>



## Example
----
1. Input : "69" Output : true
2. Input : "68" Output : false

<br></br>



## Solution
----
### Go
```go
func isStrobogrammatic (num string) bool {
    mirror := ""
    for _, v := range num {
        if v == '6' {
            mirror = "9" + mirror
        } else if v == '9' {
            mirror = "6" + mirror
        } else if v == '1' || v == '8' || v == '0' {
            mirror = string(v) + mirror
        } else {
            return false
        }
    }
    
    return mirror == num
}
```

<br>
