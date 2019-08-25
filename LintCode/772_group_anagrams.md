# <center>772 - Group Anagrams (M)</center> 



<br></br>

* Author: Jinghua ZHU <jhzhu@outlook.com>
* Tag: String, Hash, Sort
* Company: Facebook, Uber, Amazon
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/group-anagrams/description

<br></br>



## Description
----
Given an array of strings, group anagrams together.

<br></br>



## Example
----
Input:
```
["eat", "tea", "tan", "ate", "nat", "bat"]
```

Output:
```
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

<br></br>



## Solution
----
2 solutions:
1. By hashmap:

    For each string, sort it. Then put it as key in a hashmap. Do it for each string and group them.

    Time Complexity: O(NK), where N is the length of strs, and K is the maximum length of a string in strs.
    
    Space Complexity: O(NK)

2. By hashmap and sort:

    For each string, create a int array with 26 length and all elements are 0. Count the appearances of each character of the string and set it in the array. Conver the array to string and add/check it in a hashmap. Then group them.

    Time Complexity: O(NKlogK), where N is the length of strs, and K is the maximum length of a string in strs.
    
    Space Complexity: O(NK). 

<br>


### Go
```go
func GroupAnagrams(strs []string) [][]string {
    m := make(map[string][]string)
    for _, s := range strs {
        k := gaHelper(s)
        v, ok := m[k]
        var l []string
        if ok {
            l = v
        } else {
            l = make([]string, 0)
        }
        l = append(l, s)
        m[k] = l
    }
    result := make([][]string, 0)
    for _, v := range m {
        result = append(result, v)
    }
    
    return result
}

func gaHelper(s string) string {
    chars := [26]int{}
    for _, v := range s {
        chars[v - 'a'] += 1
    }
    
    return fmt.Sprintf("%v", chars)
}
```

<br>


### Java
```java
public class GroupAnagrams {
	public List<List<String>> groupAnagrams1(String[] strs) {
        HashMap<String, ArrayList> m = new HashMap<String, ArrayList>();
        for (String s : strs) {
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String key = String.valueOf(ca);
            if (!m.containsKey(key))
            	m.put(key, new ArrayList());
            m.get(key).add(s);
        }
        
        return new ArrayList(m.values());
    }
	
	public List<List<String>> groupAnagrams2(String[] strs) {
        HashMap<String, List> m = new HashMap<String, List>();
        int[] count = new int[26];
        for (String s : strs) {
            Arrays.fill(count, 0);
            for (char c : s.toCharArray())
            	count[c - 'a']++;

            StringBuilder sb = new StringBuilder("");
            for (int i = 0; i < 26; i++) {
                sb.append('#');
                sb.append(count[i]);
            }
            String key = sb.toString();
            if (!m.containsKey(key)) 
            	m.put(key, new ArrayList());
            m.get(key).add(s);
        }
        
        return new ArrayList(m.values());
    }
}
```

<br>


### Python
```python
class GroupAnagrams:
    def solution1(self, strs):
        m = {}
        for s in strs:
            l = self.process(s)
            if l in m:
                v = m[l]
                v.append(s)
                m[l] = v
            else:
                m[l] = [s]
        result = []
        for k in m:
            result.append(m[k])
        return result
    
    def process(self, s):
        l = self.init()
        for ch in s:
            index = ord(ch) - ord('a')
            l[index] += 1  # important
        return str(l)
    
    def init(self):
        l = []
        for i in range(26):
            l.append(0)
        return l
    
    def solution2(self, strs):
        m = {}
        for s0 in strs:
            s1 = self.sort_helper(s0)
            if s1 in m:
                v = m[s1]
                v.append(s0)
                m[s1] = v
            else:
                m[s1] = [s0]
        result = []
        for k in m:
            result.append(m[k])
        return result
    
    def sort_helper(self, s):
        l = list(s)
        l.sort()
        return "".join(l)
```