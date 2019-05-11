# <center>1227 - Repeated Substring Pattern</center> 



<br></br>

* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Google, Amazon
* Link: https://www.lintcode.com/problem/repeated-substring-pattern/description

<br></br>



## Description
----
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

<br></br>



## Example
----
1. Input："abcd" Output：false
2. Input: "abab" Output: true
3. Input: "aa" Output: true

<br></br>



## Solution
----
### Java
```java
public class IsRepeatedSubStr {
	/**
     * @param s: a string
     * @return: return a boolean
     */
    public boolean repeatedSubstringPattern(String s) {
        if (s == null)
            return false;
        
        int l = s.length();
        for (int i = l / 2; i > 0; i--) {
            if (l % i != 0)
                continue;
            if (check(s,i))
                return true;
        }
        
        return false;
    }
    
    private boolean check(String s, int subLen) {
        String unit = s.substring(0, subLen);
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < s.length() / subLen; i++)
            result.append(unit);
        
        return result.toString().equals(s);
    }
}
```

<br>


## Go
```go
func IsRepeatedSubstringPattern(s string) bool {
	l := len(s)
	for i := l / 2; i > 0; i-- {
		if l%i != 0 {
			continue
		}
		if irspHelper(s, i) {
			return true
		}
	}

	return false
}

func irspHelper(s string, subLen int) bool {
	subS := s[:subLen]
	result := ""
	for i := 0; i < len(s)/subLen; i++ {
		result += subS
	}

	return result == s
}
```

<br>


## Python
```python
class IsRepeatedSubstringPattern:
    def repeatedSubstringPattern(self, s: str) -> bool:
        l = len(s)
        for i in range(l//2, 0, -1):
            if l % i != 0:
                continue
            if self.check(s, i):
                return True
        
        return False
    
    def check(self, s: str, sub_len: int) -> bool:
        unit = s[:sub_len]
        result = ""
        for i in range(len(s) // sub_len):
            result += unit
        
        return result == s
```
