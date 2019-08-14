# <center>366 - Fibonacci (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/fibonacci/description

<br></br>



## Description
----
Find the Nth number in Fibonacci sequence. A Fibonacci sequence is defined as follow:
- The first two numbers are 0 and 1.
- The i th number is the sum of i-1 th number and i-2 th number.
- The first ten numbers in Fibonacci sequence is:
```
0, 1, 1, 2, 3, 5, 8, 13, 21, 34 ...
```

<br></br>



## Example
----
Input:  1 Output: 0
The first number in  Fibonacci sequence .

Input:  2 Output: 1
The second number in  Fibonacci sequence .

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param n: an integer
     * @return: an ineger f(n)
     */
    public int fibonacci(int n) {
        int a = 0, b = 1;
        for (int i = 0; i < n - 1; i++) {
            int c = a + b;
            a = b;
            b = c;
        }
        return a;
    }
}
```

<br>


### Go
```go
func fibonacci (n int) int {
    a, b := 0, 1
    for i := 0; i < n - 1; i++ {
        a, b = b, a + b
    }
    
    return a
}
```

<br>


### Python
```python
class Solution:
    """
    @param n: an integer
    @return: an ineger f(n)
    """
    def fibonacci(self, n):
        a, b = 0, 1
        for i in range(n - 1):
            a, b = b, a + b
        return a
```
