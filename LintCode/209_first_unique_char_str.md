# <center>209 - First Unique Character in a String (E)</center> 



<br></br>

* Tag: String, Hashmap
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Google, Amazon, Microsoft
* Link: https://www.lintcode.com/problem/first-unique-character-in-a-string/description

<br></br>



## Description
----
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

<br></br>



## Example
----
s = "leetcode"
return 0.

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param str: str: the given string
     * @return: char: the first unique character in a given string
     */
    public char firstUniqChar(String s) {
        HashMap<Character, Integer> count = new HashMap<Character, Integer>();
        int n = s.length();
        // build hash map : character and how often it appears
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            count.put(c, count.getOrDefault(c, 0) + 1);
        }
        
        // find the index
        for (int i = 0; i < n; i++) {
            if (count.get(s.charAt(i)) == 1) 
                return s.charAt(i);
        }
        return 'a';
    }
}
```

<br>


### Go
```go
func firstUniqChar(s string) int {
    m := make(map[int32]int)
    for _, v := range s {
        count, ok := m[v]
        if ok {
            m[v] = count + 1
        } else {
            m[v] = 1
        }
    }
    for k, v := range s {
        if m[v] == 1 {
            return k
        }
    }
    return -1
}
```

<br>


### Python
```python
class Solution:
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: int
        """
        # build hash map : character and how often it appears
        count = collections.Counter(s)
        
        # find the index
        index = 0
        for ch in s:
            if count[ch] == 1:
                return index
            else:
                index += 1 
                
        return -1
```