# <center>65 - Valid Number (H)</center> 



<br></br>

* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Hard
* Company: LinkedIn
* Link: https://leetcode.com/problems/valid-number/

<br></br>



## Description
----
Validate if a given string is numeric.

<br></br>



## Example
----
* "2e10" => true
* " -90e3   " => true
* ".1" => true
* "1." => true
* " 1e" => false
* "e3" => false
* " 6e-1" => true
* " 99e2.5 " => false
* "53.5e93" => true
* " --6 " => false
* "95a54e53" => false
* "4." => true
* ".4" => true
* "+.8" => true

<br></br>



## Solution
----
Steps:
1. Check if it contains `e` or `.`. If so, go to dedicated case to verity it.
2. Verify it as normal number.

<b>



### Go
```go
import "strings"

func IsNum(s string) bool {
	s = strings.TrimSpace(s)
	if strings.Index(s, "e") != -1 {
		return validCaseE(s)
	}
	if strings.Index(s, ".") != -1 {
		return validCaseDot(s)
	}

	return valid(s)
}

func validCaseE(s string) bool {
	strs := strings.Split(s, "e")
	if len(strs) != 2 {
		return false
	}
	if strings.Index(strs[0], ".") != -1 {
		return validCaseDot(strs[0]) && valid(strs[1])
	}

	return valid(strs[0]) && valid(strs[1])
}

func valid(s string) bool {
	if len(s) < 1 { // Important, image ""
		return false
	}

	for k, v := range s {
		// len(s) != 1 is important for only +
		if k == 0 && len(s) != 1 && (v == '+' || v == '-') {
			continue
		}
		if v < '0' || v > '9' {
			return false
		}
	}

	return true
}

func validCaseDot(s string) bool {
	strs := strings.Split(s, ".")
	if len(strs) != 2 {
		return false
	}

	hasVal1, hasVal2 := false, false // Because ".1" and "1." is valid.
	for k, v := range strs[0] {
		if k == 0 && (v == '+' || v == '-') {
			continue
		}
		if v < '0' || v > '9' {
			return false
		}
		hasVal1 = true
	}
	for _, v := range strs[1] {
		if v < '0' || v > '9' {
			return false
		}
		hasVal2 = true
	}

	return hasVal1 || hasVal2
}
```

<br>


### Java
```java
public class IsNum {
	/**
     * @param s: the string that represents a number
     * @return: whether the string is a valid number
     */
    public boolean isNumber(String s) {
        if (s == null)
            return false;
        
        s = s.trim();
        if (s.contains("e"))
            return validCaseE(s);
        
        if (s.contains("."))
            return validCaseDot(s);
        
        return valid(s);
    }
    
    private boolean validCaseE(String s) {
        String[] strs = s.split("e");
        if (strs.length != 2)
            return false;
        
        if (strs[0].contains("."))
            return validCaseDot(strs[0]) && valid(strs[1]);
        
        return valid(strs[0]) && valid(strs[1]);
    }
    
    private boolean validCaseDot(String s) {
        String[] strs = s.split("\\.");
        if (strs.length > 2 || strs.length == 0) // . -> [], 3.4 -> [3, 4], 3. -> [3], .3 -> [, 3]
            return false;

        boolean hasVal1 = false, hasVal2 = false; // Because ".1" and "1." is valid.
        for (int i = 0; i < strs[0].length(); i++) {
            char ch = strs[0].charAt(i);
            if (i == 0 && (ch == '+' || ch == '-'))
                continue;
            if (ch < '0' || ch > '9')
                return false;
            hasVal1 = true;
        }
        if (strs.length == 2)
        	for (int i = 0; i < strs[1].length(); i++) {
                char ch = strs[1].charAt(i);
                if (ch < '0' || ch > '9')
                    return false;
                hasVal2 = true;
            }
        
        
        return hasVal1 || hasVal2;
    }
    
    private boolean valid(String s) {
        if (s.length() < 1) // Important, image ""
            return false;
        
        for (int i = 0; i < s.length(); i++) {
        	char ch = s.charAt(i);
            if (i == 0 && s.length() != 1 && (ch == '+' || ch == '-'))
                continue;
            if (ch < '0' || ch > '9')
                return false;
        }  
        
        return true;
    }
}
```

<br>


## Python
```python
class IsNum:
    def isNumber(self, s: str) -> bool:
        s = s.strip()
        if (s.find('e') != -1):
            return self.validCaseE(s)
        if (s.find('.') != -1):
            return self.validCaseDot(s)
        
        return self.valid(s)
    
    def validCaseE(self, s: str) -> bool:
        strs = s.split('e')
        if len(strs) != 2:
            return False
        
        if strs[0].find('.') != -1:
            return self.validCaseDot(strs[0]) and self.valid(strs[1])
        
        return self.valid(strs[0]) and self.valid(strs[1])
    
    def valid(self, s: str) -> bool:
        if len(s) < 1: # Important, image ""
            return False
        
        for i in range(len(s)):
            if i == 0 and len(s) != 1 and (s[i] == '-' or s[i] == '+'):
                continue
            if s[i] < '0' or s[i] > '9': # Because ".1" and "1." is valid.
                return False
        
        return True
    
    def validCaseDot(self, s: str) -> bool:
        strs = s.split('.')
        if len(strs) != 2:
            return False
        
        hasVal1, hasVal2 = False, False
        s0, s1 = strs[0], strs[1]
        for i in range(len(s0)):
            ch = s0[i]
            if i == 0 and (ch == '+' or ch == '-'):
                continue
            if ch < '0' or ch > '9':
                return False
            hasVal1 = True
        for i in range(len(s1)):
            ch = s1[i]
            if ch < '0' or ch > '9':
                return False
            hasVal2 = True
            
        return hasVal1 or hasVal2            
```
