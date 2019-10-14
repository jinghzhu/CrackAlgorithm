# <center>127 - Word Ladder I (M)</center> 



<br></br>

* Tag: Graph, BFS
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Google, LinkedIn, Amazon, Facebook, Snapchat
* Link: https://leetcode.com/problems/word-ladder/

<br></br>



## Description
----
Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:
1. Only one letter can be changed at a time.
2. Each transformed word must exist in word list. The beginWord is not transformed word.

Note:
- Return 0 if there is no such transformation sequence.
- All words have the same length.
- All words contain only lowercase alphabetic characters.
- You may assume no duplicates in the word list.
- You may assume beginWord and endWord are non-empty and are not the same.

> The key difference between LeetCode and LintCode is LintCode allows if the transformed can be changed to target by only modifying one character while the target is not in dictionary. For example, start = "a", end = "c", dict = ["b"]. LintCode returns 3, whose path is a-b-c. But LeetCode returns 0.

<br></br>



## Example
----
Input：start = "a"，end = "c"，dict =["a","b","c"] Output：2

Explanation："a"->"c"

Note that the difference between LeetCode and LintCode is LintCode allows if the transformed can be changed to target by only modifying one character while the target is not in dictionary.

For example, start = "a", end = "c", dict = ["b"]. LintCode returns 3, whose path is a-b-c. But LeetCode returns 0.

<br></br>



## Solution
----
### Java
```java
public class WordLadder1 {
	/*
     * @param start: a string
     * @param end: a string
     * @param dict: a set of string
     * @return: An integer
     */
    public int lintcodeSolution(String start, String end, Set<String> dict) {
        if (start == null || end == null || dict == null)
            return 0;
        
        if (start.equals(end)) // Important.
            return 1;
        Set<String> visited = new HashSet<String>();
        visited.add(start);
        Queue<String> words = new LinkedList<String>();
        words.offer(start);
        int result = 1;
        
        while (!words.isEmpty()) {
            int l = words.size();
            for (int i = 0; i < l; i++) {
                String cur = words.poll();
                if (valid(cur, end))  // Important.
                    return ++result;
                Iterator<String> it = dict.iterator();
                while (it.hasNext()) {
                    String s = it.next();
                    if (!visited.contains(s) && valid(s, cur)) {
                    	// Important. If move this check with if (valid(cur, end))
                    	// together, there is bug.
                    	// For example, start = "a", end = "c", dict = {"b", "c"}
                    	// After 1st round, the q is {"b", "c"}. Then the first
                    	// element pop from queue is "b". "b".equals("c") is false
                    	// while valid("b", "c"), then it returns 3 but the correct
                    	// result is 2.
                        if (s.equals(end))
                            return ++result;
                        visited.add(s);
                        words.offer(s);
                    }
                }
            }
            result++;
        }
        
        return 0;
    }
    
    private boolean valid(String a, String b) {
        char[] as = a.toCharArray(), bs = b.toCharArray();
        int count = 0;
        for (int i = 0; i < as.length; i++) {
            if (as[i] != bs[i] && count == 1)
                return false;
            else if (as[i] != bs[i])
                count++;
        }
        
        return true;
    }
}
```

<br>


### Python
```python
class WordLadder1:
    def solution_leetcode(self, begin: str, end: str, words) -> int:
        if not end or not begin or not words or end not in words:
            return 0
        dist = 1
        q = [begin]
        visited, dicts = set(), set()
        visited.add(begin)
        for w in words:
            dicts.add(w)

        while len(q) > 0:
            l = len(q)
            for i in range(l):
                cur = q.pop(0)
                if cur == end:
                    return dist
                for word in words:
                    if word not in visited and self.solution_leetcode_helper(word, cur):
                        q.append(word)
                        visited.add(word)
            dist += 1

        return 0

    def solution_leetcode_helper(self, a, b):
        count = 0
        for i in range(len(a)):
            if a[i] != b[i] and count > 0:
                return False
            elif a[i] != b[i]:
                count += 1
        return True
```

<br>


### Go
```go
func LadderLengthLeetCode(begin string, end string, words []string) int {
	visited := make(map[string]bool)
	visited[begin] = true
	q := make([]string, 0)
	q = append(q, begin)
	dist := 1

	for len(q) > 0 {
		l := len(q)
		for i := 0; i < l; i++ {
			cur := q[0]
			q = q[1:]
			if cur == end {
				return dist
			}
			for _, v := range words {
				if _, ok := visited[v]; !ok && lllcHelper(v, cur) {
					visited[v] = true
					q = append(q, v)
				}
			}
		}
		dist++
	}

	return 0
}

func lllcHelper(a, b string) bool {
	count := 0
	for k := range a {
		if a[k] != b[k] && count > 0 {
			return false
		} else if a[k] != b[k] {
			count++
		}
	}

	return true
}
```
