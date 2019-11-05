# <center>370 - Convert Expression to Reverse Polish Notation (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Stack
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/convert-expression-to-reverse-polish-notation/description

<br></br>



## Description
----
Given a string array representing an expression, and return the Reverse Polish notation of this expression. (remove the parentheses)

<br></br>



## Example
----
Input: ["3", "-", "4", "+", "5"]

Output: ["3", "4", "-", "5", "+"]

<br></br>



## Solution
----
算法：
1. 遍历中缀表达式。
2. 如果是数字，直接写入后缀表达式。
3. 如果是左括号，直接压栈，等待以它为首的表达式转换完毕再出栈。
4. 如果是右括号，说明括号内的中缀表达式已扫描完毕。在栈中保留的到左括号为止的运算符已逆序，只需依次出栈放入后缀表达式即可。
5. 如果是运算符：
    1. 当该运算符优先级大于栈顶运算符优先级时，说明该运算符后一个运算对象还未被扫描放入后缀表达式。所以，把当前运算符暂存于栈中。
    2. 当该运算符优先级小于等于栈顶运算符优先级，说明栈顶运算符左右两边运算对象已放入后缀表达式。所以，把栈顶元素依次出栈，直到栈顶运算符优先级小于当前运算符。
    3. 优先级*/最高，+-次之，然后是数字，最后是括号。括号最低因为碰到运算符弹栈时，遇到括号要停止。
6. 中缀表达式遍历完毕后，把栈中元素依次出栈，放入后缀表达式。

<br>


### Go
```go
import (
    "strings"
)

func convertToRPN (expression []string) []string {
   st, res := make([]string, 0), make([]string, 0)
   st = append(st, "@")
   for _, exp := range expression {
       if strings.Index("+-*/()", exp) == -1 {
           res = append(res, exp)
           continue
       }
       if exp == "(" {
           st = append(st, exp)
       } else if exp == ")" {
           for st[len(st) - 1] != "(" {
               tmp := st[len(st) - 1]
               st = st[:len(st) - 1]
               res = append(res, tmp)
           }
           st = st[:len(st) - 1]
       } else {
           for getPriority(st[len(st) - 1]) >= getPriority(exp) {
               tmp := st[len(st) - 1]
               st = st[:len(st) - 1]
               res = append(res, tmp)
           }
           st = append(st, exp)
       }
   }
   for len(st) > 1 {
       tmp := st[len(st) - 1]
       st = st[:len(st) - 1]
       res = append(res, tmp)
   }
   
   return res
}

func getPriority(exp string) int {
    if exp == "*" || exp =="/" {
        return 2
    }
    if exp == "+" || exp == "-" {
        return 1
    }
    return 0
}
```

<br>
