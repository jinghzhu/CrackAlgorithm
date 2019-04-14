# <center>859 - Buddy String</center> 


* Tag: String
* Company: Google
* Author: Jinghua Zhu jhzhu@outlook.com

https://leetcode.com/problems/buddy-strings/

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
public class Buddy {
	/**
     * @param A: string A
     * @param B: string B
     * @return: bool
     */
    public boolean buddyStrings(String A, String B) {
        if (A == null && B == null)
            return true;
        if (A == null || B == null || A.length() != B.length())
            return false;
        
        char[] a = A.toCharArray();
        char[] b = B.toCharArray();
        HashMap<Character, Integer> m = new HashMap<Character, Integer>();
        int count = 0;
        char a0 = ' ', b0 = ' ';
        boolean flag = false;
        
        for (int i = 0; i < a.length; i++) {
            if (a[i] == b[i]) {
                if (flag == false) {
                    if (!m.containsKey(a[i]))
                        m.put(a[i], 1);
                    else
                        flag = true;
                }
                continue;
            }
                
            if (count == 0) {
                a0 = b[i];
                b0 = a[i];
                count = 1;
            } else if (count == 1) {
                if (a[i] != a0 || b[i] != b0)
                    return false;
                count = 2;
            } else {
                return false;
            }
        }
        
        if (count == 2 || flag == true)
            return true;
        
        return false;
    }
}
```