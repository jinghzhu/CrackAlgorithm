# <center>62 - Search in Rotated Sorted Array (M)</center> 



<br></br>

* Tag: Binary Search
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Company: LinkedIn, Uber, Facebook
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/search-in-rotated-sorted-array/description

<br></br>



## Description
----
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).

You are given a target value to search. If found in the array return its index, otherwise return -1.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

<br></br>



## Example
----
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

<br></br>



## Solution
----
### Java
```java
public class GetInRotatedSortedArray1 {
	/** 
     *@param A : an integer rotated sorted array
     *@param target :  an integer to be searched
     *return : an integer
     */
    public int search(int[] A, int target) {
        if (A == null || A.length == 0)
            return -1;
        
        int low = 0, high = A.length - 1;
        
        while (low + 1 < high) {
            int mid = low + (high - low) / 2;
            if (A[mid] == target)
                return mid;
            // the array between low and high is consisted of two sub-arrays.
        	// like [0, 1, 2, 3] + [-5, -4, -3, -2]
            if (A[low] > A[high]) { 
            	// target and mid in first array, like target = 1, A[mid] = 2
                if (A[mid] > target && target > A[low])
                	high = mid;
                // target and mid in second array, like target = -4, A[mid] = -3
                else if (A[mid] > target && A[mid] < A[high])
                	high = mid;
                // target in 1st, mid in 2nd, like target = 2, A[mid] = -3
                else if (A[mid] < target && A[mid] < A[high] && A[low] < target)
                	high = mid;
                // target and mid in 1st, like target = 3, A[mid] = 2
                else if (A[mid] < target && A[mid] > A[low])
                	low = mid;
                // mid in 1st, target in 2nd, like A[mid] = 2, target = -3
                else if (A[mid] > target && A[mid] > A[low] && A[high] > target)
                	low = mid;
                else // the last case
                	low = mid;
            } else { // in this case, the low-high-mid-target are in the ascending array
                if (A[mid] < target)
                    low = mid;
                else
                    high = mid;
            }
        }
        
        if (A[low] == target)
            return low;
        if (A[high] == target)
            return high;
        return -1;
    }
}
```

<br>


### Go
```go
func GetInSortedRotatedArr(nums []int, target int) int {
	l := len(nums)
	if l == 0 {
		return -1
	}

	low, high := 0, l-1

	for low+1 < high {
		mid := low + (high-low)/2
		if nums[mid] == target {
			return mid
		}
		if nums[low] <= nums[mid] && nums[mid] <= nums[high] {
			if nums[mid] < target {
				low = mid
			} else {
				high = mid
			}
		} else if nums[low] <= nums[mid] && nums[high] <= nums[low] {
			if nums[mid] < target {
				low = mid
			} else if nums[low] <= target { // important
				high = mid
			} else { // important
				low = mid
			}
		} else {
			if nums[mid] > target {
				high = mid
			} else if target <= nums[high] { // important
				low = mid
			} else { // important
				high = mid
			}
		}
	}

	if nums[low] == target {
		return low
	}
	if nums[high] == target {
		return high
	}
	return -1
}
```

<br>


### Python
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if not nums or len(nums) < 1:
            return -1
        
        low, high = 0, len(nums) - 1
        while low + 1 < high:
            mid = (low + high) // 2
            if nums[mid] == target:
                return mid
            if nums[low] <= nums[mid] <= nums[high]:
                if nums[mid] < target:
                    low = mid
                else:
                    high = mid
            elif nums[low] <= nums[mid] and nums[high] <= nums[mid]:
                if nums[mid] < target:
                    low = mid
                elif nums[low] <= target:
                    high = mid
                else:
                    low = mid
            else:
                if target < nums[mid]:
                    high = mid
                elif target <= nums[high]:
                    low = mid
                else:
                    high = mid
        
        if nums[low] == target:
            return low
        if nums[high] == target:
            return high
        return -1
```