# <center>159 - Find Minimum in Rotated Sorted Array (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Medium
* Company: Microsoft
* Link: https://www.lintcode.com/problem/find-minimum-in-rotated-sorted-array/description

<br></br>



## Description
----
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

<br></br>



## Example
----
Input: [3,4,5,1,2] 
Output: 1

<br></br>



## Solution
----
### Java
```java
public class GetMinInRotatedSortedArray1 {
    /**
     * @param nums: a rotated sorted array
     * @return: the minimum number in the array
     */
    public int findMin(int[] nums) {
        if (nums == null || nums.length == 0) 
            return -1;
        
        int low = 0, high = nums.length - 1;
        
        while (low + 1 < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] >= nums[low] && nums[mid] <= nums[high]) // for the case: 0, 1, 2
                return nums[low];
            if (nums[mid] >= nums[low]) 
                low = mid;
            else 
                high = mid;
        }
        
        if (nums[low] <= nums[high]) 
            return nums[low];
        return nums[high];
    }
}
```

<br>


### Go
```go
func GetMin(nums []int) int {
	l := len(nums)
	if l < 1 {
		panic("error")
	}

	low, high := 0, l-1
	for low+1 < high {
		mid := low + (high-low)/2
		if nums[low] <= nums[mid] && nums[mid] <= nums[high] {
			return nums[low]
		}
		if nums[low] <= nums[mid] {
			low = mid
		} else {
			high = mid
		}
	}

	if nums[low] <= nums[high] {
		return nums[low]
	}
	return nums[high]
}
```

<br>


### Python
```python
class GetMinRotatedSortedArr1:
    def solution(self, nums) -> int:
        l = len(nums)
        if l < 1:
            return None

        low, high = 0, l - 1
        while low + 1 < high:
            mid = low + (high - low) // 2
            if nums[low] <= nums[mid] <= nums[high]:
                return nums[low]
            if nums[low] <= nums[mid]:
                low = mid
            else:
                high = mid

        if nums[low] <= nums[high]:
            return nums[low]
        return nums[high]
```