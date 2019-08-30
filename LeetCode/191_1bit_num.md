# <center>191 - Reverse Bits (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Bit
* Company: Apple, Microsoft
* Difficulty: Easy
* Link: https://leetcode.com/problems/number-of-1-bits/

<br></br>



## Description
----
Write a function that takes an unsigned integer and return the number of '1' bits it has.

<br></br>



## Example
----
Input: 00000000000000000000000000001011
Output: 3

<br></br>



## Solution
----
### Go
```go
func hammingWeight(num uint32) int {
    count := 0
    for num > 0 {
        if (num & 1) == 1 {
            count++
        }
        num >>= 1
    }
    return count
}
```


### Java
```java
public int hammingWeight(int n) {
    int sum = 0;
    while (n != 0) {
        sum++;
        n &= (n - 1);
    }
    return sum;
}
```

<br>


### Python
```python
class Solution:
    """
    @param n: an unsigned integer
    @return: the number of â1' bits
    """
    def hammingWeight(self, n):
        ones = 0
        while n != 0:
            ones += (n & 1)
            n = n >> 1
        return ones
```