# <center>980 - Basic Calculator II (M)</center> 



<br></br>

* Tag: Math, Stack, String
* Difficulty: Medium
* Company: Airbnb
* Link: https://www.lintcode.com/problem/basic-calculator-ii/description

<br></br>



## Description
----
Implement a basic calculator to evaluate a simple expression string.

The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.

You may assume that the given expression is always valid.

<br></br>



## Example
----
Input: " 3/2 "

Output: 1

<br></br>



## Solution
----
算法：
1. 通过栈来实现运算，按顺序读取字符串，将第一个数入栈。
2. 之后遇到+，将下一个数num入栈。
3. 遇到-，将-num入栈。
4. 遇到乘或除，先将上一个数出栈，与当前数运算后，再将结果入栈。
5. 遍历完后，将栈中所有数相加即结果。

<br>


### Go
```go
func Calculate2(s string) int {
	s = strings.TrimSpace(s) // Important. Otherwise k == len(s) - 1 can't work.
	st := make([]int, 0)
	tmp := 0
	lastOp := '+'
	for k, v := range s {
		if v == ' ' {
			continue
		}
		if v >= '0' && v <= '9' {
			tmp = 10*tmp + int(v-'0')
		}
		if (v > '9' || v < '0') || k == len(s)-1 {
			if lastOp == '+' {
				st = append(st, tmp)
			} else if lastOp == '-' {
				st = append(st, -1*tmp)
			} else if lastOp == '*' {
				num := st[len(st)-1]
				st[len(st)-1] = num * tmp
			} else {
				num := st[len(st)-1]
				st[len(st)-1] = num / tmp
			}
			lastOp = v
			tmp = 0
		}
	}
	for len(st) > 0 {
		tmp += st[len(st)-1]
		st = st[:len(st)-1]
	}

	return tmp
}
```

<br>
