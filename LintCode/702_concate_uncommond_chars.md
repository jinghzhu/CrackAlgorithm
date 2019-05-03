# <center>702 - Concatenated String with Uncommon Characters of Two Strings</center> 


* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Microsoft
* Link: https://www.lintcode.com/problem/concatenated-string-with-uncommon-characters-of-two-strings/description

<br></br>



## Description
----
Two strings are given and you have to modify 1st string such that all the common characters of the 2nd strings have to be removed and the uncommon characters of the 2nd string have to be concatenated with uncommon characters of the 1st string.

<br></br>



## Example
----
1. Input: s1 = "aacdb", s2 = "gafd" Output: "cbgf"
1. Input: s1 = "abcs", s2 = "cxzca" Output: "bsxz"

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param s1: the 1st string
     * @param s2: the 2nd string
     * @return: uncommon characters of given strings
     */
    public String concatenetedString(String s1, String s2) {
        if (s1 == null && s2 == null)
            return null;
        if (s1 == null)
            return s2;
        if (s2 == null)
            return s1;
        
        char[] chs1 = s1.toCharArray();
        char[] chs2 = s2.toCharArray();
        
        return filter(chs1, scan(chs2)) + filter(chs2, scan(chs1));
        
    }
    
    private Set<Character> scan(char[] chs) {
        Set<Character> m = new HashSet<Character>();
        for (char ch : chs)
            m.add(ch);
        
        return m;
    }
    
    private String filter(char[] chs, Set<Character> m) {
        String s = "";
        for (char ch : chs)
            if (!m.contains(ch))
                s += String.valueOf(ch);
        
        return s;
    }
}
```

<br>


## Python
```python
class Solution:
    """
    @param s1: the 1st string
    @param s2: the 2nd string
    @return: uncommon characters of given strings
    """
    def concatenetedString(self, s1: str, s2: str) -> bool:
        return self.filter(s1, self.scan(s2)) + self.filter(s2, self.scan(s1))
    
    def scan(self, s: str) -> dict:
        d = {}
        for ch in s:
            d[ch] = 1
        
        return d
        
    def filter(self, s: str, d: dict) -> str:
        result = ""
        for ch in s:
            if ch not in d:
                result += ch
                
        return result    
```
