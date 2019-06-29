# <center>414 - Third Maximum Number (M)</center> 



<br></br>

* Tag: Array
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/third-maximum-number/

<br></br>



## Description
----
Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in `O(n)`.

<br></br>



## Example
----
1. Input: [3, 2, 1] Output: 1
2. Input: [1, 2] Output: 2
3. Input: [2, 2, 3, 1] Output: 1

<br></br>



## Solution
----
### Java
```java
public class ThirdMaxNumber {
    public int solution1(int[] nums) {
    	Integer max_one = null;
        Integer max_two = null;
        Integer max_three = null;
        
        for (Integer n : nums) {
            if (n.equals(max_one) || n.equals(max_two) || n.equals(max_three))
            	continue;
            if (max_one == null || n > max_one) {
                max_three = max_two;
                max_two = max_one;
                max_one = n;
            } else if(max_two == null || n > max_two) {
                max_three = max_two;
                max_two = n;
            } else if (max_three == null || n > max_three)
                max_three = n;
        }
        
        return max_three == null ? max_one : max_three;
    }
}
```

<br>


## Go
```go
func ThirdMax(nums []int) int {
	max1, max2, max3 := nums[0], nums[0], nums[0]
	visited2, visited3 := false, false

	for i := 1; i < len(nums); i++ {
		if max1 == nums[i] {
			continue
		}
		if nums[i] > max1 {
			if visited2 {
				max3 = max2
				visited3 = true
			} else {
				visited2 = true
			}
			max2 = max1
			max1 = nums[i]
		} else if !visited2 {
			max2 = nums[i]
			visited2 = true
		} else if visited2 && max2 == nums[i] {
			continue
		} else if nums[i] > max2 {
			visited3 = true
			max3 = max2
			max2 = nums[i]
		} else if !visited3 {
			visited3 = true
			max3 = nums[i]
		} else if visited3 && nums[i] == max3 {
			continue
		} else if nums[i] > max3 {
			max3 = nums[i]
		}
	}

	if visited3 {
		return max3
	}

	return max1
}
```

<br>


## Python
```python
import sys

class ThirdMax:
    def solution(self, nums) -> int:
        max1, max2, max3 = -sys.maxsize - 1, -sys.maxsize - 1, -sys.maxsize - 1
        for i in range(len(nums)):
            if nums[i] == max1 or nums[i] == max2 or nums[i] == max3:
                continue
            if nums[i] > max1:
                max3 = max2
                max2 = max1
                max1 = nums[i]
            elif nums[i] > max2:
                max3 = max2
                max2 = nums[i]
            elif nums[i] > max3:
                max3 = nums[i]

        if max3 > -sys.maxsize - 1:
            return max3

        return max1
```
