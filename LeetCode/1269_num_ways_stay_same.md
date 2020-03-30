# <center>1269 - Number of Ways to Stay in the Same Place After Some Steps (E)</center> 



<br></br>

* Difficulty: Hard
* Tag: DP, DFS, Recursion
* Link: https://leetcode.com/problems/number-of-ways-to-stay-in-the-same-place-after-some-steps/

<br></br>



## Description
----
You have a pointer at index 00 in an array of size arrLenarrLen. At each step, you can move 11 position to the left, 11 position to the right in the array or stay in the same place (The pointer should not be placed outside the array at any time).

Given two integers stepssteps and arrLenarrLen, return the number of ways such that your pointer still at index 00 after exactly stepssteps steps.

Since the answer may be too large, return it modulo 10^9 + 7.

<br></br>



## Example
----
Input: 3 2

Output: 4

There are 4 differents ways to stay at index 0 after 3 steps.
1. Right, Left, Stay
2. Stay, Right, Left
3. Right, Stay, Left
4. Stay, Stay, Stay

<br></br>



## Solution
----
算法：
1. 每次递归回溯时，当前位置有三种选择：原地不动；前进一步；后退一步。
2. 当走到最后一步时，检查是否仍在原地。
3. 因为是int类型，所以可以自动回溯。
4. 记忆化搜索：通过一个哈希表，记录已经探索过的分支。
5. 剪枝：如果当前走过头，或者剩下的步数不足以返回起点，返回。
6. 时间复杂度：O(N^2)，N为steps，因为每一递归层次上能走到的最大距离为初始steps，所以总时间复杂度为其平方。空间复杂度：O(N^2)

<br>


### Go
```go
const (
    mod int = 1000000007
)

func numWays(steps, arrLen int) int {
    res := make([]int, 1, 1)
    visited := make(map[string]int)
    dfs(steps, 0, arrLen, res, visited)
    
    return res[0] % mod
}

func dfs(leftSteps, pos, arrLen int, res []int, visited map[string]int) {
    if pos < 0 || pos == arrLen || pos > leftSteps {
        return
    }
    k := fmt.Sprintf("%d+%d", leftSteps, pos)
    if v, ok := visited[k]; ok {
        res[0] += v
        return
    }
    
    if leftSteps == 0 {
        if pos == 0 {
            res[0]++
        }
        return
    }
    
    ops := []int{-1, 0, 1}
    tmp := res[0]
    for _, p := range ops {
        dfs(leftSteps - 1, pos + p, arrLen, res, visited)
    }
    visited[k] = (res[0] - tmp) % mod
}
```

<br>
