# <center>282 - Expression Add Operators (H)</center> 



<br></br>

* Tag: DFS, Backtracking, Recursion, Math
* Comapny: Google, Facebook
* Difficulty: Hard
* Link: https://leetcode.com/problems/expression-add-operators/

<br></br>



## Description
----
Given a string that contains only digits 0-9 and a target value, return all possibilities to add binary operators (not unary) +, -, or * between the digits so they evaluate to the target value.

<br></br>



## Example
----
- Input: num = "123", target = 6
- Output: ["1+2+3", "1*2*3"] 

<br></br>



## Solution
----
算法：
1. DFS成功条件为，所有数字都用上，且当前值符合要求。
2. 利用string按值传递特性和拼接做回溯。
3. 引入变量lastFactor，记录最后操作的数和符号。
4. DFS扩展情况：
    - 乘号：
        1. sum = sum - lastF + lastF * 当前枚举数
		2. lastF = lastF * 当前枚举数
    - 加号和减号：
		1. sum = sum +/- 当前枚举数
		2. lastF = 当前枚举数 * 符号

<br>


### Go
```go
import "strconv"

func addOperators(num string, target int) []string {
    res := make([]string, 0)
    if len(num) < 2 {
        return res
    }
    dfs(num, "", target, 0, 0, 0, &res)
    
    return res
}

func dfs(num, path string, target, lastF, start, sum int, res *[]string) {
    l := len(num)
    if start == l {
        if sum == target {
            *res = append(*res, path)
        }
        return
    }
    
    for i := start; i < l; i++ {
        s := string(num[start : i + 1])
        v, _ := strconv.Atoi(s)
        if start == 0 {
            dfs(num, s, target, v, i + 1, v, res)
        } else {
            dfs(num, path + "+" + s, target, v, i + 1, sum + v, res)
            dfs(num, path + "*" + s, target, lastF * v, i + 1, sum - lastF + lastF * v, res)
            dfs(num, path + "-" + s, target, -v, i + 1, sum - v, res)
        }
        if v == 0 {
            break
        }
    }
}
```

<br>
