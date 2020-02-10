# <center>652 - Factorization (M)</center> 



<br></br>

* Tag: Backtracking, Recursion, DFS
* Comapny: LinkedIn, Uber
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/factorization

<br></br>



## Description
----
A non-negative numbers can be regarded as product of its factors.

Write a function that takes an integer n and return all possible combinations of its factors.

<br></br>



## Example
----
- Input: 8
- Output: [[2,2,2],[2,4]]

<br></br>



## Solution
----
算法：
1. DFS：
2. 剪枝：
    1. 每次新的循环遍历，起点是上次最后乘的值，同时可保证不会有重复结果。
    2. 如果当前值乘积大于结果，提前终止循环。
    3. 如果结果对当前值取余不为0，没必要检查。

<br>


## Go
```go
var (
	gfTmp []int = make([]int, 0)
)

// GetFactors returns all possible combinations of its factors.
func GetFactors(n int) [][]int {
	res := make([][]int, 0)
	if n == 1 {
		return res
	}
	gfDFS(n, 1, 2, &res)

	return res
}

func gfDFS(n, curRes, lastVal int, res *([][]int)) {
	if curRes == n {
		gfTmp1 := make([]int, len(gfTmp), len(gfTmp))
		copy(gfTmp1, gfTmp) // Deep copy is important.
		*res = append(*res, gfTmp1)
		return
	}

	for i := lastVal; i < n; i++ { // 每次新的循环遍历，起点是上次最后乘的值，同时可保证不会有重复结果。
		j := i * curRes
		if j > n { // 如果当前值乘积大于结果，提前终止循环。
			return
		}
		if n%i == 0 { // 如果结果对当前值取余不为0，没必要检查。
			gfTmp = append(gfTmp, i)
			gfDFS(n, j, i, res)
			l := len(gfTmp)
			gfTmp = gfTmp[:l-1] // Backtracking
		}
	}
}
```