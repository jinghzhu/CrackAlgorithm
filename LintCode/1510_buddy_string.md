# <center>1510 - Buddy String (E)</center> 


<br></br>
* Tag: String
* Company: Google
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/buddy-strings/description

<br></br>



## Description
----
Given two strings A and B of lowercase letters, return true if and only if we can swap two letters in A so that the result equals B.

1. 0 <= A.length <= 20000
2. 0 <= A.length <= 20000
3. A and B consist only of lowercase letters.

<br></br>



## Example
----
1. Input: A = "ab", B = "ba" Output: true
2. Input: A = "ab", B = "ab" Output: false
3. Input: A = "aa", B = "aa" Output: true
4. Input: A = "aaaaaaabc", B = "aaaaaaacb" Output: true
5. Input: A = "", B = "aa" Output: false

<br></br>



## Solution
----
### Go
```go
func BuddyStrings(A, B string) bool {
	if len(A) != len(B) || len(A) < 2 {
		return false
	}

	var a0, b0 uint8
	count := 0
	m := make(map[uint8]int)
	flag := false

	for i := 0; i < len(A); i++ {
		if A[i] == B[i] {
			if !flag {
				if _, ok := m[B[i]]; ok {
					flag = true
				} else {
					m[B[i]] = 1
				}
			}
			continue
		}
		if count == 0 {
			a0, b0 = B[i], A[i]
			count = 1
		} else if count == 1 {
			if a0 != A[i] || b0 != B[i] {
				return false
			}
			count = 2
		} else {
			return false
		}
	}

	if count == 2 || flag {
		return true
	}

	return false
}
```

<br>


### Java
```java
public class Solution {
    /**
     * @param A: string A
     * @param B: string B
     * @return: bool
     */
    public boolean buddyStrings(String A, String B) {
        if (A.length() != B.length()) {
            return false;
        }
        if (A.equals(B)) {
            Set<Character> s = new HashSet<Character>();
            for (char c : A.toCharArray()) {
                s.add(c);
            }
            return s.size() < A.length();
        }
        List<Integer> dif = new ArrayList<>();
        for (int i = 0; i < A.length(); ++i) {
            if (A.charAt(i) != B.charAt(i)) {
                dif.add(i);
            }
        }
        return dif.size() == 2 && A.charAt(dif.get(0)) == B.charAt(dif.get(1)) && A.charAt(dif.get(1)) == B.charAt(dif.get(0));
    }
}
```

<br>


### Python
```python
class Solution:
    """
    @param A: string A
    @param B: string B
    @return: bool
    """
    def buddyStrings(self, A, B):
        # Write your code here
        if len(A) != len(B): 
            return False
        if A == B and len(set(A)) < len(A): 
            return True
        dif = [(a, b) for a, b in zip(A, B) if a != b]
        return len(dif) == 2 and dif[0] == dif[1][::-1]
```