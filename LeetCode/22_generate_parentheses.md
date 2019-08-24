# <center>22 - Generate Parentheses (M)</center> 



<br></br>

* Tag: String, Recursion, Backtracking
* Difficulty: Medium
* Company: Google, Uber
* Link: https://leetcode.com/problems/generate-parentheses/

<br></br>



## Description
----
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

<br></br>



## Example
----
Input: 3
Output: ["((()))", "(()())", "(())()", "()(())", "()()()"]

<br></br>



## Solution
----
https://www.jiuzhang.com/solution/generate-parentheses/#tag-highlight-lang-python

回溯，逐个字符添加，生成每一种组合.
需记录的状态：当前字符串本身，剩余左右括号数量。
递归过程中解决：
1. 如果当前右括号数量等于n，当前字符串是一种组合。
2. 如果当前左括号数量等于n，当前字符串后续填充满右括号，是一种组合。
3. 如果当前左括号数量未超过n，那么：
   1. 如果左括号多于右括号，可添加一个左括号或右括号，递归进入下一层；
   2. 如果左括号不多于右括号，此时只能添加一个左括号，递归进入下一层。

<br>


### Go
```go
func GenerateParenthesis(n int) []string {
    result := make([]string, 0)
    if n < 1 {
        return result
    }
    
    gpHelper(&result, "", n, n)
    
    return result
}

func gpHelper(
	result *[]string,
	paren string, // current sub-parentheses pattern
	left, // how many left paren to add
	right int) { // how many right paren to add
    if left == 0 && right == 0 {
        *result = append(*result, paren)
        return
    }
    if left > 0 {
        gpHelper(result, paren + "(", left - 1, right)
    }
    if right > 0 && right > left {
        gpHelper(result, paren + ")", left, right - 1)
    }
}
```

<br>


### Java
```java
public class GenerateParentheses {
	/**
     * @param n: n pairs
     * @return: All combinations of well-formed parentheses
     */
    public List<String> generateParenthesis(int n) {
        List<String> result = new ArrayList<String>();
        if (n < 1)
            return result;
        
        process(result, "", n, n);
        
        return result;
    }
    
    /**
     * @param result: n pairs
     * @param cur: current sub-parentheses pattern
     * @param left: how many left paren to add
     * @param right: how many right paren to add
     * @return: nothing
     */
    private void process(List<String> result, String cur, int left, int right) {
        if (left == right && left == 0) {
            result.add(cur);
            return;
        }
        if (left > 0)
            process(result, cur + "(", left - 1, right);
        
        if (right > 0 && left < right)
            process(result, cur + ")", left, right - 1);
    }
}
```

<br>


### Python
```python
class GenerateParentheses:
    def solution(self, n):
        if n == 0:
            return []
        res = []
        self.helpler(n, n, '', res)
        return res

    def helpler(self, l, r, item, res):
        if r < l:
            return
        if l == 0 and r == 0:
            res.append(item)
        if l > 0:
            self.helpler(l - 1, r, item + '(', res)
        if r > 0:
            self.helpler(l, r - 1, item + ')', res)
```
