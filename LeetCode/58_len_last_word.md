# <center>58 - Length of Last Word</center> 


* Tag: String
* Author: Jinghua Zhu jhzhu@outlook.com

https://leetcode.com/problems/length-of-last-word/

<br></br>



## Description
----
Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0. A word is defined as a character sequence consists of non-space characters only.

<br></br>



## Example
----
Input: "Hello World"
Output: 5

<br></br>



## Solution
----
### Go
```go
func LenOfLastWord(s string) int {
	start, end := 0, len(s)-1
	for i := 0; i <= end; i++ {
		if s[i] != ' ' {
			continue
		}
		if i != end && s[i+1] != ' ' {
			start = i + 1
		}
	}
	for ; end >= 0 && s[end] == ' '; end-- {
	}

	return end - start + 1
}
```

<br>


### Java
```java
public class LastWordLen {
	/**
     * @param s: A string
     * @return: the length of last word
     */
    public int lengthOfLastWord(String s) { // "a", "b a ", " "
        if (s == null)
            return 0;
        
        int start = 0, end = s.length() - 1;
        for (int i = 0; i <= end; i++) {
        	if (s.charAt(i) != ' ')
                continue; 
        	if (i != end && s.charAt(i + 1) != ' ')
        		start = i + 1;
        }
            
        while (end >= 0 && s.charAt(end) == ' ')
            end--;

        return end - start + 1;
    }
}
```