# <center>1283 - Reverse String</center> 


* Tag: String, Two Pointer
* Author: Jinghua Zhu jhzhu@outlook.com Jianlan Zhou

https://www.lintcode.com/problem/reverse-string/description

<br></br>



## Description
----
Write a function that reverses a string. The input string is given as an array of characters char[].

Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

You may assume all the characters consist of printable ascii characters.

<br></br>



## Example
----
Input: ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]

Input: ["H","a","n","n","a","h"]
Output: ["h","a","n","n","a","H"]

<br></br>



## Solution
----
### Python
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        l = len(s)
        for i in range(l//2):
            s[i], s[l - i -1] = s[l - i -1], s[i]
```

<br>


### Go
```go
func reverseString(s []byte)  {
    i, j := 0, len(s) - 1
    for i < j {
        s[i], s[j] = s[j], s[i]
        i++
        j--
    }
}
```


### Java
```java
public class Solution {
    /**
     * @param s: a string
     * @return: return a string
     */
    public String reverseString(String s) {
        char[] ch = s.toCharArray();
        int halfLength = s.length() / 2;
        char temp;
        for (int i = 0; i < halfLength; i++) {
            temp = ch[s.length() - 1 - i];
            ch[s.length() - 1 - i] = ch[i];
            ch[i] = temp;
        }
        
        return new String(ch);
    }
}
```