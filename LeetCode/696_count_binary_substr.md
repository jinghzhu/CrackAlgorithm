# <center>696 - Count Binary Substrings</center> 



<br></br>

* Tag: String, Two Pointers
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/count-binary-substrings/

<br></br>



## Description
----
Give a string s, count the number of non-empty (contiguous) substrings that have the same number of 0's and 1's, and all the 0's and all the 1's in these substrings are grouped consecutively.

Substrings that occur multiple times are counted the number of times they occur.

<br></br>



## Example
----
1. Input: "00110011" Output: 6

    Explanation: There are 6 substrings that have equal number of consecutive 1's and 0's: "0011", "01", "1100", "10", "0011", and "01".

    Notice that some of these substrings repeat and are counted the number of times they occur.

    Also, "00110011" is not a valid substring because all the 0's (and 1's) are not grouped together.

2. Input: "10101" Output: 4

    Explanation: There are 4 substrings: "10", "01", "10", "01" that have equal number of consecutive 1's and 0's.


<br></br>



## Solution
----
### Java
```java
public class CountBinarySubStr {
	/**
     * @param s: a string
     * @return: the number of substrings
     */
    public int countBinarySubstrings(String s) {
        int count = 0;
        if (s == null)
            return count;
        
        char[] chars = s.toCharArray();
        for (int i = 0; i < s.length() - 1; i++) {
            int j = i + 1;
            if (chars[i] == chars[j])
                continue;
            count += check(chars, i);
        }
        
        return count; 
    }
    
    private int check(char[] chars, int start) {
        char left = chars[start], right = chars[start + 1];
        int i = start, j = start + 1, count = 0;
        while (i >= 0 && j < chars.length) {
            if (chars[i--] != left || chars[j++] != right)
                break;
            count++;
        }
        
        return count;
    }
}
```

<br>


## Go
```go
func CountBinarySubstrings(s string) int {
	count := 0
	for i := 0; i < len(s)-1; i++ {
		if s[i] == s[i+1] {
			continue
		}
		count += cbsHelper(s, i)
	}

	return count
}

func cbsHelper(s string, start int) int {
	left, right := s[start], s[start+1]
	i, j, count := start, start+1, 0
	for i >= 0 && j < len(s) {
		if s[i] != left || s[j] != right {
			break
		}
		count++
		i--
		j++
	}

	return count
}
```

<br>


## Python
```python
class CountBinarySubstrings:
    def countBinarySubstrings(self, s: str) -> int:
        count = 0
        for i in range(len(s) - 1):
            if s[i] == s[i + 1]:
                continue
            count += self.check(s, i)
        
        return count
    
    def check(self, s: str, start: int) -> int:
        left, right, i, j, count = s[start], s[start + 1], start, start + 1, 0
        while i >= 0 and j < len(s):
            if s[i] != left or s[j] != right:
                break
            count += 1
            i -= 1
            j += 1
        
        return count
```
