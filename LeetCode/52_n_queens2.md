# <center>52 - N-Queens II (H)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: DFS, Backtracking, Recursion
* Comapny: Zenefits
* Difficulty: Hard
* Link: https://leetcode.com/problems/n-queens-ii/

<br></br>



## Description
----
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other. Given an integer n, return the number of distinct solutions to the n-queens puzzle.

<br></br>



## Example
----
- Input: 4
- Output: 2

There are two distinct solutions to the 4-queens puzzle as shown below.

```
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

<br></br>



## Solution
----
算法：
1. DFS结束条件，当前检查深度为n。
2. 引入visited数组，用以标记哪一列已被访问。
3. 引入path数组，用以标记每行的哪一列已被访问。
4. DFS递归方式：
    1. 如果当前列已被访问，跳过。
    2. 判断是否对角线存在皇后，两个点在斜线上判断用等式abs(x1 - x2) == abs(y1 - y2)即可。

<br>


### Go
```go
var count int = 0

func totalNQueens(n int) int {
    count = 0
    path := make([]int, 0)

	nq1DFS(n, make([]bool, n, n), &path)
    
    return count
}

func nq1DFS(n int, visited []bool, path *[]int) {
	l := len(*path)
	if l == n {
		count++
		return
	}

	for i := 0; i < n; i++ {
		if visited[i] || !nq1Valid(*path, i) {
			continue
		}
		visited[i] = true
		*path = append(*path, i)
		nq1DFS(n, visited, path)
		visited[i] = false
		*path = (*path)[:len(*path)-1]
	}
}

func nq1Valid(path []int, n int) bool {
	l := len(path)
	for k, v := range path {
		if absInt(l-k) == absInt(n-v) {
			return false
		}
	}

	return true
}

func absInt(a int) int {
	if a < 0 {
		return -1 * a
	}
	return a
}
```

<br>
