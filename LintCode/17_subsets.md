# <center>17 - Subsets (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: DFS, Backtracking, Recursion
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/subsets/description

<br></br>



## Description
----
Given a set of distinct integers, nums, return all possible subsets (the power set).

<br></br>



## Example
----
Input: [1,2,3]

Output:

```
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

<br></br>



## Solution
----
算法：
1. DFS递归找所有可能。
2. 引入变量cur标示每层递归从第几个num开始处理。
3. 当cur == len(num)时，标示已经递归到底，开始回退。
4. 每层结果都应该深拷贝。

<br>


### Java
```java
public class GetSubsets1 {
	public List<List<Integer>> solution(int[] nums) {
        List<List<Integer>> results = new ArrayList<>();
        
        if (nums == null)
            return results;
        
        if (nums.length == 0) {
            results.add(new ArrayList<Integer>());
            return results;
        }
        
        helper(new ArrayList<Integer>(), nums, 0, results);
        
        return results;
    }
    
    
    // 递归三要素
    // 1. 递归的定义：在 Nums 中找到所有以 subset 开头的的集合，并放到 results
    private void helper(ArrayList<Integer> subset,
                        int[] nums,
                        int startIndex,
                        List<List<Integer>> results) {
        // 2. 递归的拆解
        results.add(new ArrayList<Integer>(subset));
        
        for (int i = startIndex; i < nums.length; i++) {
            // [1] -> [1,2]
            subset.add(nums[i]);
            // 寻找所有以 [1,2] 开头的集合，并扔到 results
            helper(subset, nums, i + 1, results);
            // [1,2] -> [1]  回溯
            subset.remove(subset.size() - 1);
        }
        
        // 3. 递归的出口
    }
}
```

<br>


### Go
```go
func GetSubsets1(nums []int) [][]int {
	res := make([][]int, 0)
	res = append(res, make([]int, 0)) // Important
	gs1DFS(nums, make([]int, 0), 0, &res)

	return res
}

func gs1DFS(nums, path []int, cur int, res *[][]int) {
	for i := cur; i < len(nums); i++ {
		path = append(path, nums[i])
		tmp := make([]int, len(path), len(path))
		copy(tmp, path)
		*res = append(*res, tmp)
		gs1DFS(nums, path, i+1, res)
		path = path[:len(path)-1]
	}
}
```

<br>
