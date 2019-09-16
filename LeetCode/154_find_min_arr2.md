# <center>154 - Find Minimum in Rotated Sorted Array II (H)</center> 



<br></br>

* Tag: Binary Search
* Difficulty: Hard
* Link: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/

<br></br>



## Description
----
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

The array may contain duplicates.

<br></br>



## Example
----
Input: [3,4,5,1,2] 
Output: 1

<br></br>



## Solution
----
伪二分查找，因为可能有重复，可能出现[1,0,1,1]或[1,2,2]这种传统二分解法无法解决的情况。
1. 对于第一种情况，即[1,0,1,1]，移动high指针。
2. 对于第二种情况，检查是否满足nums[low] <= nums[mid] && nums[mid] <= nums[high]。当然，前提是已经过滤了第一种情况。

<br>


### Java
```java
public class GetMinInRotatedSortedArray2 {
	public int solution1(int[] num) {
        int min = num[0];
        for (int i = 1; i < num.length; i++) {
            if (num[i] < min)
                min = num[i];
        }
        return min;
    }
	
	public int soltuyion2(int[] nums) {
        if (nums == null || nums.length == 0)
            return -1;
        
        int start = 0, end = nums.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            // It means it's fine to remove end the smallest element won't be removed.
            if (nums[mid] == nums[end])
                end--;
            else if (nums[mid] < nums[end])
                end = mid;
            else
                start = mid + 1;
        }
        
        if (nums[start] <= nums[end])
            return nums[start];
        return nums[end];
    }
}
```

<br>


### Go
```go
func GetMin2(nums []int) int {
	l := len(nums)
	if l < 1 {
		panic("error")
	}
	low, high := 0, l-1
	for low+1 < high {
		mid := low + (high-low)/2
		if nums[mid] == nums[low] && nums[mid] == nums[high] { // [1,0,1,1]
			high--
		} else if nums[low] <= nums[mid] && nums[mid] <= nums[high] { // [1,3,3]
			return nums[low]
		} else if nums[low] <= nums[mid] {
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
class GetMinRotatedSortedArr2:
    # 伪二分查找，因为可能有重复，可能出现[1, 0, 1, 1]或[1, 2, 2]这种传统二分解法无法解决的情况。
    # 对于第一种情况，即[1, 0, 1, 1]，移动high指针。
    # 对于第二种情况，检查是否满足nums[low] <= nums[mid] & & nums[mid] <= nums[high]。当然，前提是已经过滤了第一种情况。
    def solution(self, nums) -> int:
        l = len(nums)
        if l < 1:
            return None
        low, high = 0, l - 1
        while low + 1 < high:
            mid = low + (high - low) // 2
            if nums[low] == nums[mid] == nums[high]:
                high -= 1
            elif nums[low] <= nums[mid] <= nums[high]:
                return nums[low]
            elif nums[low] <= nums[mid]:
                low = mid
            else:
                high = mid

        if nums[low] <= nums[high]:
            return nums[low]
        return nums[high]
```