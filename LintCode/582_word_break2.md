# <center>582 - Word Break II (H)</center> 



<br></br>

* Tag: DP, Backtrack, Recursion, DFS, Divide and Conquer
* Difficulty: Hard
* Company: Google, Snapchat, Uber, Dropbox, Twitter
* Link: https://www.lintcode.com/problem/word-break-ii

<br></br>



## Description
----
Given a string s and a dictionary of words dict, add spaces in s to construct a sentence where each word is a valid dictionary word.

Return all such possible sentences.

<br></br>



## Example
----
- Input："lintcode"，["de","ding","co","code","lint"]
- Output：["lint code", "lint co de"]

<br></br>



## Solution
----
算法：
1. 每次递归时，只负责找到第一个在字典中的子串，把其拆分出来，继续递归。
2. 记忆化搜索：由于可能重复处理拆分的字符串，所以引入一个哈希表，记录所有已拆分出来的子串。

<br>


### Go
```go
func GetWordBreak2(s string, wordDict []string) []string {
	visited := make(map[string][]string)
	wordHash := make(map[string]bool)
	for _, v := range wordDict {
		wordHash[v] = true
	}

	return wb2Helper(s, wordHash, visited)
}

func wb2Helper(s string, wordHash map[string]bool, visited map[string][]string) []string {
	if v, ok := visited[s]; ok {
		return v
	}
	res := make([]string, 0)
	if len(wordHash) < 1 || len(s) < 1 {
		return res
	}
	if _, ok := wordHash[s]; ok {
		res = append(res, s)
	}

	for i := 1; i < len(s); i++ {
		subS := s[:i]
		if _, ok := wordHash[subS]; !ok {
			continue
		}
		segmentations := wb2Helper(s[i:], wordHash, visited)
		for _, v := range segmentations {
			res = append(res, subS+" "+v)
		}
	}
	visited[s] = res

	return res
}
```

<br>
