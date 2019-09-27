# <center>231 - Power of Two (E)</center> 



<br></br>

* Tag: Bit, Math
* Company: Google
* Difficulty: Easy
* Link: https://leetcode.com/problems/power-of-two/submissions/

<br></br>



## Description
----
Given an integer, write a function to determine if it is a power of two.

<br></br>



## Example
----
Input: 16
Output: true

<br></br>



## Solution
----
### Java
```java
public class IsTwoPower {
	/**
     * @param x: an int data
     * @return a boolean type data
     */
	public boolean isTwoPower(int x) {
		return ((x & (x - 1)) == 0) && (x > 0);
	}
}
```

<br>


### Python
```python
class IsTwoPower:
    """
    @param n: an integer
    @return: if n is a power of two
    """
    def solution(self, x):
        return x and (x & (x - 1) == 0) and (x > 0)
```

<br>


### Go
```go
func IsTwoPower(x int) bool {
	return ((x & (x - 1)) == 0) && (x > 0)
}
```
