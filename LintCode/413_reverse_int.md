# <center>413 - Reverse Integer (E)</center> 



<br></br>

* Tag: Bit, Math
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Apple
* Link: https://www.lintcode.com/problem/reverse-integer/description

<br></br>



## Description
----
Given a 32-bit signed integer, reverse digits of an integer.

<br></br>



## Example
----
1. Input: 123 Output: 321
2. Input: -123 Output: -321
3. Input: 120 Output: 21

<br></br>



## Solution
----
### Java
```java
public class ReverseInt {
	// Employ long to help to deal with the overflow case.
    public int reverse(int x) {
        long tmp=0;
        while(x != 0) {
            tmp = tmp * 10 + x % 10;
            if(tmp > Integer.MAX_VALUE || tmp < Integer.MIN_VALUE)
                return 0;
            x /= 10;
        }
        
        return (int)tmp;
    }
}
```

<br>


### Python
```python
class Solution:
    def reverse(self, x: int) -> int:
        min_int, max_int, result, flag = -1 * (1 << 31), 0, 0, 1
        for i in range(31):
            max_int = (max_int << 1) | 1
        if x < 0:
            flag = -1
            x *= -1
        while x != 0:
            result = 10 * result + x % 10
            if result > max_int:
                return 0
            x //= 10
        result *= flag
        if result < min_int:
            return 0
        return result
```

<br>


### Go
```go
func ReverseInt(n int) int {
	var result, intMin, intMax int64
	for i := 0; i < 31; i++ {
		intMax = (intMax << 1) | 1
	}
	intMin = -1 * (1 << 31)
	for ; n != 0; n /= 10 {
		result = result*10 + int64(n%10)
		if result > intMax || result < intMin {
			return 0
		}
	}

	return int(result)
}
```
