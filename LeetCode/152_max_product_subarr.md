# <center>152 - Maximum Product Subarray (M)</center> 



<br></br>

* Tag: DP
* Difficulty: Medium
* Company: LinkedIn
* Link: https://leetcode.com/problems/maximum-product-subarray/

<br></br>



## Description
----
Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

<br></br>



## Example
----
1. Input: [2,3,-2,4] Output: 6 Explanation: [2,3] has the largest product 6.
2. Input: [-2,0,-1] Output: 0 Explanation: The result cannot be 2, because [-2,-1] is not a subarray.

<br></br>



## Solution
----
### Java
```java
class Solution {
    public int maxProduct(int[] nums) {
        if (nums == null || nums.length < 1)
            return 0;
        
        int[] max = new int[nums.length];
        int[] min = new int[nums.length];
        min[0] = max[0] = nums[0];
        int result = nums[0];
        
        for (int i = 1; i < nums.length; i++) {
            min[i] = max[i] = nums[i];
            if (nums[i] > 0) {
                max[i] = Math.max(max[i], max[i - 1] * nums[i]);
                min[i] = Math.min(min[i], min[i - 1] * nums[i]);
            } else if (nums[i] < 0) {
                max[i] = Math.max(max[i], min[i - 1] * nums[i]);
                min[i] = Math.min(min[i], max[i - 1] * nums[i]);
            }
            result = Math.max(result, max[i]);
        }
        
        return result;
    }
}
```

<br>


### Python
```python
class Solution:
    """
    @param nums: An array of integers
    @return: An integer
    """
    def maxProduct(self, nums):
        if not nums:
            return 0
            
        result, l = nums[0], len(nums)
        max_val, min_val = [0] * l, [0] * l
        max_val[0], min_val[0] = nums[0], nums[0]
        
        for i in range(1, l):
            max_val[i], min_val[i] = nums[i], nums[i]
            if nums[i] > 0:
                max_val[i] = max(max_val[i], max_val[i - 1] * nums[i])
                min_val[i] = min(min_val[i], min_val[i - 1] * nums[i])
            elif nums[i] < 0:
                max_val[i] = max(max_val[i], min_val[i - 1] * nums[i])
                min_val[i] = min(min_val[i], max_val[i - 1] * nums[i])
            result = max(result, max_val[i])
        
        return result
```

<br>


### Go
```go
func maxProduct(nums []int) int {
    l := len(nums)
    if l < 1 {
        return 0
    }
    
    min, max := make([]int, l, l), make([]int, l, l)
    min[0], max[0] = nums[0], nums[0]
    result := nums[0]
    for i := 1; i < l; i++ {
        min[i], max[i] = nums[i], nums[i]
        if nums[i] > 0 {
            max[i] = getMaxInt(nums[i], nums[i] * max[i - 1])
            min[i] = getMinInt(nums[i], nums[i] * min[i - 1])
        } else if nums[i] < 0 {
            max[i] = getMaxInt(nums[i], nums[i] * min[i - 1])
            min[i] = getMinInt(nums[i], nums[i] * max[i - 1])
        }
        result = getMaxInt(result, max[i])
    }
    
    return result
}

func getMaxInt(a, b int) int {
    if a >= b {
        return a
    }
    return b
}

func getMinInt(a, b int) int {
    if a <= b {
        return a
    }
    return b
}
```
