# <center>136 - Single Number I (E)</center> 



<br></br>

* Tag: Bit
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: AirBnB
* Link: https://leetcode.com/problems/single-number/

<br></br>



## Description
----
Given a non-empty array of integers, every element appears twice except for one. Find that single one.

<br></br>



## Example
----
Input: [2,2,1]
Output: 1

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param A: An integer array
     * @return: An integer
     */
    public int singleNumber(int[] A) {
        if (A.length == 0)
           return 0;
           
       int result = A[0];
       for (int i = 1; i < A.length; i++)
           result ^= A[i];
       
       return result;
    }
}
```

<br>


### Python
```python
class SingleNum1:
    def solution(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        return 2*(sum(set(nums))) - sum(nums)
```

<br>


### Go
```go
func singleNumber(nums []int) int {
    result := 0
    for _, v := range nums {
        result ^= v
    }
    
    return result
}
```
