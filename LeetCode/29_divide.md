# <center>29 - Divide Two Integers (M)</center> 



<br></br>

* Tag: Math, Binary Search
* Difficulty: Medium
* Link: https://leetcode.com/problems/divide-two-integers/

<br></br>



## Description
----
Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

<br></br>



## Example
----
1. Input: dividend = 10, divisor = 3 Output: 3
2. Input: dividend = 7, divisor = -3 Output: -2

<br></br>



## Solution
----
### Java
```java
public class Divide {
	public int divide(int dividend, int divisor) {
		if (divisor == 0) 
            return dividend >= 0? Integer.MAX_VALUE : Integer.MIN_VALUE;
        if (dividend == 0) 
            return 0;
        if (dividend == Integer.MIN_VALUE && divisor == -1) // Important
            return Integer.MAX_VALUE;
            
		boolean isNegative = (dividend < 0 && divisor > 0) || 
                (dividend > 0 && divisor < 0);
		// important to convert them into Long type before calling Math.abs().
		// Otherwise if x = -2147483648, the result is 2147483647 because of overflow issue.
		long a = Math.abs((long)dividend), b = Math.abs((long)divisor);
		long low = 0, high = a;
		int result = 0 ; // must be initialized to meet grammar
		while (low + 1 < high) {
			long mid = (high - low) / 2 + low;
			if (mid * b == a) {
				result = (int)mid;
				break;
			}
			if (mid * b > a)
				high = mid;
			else
				low = mid;
		}
		
		if (low + 1 >= high && high *b <= a)
			result = (int)high;
		else if (low + 1 >= high)
			result = (int)low;
		
		return isNegative ? -result: result;
	}
}
```

<br>


### Go
```go
func Divide(dividend int, divisor int) int {
	if divisor == 0 || dividend == 0 {
		return 0
	}
	if dividend == -2147483648 && divisor == -1 { // Important
		return 2147483647
	}
	negative := 1
	if (dividend < 0 && divisor > 0) || (dividend > 0 && divisor < 0) {
		negative = -1
	}
	dividend64, divisor64 := int64(math.Abs(float64(dividend))), int64(math.Abs(float64(divisor)))
	low, high := int64(0), dividend64
	for low+1 < high {
		mid := low + (high-low)/2
		if mid*divisor64 == dividend64 {
			low = mid
			break
		}
		if mid*divisor64 > dividend64 {
			high = mid
		} else {
			low = mid
		}
	}
	if high*divisor64 <= dividend64 {
		return int(high) * negative
	}
	return int(low) * negative
}
```

<br>


### Python
----
```python
class Divide:
    def solution(self, dividend: int, divisor: int) -> int:
        if divisor == 0 or dividend == 0:
            return dividend
        if dividend == -2147483648 and divisor == -1:
            return 2147483647
        low, high, negative = 0, abs(dividend), 1
        if (dividend > 0 and divisor < 0) or (dividend < 0 and divisor > 0):
            negative = -1
        while low + 1 < high:
            mid = low + (high - low) // 2
            if abs(divisor) * mid == abs(dividend):
                low = mid
                break
            if abs(divisor) * mid > abs(dividend):
                high = mid
            else:
                low = mid

        if high * abs(divisor) <= abs(dividend):
            return high * negative
        return low * negative
```