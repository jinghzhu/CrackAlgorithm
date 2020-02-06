# <center>434 - Number of Islands II (M)</center> 



<br></br>

* Tag: Union Find
* Difficulty: Medium
* Company: Google
* Link: https://www.lintcode.com/problem/number-of-islands-ii

<br></br>



## Description
----
Given a n,m which means the row and column of the 2D matrix and an array of pair A( size k). Originally, the 2D matrix is all 0 which means there is only sea in the matrix. The list pair has k operator and each operator has two integer A[i].x, A[i].y means that you can change the grid matrix[A[i].x][A[i].y] from sea to island. Return how many island are there in the matrix after each operator.

<br></br>



## Example
----
Input: n = 4, m = 5, A = [[1,1],[0,1],[3,3],[3,4]]

Output: [1,1,2,2]

```
0.  00000
    00000
    00000
    00000
1.  00000
    01000
    00000
    00000
2.  01000
    01000
    00000
    00000
3.  01000
    01000
    00000
    00010
4.  01000
    01000
    00000
    00011
```

<br></br>



## Solution
----
## Go
```go
func CountIslands2(n, m int, operators []*point) []int {
	res := make([]int, 0)
	data, unionM := islands2Init(n, m)
	count := 0
	moveX, moveY := []int{0, 0, 1, -1}, []int{1, -1, 0, 0}

	for _, ops := range operators {
		x, y := ops.X, ops.Y
		if data[x][y] == 1 {
			res = append(res, count)
			continue
		}
		count++
		data[x][y] = 1
		k := islandsTranslateToUnionID(x, y, m)
		for i := 0; i < 4; i++ {
			newX, newY := x+moveX[i], y+moveY[i]
			newK := islandsTranslateToUnionID(newX, newY, m)
			if newX >= 0 && newX < n && newY >= 0 && newY < m && data[newX][newY] == 1 && !islandsIsConnected(unionM, k, newK) {
				count--
				islands2Union(unionM, k, newK)
			}
		}
		res = append(res, count)
	}

	return res
}

func islands2Find(m map[int]int, x int) int {
	for x != m[x] {
		m[x] = m[m[x]]
		x = m[x]
	}

	return x
}

func islandsIsConnected(m map[int]int, k1, k2 int) bool {
	return islands2Find(m, k1) == islands2Find(m, k2)
}

func islands2Union(m map[int]int, x, y int) {
	rootX, rootY := islands2Find(m, x), islands2Find(m, y)
	if rootX != rootY {
		m[rootX] = rootY
	}
}

func islands2Init(n, m int) ([][]int, map[int]int) {
	data := make([][]int, 0, n)
	unionM := make(map[int]int)
	for i := 0; i < n; i++ {
		data = append(data, make([]int, m, m))
		for j := 0; j < m; j++ {
			id := islandsTranslateToUnionID(i, j, m)
			unionM[id] = id
		}
	}

	return data, unionM
}

func islandsTranslateToUnionID(x, y, m int) int {
	return m*x + y
}
```
