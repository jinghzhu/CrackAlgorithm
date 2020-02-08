# <center>433 - Number of Islands (M)</center> 



<br></br>

* Tag: BFS, Union Find
* Comapny: Google, Uber, Microsoft, Facebook, Amazon
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/number-of-islands/description

<br></br>



## Description
----
Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

<br></br>



## Example
----
Input:

```
11110
11010
11000
00000
```

Output: 1

<br></br>



## Solution
----
算法：
1. 并查集
2. 引入一个方法把[x, y]转为固定的ID，作为并查集标记。
3. 初始化并查集，每个集合都指向自己ID，count标记共有多少集合。
4. 遍历整个二维数组，对符合条件对每个“岛”，检查和合并其与隔壁的“岛”。
5. 返回count。

<br>


## Go
```go
func CountIslands1(grid [][]bool) int {
	if len(grid) < 1 {
		return 0
	}

	m, n := len(grid), len(grid[0])
	unionM, count := islands1InitUnion(grid)
	moveX, moveY := []int{-1, 1, 0, 0}, []int{0, 0, -1, 1}

	for x := 0; x < m; x++ {
		for y := 0; y < n; y++ {
			if !grid[x][y] {
				continue
			}
			for i := 0; i < 4; i++ {
				newX, newY := x+moveX[i], y+moveY[i]
				k1, k2 := islands1GenerateID(n, x, y), islands1GenerateID(n, newX, newY)
				if newX >= 0 && newX < m && newY >= 0 && newY < n && grid[newX][newY] && !islands1IsConnected(unionM, k1, k2) {
					islands1Union(unionM, k1, k2)
					count--
				}
			}
		}
	}

	return count
}

func islands1InitUnion(grid [][]bool) (map[int]int, int) {
	m, n, count := len(grid), len(grid[0]), 0
	unions := make(map[int]int)
	for x := 0; x < m; x++ {
		for y := 0; y < n; y++ {
			if grid[x][y] {
				k := islands1GenerateID(n, x, y)
				unions[k] = k
				count++
			}
		}
	}

	return unions, count
}

func islands1GenerateID(n, x, y int) int {
	return n*x + y
}

func islands1Find(unionM map[int]int, id int) int {
	for unionM[id] != id {
		unionM[id] = unionM[unionM[id]]
		id = unionM[id]
	}

	return id
}

func islands1IsConnected(unionM map[int]int, k1, k2 int) bool {
	return islands1Find(unionM, k1) == islands1Find(unionM, k2)
}

func islands1Union(unionM map[int]int, k1, k2 int) {
	root1, root2 := islands1Find(unionM, k1), islands1Find(unionM, k2)
	if root1 != root2 {
		unionM[root1] = root2 // important
	}
}
```
