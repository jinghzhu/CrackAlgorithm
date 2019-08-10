# <center>423 - Valid Parentheses (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Stack
* Difficulty: Easy
* Company: Uber, Google, Facebook, Microsoft, Amazon, Airbnb, Twitter
* Link: https://www.lintcode.com/problem/valid-parentheses/description

<br></br>



## Description
----
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

<br></br>



## Example
----
Input: "()[]{}"
Output: true

Input: "(]"
Output: false


<br></br>



## Solution
----
### Java
```java
public class IsParentheses {
	/**
     * @param s: A string
     * @return: whether the string is a valid parentheses
     */
	public boolean solution(String s) {
		if (s == null || s.length() % 2 != 0)
			return false;
		
		Stack<Character> st = new Stack<Character>();
		for (int i = 0; i < s.length(); i++) {
			char ch = s.charAt(i);
			if (ch == '(' || ch == '{' || ch == '[') {
				st.push(ch);
				continue;
			}
			if (st.isEmpty()) // important
				return false;
			if (ch == ')') {
				char ch1 = st.pop();
				if (ch1 != '(')
					return false;
			} else if (ch == '}') {
				char ch1 = st.pop();
				if (ch1 != '{')
					return false;
			} else {
				char ch1 = st.pop();
				if (ch1 != '[')
					return false;
			}
		}
		if (!st.isEmpty()) // important
			return false;
		
		return true;
	}
}
```

<br>


### Go
```go
func IsParentheses(s string) bool {
	if s == "" || len(s)%2 != 0 {
		return false
	}

	st := make([]string, len(s)/2, len(s)/2)
	pos := -1
	for i := 0; i < len(s); i++ {
		curStr := string(s[i])
		if curStr == "(" || curStr == "[" || curStr == "{" {
			pos++
			if pos == len(st) { // important
				return false
			}
			st[pos] = curStr
			continue
		}
		if pos == -1 { // important
			return false
		}
		if curStr == ")" {
			if st[pos] != "(" {
				return false
			}
		} else if curStr == "]" {
			if st[pos] != "[" {
				return false
			}
		} else if st[pos] != "{" {
			return false
		}
		pos--
	}

	if pos == -1 { // important
		return true
	}

	return false
}
```

<br>


### Python
----
```python
class Solution:
    def isValid(self, s: str) -> bool:
        if len(s) % 2 != 0:
            return False
        st = []
        for i in range(len(s)):
            if len(st) == 0 or s[i] == '{' or s[i] == '[' or s[i] =='(':
                st.append(s[i])
            elif self.check(st[-1], s[i]):
                st.pop(-1)
            else:
                return False
        
        if len(st) > 0:
            return False
        
        return True
    
    def check(self, i, j):
        if i == '(':
            return j == ')'
        if i == '{':
            return j == '}'
        return j == ']'
```