# <center>406 - Minimum Size Subarray Sum (M)</center> 



<br></br>

* Tag: Array, Two Pointers
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Facebook
* Link: https://www.lintcode.com/problem/minimum-size-subarray-sum

<br></br>



## Description
----
Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return -1 instead.

<br></br>



## Example
----
Input: [2,3,1,2,4,3], s = 7

Output: 2

Explanation: The subarray [4,3] has the minimal length under the problem constraint.

<br></br>



## Solution
----
### Java
```java
public class MinSizeSubArrSum {
	public int minSubArrayLen(int s, int[] nums) {
        if (nums == null)
            return 0;
        
        int sum = 0, ans = Integer.MAX_VALUE;
        for (int l = 0, r = 0; r < nums.length; r++) {
            sum += nums[r];
            while (l <= r && sum >= s) {
                ans = Math.min(ans, r - l + 1);
                sum -= nums[l++];
            }
        }
        
        return ans == Integer.MAX_VALUE? 0 : ans;
    }
}
```

<br>


### Go
```go
func MinSubArrayLen(s int, nums []int) int {
	min, l, sum := len(nums)+1, 0, 0
	for r := 0; r < len(nums); r++ {
		sum += nums[r]
		for l <= r && sum >= s {
			min = minInt(min, r-l+1)
			sum -= nums[l]
			l++
		}
	}

	if min == len(nums)+1 {
		return -1
	}
	return min
}
```

<br>


### Python
```python
class MinSizeSubArrSum:
    """
    @param nums: an array of integers
    @param s: An integer
    @return: an integer representing the minimum size of subarray
    """
    def soltuion(self, nums, s):
        if not nums:
            return -1

        res, left, l, temp = len(nums) + 1, 0, len(nums), 0
        for right in range(l):
            temp += nums[right]
            while temp >= s:
                res = min(res, right - left + 1)
                temp -= nums[left]
                left += 1

        if res == l + 1:
            return -1
        return res
```