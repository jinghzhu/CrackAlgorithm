# <center>100 - Remove Duplicates from Sorted Array (E)</center> 



<br></br>

* Tag: Array, Two Pointer
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Facebook, Bloomberg, Microsoft
* Link: https://www.lintcode.com/problem/remove-duplicates-from-sorted-array/description

<br></br>



## Description
----
Given a sorted array, 'remove' the duplicates in place such that each element appear only once and return the 'new' length.

Do not allocate extra space for another array, you must do this in place with constant memory.
<br></br>



## Example
----
1. Input: [] Output: 0

2. Input: [1,1,2] Output: 2	

<br></br>



## Solution
----
### Java
```java
public class RemoveDuplicatesFromSortedArray {
	public int solution(int[] A) {
        if (A == null) 
            return 0;
        
        int size = 0;
        for (int i = 0; i < A.length; i++)
            if (A[i] != A[size])
                A[++size] = A[i]; // 一个游标指向已去重的部分下一个空位，只要a[i] != a[i-1],将a[i]填入之前空位。
        
        return size + 1;
    }
}
```

<br>


## Go
```go
func RemoveDuplicatesFromSortedArr(nums []int) int {
	if nums == nil {
		return 0 // Important
	}

	i := 0
	for j := 0; j < len(nums); j++ {
		if nums[j] != nums[i] {
			i++
			nums[i] = nums[j] // 一个游标指向已去重的部分下一个空位，只要a[i] != a[i-1],将a[i]填入之前空位。
		}
	}

	return i + 1
}
```

<br>


## Python
```python
class RemoveDuplicatedFromSortedArr:
    """
    @param: nums: An ineger array
    @return: An integer
    """

    def solution(self, nums):
        if len(nums) == 0:
            return 0

        i = 0
        for j in range(len(nums)):
            if nums[i] != nums[j]:
                i += 1  # 一个游标指向已去重的部分下一个空位，只要a[i] != a[i-1],将a[i]填入之前空位。
                nums[i] = nums[j]

        return i + 1
```
