# <center>1536 - Find First and Last Position of Element in Sorted Array (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/find-first-and-last-position-of-element-in-sorted-array/description

<br></br>



## Description
----
Given an array of integers nums sorted in ascending order, find the starting and ending position of a given target value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return [-1, -1].

<br></br>



## Example
----
1. Input: nums = [5,7,7,8,8,10], target = 8 Output: [3,4]
2. Input: nums = [5,7,7,8,8,10], target = 6 Output: [-1,-1]

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param nums: the array of integers
     * @param target: 
     * @return: the starting and ending position
     */
    public List<Integer> searchRange(List<Integer> nums, int target) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        result.add(-1); result.add(-1);
        if (nums == null || nums.size() < 1)
            return result;
        
        int low = 0, high = nums.size() - 1;
        while (low + 1 < high) {
            int mid = low + (high - low) / 2;
            if (nums.get(mid) < target)
                low = mid;
            else
                high = mid;
        }
        
        if (nums.get(low) == target)
            result.set(0, low);
        else if (nums.get(high) == target)
            result.set(0, high);
            
        low = 0;
        high = nums.size() - 1;
        while (low + 1 < high) {
            int mid = low + (high - low) / 2;
            if (nums.get(mid) <= target)
                low = mid;
            else
                high = mid;
        }
        
        if (nums.get(high) == target)
            result.set(1, high);
        else if (nums.get(low) == target)
            result.set(1, low);
        
        return result;
    }
}
```

<br>
