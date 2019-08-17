# <center>503 - Next Greater Element II (M)</center> 



<br></br>

* Tag: Array
* Company: Google
* Difficulty: Medium
* Link: https://leetcode.com/problems/next-greater-element-ii/

<br></br>



## Description
----
Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

<br></br>



## Example
----
Input: [1,2,1]
Output: [2,-1,2]

The first 1's next greater number is 2; 
The number 2 can't find next greater number; 
The second 1's next greater number needs to search circularly, which is also 2.

<br></br>



## Solution
----
### Go
```go
func nextGreaterElements(nums []int) []int {
    result := make([]int, 0)
    for k := range nums {
        if k > 0 && nums[k] == nums[k - 1] {
            result = append(result, result[len(result) - 1])
        } else {
            result = append(result, getNext(nums, k))
        }
    }
    
    return result
}

func getNext(nums []int, k int) int {
    for i := k; i < len(nums); i++ {
        if nums[i] > nums[k] {
            return nums[i]
        }
    }
    for i := 0; i < k; i++ {
        if nums[i] > nums[k] {
            return nums[i]
        }
    }
    
    return -1
}
```

<br>


### Python
```python
class Solution:
    """
    @param nums: an array
    @return: the Next Greater Number for every element
    """
    def nextGreaterElements(self, nums):
        result = []
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i - 1]:
                result.append(result[-1])
            else:
                result.append(self.get_next(nums, i))
        return result
        
    def get_next(self, nums, k):
        for i in range(k + 1, len(nums)):
            if nums[i] > nums[k]:
                return nums[i]
        for i in range(k):
            if nums[i] > nums[k]:
                return nums[i]
        return -1

```

<br>
