# <center>463 - Island Perimeter (E)</center> 



<br></br>

* Tag: Array
* Company: Google
* Difficulty: Easy
* Link: https://leetcode.com/problems/island-perimeter/

<br></br>



## Description
----
You are given a map in form of a two-dimensional integer grid where 1 represents land and 0 represents water. Grid cells are connected horizontally/vertically (not diagonally). The grid is completely surrounded by water, and there is exactly one island (i.e., one or more connected land cells). The island doesn't have "lakes" (water inside that isn't connected to the water around the island). One cell is a square with side length 1. The grid is rectangular, width and height don't exceed 100. Determine the perimeter of the island.

<br></br>



## Example
----
Input:
```
[[0,1,0,0],
 [1,1,1,0],
 [0,1,0,0],
 [1,1,0,0]]
```

Output: 16 The perimeter is the 16 yellow stripes in the image below:

![](./Images/island_perimeter.png)

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param grid: a 2D array
     * @return: the perimeter of the island
     */
    public int islandPerimeter(int[][] grid) {
        if (grid == null)
            return 0;
        
        int result = 0;
        for (int i = 0; i < grid.length; i++)
            for (int j = 0; j < grid[i].length; j++)
                if (grid[i][j] == 1)
                    result += calculate(grid, i, j);
        
        return result;
    }
    
    private int calculate(int[][] grid, int i, int j) {
        int count = 0, l1 = grid.length, l2 = grid[0].length;
        if (j == 0 || grid[i][j - 1] == 0)
            count++;
        if (i == 0 || grid[i - 1][j] == 0)
            count++;
        if (j == l2 - 1 || grid[i][j + 1] == 0)
            count++;
        if (i == l1 - 1 || grid[i + 1][j] == 0)
            count++;
        
        return count;
    }
}
```

<br>


### Go
```go
func islandPerimeter(grid [][]int) int {
    result := 0
    for i := 0; i < len(grid); i++ {
        for j := 0; j < len(grid[i]); j++ {
            if grid[i][j] == 1 {
                result += calculate(grid, i, j)
            }
        }
    }
    
    return result
}

func calculate(grid [][]int, i, j int) int {
    count, l1, l2 := 0, len(grid), len(grid[0])
    if j == 0 || grid[i][j - 1] == 0 {
        count++
    }
    if i == 0 || grid[i - 1][j] == 0 {
        count++
    }
    if j == l2 - 1 || grid[i][j + 1] == 0 {
        count++
    }
    if i == l1 - 1 || grid[i + 1][j] == 0 {
        count++
    }
    
    return count
}
```

<br>


### Python
```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        if not grid:
            return 0
        result, l1, l2 = 0, len(grid), len(grid[0])
        for i in range(l1):
            for j in range(l2):
                if grid[i][j] == 1:
                    result += self.calculate(grid, i, j)
        return result
    
    def calculate(self, a, i, j):
        count, l1, l2 = 0, len(a), len(a[0])
        if j == 0 or a[i][j - 1] == 0:
            count += 1
        if i == 0 or a[i - 1][j] == 0:
            count += 1
        if j == l2 - 1 or a[i][j + 1] == 0:
            count += 1
        if i == l1 - 1 or a[i + 1][j] == 0:
            count += 1
        return count
```