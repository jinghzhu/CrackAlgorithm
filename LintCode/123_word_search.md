# <center>123 - Word Search (M)</center> 



<br></br>

* Tag: DFS, Recursion, Backtracking
* Difficulty: Medium
* Company: Bloomberg, Microsoft, Facebook
* Link: https://leetcode.com/problems/word-search/

<br></br>



## Description
----
Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

<br></br>



## Example
----
```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
```
Given word = "ABCCED", return true.

Given word = "SEE", return true.

Given word = "ABCB", return false.

<br></br>



## Solution
----
算法：
1. 往下一层递归时就上下左右移动。
2. 引入变量start，标记当前递归检查第几个字符。如果start == len(str)，返回true。
3. 为方便回溯和避免重复访问，每次都把board当前访问到字符暂时替换。

<br>


### Go
```go
func exist(board [][]byte, word string) bool {
    if len(word) < 1 {
        return true
    }

    for i := 0; i < len(board); i++ {
        for j := 0; j < len(board[0]); j++ {
            if board[i][j] == word[0] && dfs(board, i, j, 0, word) {
                return true
            }
        }
    }
    
    return false
}

func dfs(board [][]byte, i, j, start int, word string) bool {
    if start == len(word) {
        return true
    }
    if i < 0 || j < 0 || i >= len(board) || j >= len(board[0]) || board[i][j] != word[start] {
        return false
    }
    board[i][j] = ','
    start++
    flag := dfs(board, i + 1, j, start, word) || dfs(board, i - 1, j, start, word) || dfs(board, i, j + 1, start, word) || dfs(board, i, j - 1, start, word)
    start--
    board[i][j] = word[start]
    
    return flag
}
```

<br>
