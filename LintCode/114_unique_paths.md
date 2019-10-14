# <center>114 - Unique Paths (E)</center> 



<br></br>

* Tag: DP, 2D Array
* Difficulty: Easy
* Link: https://leetcode.com/problems/unique-paths/

<br></br>



## Description
----
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

<br></br>



## Example
----
Input: m = 3, n = 2

Output: 3

Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Right -> Down
2. Right -> Down -> Right
3. Down -> Right -> Right

<br></br>



## Solution
----
### Java
```java
public class UniquePaths {
	/**
     * @param m: positive integer (1 <= m <= 100)
     * @param n: positive integer (1 <= n <= 100)
     * @return: An integer
     */
    public int uniquePaths(int m, int n) {
        if (m < 1 || n < 1)
            return 0;
        
        // 状态定义：f[m][n]表示有多少种方法移动到m行n列。
        int[][] f = new int[m][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (i == 0 || j == 0) // important 边界条件
                    f[i][j] = 1;
                else
                    f[i][j] = f[i - 1][j] + f[i][j - 1]; // 状态转移方程
            }
        }
        
        return f[m - 1][n - 1];
    }
}
```

<br>


### Python
```python
class UniquePaths:
    def solution(self, m: int, n: int) -> int:
        if m < 1 or n < 1:
            return 0

        # 状态定义：f[m][n]表示有多少种方法移动到m行n列。
        f = [[0 for i in range(n)] for j in range(m)]
        for i in range(m):
            for j in range(n):
                if i == 0 or j == 0:  # important 边界条件
                    f[i][j] = 1
                else:
                    f[i][j] = f[i - 1][j] + f[i][j - 1]  # 状态转移方程

        return f[m - 1][n - 1]
```

<br>


### Go
```go
func UniquePaths(m, n int) int {
	if m < 0 || n < 0 {
		return 0
	}

	// 状态定义：f[m][n]表示有多少种方法移动到m行n列。
	f := make([][]int, m, m)
	for k := range f {
		f[k] = make([]int, n, n)
	}
	for i := 0; i < m; i++ {
		for j := 0; j < n; j++ {
			if i == 0 || j == 0 {
				f[i][j] = 1 // important 边界条件
			} else {
				f[i][j] = f[i-1][j] + f[i][j-1] // 状态转移方程
			}
		}
	}

	return f[m-1][n-1]
}
```
