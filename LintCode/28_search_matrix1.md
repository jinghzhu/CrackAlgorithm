# <center>28 - Search a 2D Matrix (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/search-a-2d-matrix/description

<br></br>



## Description
----
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
1. Integers in each row are sorted from left to right.
2. The first integer of each row is greater than the last integer of the previous row.

<br></br>



## Example
----
Input:
    matrix = [
        [1,   3,  5,  7],
        [10, 11, 16, 20],
        [23, 30, 34, 50]
        ]
    target = 3

Output: true

<br></br>



## Solution
----
### Java
```java
public class GetFromMatirx1 {
	public boolean solution1(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || target < matrix[0][0] 
        		|| target > matrix[matrix.length - 1][matrix[0].length - 1]) // Important
        	return false;
        
        int row = findRow(matrix, target);
        if (matrix[row][0] == target) // just for performance improvement
        	return true;
        
        return binarySearch(matrix, row, target);
    }
    
    private int findRow(int[][] matrix, int target) {
    	int low = 0, high = matrix.length - 1;
    	
    	while (low + 1 < high) {
    		int mid = low + (high - low) >> 1;
    		if (matrix[mid][0] == target) 
    			return mid;
    		if (matrix[mid][0] < target) 
    			low = mid;
    		else 
    			high = mid;
    	}
    	
    	if (matrix[high][0] <= target) // a tricky bug here, need firstly check the high position
    		return high;
    	return low;
    }
    
    private boolean binarySearch(int[][] matrix, int row, int target) {
    	int low = 0, high = matrix[row].length - 1;
    	
    	while (low + 1 < high) {
    		int mid = low + (high - low) >> 1;
    		if (matrix[row][mid] == target) 
    			return true;
    		if (matrix[row][mid] < target) 
    			low = mid;
    		else 
    			high = mid;
    	}
    	
    	return matrix[row][low] == target || matrix[row][high] == target;
    }
    
    // only binary search once
    public boolean solution2(int[][] matrix, int target) {
        if (matrix == null || matrix.length == 0 || matrix[0] == null || matrix[0].length == 0) 
            return false;
        
        int row = matrix.length, column = matrix[0].length;
        int start = 0, end = row * column - 1;
        
        while (start + 1 < end) {
            int mid = start + (end - start) >> 1;
            int number = matrix[mid / column][mid % column];
            if (number == target) 
                return true;
            if (number < target) 
                start = mid;
            else 
                end = mid;
        }
        
        return matrix[start / column][start % column] == target || matrix[end / column][end % column] == target;
    }
}
```

<br>


### Go
```go
func GetFromMatrix1(matrix [][]int, target int) bool {
	l := len(matrix)
	if l == 0 {
		return false
	}
	low, high := 0, l-1
	for low+1 < high {
		mid := low + (high-low)/2
		if len(matrix[mid]) == 0 {
			return false
		}
		if matrix[mid][0] == target {
			return true
		}
		if matrix[mid][0] > target {
			high = mid
		} else {
			low = mid
		}
	}
	nums := matrix[low]
	if len(matrix[high]) > 0 && target >= matrix[high][0] {
		nums = matrix[high]
	}
	if len(nums) < 1 {
		return false
	}
	low, high = 0, len(nums)-1
	for low+1 < high {
		mid := low + (high-low)/2
		if nums[mid] == target {
			return true
		}
		if nums[mid] < target {
			low = mid
		} else {
			high = mid
		}
	}

	return nums[low] == target || nums[high] == target
}
```

<br>


### Python
```python
class GetFromMatrix1:
    def solution(self, matrix, target: int) -> bool:
        if not matrix or len(matrix) == 0:  # important
            return False

        l = len(matrix)
        low, high = 0, l - 1
        while low + 1 < high:
            mid = (low + high) // 2
            if len(matrix[mid]) == 0:  # important
                return False
            if matrix[mid][0] == target:
                return True
            if target < matrix[mid][0]:
                high = mid
            else:
                low = mid
        nums = matrix[low]
        if len(matrix[high]) > 0 and target >= matrix[high][0]:
            nums = matrix[high]
        if len(nums) < 1:
            return False
        low, high = 0, len(nums) - 1
        while low + 1 < high:
            mid = (low + high) // 2
            if nums[mid] == target:
                return True
            if nums[mid] > target:
                high = mid
            else:
                low = mid

        return nums[low] == target or nums[high] == target
```