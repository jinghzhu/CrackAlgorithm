# <center>1347 - Factorial Trailing Zeroes (E)</center> 



<br></br>

* Tag: Math, Recursion
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/factorial-trailing-zeroes/description

<br></br>



## Description
----
Given an integer n, return the number of trailing zeroes in n!.

<br></br>



## Example
----
1. Input: n = 5 Output: 1 1*2*3*4*5=120
2. Input: n = 10 Output: 2 1*2*3*4*5*6*7*8*9*10=3628800

<br></br>



## Solution
----
https://www.jiuzhang.com/solution/factorial-trailing-zeroes/#tag-highlight-lang-python

> 最终尾随零数量和质因子中2和5数量有关，易想到5数量比2少，所以只需算出n!质因子5数量即可。
> 考虑1 ~ n!间5的倍数，25的倍数，125的倍数，625的倍数.....可算出答案。

<br>


### Java
```java
public class FactorialTrailingZeroes {
	public int trailingZeroes (int n) {
        if (n >= 5)
            return n / 5 + trailingZeroes (n / 5);
        return 0;
    }
}
```

<br>


### Python
```python
class FactorialTrailingZeroes:
    """
    @param n: a integer
    @return: return a integer
    """
    def solution(self, n):
        return 0 if n == 0 else n // 5 + self.trailingZeroes(n // 5)
```

<br>


### Go
```go
func TrailingZeroes(n int) int {
	if n >= 5 {
		return n/5 + TrailingZeroes(n/5)
	}
	return 0
}
```
