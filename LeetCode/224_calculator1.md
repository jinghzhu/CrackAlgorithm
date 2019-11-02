# <center>224 - Basic Calculator (H)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math, Stack
* Difficulty: Hard
* Link: https://leetcode.com/problems/basic-calculator/

<br></br>



## Description
----
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open '(' and closing parentheses ')', the plus '+' or minus sign '-', non-negative integers and empty spaces ' '.

You may assume that the given expression is always valid.

<br></br>



## Example
----
Input："(1+(4+5+2)-3)+(6+8)" 

Output：23

<br></br>



## Solution
----
算法：
1. 需三个变量和一个栈：tmp表示当前的操作数，sign表示当前的操作数应该被加还是被减，result表示结果。
2. 初始tmp和result = 0，sign = 1。开始遍历字符串。
3. 碰到数字则追加到tmp尾端。
4. 碰到加号说明上一个数字已计算至tmp，把tmp * sign加到result，然后把sign置1 (因为当前碰到了加号)。
5. 碰到减号，同上，不同的在于最后把sign置为-1。
6. 碰到左括号，说明要优先计算右边表达式。需将tmp和sign压栈（注意，此时sign表示的是括号内的表达式应被result加还是减），然后初始化result和sign，准备计算括号内的表达式。
7. 碰到右括号，说明一个括号内表达式计算完。从栈取出括号前的sign和result，与当前result相加运算。注意，是原来result + sign * 当前result。
8. 循环结束后tmp可能还有数字，需加到result。比如"1+2"，2不会在循环内加到结果。

<br>


### Go
```go
func Calculate1(s string) int {
	tmp, result, sign := 0, 0, 1
	st := make([]int, 0)
	for _, v := range s {
		if v >= '0' && v <= '9' {
			tmp = 10*tmp + int(v-'0')
		} else if v == '+' {
			result += sign * tmp
			tmp, sign = 0, 1
		} else if v == '-' {
			result += sign * tmp
			tmp, sign = 0, -1
		} else if v == ' ' {
			continue
		} else if v == '(' {
			st = append(st, result, sign)
			result, sign = 0, 1
		} else {
			result += sign * tmp
			sign = st[len(st)-1]
			st = st[:len(st)-1]
			tmp = st[len(st)-1]
			st = st[:len(st)-1]
			result = tmp + sign*result
			tmp, sign = 0, 1
		}
	}

	return result + sign*tmp // important
}
```

<br>
