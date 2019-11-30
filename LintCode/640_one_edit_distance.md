# <center>640 - One Edit Distance (M)</center> 



<br></br>

* Tag: String
* Difficulty: Medium
* Company: Uber, Facebook, Snapchat, Twitter
* Link: https://www.lintcode.com/problem/one-edit-distance

<br></br>



## Description
----
Given two strings S and T, determine if they are both one edit distance apart. One ediit distance means doing one of these operation:
1. insert one character in any position of S
2. delete one character in S
3. change one character in S to other character

<br></br>



## Example
----
Input: s = "aDb", t = "adb" 
Output: true

<br></br>



## Solution
----
### Go
```go
func isOneEditDistance (s string, t string) bool {
    lenS, lenT := len(s), len(t)
    if lenS - lenT == 1 {
        return case1(s, t)
    }
    if lenS - lenT == -1 {
        return case1(t, s)
    }
    return case2(s, t)
}

func case1(s, t string) bool {
    i, lenT := 0, len(t)
    for ; i < lenT && s[i] == t[i]; i++ {
    }
    return t == (s[:i] + s[i+1:])
}

func case2(s, t string) bool {
    if len(s) != len(t) {
        return false
    }
    flag := false
    for k := range s {
        if flag && s[k] != t[k] {
            return false
        }
        if s[k] != t[k] {
            flag = true
        }
    }
    
    return flag == true
}
```
