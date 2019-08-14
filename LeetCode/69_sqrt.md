# <center>69 - Sqrt (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math, Binary Search
* Company: Apple, Facebook
* Difficulty: Easy
* Link: https://leetcode.com/problems/sqrtx/

<br></br>



## Description
----
Implement `int sqrt(int x)`.

Compute and return the square root of x, where x is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

<br></br>



## Example
----
Input: 8
Output: 2

<br></br>



## Solution
----
### Go
```go
func Sqrt(data int) int {
	if data == 0 { // Important
		return 0
	}

	low, high := 1, data
	for low+1 < high {
		mid := (high-low)/2 + low
		temp := int64(mid * mid) // int64 is important in case mid*mid > Integer max value.
		if temp == int64(data) {
			return mid
		}
		if temp < int64(data) {
			low = int(temp)
		} else {
			high = int(temp)
		}
	}

	return low
}
```

<br>


### Java
```java
public class Sqrt {
	public int solution(int x) {
		if (x == 0) // Important.
			return 0;
		
		long low = 1, high = x; // Long is important in case mid*mid > Integer max value.
		
		while (low + 1 < high) {
			long mid = (high - low) / 2 + low;
			if (x == mid * mid)
				return (int)mid;
			if (mid * mid > x) 
				high = mid;
			else
				low = mid;
		}
		
		return (int)low;
	}
}
```

<br>


### Python
```python
class Sqrt:
    """
    @param x: An integer
    @return: The sqrt of x
    """
    def solution(self, x):
        if x == 0:  # Important
            return x

        low, high = 1, x
        while low + 1 < high:
            mid = (high - low) // 2 + low
            if mid * mid == x:
                return mid
            if mid * mid > x:
                high = mid
            else:
                low = mid
        return low
```