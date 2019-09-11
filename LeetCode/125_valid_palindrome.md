# <center>125 - Valid Palindrome (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Tag: String, Two Pointers
* Company: Microsoft, Uber, Facebook
* Link: https://leetcode.com/problems/valid-palindrome/

<br></br>



## Description
----
Determine if the string is a palindrome, considering only alphanumeric characters and ignoring cases.

<br></br>



## Example
----
1: Input: "A man, a plan, a canal: Panama" Output: true
2: Input: "race a car" Output: false

<br></br>



## Solution
----
### Go
```go
func IsPalindrome(s string) bool {
	s = paliTranslate(s)
	if s == "" {
		return true
	}
	i, j := 0, len(s)-1
	for i <= j {
		if s[i] != s[j] {
			return false
		}
		i++
		j--
	}

	return true
}

func paliTranslate(s0 string) string {
	s1 := ""
	for _, v := range s0 {
		if (v >= 'a' && v <= 'z') || (v >= '0' && v <= '9') {
			s1 += string(v)
		} else if v >= 'A' && v <= 'Z' {
			s1 += string(v + 32)
		}
	}

	return s1
}
```

<br>


### Java
```java
public class Ispalindrome {
	public boolean isPalindrome(String s) {
        if (s == null)
            return true;
        
        char[] charArr = s.toCharArray();
        int left = 0, right = charArr.length - 1;
        
        while (left < right) {
            if (!isAlphaNum(charArr[left]))
            	++left;
            else if (!isAlphaNum(charArr[right]))
            	--right;
            else if ((charArr[left] + 32 - 'a') %32 != (charArr[right] + 32 - 'a') % 32)
            	return false;
            else {
                ++left;
                --right;
            }
        }
        
        return true;
    }
    
    private boolean isAlphaNum(char ch) {
        if (ch >= 'a' && ch <= 'z')
        	return true;
        if (ch >= 'A' && ch <= 'Z')
        	return true;
        if (ch >= '0' && ch <= '9') 
        	return true;
        
        return false;
    }
}
```