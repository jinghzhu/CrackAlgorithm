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
1. 以[4, 5, 1, 2, 3]为例，设[4, 5]为前半段，[1, 2, 3]为后半段。
2. 按正常取中间值。按如下情况判断下一步low和high：
    1. 判断当前nums[low]、nums[mid]和nums[high]是否正常，即满足nums[low] < nums[mid] < nums[high]。如果是，按正常二分查找进行。
    2. 判断mid落在哪一段
        1. 如果落在前半段，即nums[low] < nums[mid] && nums[mid] > nums[high]
		    1. 如果target < nums[mid] && target >= nums[low]，说明在mid前，即high = mid。
			2. 否则说明在mid后，即low = mid。
		2. 如果落在后半段，即nums[low] > nums[high] && nums[mid] < nums[high]
			1. 如果target > nums[mid] && target <= nums[high]，说明在mid后，即low = mid。
			2. 否则说明在mid前，即high = mid。

<br>


### Java
```java
class Solution {
    public int search(int[] nums, int target) {
        if (nums == null || nums.length < 1)
            return -1;
        int low = 0, high = nums.length - 1;
        while (low + 1 < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target)
                return mid;
            if (nums[low] < nums[mid] && nums[mid] < nums[high]) {
                if (nums[mid] > target)
                    high = mid;
                else
                    low = mid;
            } else if (nums[mid] > nums[low]) {
                if (target < nums[mid] && target >= nums[low])
                    high = mid;
                else
                    low = mid;
            } else {
                if (target > nums[mid] && target <= nums[high])
                    low = mid;
                else
                    high = mid;
            }
        }
        
        if (nums[low] == target)
            return low;
        if (nums[high] == target)
            return high;
        return -1;
    }
}
```

<br>


### Go
```go
/**
 * @param A: an integer rotated sorted array
 * @param target: an integer to be searched
 * @return: an integer
 */
func search (nums []int, target int) int {
    low, high, l := 0, len(nums) - 1, len(nums)
    if l < 1 {
        return -1
    }
    for low + 1 < high {
        mid := low + (high - low) / 2
        if nums[mid] == target {
            return mid
        }
        if nums[low] < nums[mid] && nums[mid] < nums[high] {
            if nums[mid] < target {
                low = mid
            } else {
                high = mid
            }
        } else if nums[mid] > nums[low] {
            if target < nums[mid] && target >= nums[low] {
                high = mid
            } else {
                low = mid
            }
        } else {
            if target > nums[mid] && target <= nums[high] {
                low = mid
            } else {
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
    """
    @param A: an integer rotated sorted array
    @param target: an integer to be searched
    @return: an integer
    """
    def search(self, nums, target):
        if not nums or len(nums) < 1:
            return -1
        
        low, high = 0, len(nums) - 1
        while low + 1 < high:
            mid = low + (high - low) // 2
            if nums[mid] == target:
                return mid
            if nums[low] < nums[mid] < nums[high]:
                if nums[mid] > target:
                    high = mid
                else:
                    low = mid
            elif nums[low] < nums[mid]:
                if nums[low] <= target < nums[mid]:
                    high = mid
                else:
                    low = mid
            else:
                if nums[mid] < target <= nums[high]:
                    low = mid
                else:
                    high = mid
        
        if nums[low] == target:
            return low
        if nums[high] == target:
            return high
        return -1

```