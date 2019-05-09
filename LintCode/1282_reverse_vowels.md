# <center>1282 - Reverse Vowels of a String</center> 



<br></br>

* Tag: String, Two Pointers
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Google
* Link: https://www.lintcode.com/problem/reverse-vowels-of-a-string/description

<br></br>



## Description
----
Write a function that takes a string as input and reverse only the vowels of a string.

<br></br>



## Example
----
1. Input: "hello" Output: "holle"
2. Input: "leetcode" Output: "leotcede"

<br></br>



## Solution
----
### Java
```java
public class Solution {
    private String vowels = "aeiouAEIOU";
    
    /**
     * @param s: a string
     * @return: reverse only the vowels of a string
     */
    public String reverseVowels(String s) {
        if (s == null)
            return s;
        
        char[] charArr = s.toCharArray();
        int i = 0, j = charArr.length - 1;
        while (i < j) {
            while (i < j && !isVowel(charArr[i]))
                i++;
            while (i < j && !isVowel(charArr[j]))
                j--;
            if (i < j) {
                char ch = charArr[i];
                charArr[i] = charArr[j];
                charArr[j] = ch;
                i++;
                j--;
            }
        }
        
        return String.valueOf(charArr);
    }
    
    private boolean isVowel(char ch) {
        return vowels.contains(String.valueOf(ch));
    }
}
```

<br>


## Go
```go
import "strings"

const vowels string = "aeiouAEIOU"

func reverseVowels(s string) string {
    chars := []byte(s)
    i, j := 0, len(s) - 1
    for i < j {
        for i < j && !isVowel(chars[i]) {
            i++
        }
        for i < j && !isVowel(chars[j]) {
            j--
        }
        if i < j {
            chars[i], chars[j] = chars[j], chars[i]
            i++
            j--
        }
    }
    
    return string(chars)
}

func isVowel(char byte) bool {
    return strings.Index(vowels, string(char)) > -1
}
```

<br>


## Python
```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        vowels = set(["a", "e", "i", "o", "u", "A", "E", "I", "O", "U"])
        res = list(s)
        start, end = 0, len(res)-1
        while start <= end:
            while start <= end  and res[start] not in vowels:
                start += 1
            while start <= end and res[end] not in vowels:
                end -= 1
            if start <= end:
                res[start], res[end] = res[end], res[start]
                start += 1
                end -= 1
                
        return "".join(res)
```
