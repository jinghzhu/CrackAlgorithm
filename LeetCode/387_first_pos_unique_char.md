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
public class Solution {
    /**
     * @param s: a string
     * @return: it's index
     */
    public int firstUniqChar(String s) {
        if (s == null)
            return -1;
            
        HashMap<Character, Integer> cache = new HashMap<Character, Integer>();
        char[] charArr = s.toCharArray();
        for (char ch : charArr) {
            if (!cache.containsKey(ch)) {
                cache.put(ch, 1);
                continue;
            }
            int count = cache.get(ch);
            cache.put(ch, count + 1);
        }
        for (int i = 0; i < charArr.length; i++)
            if (cache.get(charArr[i]) == 1)
                return i;
        
        return -1;
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