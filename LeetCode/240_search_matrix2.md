# <center>240 - Search a 2D Matrix II (M)</center> 



<br></br>

* Tag: 2D Array
* Company: Google, Apple, Amazon
* Difficulty: Medium
* Link: https://leetcode.com/problems/search-a-2d-matrix-ii/

<br></br>



## Description
----
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
1. Integers in each row are sorted in ascending from left to right.
2. Integers in each column are sorted in ascending from top to bottom.

<br></br>



## Example
----
Consider the following matrix:
```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```

Given target = 5, return true.
Given target = 20, return false.

<br></br>



## Solution
----
### Java
```java
public class GetFromMatrix2 {
	/**
     * @param matrix: A list of lists of integers
     * @param: A number you want to search in the matrix
     * @return: An integer indicate the occurrence of target in the given matrix
     */
	// Best: O(m), Avg = Worst: O(m + n)
	// 最坏情况是每行每列都有目标元素。因为每行每列不能有重复，而且有序。所以，该算法最多只会查找m+n个元素。
    public int searchMatrix(int[][] matrix, int target) {
        int r = matrix.length - 1;
        int c = 0;
        while (r >= 0 && c < matrix[0].length) {
            if (target == matrix[r][c]) {
                return true;
            }
            if (target < matrix[r][c]) {
                r--;
            } else {
                c++;
            }
        }

        return false;
    }
}
```

<br>


### Go
```go
func searchMatrix (matrix [][]int, target int) int {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return false
    }
    x, y := len(matrix) - 1, 0
    for x >= 0 && y < len(matrix[0]) {
        if matrix[x][y] == target {
            return true
        } else if matrix[x][y] > target {
            x--
        } else {
            y++
        }
    }
    
    return false
}
```

<br>


### Python
```python
class Solution:
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if not matrix or len(matrix) == 0 or len(matrix[0]) == 0:
            return False
        x, y = len(matrix) - 1, 0
        while x >= 0 and y < len(matrix[0]):
            if matrix[x][y] == target:
                return True
            if matrix[x][y] > target:
                x -= 1
            else:
                y += 1
        
        return False
```