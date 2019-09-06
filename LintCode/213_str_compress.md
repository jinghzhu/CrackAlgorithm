# <center>213 - String Compression (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String
* Company: Snapchat, Microsoft, Lyft
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/string-compression/description

<br></br>



## Description
----
Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3.

If the "compressed" string would not become smaller than the original string, your method should return the original string.

You can assume the string has only upper and lower case letters (a-z).

<br></br>



## Example
----
1. Input: str = "aabcccccaaa" Output: "a2b1c5a3"
2. Input: str = "aabbcc" Output: "aabbcc"

<br></br>



## Solution
----
### Java
```java
public class Compression {
	/**
     * @param originalString: a string
     * @return: a compressed string
     */
    public String compressLintCode(String originalString) {
        if (originalString == null || originalString.length() < 2)
            return originalString;
        
        char[] originalChar = originalString.toCharArray();
        String result = String.valueOf(originalChar[0]);
        int count = 0, i = 1;
        char last = originalChar[0];
        
        while (i < originalChar.length) {
            if (originalChar[i] == last) {
                count++;
            } else {
                result += String.valueOf(count+1);
                count = 0;
                result += String.valueOf(originalChar[i]);
            }
            last = originalChar[i++];
        }
        
        result += String.valueOf(count+1); // Important.
        
        if (result.length() < originalString.length())
            return result;
        
        return originalString;
    }
}
```

<br>


### Python
```python
class Compress:
    """
    @param originalString: a string
    @return: a compressed string
    """
    def solution_lintcode(self, originalString):
        result = ""
        l = len(originalString)
        if l < 3:
            return originalString
        i = 0
        while i < l:
            j, count = i + 1, 1
            while j < l and originalString[j] == originalString[i]:
                j, count = j + 1, count + 1
            result += originalString[i] + str(count)
            i = j
        if len(result) >= l:
            return originalString
        return result
```