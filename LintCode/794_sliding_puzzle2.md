# <center>794 - Sliding Puzzle II (H)</center> 



<br></br>

* Tag: BFS
* Difficulty: Hard
* Link: https://www.lintcode.com/problem/sliding-puzzle-ii

<br></br>



## Description
----
On a 3x3 board, there are 8 tiles represented by the integers 1 through 8, and an empty square represented by 0.

A move consists of choosing 0 and a 4-directionally adjacent number and swapping it.

Given an initial state of the puzzle board and final state, return the least number of moves required so that the initial state to final state.

If it is impossible to move from initial state to final state, return -1.

<br></br>



## Example
----
Input:

```
[
 [2,8,3],
 [1,0,4],
 [7,6,5]
]
[
 [1,2,3],
 [8,0,4],
 [7,6,5]
]
```

Output: 4

```
[                 [
 [2,8,3],          [2,0,3],
 [1,0,4],   -->    [1,8,4],
 [7,6,5]           [7,6,5]
]                 ]

[                 [
 [2,0,3],          [0,2,3],
 [1,8,4],   -->    [1,8,4],
 [7,6,5]           [7,6,5]
]                 ]

[                 [
 [0,2,3],          [1,2,3],
 [1,8,4],   -->    [0,8,4],
 [7,6,5]           [7,6,5]
]                 ]

[                 [
 [1,2,3],          [1,2,3],
 [0,8,4],   -->    [8,0,4],
 [7,6,5]           [7,6,5]
]                 ]
```

<br></br>



## Solution
----
算法：
1. 把初始和最终状态转为字符串。
2. BFS，把每次移动当作一个字符串做比较。

<br>


### Go
```go
func SlidingPuzzle2(initState, finalState [][]int) int {
	initS, finalS := sp2InitStr(initState), sp2InitStr(finalState)
	visited := make(map[string]bool)
	visited[initS] = true
	q := make([]string, 0)
	q = append(q, initS)
	round := 0

	for len(q) > 0 {

		round++
		l := len(q)
		for i := 0; i < l; i++ {
			s := q[0]
			q = q[1:]
			for _, v := range sp2GetNext(s) {
				if v == finalS {
					return round
				}
				if visited[v] {
					continue
				}
				visited[v] = true
				q = append(q, v)
			}
		}
	}

	return -1
}

func sp2InitStr(state [][]int) string {
	s := ""
	for x := range state {
		for y := range state {
			s += fmt.Sprintf("%d", state[x][y])
		}
	}

	return s
}

func sp2GetNext(s string) []string {
	res := make([]string, 0)
	opsX, opsY := []int{0, 0, 1, -1}, []int{1, -1, 0, 0}
	zeroIndex := strings.Index(s, "0")
	x, y := zeroIndex/3, zeroIndex%3
	for i := 0; i < 4; i++ {
		x1, y1 := x+opsX[i], y+opsY[i]
		if x1 < 0 || x1 >= 3 || y1 < 0 || y1 >= 3 {
			continue
		}
		bytes := []byte(s)
		bytes[zeroIndex], bytes[x1*3+y1] = bytes[x1*3+y1], bytes[zeroIndex]
		res = append(res, string(bytes[:]))
	}

	return res
}
```