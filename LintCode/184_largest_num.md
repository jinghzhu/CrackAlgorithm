# <center>184 - Largest Number (M)</center> 



<br></br>

* Tag: Sort
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/largest-number

<br></br>



## Description
----
Given a list of non negative integers, arrange them such that they form the largest number.

<br></br>



## Example
----
Input:[1, 20, 23, 4, 8]

Output:"8423201"

<br></br>



## Solution
----
### Go
```go
func GetLargestNumber(nums []int) string {
	l := len(nums)
	res := make([]string, 0, l)
	for _, v := range nums {
		res = append(res, fmt.Sprintf("%d", v))
	}
	glnSort(res, 0, l-1)
	s := ""
	for _, v := range res {
		s += v
	}
	i := 0
	for ; i < len(s) && s[i] == '0'; i++ {
	}
	if i == len(s) { // "00" Important
		return "0"
	}

	return s[i:]
}

func glnSort(res []string, start, end int) {
	if start < 0 || end >= len(res) || start >= end {
		return
	}
	p := glnGetPartition(res, start, end)
	glnSort(res, start, p-1)
	glnSort(res, p+1, end)
}

func glnGetPartition(res []string, start, end int) int {
	if start < 0 || end >= len(res) || start >= end {
		return -1
	}
	i, pivot := start-1, res[end]
	for j := start; j < end; j++ {
		if res[j]+pivot > pivot+res[j] {
			i++
			res[i], res[j] = res[j], res[i]
		}
	}
	res[i+1], res[end] = res[end], res[i+1]

	return i + 1
}

```

<br>
