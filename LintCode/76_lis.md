# <center>76 - Longest Increasing Subsequence (M)</center> 



<br></br>

* Tag: DP, Binary Search
* Difficulty: Medium
* Company: Microsoft
* Link: https://www.lintcode.com/problem/longest-increasing-subsequence

<br></br>



## Description
----
Given an unsorted array of integers, find the length of longest increasing subsequence.

<br></br>



## Example
----
Input: [10,9,2,5,3,7,101,18]

Output: 4 

The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 

<br></br>



## Solution
----
DP算法：
1. 状态定义：f(n)表示对第n个元素，最长上升序列长度。
2. 状态转移方程：f(i) = 1 + max(f(0), f(1), ..., f(i - 1)) if nums[i] > nums[0, 1, ..., i-1]
3. 初始条件和边界：f(0) = 1，开辟空间长度为len(nums)
4. 计算顺序：从左到右
5. Space: O(n) Time: O(n^2)

二分查找算法：
1. 时间复杂度O(nlogn)
2. 通过归纳发现。
3. 建辅助数组tmp和下标index，index标示tmp中到哪是当前找到的LIS。
    1. 初始化，tmp[0] = nums[0]
    2. 从第二个元素开始遍历nums，对于nums[i]
        1. 如果nums[i] < tmp[0]，说明存在新的可能，把tmp[0] = nums[i]。即使不是，index范围仍旧涵盖最终结果。
        2. 如果nums[i] > tmp[index]，则tmp[++index] = nums[i]，即符合之前找到所有的LIS可能。
	    3. 此时，通过二分查找，找到tmp中刚好比nums[i]大或相同的元素位置，替换为nums[i]。
        4. 最终最长子序列长度为index + 1，但tmp中保存的未必是最长上升序列。
        5. 由于是找最长上升序列，所谓的tmp某个位置值替换不是打乱，而是保留可能。

<br>


### Go
```go
func lengthOfLIS(nums []int) int {
    l := len(nums)
    if l < 1 {
        return 0
    }
    tmp := make([]int, l, l)
    tmp[0] = nums[0]
    index := 0
    for i := 1; i < l; i++ {
        if nums[i] < tmp[0] {
            tmp[0] = nums[i]
        } else if nums[i] > tmp[index] {
            index++
            tmp[index] = nums[i]
        } else {
            tmp[lisBinarySearch(tmp, 0, index, nums[i])] = nums[i]
        }
    }
    
    return index + 1
}

func lisBinarySearch(tmp []int, start, end, target int) int {
    for start + 1 < end {
        mid := start + (end - start) / 2
        if tmp[mid] == target {
            return mid
        }
        if tmp[mid] < target {
            start = mid
        } else {
            end = mid
        }
    }
    
    if tmp[start] >= target {
        return start
    }
    return end
}
```

```go
func lengthOfLIS(nums []int) int {
    l := len(nums)
    if l < 2 {
        return l
    }
    f := make([]int, l, l)
    max := 0
    for i := 0; i < l; i++ {
        f[i] = 1
        for j := 0; j < i; j++ {
            if nums[j] < nums[i] {
                f[i] = maxInt(f[i], f[j] + 1)
            }
        }
        max = maxInt(max, f[i])
    }
    
    return max
}

func maxInt(a, b int) int {
    if a >= b {
        return a
    }
    return b
}
```

<br>
