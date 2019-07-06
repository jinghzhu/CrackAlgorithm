# <center>48 - Rotate Image (M)</center> 



<br></br>

* Tag: Array, Matrix
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Amazon, Microsoft, Apple
* Link: https://leetcode.com/problems/rotate-image/

<br></br>



## Description
----
You are given an n x n 2D matrix representing an image. Rotate the image by 90 degrees (clockwise).

You have to rotate the image in-place, which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

<br></br>



## Example
----
Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],

rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

<br></br>



## Solution
----
Steps: (for easy description, assume it is a 3x3 matrix.)
1. Following the diagonal, only deal with the out line of each sub-matrix.

    For example, the 1st round is for the sub-matrix rounded by [0,0], [0,2], [2,2] and [2,0]. The out line is [0,0] -> [0,2], ..., [2,0] -> [0.0].

2. Starting from the first outline, which is [0,0] -> [0,2] in our case, place each element to its final position one by one. 

    For example, the first element is at [0,0]. Then place it to [0,2]. Now, place the old [0,2] to [2,2]. It will perform the placement 4 times for each element.
    
    Please note that don't try to place the last element, which is [0,2] in our case. Because it has already been updated in the first iteration of placement.

<br>


### Java
```java
public class RotateImage {
	/**
     * @param matrix: a lists of integers
     * @return: nothing
     */
    public void rotate(int[][] matrix) {
        if (matrix == null)
            return;
        
        int l = matrix.length;
        for (int i = 0; i < l / 2; i++)
        	// l-i-1 is important. No need to deal with the last element in each round.
    		// Because it is already processed in the 1st for iteration.
            for (int j = i; j < l - i - 1; j++) {
            	// How to easily find the x-y position in each placement?
    			// Just image whether the row or column will change in each iteration. If
            	// not changed, use i to calculate it. Otherwise, use j.
    			// For example, for the iterations for [0,0] - [0,2] => [0,2] - [2,2], after
            	// the placement, the column of each new position is 2 which is not changed,
            	// like [0,0] => [0,2], [0,1] => [1,2], and [0,2] => [2,2]. So, it should be
            	// matrix[j][l-1-i] = matrix[i][j].
                int temp = matrix[j][l - i - 1];
                matrix[j][l - i - 1] = matrix[i][j];
                
                int k = temp;
                temp = matrix[l - i - 1][l - 1 - j];
                matrix[l - i - 1][l - 1 - j] = k;
                
                k = temp;
                temp = matrix[l - 1 - j][i];
                matrix[l - 1 - j][i] = k;
                
                matrix[i][j] = temp;
            }
    }
}
```

<br>


### Python
```python
class RotateImage:
    def rotate(self, matrix) -> None:
        l = len(matrix)
        for i in range(l // 2):
            j = i
            #  l - i - 1 is important. No need to deal with the last element in each round.
            #  Because it is already processed in the 1st for iteration.
            while j < l - i - 1:
                #  How to easily find the x-y position in each placement?
                #  Just image whether the row or column will change in each iteration. If not changed, use i to
                #  calculate it.Otherwise, use j.
                #  For example, for the iterations for[0, 0] -[0, 2] = >[0, 2] -[2, 2], after the placement, the
                #  column of each new position is 2 which is not changed, like[0, 0] = > [0, 2], [0, 1] = > [1, 2], and
                #  [0, 2] = > [2, 2]. So, it should be matrix[j][l - 1 - i] = matrix[i][j].
                temp = matrix[j][l - i - 1]
                matrix[j][l - i - 1] = matrix[i][j]
                matrix[l - 1 - i][l - 1 - j], temp = temp, matrix[l - 1 - i][l - 1 - j]
                matrix[l - 1 - j][i], temp = temp, matrix[l - 1 - j][i]
                matrix[i][j] = temp
                j += 1
```

<br>


### Go
```go
func RotateImage(matrix [][]int) {
	l := len(matrix)
	for i := 0; i < l/2; i++ {
		// l-i-1 is important. No need to deal with the last element in each round.
		// Because it is already processed in the 1st for iteration.
		for j := i; j < l-i-1; j++ {
			// How to easily find the x-y position in each placement?
			// Just image whether the row or column will change in each iteration. If not changed, use i to calculate it.
			// Otherwise, use j.
			// For example, for the iterations for [0,0] - [0,2] => [0,2] - [2,2], after the placement, the column of each
			// new position is 2 which is not changed, like [0,0] => [0,2], [0,1] => [1,2], and [0,2] => [2,2]. So, it
			// should be matrix[j][l-1-i] = matrix[i][j].
			temp := matrix[j][l-1-i]
			matrix[j][l-1-i] = matrix[i][j]
			matrix[l-1-i][l-1-j], temp = temp, matrix[l-1-i][l-1-j]
			matrix[l-1-j][i], temp = temp, matrix[l-1-j][i]
			matrix[i][j] = temp
		}
	}
}
```
