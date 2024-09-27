# <center>704 - Binary Search (E)</center>



<br></br>

* Tag: Binary Search
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/binary-search/

<br></br>



## Description
Given a sorted (in ascending order) integer array nums of n elements and a target value, write a function to search target in nums. If target exists, then return its index, otherwise return -1.

<br></br>



## Example
Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4

<br></br>



## Solution
### Go
```go
func BinarySearch(data []int, target int) int {
	if data == nil {
		return -1
	}

	return binarySearch(data, target, 0, len(data)-1)
}

func binarySearch(data []int, target, low, high int) int {
	if low > high {
		return -1
	}

	mid := (high-low)/2 + low
	if data[mid] == target {
		return mid
	}

	if data[mid] < target {
		return binarySearch(data, target, mid+1, high)
	}

	return binarySearch(data, target, low, mid-1)
}
```

```go
func BinarySearchN2(nums []int, target int) int {
	if nums == nil {
		return -1
	}

	low, high := 0, len(nums)-1
	for low <= high {
		mid := (high-low)/2 + low
		if nums[mid] == target {
			return mid
		}

		if nums[mid] > target {
			high = mid - 1
		} else {
			low = mid + 1
		}
	}

	return -1
}
```

<br>


### Python
```python
class BinarySearch:
    def search1(self, nums, target: int) -> int:
        return self.search1_helper(nums, target, 0, len(nums) - 1)

    def search1_helper(self, nums, target, low, high):
        if low < 0 or high >= len(nums) or low > high:
            return -1

        mid = low + (high - low) // 2
        if nums[mid] == target:
            return mid
        if nums[mid] < target:
            return self.search1_helper(nums, target, mid + 1, high)
        return self.search1_helper(nums, target, low, mid - 1)

    def search2(self, nums, target: int) -> int:
        low, high = 0, len(nums) - 1
        while low + 1 < high:
            mid = low + (high - low) // 2
            if nums[mid] == target:
                return mid
            if nums[mid] < target:
                low = mid
            else:
                high = mid

        if nums[low] == target:
            return low
        if nums[high] == target:
            return high

        return -1
```

<br>


### Java
```java
public class BinarySearch {
	public int search(int[]nums, int target) {
		if (nums == null || nums.length == 0 || target < nums[0] || target > nums[nums.length - 1])
			return -1;

		return search(nums, target, 0, nums.length - 1);
	}

    private int search(int[] nums, int target, int low, int high) {
        if (high < low || low < 0 || high > nums.length - 1)
            return -1;

        int mid = (high - low) / 2 + low;
        if (nums[mid] == target)
            return mid;
        if (nums[mid] > target)
            return search(nums, target, low, mid - 1);

        return search(nums, target, mid + 1, high);
    }

	public int searchN(int[] nums, int target) {
	    if (nums == null || nums.length == 0)
	        return -1;

	    int low = 0, high = nums.length - 1;
	    while (low + 1 < high) {
	        int mid = (high - low) / 2 + low;
	        if (nums[mid] == target)
	            return mid;
	        if (nums[mid] < target)
	        	low = mid;
	        else
	        	high = mid;
	    }

	    if (nums[low] == target)
	        return low;
	    if (nums[high] == target)
	        return high;
	    return -1;
	}
}
```