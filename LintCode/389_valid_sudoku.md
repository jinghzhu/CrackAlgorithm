# <center>389 - Valid Sudoku (E)</center> 



<br></br>

* Tag: Matrix
* Company: Apple, SnapChat, Uber
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/valid-sudoku/description

<br></br>



## Description
----
Determine whether a Sudoku is valid.

The Sudoku board could be partially filled, where empty cells are filled with the character ..

<br></br>



## Example
----
Input:
```
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```
Output: true

Input:
```
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
```
Output: false


<br></br>



## Solution
----
### Java
```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        boolean[] visited = new boolean[9];
	    
        for(int i=0; i<9; i++) {
		    Arrays.fill(visited, false);
		    for(int j=0; j<9; j++)
			    if(!isValid(visited, board[i][j]))
				    return false;
	    }
        
	    for(int i=0; i<9; i++) {
		    Arrays.fill(visited, false);
		    for(int j=0; j<9; j++)
			    if(!isValid(visited, board[j][i]))
				    return false;
		}

	    for(int i=0; i<9; i+=3)
		    for(int j=0; j<9; j+=3) {
			    Arrays.fill(visited, false);
			    for(int k=0; k<9; k++)
				    if(!isValid(visited, board[i+k/3][j+k%3]))
					    return false;
			}
	
	    return true;
    }
    
    
    private boolean isValid(boolean[] visited, char ch){
	    if(ch == '.')
		    return true;
	
	    int num = ch - '0';
	    if(num < 1 || num > 9 || visited[num-1])
		    return false;
	
	    visited[num-1] = true;
	    return true;
    }
}
```

<br>


### Python
```python
class Solution:
    """
    @param board: the board
    @return: whether the Sudoku is valid
    """
    def isValidSudoku(self, board):
        big = set()
        for i in range(0, 9):
            for j in range(0, 9):
                if board[i][j] != '.':
                    cur = board[i][j]
                    if (i, cur) in big or (cur, j) in big or (int(i / 3), int(j / 3), cur) in big:
                        return False
                    big.add((i, cur))
                    big.add((cur, j))
                    big.add((int(i / 3), int(j / 3), cur))
        return True

```
