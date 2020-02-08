# <center>347 - Top K Frequent Elements (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Heap, Hash
* Comapny: Yelp, Pocket Gems
* Difficulty: Medium
* Link: https://leetcode.com/problems/top-k-frequent-elements/

<br></br>



## Description
----
Given a non-empty array of integers, return the k most frequent elements.

<br></br>



## Example
----
- Input: nums = [1,1,1,2,2,3], k = 2
- Output: [1,2]

<br></br>



## Solution
----
算法：堆排序

<br>


## Go
```go
func GetTopK(nums []int, k int) []int {
	res := make([]int, 0, k)
	h, m := topkHeapify(nums)
	for i := 0; i < k; i++ {
		res = append(res, h[0])
		l := len(h)
		h[0], h[l-1] = h[l-1], h[0]
		h = h[:l-1]
		topkShift(h, m, 0)
	}

	return res
}

func topkHeapify(nums []int) ([]int, map[int]int) {
	m := make(map[int]int)
	h := make([]int, 0)
	for _, v := range nums {
		count, ok := m[v]
		if !ok {
			h = append(h, v)
			count = 0
		}
		m[v] = count + 1
	}

	l := len(h)
	for i := l / 2; i >= 0; i-- {
		topkShift(h, m, i)
	}

	return h, m
}

func topkShift(h []int, m map[int]int, i int) {
	l := len(h)
	for i < l {
		child := i*2 + 1
		if child+1 < l && m[h[child+1]] > m[h[child]] {
			child++
		}
		if child < l && m[h[child]] > m[h[i]] {
			h[child], h[i] = h[i], h[child]
			i = child
		} else {
			break
		}
	}
}
```
