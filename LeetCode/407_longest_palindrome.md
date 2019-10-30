# <center>407 - Longest Palindrome (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String, Hash
* Company: Amazon, Google
* Difficulty: Easy
* Link: https://leetcode.com/problems/longest-palindrome/

<br></br>



## Description
----
Given a string which consists of lowercase or uppercase letters, find the length of the longest palindromes that can be built with those letters.

This is case sensitive, for example "Aa" is not considered a palindrome here.

<br></br>



## Example
----
Input : s = "abccccdd"

Output : 7

One longest palindrome that can be built is "dccaccd", whose length is `7`.

<br></br>



## Solution
----
两种算法：
1. HashMap记录每个字符出现次数。因为回文串要求左右相同位置的字符相等，所以出现偶数次的字符肯定能全部使用。对于出现奇数次的字符，如果次数大于1，比如3，那么次数-1是偶数，且可以利用。因为回文串中间字符可以是独立的，所以有奇数情况最后要+1。
2. HashSet记录哪些字符是奇数次。最后长度就是字符串长度 - 出现奇数次字符的个数。如果有出现奇数次的字符，最后要+1。

<br>

### Go
```go
func LongestPalindrome1(s string) int {
	m := make(map[int32]int)
	for _, v := range s {
		count, ok := m[v]
		if !ok {
			count = 0
		}
		m[v] = count + 1
	}
	evenCount := 0
	oddExist := false
	for _, v := range m {
		if v%2 == 0 {
			evenCount += v
			continue
		} else {
			oddExist = true
			evenCount += v - 1
		}
	}

	if oddExist {
		evenCount++
	}
	return evenCount
}
```

```go
func LongestPalindrome2(s string) int {
	m := make(map[int32]bool)
	for _, v := range s {
		_, ok := m[v]
		if !ok {
			m[v] = true
		} else {
			delete(m, v)
		}
	}
	l := len(m)
	if l > 0 {
		return len(s) - l + 1
	}

	return len(s) - l
}
```

<br>
