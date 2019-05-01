# <center>1535 - To Lower Case (E)</center> 


* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy

https://www.lintcode.com/problem/to-lower-case/description

<br></br>



## Description
----
Implement function ToLowerCase() that has a string parameter str, and returns the same string in lowercase.

<br></br>



## Solution
----
### Go
```go
func toLowerCase(str string) string {
    s := ""
    for _, v := range str {
        if v >= 'A' && v <= 'Z' {
            v += 32
        }
        s += string(v)
    }
    
    return s
}
```

<br>


### Java
```java
public class Solution {
    /**
     * @param str: the input string
     * @return: The lower case string
     */
    public String toLowerCase(String str) {
        if (str == null)
            return str;
        
        char[] chs = str.toCharArray();
        for (int i = 0; i < chs.length; i++)
            if (chs[i] >= 'A' && chs[i] <= 'Z')
                chs[i] += 32;
        
        return String.valueOf(chs);
    }
}
```

<br>


## Python
```python
class Solution:
    def toLowerCase(self, s0: str) -> str:
        s1 = ""
        for ch in s0:
            ascii_id = ord(ch)
            if ascii_id >= 65 and ascii_id <= 90:
                ascii_id += 32
            s1 += chr(ascii_id)
        
        return s1
                
```
