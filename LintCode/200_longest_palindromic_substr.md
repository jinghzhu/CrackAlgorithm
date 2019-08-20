# <center>200 - Longest Palindromic Substring (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String, Hash, Two Pointers
* Company: Amazon, Uber, Microsoft
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/longest-palindromic-substring/description

<br></br>



## Description
----
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

<br></br>



## Example
----
Input: "babad"
Output: "bab"

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param s: input string
     * @return: the longest palindromic substring
     */
    public String longestPalindrome(String s) {
        if (s == null || s.length() < 2) // important
            return s;
        String result = "";
        for (int i = 0; i < s.length() - 1; i++) {
            String temp = getPalindrome(s, i, i); // odd 
            if (temp.length() > result.length())
                result = temp;
            temp = getPalindrome(s, i, i + 1);
            if (temp.length() > result.length())
                result = temp;
        }
        
        return result;
    }
    
    private String getPalindrome(String s, int i, int j) {
        while (i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j)) {
            i--;
            j++;
        }
        
        return s.substring(i + 1, j);
    }
}
```

<br>


### Python
```python
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) < 2:
            return s
        result = ""
        for i in range(len(s) - 1):
            temp = self.get_palindrome(s, i, i)
            if len(temp) > len(result):
                result = temp
            temp = self.get_palindrome(s, i, i + 1)
            if len(temp) > len(result):
                result = temp
        return result
    
    def get_palindrome(self, s, i, j):
        while i >= 0 and j < len(s) and s[i] == s[j]:
            i -= 1
            j += 1
        return s[i+1:j]
```

<br>


### Go
```go
func longestPalindrome(str string) string {
    l := len(str)
    if l < 2 {
        return str
    }
    result := ""
    for i := 0; i < l - 1; i++ {
        temp := getPalindromic(str, i, i) // for odd
        if len(temp) > len(result) {
            result = temp
        }
        temp = getPalindromic(str, i, i + 1) // for even
        if len(temp) > len(result) {
            result = temp
        }
    }
    return result
}

func getPalindromic(str string, i, j int) string {
    for i >= 0 && j < len(str) && str[i] == str[j] {
        i--
        j++
    }
    
    return str[(i+1):j]
}
```
