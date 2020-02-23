# <center>320 - Generalized Abbreviation (M)</center> 



<br></br>

* Tag: DFS, Recursion, Backtracking, String
* Difficulty: Medium
* Company: Google
* Link: https://leetcode.com/problems/generalized-abbreviation

<br></br>



## Description
----
Write a function to generate the generalized abbreviations of a word.

<br></br>



## Example
----
Input: word = "word"

Output: ["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]

<br></br>



## Solution
----
算法：
1. 每次递归回溯时，当前位置压缩或不压缩。
2. 当pos为字符串末尾时，判断是否有压缩（count > 0），否则无需拼接字符串。
3. 回溯时，如果回到最上一层，即count == 0，此时cur无需拼接数字。

<br>


### Go
```go
func GetAbbreviations(word string) []string {
	res := make([]string, 0)
	gaDFS(&res, "", word, 0, 0)
	sort.Strings(res)

	return res
}

func gaDFS(res *[]string, cur, word string, pos, count int) {
	if pos == len(word) {
		if count > 0 { // Important
			cur += fmt.Sprintf("%d", count)
		}
		*res = append(*res, cur)

		return
	}
	gaDFS(res, cur, word, pos+1, count+1)
	if count > 0 { // Important
		cur += fmt.Sprintf("%d", count) + string(word[pos])
	} else {
		cur += string(word[pos])
	}
	gaDFS(res, cur, word, pos+1, 0)
}
```

<br>
