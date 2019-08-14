# <center>13 - strStr (E)</center> 



<br></br>

* Tag: String
* Company: Microsoft, Apple, Facebook
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/strstr/description

<br></br>



## Description
----
For a given source string and a target string, output the first index (from 0) of target string in source string. If target does not exist, return -1.

<br></br>



## Example
----
1. If source = "source" and target = "target", return -1.
2. If source = "abcdabcdefg" and target = "bcd", return 1.

<br></br>



## Solution
----
### Go
```go
func StrStr(haystack, needle string) int {
	if needle == "" {
		return 0
	}

	len1, len2 := len(haystack), len(needle)
	for i := 0; i < len1-len2+1; i++ {
		j, i1 := 0, i
		for ; j < len2; j++ {
			if haystack[i1] != needle[j] {
				break
			}
			i1++
		}
		if j == len2 {
			return i
		}
	}

	return -1
}
```

<br>


### Java
```java
public class StrStr {
	/**
     * @param source string to be scanned.
     * @param target string containing the sequence of characters to match.
     * @return a index to the first occurrence of target in source,
     * or -1  if target is not part of source.
     */
    public int strStr(String source, String target) {
        if (source == null || target == null || source.length() < target.length())
            return -1;
        
        for (int i = 0; i <= source.length() - target.length(); i++) { // i <= source.length() - target.length()
            int j = 0;
            for (j = 0; j < target.length(); j++)
                if (source.charAt(i + j) != target.charAt(j)) // i + j
                    break;
            if (j == target.length())
                return i;
        }
        
        return -1;
    }
}
```

<br>


### Python
```python
class StrStr:
    def solution(self, a: str, b: str) -> int:
        if b is None or a is None:
            return -1
        if b == "":
            return 0
        for i in range(len(a) - len(b) + 1):
            flag = True
            k = i
            for j in range(len(b)):
                if a[k] != b[j]:
                    flag = False
                    break
                k += 1
            if flag:
                return i
        return -1
```