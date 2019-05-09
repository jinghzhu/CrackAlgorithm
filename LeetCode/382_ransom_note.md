# <center>383 - Ransom Note</center> 



<br></br>

* Tag: String, Hash
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Apple
* Link: https://leetcode.com/problems/ransom-note/

<br></br>



## Description
----
Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.

Each letter in the magazine string can only be used once in your ransom note.

<br></br>



## Example
----
1. Input : ransomNote = "aa", magazine = "aab" Output : true
2. Input : ransomNote = "aaa", magazine = "aab" Output : false

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param ransomNote: a string
     * @param magazine: a string
     * @return: whether the ransom note can be constructed from the magazines
     */
    public boolean canConstruct(String ransomNote, String magazine) {
        if (ransomNote == null)
            return true;
        if((ransomNote != null && magazine == null) || ransomNote.length() < magazine.length())
            return false;
        
        char[] chArr1 = ransomNote.toCharArray();
        HashMap<Character, Integer> m = new HashMap<Character, Integer>();
        for (char ch : chArr1) {
            int count = 1;
            if (m.containsKey(ch)) {
                count = m.get(ch) + 1;
            }
            m.put(ch, count);
        }
        
        char[] chArr2 = magazine.toCharArray();
        for (char ch : chArr2) {
            if (m.size() == 0)
                return true;
            if (m.containsKey(ch)) {
                int count = m.get(ch);
                if (count == 1)
                    m.remove(ch);
                else
                    m.put(ch, count - 1);
            }
        }
        
        return m.size() == 0;
    }
}
```

<br>


## Go
```go
func canConstruct(ransomNote, magazine string) bool {
    if len(ransomNote) > len(magazine) {
        return false
    }

    m := make(map[int32]int)

    for _, v := range ransomNote {
        count, ok := m[v]
        if ok {
            m[v] = count + 1
        } else {
            m[v] = 1
        }
    }

    for _, v := range magazine {
        if len(m) == 0 {
            return true
        }
        count, ok := m[v]
        if !ok {
            continue
        }
        if count == 1 {
            delete(m, v)
        } else {
            m[v] = count - 1
        }
    }
    
    return len(m) == 0
}
```

<br>


## Python
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        if len(ransomNote) > len(magazine):
            return False
            
        m = {}
        for i in range(len(ransomNote)):
            ch = ransomNote[i]
            if ch not in m:
                m[ch] = 1
            else:
                count = m[ch]
                m[ch] = count + 1
        for i in range(len(magazine)):
            if len(m) == 0:
                return True
            ch = magazine[i]
            if ch in m:
                count = m[ch]
                if count == 1:
                    del m[ch]
                else:
                    m[ch] = count - 1
        
        return len(m) == 0
```
