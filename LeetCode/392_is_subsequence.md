# <center>392 - Is Subsequence (E)</center> 



<br></br>

* Tag: String, Greedy
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/is-subsequence/

<br></br>



## Description
----
Given a string `s` and a string `t`, check if `s` is subsequence of `t`.

You may assume that there is only lower case English letters in both `s` and `t`. `t` is potentially a very long (length ~= 500,000) string, and `s` is a short string (length <= 100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

<br></br>



## Example
----
1. Input: s = "abc", t = "ahbgdc" Output: true
2. Input: s = `"axc"`, t = `"ahbgdc"` Output: false

<br></br>



## Solution
----
贪婪算法并使用双指针，一个指向`s`，另一个指向`t`。当`s[i] == t[j]`时，`i, j = i + 1, j + 1`。最后检查`i == len(s)`。

<br>


### Go
```go
func IsSubsequence(s, t string) bool {
	if len(s) > len(t) {
		return false
	}

	lenS, lenT, i, j := len(s), len(t), 0, 0
	for i < lenS && j < lenT && lenT-j >= lenS-i {
		if s[i] == t[j] {
			i++
		}
		j++
	}

	return i == lenS
}
```

<b>


### Java
```java
public boolean solution1(String s, String t) {
    if (s == null || t == null || s.length() > t.length())
        return false;
        
    int i = 0, j = 0, lenS = s.length(), lenT = t.length();
    while ( i < lenS && j < lenT && (lenT - i >= lenS - j)) {
        if (s.charAt(i) == t.charAt(j))
            i++;
        j++;
    }
        
    return i == lenS;
}
```

<br>


### Python
```python
def solution1(s: str, t: str) -> bool:
    if len(s) > len(t):
        return False
        
    i, j, len_s, len_t = 0, 0, len(s), len(t)
    while i < len_s and j < len_t and len_t - j >= len_s - i:
        if s[i] == t[j]:
            i += 1
        j += 1
            
    return i == len_s
```

<br>
