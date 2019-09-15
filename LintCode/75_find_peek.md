# <center>75 - Find Peek Elements (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/find-peak-element/description

<br></br>



## Description
----
A peak element is an element that is greater than its neighbors. Given an input array nums, where nums[i] ≠ nums[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

<br></br>



## Example
----
Input: nums = [1,2,3,1]
Output: 2

<br></br>



## Solution
----
### Java
```java
public class GetPeak {
	public int findPeak(int[] nums) {
		if (nums == null) 
			return -1;
		
		int low = 1, high = nums.length - 1;
		while (low + 1 < high) {
			int mid = low + (high - low) / 2;
			if (nums[mid - 1] < nums[mid] && nums[mid] > nums[mid + 1])
				return mid;
			if (nums[mid] > nums[mid - 1])  // mid左侧数据呈上峰趋势
				low = mid;
			else // mid左侧数据呈下峰趋势
				high = mid;
		}
		
		if (nums[low] < nums[high]) 
			return high;
		return low;
	}
}
```

<br>


### Go
```go
func GetPeakElement(nums []int) int {
	low, high := 0, len(nums)-1
	for low+1 < high {
		mid := low + (high-low)/2
		if nums[mid-1] < nums[mid] && nums[mid] > nums[mid+1] {
			return mid
		}
		if nums[mid-1] < nums[mid] {
			low = mid
		} else {
			high = mid
		}
	}

	if nums[low] < nums[high] {
		return high
	}
	return low
}
```

<br>


### Python
----
```python
class GetPeakElement:
    def solution(self, nums) -> int:
        low, high = 0, len(nums) - 1
        while low + 1 < high:
            mid = low + (high - low) // 2
            if nums[mid - 1] < nums[mid] and nums[mid] > nums[mid + 1]:
                return mid
            if nums[mid - 1] < nums[mid]:
                low = mid
            else:
                high = mid

        if nums[low] < nums[high]:
            return high
        return low
```