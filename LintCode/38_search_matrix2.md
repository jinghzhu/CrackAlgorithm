# <center>38 - Search a 2D Matrix II (M)</center> 



<br></br>

* Tag: 2D Array
* Company: Google, Apple, Amazon
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/search-a-2d-matrix-ii/description

<br></br>



## Description
----
Write an efficient algorithm that searches for a value in an m x n matrix, return the occurrence of it.

This matrix has the following properties:
1. Integers in each row are sorted from left to right.
2. Integers in each column are sorted from up to bottom.
3. No duplicate integers in each row or column.

<br></br>



## Example
----
Input:
    [
      [1, 3, 5, 7],
      [2, 4, 7, 8],
      [3, 5, 9, 10]
    ]
    target = 3

Output:2

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
        int ans = 0;
        while (r >= 0 && c < matrix[0].length) {
            if (target == matrix[r][c]) {
                ans++;
                r--;
                c++; // Because [x-1][y] must < [x][y]
            } else if (target < matrix[r][c]) {
                r--;
            } else {
                c++;
            }
        }
        return ans;
    }
}
```

<br>


### Go
```go
func searchMatrix (matrix [][]int, target int) int {
    if len(matrix) == 0 || len(matrix[0]) == 0 {
        return 0
    }
    x, y, count := len(matrix) - 1, 0, 0
    for x >= 0 && y < len(matrix[0]) {
        if matrix[x][y] == target {
            count++
            x--
            y++
        } else if matrix[x][y] > target {
            x--
        } else {
            y++
        }
    }
    
    return count
}
```

<br>


### Python
```python
class Solution:
    """
    @param matrix: A list of lists of integers
    @param target: An integer you want to search in matrix
    @return: An integer indicate the total occurrence of target in the given matrix
    """
    def searchMatrix(self, matrix, target):
        count = 0
        if not matrix or len(matrix) == 0 or len(matrix[0]) == 0:
            return 0
        x, y = len(matrix) - 1, 0
        while x >= 0 and y < len(matrix[0]):
            if matrix[x][y] == target:
                x -= 1
                y += 1  # Because [x-1][y] must < [x][y]
                count += 1
            elif matrix[x][y] > target:
                x -= 1
            else:
                y += 1
        
        return count
```