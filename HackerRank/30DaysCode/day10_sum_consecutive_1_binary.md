# <center>Day 10: Binary Numbers - Consecutive 1's in Binary Numbers (E)</center> 



<br></br>

* Tag: Bit
* Difficulty: Easy
* Author: Jinghua Zhu jhzhu@outlook.com
* Link: https://www.hackerrank.com/challenges/linkedin-practice-binary-numbers/problem

<br></br>



## Description
----
Given a base-10 integer n, finds the base-10 integer denoting the maximum number of consecutive 1's in n's binary representation.

<br></br>



## Example
----
1. Input - 5 Output - 1
 2. Input - 13 Output - 2

<br></br>



## Solution
----
### Go
```go
func SumConsecutive1Binary(x uint64) int {
	count, max := 0, 0
	for ; x != 0; x >>= 1 {
		if x&1 != 0 {
			count++
		} else {
			count = 0
		}
		if max < count {
			max = count
		}
	}

	return max
}
```

<br>


### Java
```java
public class SumConsecutive1Binary {
	public int solution(int n) {
		int result = 0, count = 0;
        for (; n != 0; n >>= 1) {
            if ((n & 1) == 1)
                count++;
            else
                count = 0;
            result = Math.max(count, result);
        }

        return result;
	}
}
```

<br>


### Python
```python
def sum_consecutive_1_binary(n):
    count, tmp = 0, 0
    while n != 0:
        if (n & 1) == 1:
            tmp += 1
        else:
            tmp = 0
        count = max(count, tmp)
        n >>= 1

    return count
```