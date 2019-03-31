# <center>387 - First Position Unique Character</center> 


* Tag: String
* Company: Microsoft, Bloomberg, Amazon
* Author: Jinghua Zhu jhzhu@outlook.com

https://leetcode.com/problems/first-unique-character-in-a-string/

<br></br>



## Description
----
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

<br></br>



## Example
----
* Input : s = "lintcode" Output : 0
* Input : s = "lovelintcode" Output : 2

<br></br>



## Solution
----
### Java
```java
class Solution {
    public String toGoatLatin(String S) {
        String[] words = S.split(" ");
        
        for(int i = 0; i < words.length; i++) {
            // Apply rule 1 and 2
            String wordLower = words[i].toLowerCase();
            char first = wordLower.charAt(0);
            if("aeiou".indexOf(first) < 0) // not a vowel
                words[i] = words[i].substring(1) + words[i].charAt(0) + "ma";
            else // is vowel
                words[i] = words[i] + "ma";
            
            // Apply rule 3
            int index = i + 1;
            String as = "";
            for(int j = 0; j < index; j++)
                as = as + "a";
                
            words[i] = words[i] + as;
        }
        
        return String.join(" ", words);
    }
}
```

<br>


### Go
```go
func firstUniqChar(s string) int {
    cache := make(map[int32]int)
    for _, v := range s {
        count, ok := cache[v]
        if !ok {
            cache[v] = 1
        } else {
            cache[v] = count + 1
        }
    }
    for k, v := range s {
        if cache[v] == 1 {
            return k
        }
    }
    
    return -1
}
```