# <center>37 - Sudoku Solver (H)</center> 



<br></br>

* Tag: Backtracking, DFS, Recursion
* Difficulty: Hard
* Company: Snapchat, Uber
* Link: https://leetcode.com/problems/sudoku-solver/

<br></br>



## Description
----
Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy all of the following rules:
1. Each of the digits 1-9 must occur exactly once in each row.
2. Each of the digits 1-9 must occur exactly once in each column.
3. Each of the the digits 1-9 must occur exactly once in each of the 9 3x3 sub-boxes of the grid.

Empty cells are indicated by the character '.'.

<br></br>



## Solution
----
算法：
1. 找到第一个为0的位置传入dfs中
2. 遇到非0的点位置＋1继续往下找，直到找到为零的位置进入dfs。

<br>


### Java
```java
public class Sudoku {
	public void solution(int[][] board) {
        canSolve(board, 0, 0);
    }
    
    private boolean canSolve(int[][] board, int i, int j) {
        if (i == 9 && j == 0) {
            return true;
        }
        
        int nextI = j + 1 == 9 ? i + 1 : i;
        int nextJ = j + 1 == 9 ? 0 : j + 1;
        if (board[i][j] != 0) {
            return canSolve(board, nextI, nextJ);
        }
        
        for (int c = 1; c <= 9; c++) {
            if (isValid(board, i, j, c)) {
                board[i][j] = c;
                if (canSolve(board, nextI, nextJ)) {
                    return true;
                }
                board[i][j] = 0;
            }
        }
        return false;
    }
    
    private boolean isValid(int[][] board, int row, int col, int c) {
        for (int i = 0; i < 9; i++) {
            if (board[row][i] == c) return false;
            if (board[i][col] == c) return false;

            int x = row / 3 * 3 + i / 3;
            int y = col / 3 * 3 + i % 3;
            if (board[x][y] == c) return false;
        }
        return true;
    }
}
```