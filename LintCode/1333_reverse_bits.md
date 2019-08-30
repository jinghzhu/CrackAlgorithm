# <center>133 - Reverse Bits (E)</center> 



<br></br>

* Tag: Bit
* Company: Apple, Airbnb, Amazon
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/reverse-bits/description

<br></br>



## Description
----
Reverse bits of a given 32 bits unsigned integer.

<br></br>



## Example
----
```
Input: 00000010100101000001111010011100
Output: 00111001011110000010100101000000
Explanation: 
The input binary string 00000010100101000001111010011100 represents the unsigned integer 43261596
so return 964176192 which its binary representation is 00111001011110000010100101000000.
```

```
Input: 11111111111111111111111111111101
Output: 10111111111111111111111111111111
Explanation: 
The input binary string 11111111111111111111111111111101 represents the unsigned integer 4294967293
so return 3221225471 which its binary representation is 10101111110010110010011101101001.
```

<br></br>



## Solution
----
### Go
```go
func ReverseBits(n uint32) uint32 {
	var result uint32
	for i := 0; i < 32; i++ {
		result = (result << 1) | (n & 1)
		n >>= 1
	}

	return result
}
```


### Java
```java
public class ReverseBits {
	// treat n as an unsigned value
    public long reverseBits(int n) {
        int reversed = 0;
        for (int i = 0; i < 32; i++) {
            reversed = (reversed << 1) | (n & 1);
            n = (n >> 1);
        }
        
        return reversed;
    }
}
```

<br>


### Python
```python
class ReverseBits:
    """
    @param n: an integer
    @return: return an integer
    """
    def solution(self, n):
        res = 0
        for i in range(32):
            res = res << 1
            res += n & 1
            n = n >> 1
        return res
```