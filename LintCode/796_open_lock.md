# <center>796 - Open The Lock (H)</center> 



<br></br>

* Tag: Graph, BFS, String
* Difficulty: Hard
* Link: https://www.lintcode.com/problem/open-the-lock/description

<br></br>



## Description
----
 You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'. The wheels can rotate freely and wrap around: for example we can turn '9' to be '0', or '0' to be '9'. Each move consists of turning one wheel one slot.

The lock initially starts at '0000', a string representing the state of the 4 wheels.

You are given a list of deadends dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a target representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or -1 if it is impossible. 

<br></br>



## Example
----
- Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
- Output: 6
- Explanation: A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202". Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid, because the wheels of the lock become stuck after the display becomes the dead end "0102".

<br></br>



## Solution
----
### Java
```java
public class OpenLock {
	/**
     * @param deadends: the list of deadends
     * @param target: the value of the wheels that will unlock the lock
     * @return: the minimum total number of turns 
     */
    public int openLock(String[] deadends, String target) {
        if (target == null || deadends == null)
            return -1;
        String start = "0000";
    
        Set<String> deadset = new HashSet<String>();
        for (String s : deadends)
            deadset.add(s);
        if (deadset.contains(start)) // Important. ["0000"] -> "8888"
            return -1;
    
        // BFS pattern
        Queue<String> q = new LinkedList<String>();
        Set<String> visited = new HashSet<String>();
        q.offer(start);
        visited.add(start);
        int dist = 0;
    
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                String cur = q.poll();
                if (target.equals(cur))
                    return dist;
                for (int k = 0; k < 4; k++) {
                    String next = cur.substring(0, k) + (cur.charAt(k) - '0' - 1 + 10 ) % 10 + cur.substring(k + 1);
                    if (!visited.contains(next) && !deadset.contains(next)) {
                        q.offer(next);
                        visited.add(next);
                    }
                    next = cur.substring(0, k) + (cur.charAt(k) - '0' + 1) % 10 + cur.substring(k + 1);
                    if (!visited.contains(next) && !deadset.contains(next)) {
                        q.offer(next);
                        visited.add(next);
                    }
                }
            }
            dist++;
        }
    
        return -1;
    }
}
```

<br>


### Python
```python
class Solution:
    def openLock(self, deadends: List[str], target: str) -> int:
        # pre_check(deadends, target)
        start = "0000"
        deads = set()
        for d in deadends:
            deads.add(d)
        if start in deads:
            return -1  # Important. ["0000"] -> "8888"
        q = [start]
        visited = set()
        visited.add(start)
        dist = 0
        
        while len(q) > 0:
            l = len(q)
            for i in range(l):
                cur = q.pop(0)
                if cur == target:
                    return dist
                for j in range(4):
                    tmp = cur[:j] + str((ord(cur[j]) - ord('0') + 1 - 10) % 10) + cur[j + 1:]
                    if tmp not in deads and tmp not in visited:
                        q.append(tmp)
                        visited.add(tmp)
                    tmp = cur[:j] + str((ord(cur[j]) - ord('0') - 1 + 10) % 10) + cur[j + 1:]
                    if tmp not in deads and tmp not in visited:
                        q.append(tmp)
                        visited.add(tmp)
            dist += 1
        
        return -1
```

<br>


### Go
```go
func OpenLock(deadends []string, target string) int {
	// prCheck(deadends, target)
	start := "0000"
	deads := make(map[string]bool)
	for _, v := range deadends {
		deads[v] = true
	}
	if _, ok := deads[start]; ok {
		return -1 // Important. ["0000"] -> "8888"
	}
	visited := make(map[string]bool)
	visited[start] = true
	q := make([]string, 0)
	q = append(q, start)
	result := 0

	for len(q) > 0 {
		l := len(q)
		for i := 0; i < l; i++ {
			cur := q[0]
			q = q[1:]
			if cur == target {
				return result
			}
			for j := 0; j < 4; j++ {
				next := cur[:j] + fmt.Sprintf("%d", (cur[j]-'0'+1+10)%10) + cur[j+1:]
				_, ok1 := visited[next]
				_, ok2 := deads[next]
				if !ok1 && !ok2 {
					visited[next] = true
					q = append(q, next)
				}
				next = cur[:j] + fmt.Sprintf("%d", (cur[j]-'0'-1+10)%10) + cur[j+1:]
				_, ok1 = visited[next]
				_, ok2 = deads[next]
				if !ok1 && !ok2 {
					visited[next] = true
					q = append(q, next)
				}
			}
		}
		result++
	}

	return -1
}
```
