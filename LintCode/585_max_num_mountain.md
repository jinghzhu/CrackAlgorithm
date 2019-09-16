# <center>585 - Maximum Number in Mountain Sequence (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/maximum-number-in-mountain-sequence/description

<br></br>



## Description
----
Given a mountain sequence of n integers which increase firstly and then decrease, find the mountain top.

<br></br>



## Example
----
Input: nums = [1, 2, 4, 8, 6, 3] 
Output: 8

<br></br>



## Solution
----
### Java
```java
public class GetPeakInMountainSeq {
	/**
     * @param nums a mountain sequence which increase firstly and then decrease
     * @return then mountain top
     */
    public int mountainSequence(int[] nums) {
        if (nums == null || nums.length == 0)
            return -1;
        
        int low = 0, high = nums.length - 1, mid = 0;
        while (low + 1 < high) {
            mid = (high - low) / 2 + low;
            int prev = mid - 1, next = mid + 1;
            if (nums[prev] < nums[mid] && nums[mid] > nums[next])
            	return nums[mid];
            else if (nums[prev] < nums[mid] && nums[mid] < nums[next]) 
            	low = mid;
            else 
            	high = mid;
        }
        
        return nums[low] > nums[high] ? nums[low]: nums[high];
    }
}
```
