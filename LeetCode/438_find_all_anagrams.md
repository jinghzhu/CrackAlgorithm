# <center>438 - Find All Anagrams in a String (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Slip Window, Hash, String
* Difficulty: Medium
* Company: Amazon
* Link: https://leetcode.com/problems/find-all-anagrams-in-a-string/

<br></br>



## Description
----
Given a string s and a non-empty string p, find all the start indices of p's anagrams in s.

Strings consists of lowercase English letters only and the length of both strings s and p will not be larger than 20,100.

The order of output does not matter.

<br></br>



## Example
----
Input: s: "cbaebabacd" p: "abc" Output: [0, 6]

Explanation:
- The substring with start index = 0 is "cba", which is an anagram of "abc".
- The substring with start index = 6 is "bac", which is an anagram of "abc".

Input: s: "abab" p: "ab" Output: [0, 1, 2]

Explanation:
- The substring with start index = 0 is "ab", which is an anagram of "ab".
- The substring with start index = 1 is "ba", which is an anagram of "ab".
- The substring with start index = 2 is "ab", which is an anagram of "ab".

<br></br>



## Solution
----
### Java
```java
public class GetAllAnagrams {
	/**
     * @param s: a string
     * @param p: a string
     * @return: a list of index
     */
    public List<Integer> findAnagrams(String s, String p) {
    	List<Integer> list = new ArrayList<>();
        if (s == null || s.length() == 0 || p == null || p.length() == 0)
        	return list;
        
        int[] hash = new int[256]; 
        for (char c : p.toCharArray())
            hash[c]++;
        int left = 0, right = 0, count = p.length();
        
        while (right < s.length()) {
            if (hash[s.charAt(right)] >= 1)
                count--;
            hash[s.charAt(right)]--;
            right++;
            
            if (count == 0)
                list.add(left);

            if (right - left == p.length() ) {
                if (hash[s.charAt(left)] >= 0)
                    count++;
                hash[s.charAt(left)]++;
                left++;
            }
        }
        
        return list;
   }
}
```

<br>


### Go
```go
func FindAnagrams(s, p string) []int {
	result := make([]int, 0)
	lenS, lenP := len(s), len(p)
	if lenS < lenP { // Important.
		return result
	}

	// Init map.
	m := make(map[uint8]int)
	for i := 0; i < lenP; i++ {
		val, ok := m[p[i]]
		if ok {
			m[p[i]] = val + 1
		} else {
			m[p[i]] = 1
		}
	}

	// Init window.
	start, end, count := 0, lenP-1, lenP
	for i := 0; i <= end; i++ {
		val, ok := m[s[i]]
		if !ok {
			continue
		}
		m[s[i]] = val - 1
		if val-1 >= 0 {
			count--
		}
	}

	// Slip window.
	for end < lenS {
		if count == 0 {
			result = append(result, start)
		}
		if end == lenS-1 { // Important.
			break
		}
		if val, ok := m[s[start]]; ok {
			m[s[start]] = val + 1
			if val+1 > 0 {
				count++
			}
		}
		start, end = start+1, end+1
		if val, ok := m[s[end]]; ok {
			m[s[end]] = val - 1
			if val-1 >= 0 {
				count--
			}
		}
	}

	return result
}
```

<br>


### Python
```python
class GetAllAnagrams:
    def solution(self, s: str, p: str):
        result = []
        if len(s) < len(p):  # Important
            return result

        # Init map.
        m = {}
        for i in range(len(p)):
            if p[i] not in m:
                m[p[i]] = 1
            else:
                m[p[i]] += 1

        # Init window.
        start, end, count = 0, len(p) - 1, len(p)
        for i in range(end + 1):
            if s[i] not in m:
                continue
            m[s[i]] -= 1
            if m[s[i]] >= 0:
                count -= 1

        # Slip window.
        while end < len(s):
            if count == 0:
                result.append(start)
            if end == len(s) - 1:  # Important
                break
            if s[start] in m:
                m[s[start]] += 1
                if m[s[start]] > 0:
                    count += 1
            start, end = start + 1, end + 1
            if s[end] in m:
                m[s[end]] -= 1
                if m[s[end]] >= 0:
                    count -= 1

        return result
```