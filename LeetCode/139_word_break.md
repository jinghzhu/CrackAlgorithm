# <center>107 - Word Break (M)</center> 



<br></br>

* Tag: DP, Divide and Conquer
* Difficulty: Medium
* Company: Google, Bloomberg, Uber, Yahoo, Facebook
* Link: https://leetcode.com/problems/word-break/

<br></br>



## Description
----
Given a string s and a dictionary of words dict, determine if s can be broken into a space-separated sequence of one or more dictionary words.

<br></br>



## Example
----
- Input:  "lintcode", ["lint", "code"]
- Output:  true

<br></br>



## Solution
----
算法：
1. 基于递归的记忆化搜索。每次递归时，只选择首部满足字典的，然后拆分出来子串递归处理，但会超时。
2. DP：
    1. 状态定义：f[n]表示s[:n+1]子串符合要求。
    2. 状态转换方程：f[n] = (f[n-1] && s[n:n+1] in dict) || (f[n-2] && s[n-1:n+1] in dict) || ...) 因为字典中候选集有限，设最大的候选字符串长度为maxL，因为状态转换时，无论怎样都不可能存在大于maxL长度的子串存在于字典中，所以检查时可以优化只坚持到f[n-maxL]为止。此外，如果当前子字符串s[:n+1]本身就在字典中，就可避免检查。
    3. 初始状态：可以不设。
    4. 计算方向：从左到右。

<br>


### Go
```go
func WordBreak1(s string, wordDict []string) bool {
	l1, l2 := len(s), len(wordDict)
	if l1 < 1 && l2 < 1 {
		return true
	}
	if l1 < 1 || l2 < 1 {
		return false
	}
	dict, maxL := wbHelper(wordDict)
	f := make([]bool, l1, l1)

	for i := 0; i < l1; i++ {
		if _, ok := dict[s[:i+1]]; ok {
			f[i] = true
			continue
		}
		for j := maxInt(0, i-maxL); j < i; j++ {
			if !f[j] {
				continue
			}
			if _, ok := dict[s[j+1:i+1]]; ok {
				f[i] = true
				break
			}
		}
	}

	return f[l1-1]
}
```

```go
func wbHelper(wordDict []string) (map[string]bool, int) {
	maxL := 0
	m := make(map[string]bool)
	for _, v := range wordDict {
		m[v] = true
		maxL = maxInt(maxL, len(v))
	}

	return m, maxL
}

func maxInt(args ...int) int {
	max := args[0]
	for _, v := range args {
		if v > max {
			max = v
		}
	}

	return max
}
```

```go
func WordBreak2(s string, wordDict []string) bool {
	l := len(s)
	if l < 1 {
		return true
	}
	if len(wordDict) < 1 {
		return false
	}
	dict, maxL := wbHelper(wordDict)
	f := make([]bool, l+1, l+1)
	f[0] = true

	for i := 1; i < l+1; i++ {
		for j := maxInt(0, i-maxL); j < i; j++ {
			if !f[j] {
				continue
			}
			if _, ok := dict[s[j:i]]; ok {
				f[i] = true
				break
			}
		}
	}

	return f[l]
}
```

<br>
