# <center>18 - Subsets II (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: DFS, Backtracking, Recursion
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/subsets-ii/description

<br></br>



## Description
----
Given a collection of integers that might contain duplicates, nums, return all possible subsets (the power set).

<br></br>



## Example
----
Input: [1,2,2]

Output:

```
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

<br></br>



## Solution
----
算法：
1. 以DFS方式获得所有可能。
2. 递归时引入变量cur，标示当前递归处理第几个num。
3. 当cur == len(num)时，结束递归。
4. 引入path标示每种可能，深拷贝到最终结果。
5. 因为有重复，递归前先排序，然后每次遍历时，如果满足i != cur && nums[i-1] == nums[i]，说明当前这种可能已递归。
6. 也可引入哈希表缓存已获得哪些结果。

<br>


### Go
```go
import (
	"sort"
)

func GetSubsets2(nums []int) [][]int {
	res := make([][]int, 0)
	res = append(res, make([]int, 0)) // Important
	sort.Ints(nums)                   // important
	gs2DFS(nums, make([]int, 0), 0, &res)

	return res
}

func gs2DFS(nums, path []int, cur int, res *[][]int) {
	for i := cur; i < len(nums); i++ {
		if i != cur && nums[i] == nums[i-1] { // 去重
			continue
		}
		path = append(path, nums[i])
		tmp := make([]int, len(path), len(path))
		copy(tmp, path)
		*res = append(*res, tmp)
		gs2DFS(nums, path, i+1, res)
		path = path[:len(path)-1]
	}
}
```

<br>
