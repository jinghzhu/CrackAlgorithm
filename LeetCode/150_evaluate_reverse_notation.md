# <center>150 - Evaluate Reverse Polish Notation (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Stack
* Difficulty: Medium
* Company: LinkedIn
* Link: https://leetcode.com/problems/evaluate-reverse-polish-notation/

<br></br>



## Description
----
Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are +, -, *, /. Each operand may be an integer or another expression.

<br></br>



## Example
----
Input: ["2", "1", "+", "3", "*"] 

Output: 9

Explanation: ["2", "1", "+", "3", "*"] -> (2 + 1) * 3 -> 9

<br></br>



## Solution
----
遍历表达式:
- 碰到数字则入栈
- 碰到运算符则连续从栈中取出2个元素, 使用该运算符运算然后将结果入栈

最后栈中剩余一个数字, 就是结果.

<br>


### Go
```go
func evalRPN(tokens []string) int {
    st := make([]int, 0)
    for _, v := range tokens {
        if v == "+" {
            l := len(st)
            v1, v2 := st[l - 1], st[l - 2]
            st = st[:l - 1]
            st[l - 2] = v1 + v2
        } else if v == "-" {
            l := len(st)
            v1, v2 := st[l - 1], st[l - 2]
            st = st[:l - 1]
            st[l - 2] = v2 - v1
        } else if v == "*" {
            l := len(st)
            v1, v2 := st[l - 1], st[l - 2]
            st = st[:l - 1]
            st[l - 2] = v1 * v2
        } else if v == "/" {
            l := len(st)
            v1, v2 := st[l - 1], st[l - 2]
            st = st[:l - 1]
            st[l - 2] = v2 / v1
        } else {
            v1, _ := strconv.Atoi(v)
            st = append(st, v1)
        }
    }
    
    return st[0]
}
```

<br>
