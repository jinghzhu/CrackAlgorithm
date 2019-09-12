# <center>796 - Rotate String (E)</center> 



<br></br>

* Difficulty: Easy
* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Link: https://leetcode.com/problems/rotate-string/

<br></br>



## Description
----
A shift on A consists of taking string A and moving the leftmost character to the rightmost position. 

<br></br>



## Example
----
1. Input: A = 'abcde', B = 'cdeab' Output: true
2. Input: A = 'abcde', B = 'abced'

<br></br>



## Solution
----
### Java
```java
public class RotateString {
	public boolean LeetCodeRotateString(String A, String B) {
        return A.length() == B.length() && (A + A).contains(B);
    }
```

<br>


### Go
```go
func RotateStringRight(a, b str) bool {
	return len(a) == len(b) && strings.Contains(a+a, b)
}
```

<br>


### Python
```python
class RotateString:
    def solution(self, A, B):
        return len(A) == len(B) and B in A+A
```