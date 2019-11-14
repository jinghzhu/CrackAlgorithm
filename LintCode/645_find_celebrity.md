# <center>645 - Find the Celebrity (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Facebook, LinkedIn
* Link: https://www.lintcode.com/problem/find-the-celebrity

<br></br>



## Description
----
Suppose you are at a party with n people (labeled from 0 to n - 1) and among them, there may exist one celebrity. The definition of a celebrity is that all the other n - 1 people know him/her but he/she does not know any of them.

Now you want to find out who the celebrity is or verify that there is not one. The only thing you are allowed to do is to ask questions like: "Hi, A. Do you know B?" to get information of whether A knows B. You need to find out the celebrity (or verify there is not one) by asking as few questions as possible (in the asymptotic sense).

You are given a helper function bool knows(a, b) which tells you whether A knows B. Implement a function int findCelebrity(n), your function should minimize the number of calls to knows.

<br></br>



## Example
----
Input:
```
2 // next n * (n - 1) lines 
0 knows 1
1 does not know 0
```

Output: 1 Everyone knows 1,and 1 knows no one.

<br></br>



## Solution
----
算法：
1. 常规做法。人与人关系可用一个有向图建模（非对称），相当于找出度为0，入度为n-1的节点。具体实现，用嵌套for循环统计每个人认识几个人，以及几个人认识自己。时间复杂度O(n^2)。
2. 核心在于调用一次knows(a,b)至少剔除一个候选人：
   - 如果true，即a认识b，可判断a不可能是celebrity；
   - 如果false，即a不认识b，可判断b不可能是celebrity。
   所以，调用n-1次knows()，能剔除n-1个候选人。剩下的可能是celebrity，也可能不是，须进一步判断。

<br>


### Go
```go
func GetCelebrity(n int) int {
	if n < 1 {
		return -1
	}
	if n == 1 {
		return 0
	}
	celebrity := 0
	for i := 1; i < n; i++ {
		if gcKnow(celebrity, i) {
			celebrity = i
		}
	}

	for i := 0; i < n; i++ {
		if i == celebrity {
			continue
		}
		if gcKnow(celebrity, i) || gcKnow(i, celebrity) {
			return - 1
		}
	}

	return celebrity
}
```

<br>


### Python
```python
class GetCelebrity:
    def soltuion1(self, n):
        whoKnowMe, iKnowWho = {}, {}
        for i in range(n):
            for j in range(n):
                if i == j:
                    continue
                if self.knows(i, j):
                    if i in iKnowWho:
                        iKnowWho[i] = iKnowWho[i] + 1
                    else:
                        iKnowWho[i] = 1
                    if j in whoKnowMe:
                        whoKnowMe[j] = whoKnowMe[j] + 1
                    else:
                        whoKnowMe[j] = 1
        for i in range(n):
            if i not in iKnowWho and (i not in whoKnowMe or whoKnowMe[i] == n - 1):
                return i

        return -1
```