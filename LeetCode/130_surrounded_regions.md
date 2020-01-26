# <center>130 - Surrounded Regions (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: BFS
* Difficulty: Medium
* Link: https://leetcode.com/problems/surrounded-regions/

<br></br>



## Description
----
Given a 2D board containing 'X' and 'O', capture all regions surrounded by 'X'.

A region is captured by flipping all 'O''s into 'X''s in that surrounded region.

<br></br>



## Example
----
Input:

```
  X X X X
  X O O X
  X X O X
  X O X X
```

Output:

```
  X X X X
  X X X X
  X X X X
  X O X X
```

<br></br>



## Solution
----
### Go
func solve(board [][]byte)  {
    visited := make(map[string]bool)
    for x := range board {
        for y := range board[x] {
            if board[x][y] == 'X' || visited[translateToStr(x, y)] {
                continue
            }
            visitedList, canCaptured := check(board, visited, x, y)
            if canCaptured {
                for len(visitedList) > 0 {
                    x1, y1 := visitedList[0], visitedList[1]
                    visitedList = visitedList[2:]
                    board[x1][y1] = 'X'
                }
            } else {
                for len(visitedList) > 0 {
                    x1, y1 := visitedList[0], visitedList[1]
                    visitedList = visitedList[2:]
                    visited[translateToStr(x1, y1)] = true
                }
            }
        }
    }
}

func check(board [][]byte, visited map[string]bool, x, y int) ([]int, bool) {
    list := make([]int, 0)
    list = append(list, x, y)
    visited1 := make(map[string]bool)
    visited1[translateToStr(x, y)] = true
    q := make([]int, 0)
    q = append(q, x, y)
    opsX, opsY := []int{1, -1, 0, 0}, []int{0, 0, 1, -1}
    
    for len(q) > 0 {
        x, y = q[0], q[1]
        q = q[2:]
        for i := 0; i < 4; i++ {
            x1, y1 := x + opsX[i], y + opsY[i]
            if x1 < 0 || x1 >= len(board) || y1 < 0 || y1 >= len(board[0]) || visited[translateToStr(x1, y1)] {
                return list, false
            }
            if board[x1][y1] == 'O' && !visited[translateToStr(x1, y1)] {
                visited1[translateToStr(x1, y1)] = true
                q, list = append(q, x1, y1), append(list, x1, y1)
            }
        }
    }
    
    return list, true
}

func translateToStr(x, y int) string {
    return fmt.Sprintf("%d+%d", x, y)
}