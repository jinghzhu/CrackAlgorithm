# <center>81 - Search in Rotated Sorted Array (M)</center> 



<br></br>

* Tag: Binary Search
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Link: https://leetcode.com/problems/search-in-rotated-sorted-array-ii/

<br></br>



## Description
----
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,0,1,2,2,5,6] might become [2,5,6,0,0,1,2]).

You are given a target value to search. If found in the array return true, otherwise return false.

<br></br>



## Example
----
Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false

<br></br>



## Solution
----
As there may be duplicates, need to consider such case `[0,0,0,1,0]`.

Solutions:
1. If `nums[low] == nums[mid] && nums[mid] == nums[high]`, can't directly check which half the target may be in. So, do `low++` or `high--` to narrow down search scope.
2. Other cases, just follow normal binary search algorithm.

Time complexity:
* Best = Avg: O(logn)
* Worst: O(n)

<br>


### Java
```java
public class GetInRotatedSortedArray2 {
	public boolean solution1(int[] A, int target) {
        if (A == null || A.length == 0)
             return false;
        for (int i : A)
            if (i == target)
                return true;
        
        return false;
    }
	
	/**
     * @param A: an integer rotated sorted array and duplicates are allowed
     * @param target: An integer
     * @return: a boolean 
     */
    public boolean solution2(int[] A, int target) {
        if (A == null || A.length == 0)
            return false;
    
        int low = 0, high = A.length - 1;
        while(low + 1 < high) {
            int mid = low + (high - low) / 2;
            if (A[mid] == target)
                return true;
            if (A[low] == A[mid] && A[mid] == A[high]) {
            	// [0,0,0,1,0]
                high--;
            } else if (A[low] <= A[mid] && A[mid] <= A[high]) {
            	// low and high in normal case - [0,1,2,...,5,6]
                if (target < A[mid])
                    high = mid;
                else
                    low = mid;
            } else if (A[low] <= A[mid] && A[mid] >= A[high]) {
            	// [3,4,...,6,7,0,...,2] low in first half, which is [3,4,...,6], and high in
            	// second half, which is [0,...2]. Assume mid is in first half. Then check 2
            	// cases for target position, in first or second half.
                if (target > A[mid])
                    low = mid;
                else if (A[low] <= target)
                    high = mid;
                else
                    low = mid;
            } else {
            	// [3,4,...,6,7,0,...,2] low in first half, which is [3,4,...,6], and high in
            	// second half, which is [0,...2]. Assume mid is in second half. Then check 2
            	// cases for target position, in first or second half.
                if (target > A[mid] && target <= A[high])
                    low = mid;
                else
                    high = mid;
            }
        }
    
        return A[low] == target || A[high] == target;
    }
}
```

<br>


### Go
```go
func GetInRotatedSortedArr2(nums []int, target int) bool {
	if len(nums) < 1 {
		return false
	}

	low, high := 0, len(nums)-1
	for low+1 < high {
		mid := low + (high-low)/2
		if nums[mid] == target {
			return true
		}
		if nums[low] == nums[mid] && nums[mid] == nums[high] {
			// [0,0,0,1,0]
			high--
		} else if nums[low] <= nums[mid] && nums[mid] <= nums[high] {
			// low and high in normal case - [0,1,2,...,5,6]
			if target < nums[mid] {
				high = mid
			} else {
				low = mid
			}
		} else if nums[low] <= nums[mid] && nums[mid] >= nums[high] {
			// [3,4,...,6,7,0,...,2] low in first half, which is [3,4,...,6], and high in
			// second half, which is [0,...2]. Assume mid is in first half. Then check 2
			// cases for target position, in first or second half.
			if target < nums[mid] && target >= nums[low] {
				high = mid
			} else {
				low = mid
			}
		} else {
			// [3,4,...,6,7,0,...,2] low in first half, which is [3,4,...,6], and high in
			// second half, which is [0,...2]. Assume mid is in second half. Then check 2
			// cases for target position, in first or second half.
			if target > nums[mid] && target <= nums[high] {
				low = mid
			} else {
				high = mid
			}
		}
	}

	return nums[low] == target || nums[high] == target
}
```

<br>


### Python
```python
class Solution:
    def search(self, nums: List[int], target: int) -> bool:
        if not nums or len(nums) < 1:
            return False
        
        low, high = 0, len(nums) - 1
        while low + 1 < high:
            mid = (low + high) // 2
            if nums[mid] == target:
                return True
            if nums[low] == nums[mid] == nums[high]:
                high -= 1
            elif nums[low] <= nums[mid] <= nums[high]:
                if target > nums[mid]:
                    low = mid
                else:
                    high = mid
            elif nums[low] <= nums[mid] and nums[mid] >= nums[high]:
                if target >= nums[low] and target < nums[mid]:
                    high = mid
                else:
                    low = mid
            else:
                if target > nums[mid] and target <= nums[high]:
                    low = mid
                else:
                    high = mid
        
        return nums[low] == target or nums[high] == target
```