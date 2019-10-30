# <center>604 - Window Sum (E)</center> 



<br></br>

* Tag: Array, Slip Windown
* Company: Amazon
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/window-sum

<br></br>



## Description
----
Given an array of n integers, and a moving window(size k), move the window at each iteration from the start of the array, find the sum of the element inside the window at each moving.

<br></br>



## Example
----
Input：array = [1,2,7,8,5], k = 3

Output：[10,17,20]

Explanation：
1 + 2 + 7 = 10
2 + 7 + 8 = 17
7 + 8 + 5 = 20

<br></br>



## Solution
----
### Go
```go
func SumWin(nums []int, k int) []int {
	result := make([]int, 0)
	l := len(nums)
	if k < 1 || k > l {
		return result
	}
	i, sum := 0, 0
	for ; i < k; i++ {
		sum += nums[i]
	}
	result = append(result, sum)
	for ; i < l; i++ {
		sum = sum - nums[i-k] + nums[i]
		result = append(result, sum)
	}

	return result
}
```

<br>
