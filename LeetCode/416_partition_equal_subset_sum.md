# <center>416 - Partition Equal Subset Sum (M)</center> 



<br></br>

* Tag: DP
* Difficulty: Medium
* Company: LinkedIn, Amazon, eBay
* Link: https://leetcode.com/problems/partition-equal-subset-sum/

<br></br>



## Description
----
Given a non-empty array containing only positive integers, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

<br></br>



## Example
----
Input: nums = [1, 5, 11, 5]

Output: true [1, 5, 5], [11]

<br></br>



## Solution
----
算法：
1. 等价与背包问题，能否背到总价值的一半。
2. 状态定义：dp[i]表示是否存在nums中任意一个集合和为i。
3. 状态转换：dp[i] = dp[i] | dp[i - nums[i]]
4. 计算方向：从dp[sum/2]到dp[1]，从nums[0]到nums[l-1]，不可逆向。

举例：nums = {1,5,11,5}

```
		<------------------------------------------计算方向
	|		0	1	2	3	4	5	6	7	8	9	10	11
	|	1	t	t	f	f	f	f	f	f	f	f	f	f
	|	5						t	t	f	f	f	f	f
	|	11											t	t
	v	5						t	t	t	f	f	t	t
```

<br>


### Go
```go
func canPartition(nums []int) bool {
    sum := 0
    for _, v := range nums {
        sum += v
    }
    if sum % 2 != 0 {
        return false
    }
    
    sum /= 2
    dp := make([]bool, sum + 1, sum + 1)
    dp[0] = true
    for i := 0; i < len(nums); i++ {
        for j := sum; j >= nums[i]; j-- {
            dp[j] = dp[j] || dp[j - nums[i]]
        }
    }
    
    return dp[sum]
}
```

<br>
