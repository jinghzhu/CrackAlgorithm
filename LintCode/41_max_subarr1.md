# <center>41 - Maximum Subarray (E)</center> 



<br></br>

* Tag: Greedy, Array, DP
* Company: LinkedIn, Microsoft
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/maximum-subarray/description

<br></br>



## Description
----
Given an array of integers, find a contiguous subarray which has the largest sum.

<br></br>



## Example
----
Input: [−2,2,−3,4,−1,2,1,−5,3]
Output: 6
Explanation: the contiguous subarray [4,−1,2,1] has the largest sum = 6.

<br></br>



## Solution
----
### Java
```java
public class MaxSubArr {
	// State Definition: cache[i] denotes the max sub-array value for the array from 0 to i.
	// State Transformation Equation: cache[i] = max(nums[i], nums[i] + cache[i-1])
	// Time: O(n) Space: O(n)
	public int solution1(int[] nums) {
		if (nums == null)
			return Integer.MIN_VALUE;
		
		int[] cache = new int[nums.length];
		cache[0] = nums[0];
		for (int i = 1; i < nums.length; i++)
			cache[i] = Math.max(nums[i], cache[i - 1] + nums[i]);
		int result = cache[0];
		for (int i = 1; i < cache.length; i++)
			result = Math.max(cache[i], result);
		
		return result;
	}
	
	// Time: O(n) Space: O(1)
	public int solution2(int[] nums) {
		if (nums == null)
			return Integer.MIN_VALUE;
		
		int result = nums[0], temp = nums[0];
		for (int i = 1; i < nums.length; i++) {
			temp = Math.max(nums[i], nums[i] + temp);
			result = Math.max(result, temp);
		}
		
		return result;
	}
}
```

<br>


### Go
```go
func MaxSubArr(nums []int) int {
	l := len(nums)
	if l == 0 {
		return types.IntegerMin
	}
	result, temp := nums[0], nums[0]
	for i := 1; i < l; i++ {
		temp = max(temp, temp+nums[i])
		result = max(result, temp)
	}

	return result
}

func max(a, b int) int {
	if a <= b {
		return b
	}

	return a
}
```

<br>


### Python
```python
class MaxSubarr:
    def solution(self, nums) -> int:
        result, temp = nums[0], nums[0]
        for i in range(1, len(nums)):
            temp = max(nums[i], nums[i] + temp)
            result = max(result, temp)
        return result
```
