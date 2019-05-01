# <center>392 - Is Subsequence (M)</center> 


* Tag: String, Greedy
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium

https://leetcode.com/problems/is-subsequence/

<br></br>



## Description
----
Given a string `s` and a string `t`, check if `s` is subsequence of `t`.

You may assume that there is only lower case English letters in both `s` and `t`. `t` is potentially a very long (length ~= 500,000) string, and `s` is a short string (length <= 100).

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, `"ace"` is a subsequence of `"abcde"` while `"aec"` is not).

<br></br>



## Example
----
1. Input: s = "abc", t = "ahbgdc" Output: true
2. Input: s = `"axc"`, t = `"ahbgdc"` Output: false

<br></br>



## Solution
----
### Greedy
使用双指针，一个指向`s`，另一个指向`t`。当`s[i] == t[j]`时，`i, j = i + 1, j + 1`。最后检查`i == len(s)`。

Go:
```go
func IsSubsequence(s, t string) bool {
	if len(s) > len(t) {
		return false
	}

	lenS, lenT, i, j := len(s), len(t), 0, 0
	for i < lenS && j < lenT && lenT-j >= lenS-i {
		if s[i] == t[j] {
			i++
		}
		j++
	}

	return i == lenS
}
```

Java:
```java
public boolean solution1(String s, String t) {
    if (s == null || t == null || s.length() > t.length())
        return false;
        
    int i = 0, j = 0, lenS = s.length(), lenT = t.length();
    while ( i < lenS && j < lenT && (lenT - i >= lenS - j)) {
        if (s.charAt(i) == t.charAt(j))
            i++;
        j++;
    }
        
    return i == lenS;
}
```

Python3:
```python
def solution1(s: str, t: str) -> bool:
    if len(s) > len(t):
        return False
        
    i, j, len_s, len_t = 0, 0, len(s), len(t)
    while i < len_s and j < len_t and len_t - j >= len_s - i:
        if s[i] == t[j]:
            i += 1
        j += 1
            
    return i == len_s
```

<br>


### Binary Search
步骤：
1. 遍历字符串`t`，对每个字符，将其出现位置放入字典。
2. 遍历字符串`s`，对每个字符，在字典中寻找是否有匹配的。匹配条件要同时满足：
    1. 是同一个字符。
    2. 该字符在`t`中位置需在剩余的子串中。所谓“剩余的子串”，是指每次`s`和`t`有个字符匹配时，`s`剩余的字符只能在`t`中该位置之后寻找。

Go:
```go
func IsSubsequence2(s, t string) bool {
	if len(s) > len(t) {
		return false
	}
	m := make(map[int32][]int)
	for k, v := range t {
		l, ok := m[v]
		if !ok {
			l = make([]int, 0)
		}
		l = append(l, k)
		m[v] = l
	}
	index := -1 // important
	for _, v := range s {
		index = isSearch(m, v, index)
		if index == -1 {
			return false
		}
	}

	return true
}

func isSearch(m map[int32][]int, v int32, index int) int {
	l, ok := m[v]
	if !ok {
		return -1
	}
	low, high := 0, len(l)-1
	for low+1 < high {
		mid := (high-low)/2 + low
		if l[mid] > index {
			high = mid
		} else {
			low = mid
		}
	}
	if l[low] > index {
		return l[low]
	}
	if l[high] > index {
		return l[high]
	}

	return -1
}
```

Java:
```java
public class IsSubSequence {
	public boolean isSubsequence(String s, String t) {
        if (s == null || t == null || s.length() > t.length())
            return false;
        
        HashMap<Character, List<Integer>> m = new HashMap<Character, List<Integer>>();
        for (int i = 0; i < t.length(); i++) {
            List<Integer> l = null;
            if (!m.containsKey(t.charAt(i)))
                l = new ArrayList<Integer>();
            else
                l = m.get(t.charAt(i));
            l.add(i);
            m.put(t.charAt(i), l);
        }
        int index = -1; // important
        for (int i = 0; i < s.length(); i++) {
            index = search(m, s.charAt(i), index);
            if (index == -1)
                return false;
        }
        
        return true;
    }
    
    private int search(HashMap<Character, List<Integer>> m, char ch, int index) {
        if (!m.containsKey(ch))
            return -1;
        
        List<Integer> l = m.get(ch);
        int low = 0, high = l.size() - 1;
        
        while (low + 1 < high) {
            int mid = (high - low) / 2 + low;
            if (l.get(mid) > index)
                high = mid;
            else
                low = mid;
        }
        
        if (l.get(low) > index)
            return l.get(low);
        
        if (l.get(high) > index)
            return l.get(high);
        
        return -1;
    }
}
```

Python3:
```python
class IsSubsequence:
    def solution(self, s: str, t: str) -> bool:
        if len(s) > len(t):
            return False
        
        d = {}
        for i in range(len(t)):
            ch = t[i]
            if ch in d:
                l = d[ch]
                l.append(i)
                d[ch] = l
            else:
                d[ch] = [i]
        
        index = -1
        for i in range(len(s)):
            index = self.search(d, s[i], index)
            if index == -1:
                return False
        
        return True
    
    def search(self, d: dict, ch: str, index: int) -> int:
        if ch not in d:
            return -1
        
        l = d[ch]
        low, high = 0, len(l) - 1
        
        while low + 1 < high:
            mid = (high - low) // 2 + low
            if l[mid] > index:
                high = mid
            else:
                low = mid
                
        if l[low] > index:
            return l[low]
        if l[high] > index:
            return l[high]
        
        return -1
```