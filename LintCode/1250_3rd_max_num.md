# <center>1250 - Third Maximum Number (E)</center> 



<br></br>

* Tag: Array
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Facebook, Amazon, Microsoft, Google
* Link: https://www.lintcode.com/problem/third-maximum-number/description

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
算法：
1. 设置三个变量，`max1`，`max2`和`max3`，分别表示第一、第二和第三大的值。初始化为`nums[0]`。
2. 设置两个变量，`visited2`和`visited3`，分别表示`max2`和`max3`是否是真实值。因为，它们初始化为第一个值，可能第一个值就是最大值，或数组不存在第二或第三大的值。
3. 遍历数组：
	1. 如果当前值等于`max1`，`max2`和`max3`中任何一个，继续遍历。
    2. 如果当前值大于`max1`，依次更新`max1`，`max2`和`max3`，并设置`visited2`。
    3. 如果当前值小于`max1`大于`max2`，且`visited2`为真（即`max2`为真实值），依次更新max2`和`max3`，并设置`visited3`。
    4. 按照上一步处理`max3`情况。
4. 因为在遍历时，虽然有设置`visited3`，但这没有覆盖所有情况，只是为了方便比较和更新。所以，这里检查`max3`是否为真实值，依靠判断`max1`，`max2`和`max3`是否不同。

<br>


## Go
```go
func thirdMax(nums []int) int {
    max1, max2, max3 := nums[0], nums[0], nums[0]
    visited2, visited3 := false, false

	for _, v := range nums {
		if max1 == v || max2 == v || max3 == v {
			continue
		}
		if v > max1 {
			max3, max2, max1 = max2, max1, v
			visited2 = true
		} else if !visited2 || v > max2 {
			max3, max2 = max2, v
			visited2 = true
		} else if !visited3 || v > max3 {
		    max3 = v
		    visited3 = true
		}
	}

	if max1 == max2 || max2 == max3 {
	    return max1
	}

	return max3
}
```

<br>
