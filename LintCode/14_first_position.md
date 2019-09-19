# <center>14 - First Position of Target (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/first-position-of-target/description

<br></br>



## Description
----
For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n) time complexity.

If the target number does not exist in the array, return -1.

<br></br>



## Example
----
Input:  [1,4,4,5,7,7,8,9,9,10]，1
Output: 0

<br></br>



## Solution
----
### Java
```java
class Solution {
    /**
     * @param nums: The integer array.
     * @param target: Target to find.
     * @return: The first position of target. Position starts from 0.
     */
    public int binarySearch(int[] nums, int target) {
        if (nums == null || nums.length == 0) {
            return -1;
        }
        
        int low = 0, high = nums.length - 1;
        while (low + 1 < high) {
            int mid = low + (high - low) / 2;
            if (nums[mid] == target) {
                high =  mid;
            } else if (nums[mid] < target) {
                low = mid;
            } else {
                high = mid;
            }
        }
        
        if (nums[low] == target) {
            return low;
        } else if (nums[high] == target) {
            return high;
        } else {
            return -1;
        }
    }
}
```