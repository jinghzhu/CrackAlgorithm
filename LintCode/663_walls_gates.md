# <center>663 - Walls and Gates (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: BFS
* Company: Google, Facebook
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/walls-and-gates

<br></br>



## Description
----
You are given a m x n 2D grid initialized with these three possible values.
1. -1 - A wall or an obstacle.
2. 0 - A gate.
3. INF - Infinity means an empty room. We use the value 2^31 - 1 = 2147483647 to represent INF as you may assume that the distance to a gate is less than 2147483647.

Fill each empty room with the distance to its nearest gate. If it is impossible to reach a Gate, that room should remain filled with INF

<br></br>



## Example
----
Input: [[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]

Output: [[3,-1,0,1],[2,2,1,-1],[1,-1,2,-1],[0,-1,3,4]]

Explanation: the 2D grid is:

```
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
```

the answer is:

```
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```

<br></br>



## Solution
----
算法:
1. 以每个房间为起点BFS。
2. 以每个门为起点BFS，核心点是
    - 队列先进先出，保证了离各自门最近的各个点都会先被遍历。
    - 判断下一个访问的是通过检查值是否为Integer.MAX_VALUE，以此扮演visited。

<br>


### Go
```go
func GetRoomDistance2(rooms *[][]int) {
	q := make([]int, 0)
	for k1, v1 := range *rooms {
		for k2, v2 := range v1 {
			if v2 == 0 {
				q = append(q, k1, k2)
			}
		}
	}
	ops1 := []int{1, -1, 0, 0}
	ops2 := []int{0, 0, 1, -1}

	for len(q) > 0 {
		x, y := q[0], q[1]
		q = q[2:]
		for i := 0; i < 4; i++ {
			m, n := x+ops1[i], y+ops2[i]
			if m >= 0 && m < len(*rooms) && n >= 0 && n < len((*rooms)[0]) && (*rooms)[m][n] == 2147483647 {
				q = append(q, m, n)
				(*rooms)[m][n] = (*rooms)[x][y] + 1
			}
		}
	}
}
```

### Python
```python
from collections import deque

class Solution:
    """
    @param rooms: m x n 2D grid
    @return: nothing
    """
    def wallsAndGates(self, matrix):
        neighbors = [(0, 1), (0, -1), (1, 0), (-1, 0)]
        q = deque([])
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                if matrix[i][j] == 0:
                    q.append((i, j))
        while len(q) > 0:
            node = q.popleft()
            for neighbor in neighbors:
                new_x, new_y = node[0] + neighbor[0], node[1] + neighbor[1]
                if new_x >= 0 and new_x < len(matrix) and new_y >= 0 and new_y < len(matrix[0]) and (matrix[new_x][new_y] == 2147483647 or matrix[new_x][new_y] > matrix[node[0]][node[1]] + 1):
                    matrix[new_x][new_y] = matrix[node[0]][node[1]] + 1
                    q.append((new_x, new_y))
        return matrix
```
