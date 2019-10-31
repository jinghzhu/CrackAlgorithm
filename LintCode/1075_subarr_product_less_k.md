# <center>1075 - Subarray Product Less Than K (M)</center> 



<br></br>

* Tag: Array, Slip Window
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/subarray-product-less-than-k

<br></br>



## Description
----
Your are given an array of positive integers nums.

Count and print the number of (contiguous) subarrays where the product of all the elements in the subarray is less than k.

<br></br>



## Example
----
Input: nums = [10, 5, 2, 6], k = 100

Output: 8

Explanation: The 8 subarrays that have product less than 100 are: [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6].
Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.

<br></br>



## Solution
----
滑动窗口，只是在左边滑而非右边。

<br>


### Go
```go
func CountSubArrProductLessThanK(nums []int, k int) int {
	if k <= 1 {
		return 0 // important
	}
	count, product, l, left := 0, 1, len(nums), 0
	for i := 0; i < l; i++ {
		product *= nums[i]
		for product >= k {
			product /= nums[left]
			left++
		}
		count += i - left + 1
	}

	return count
}
```

<br>
