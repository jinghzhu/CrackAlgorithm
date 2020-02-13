# <center>836 - Partition to K Equal Sum Subsets (H)</center> 



<br></br>

* Tag: DFS, Backtracking, Recursion
* Difficulty: Hard
* Company: LinkedIn
* Link: https://www.lintcode.com/problem/partition-to-k-equal-sum-subsets/description

<br></br>



## Description
----
Given an array of integers nums and a positive integer k, find whether it's possible to divide this array into k non-empty subsets whose sums are all equal.

<br></br>



## Example
----
Input: nums = [4, 3, 2, 3, 5, 2, 1], k = 4

Output: True

It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

<br></br>



## Solution
----
1. 使用DFS搜索符合要求的组合。
2. 引入哈希visited，标记哪些元素已被使用。
3. 引入变量start标记当前递归从哪个位置开始搜索。
4. 引入变量k，标记当前递归还要找几个子集。
5. 引入变量sum，标记当前递归尝试搜寻的子集和。

<br>


### Go
```go
func IsPartitionKSubsets(nums []int, k int) bool {
	l := len(nums)
	if l < 1 || k < 1 {
		return false
	}
	sum := 0
	for _, v := range nums {
		sum += v
	}
	if sum%k != 0 {
		return false
	}

	return pksDFS(nums, make(map[int]bool), 0, k, 0, sum/k)
}

func pksDFS(nums []int, visited map[int]bool, start, k, sum, target int) bool {
	if k == 1 {
		return true
	}
	if sum == target {
		return pksDFS(nums, visited, 0, k-1, 0, target)
	}
	for i := start; i < len(nums); i++ {
		if visited[i] || sum+nums[i] > target {
			continue
		}
		visited[i] = true
		if pksDFS(nums, visited, i+1, k, sum+nums[i], target) {
			return true
		}
		visited[i] = false
	}

	return false
}
```

<br>
