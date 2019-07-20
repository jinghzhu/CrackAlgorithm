# <center>384 - Longest Substring Without Repeating Characters(M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String, Hash, Two Pointers
* Company: Amazon
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/longest-substring-without-repeating-characters/description

<br></br>



## Description
----
Given a string, find the length of the longest substring without repeating characters.

<br></br>



## Example
----
1. Input: "abcabcbb" Output: 3
2. Input: "bbbbb" Output: 1

<br></br>



## Solution
----
双指针做法：
1. 用`index`记录当前正在观察的子串起始位置。
2. 用哈希表记录每个字符最后一次出现的位置。
3. 设当前字符出现的位置`i`且其最后一次出现的位置为`j`。如果`j >= index`，说明从`index`到`i`的子字符串出现重复字符，则：
    1. 该子字符串长度为`i-index`，与缓存的最大长度比较。
    2. 新的待观察子字符串起始位置`index`为`j+1`。因为当前字符在老`index`至`i`区间已重复出现，无须再观察。
4. 注意，当结束循环时，应比较`len(s) - index > max`。因为，可能最大子字符串结束位置在末尾（比如`ababc`），而之前比较都是要求有重复字符的前提。

<br>

### Java
```java
public class LongestSubStrWithoutRepeatChar {
	/**
     * @param s: a string
     * @return: an integer
     */
    public int lengthOfLongestSubstring(String s) {
        if (s == null)
            return 0;
        
        HashMap<Character, Integer> m = new HashMap<Character, Integer>();
        int max = 0, index = 0;
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (m.containsKey(ch)) {
            	int indexTmp = m.get(ch);
                if (indexTmp >= index) {
                    max = Math.max(max, i - index);
                    index = indexTmp + 1;
                }
            }
            m.put(ch, i);
        }
        max = Math.max(s.length() - index, max); // Important, image case "abc".
        
        return max;
    }
}
```

<br>


### Python
```python
class LongestSubstrWithoutRepeatChar:
    def solution(self, s: str) -> int:
        index, max_len = 0, 0
        m = {}
        for i in range(len(s)):
            ch = s[i]
            if ch in m and m[ch] >= index:
                max_tmp, index = i - index, m[ch] + 1
                max_len = max(max_len, max_tmp)
            m[ch] = i

        max_len = max(len(s) - index, max_len)
        return max_len
```

<br>


### Go
```go
func LongestSubstrWithoutRepeatChar (s string) int {
    index, max := 0, 0
    m := make(map[int32]int)
    for k, v := range s {
        pos, ok := m[v]
        if ok && pos >= index {
            if k - index > max {
                max = k - index
            }
            index = pos + 1
        }
        m[v] = k
    }
    if len(s) - index > max { // Important, image "abc"
        max = len(s) - index
    }
    
	return max
}
```
