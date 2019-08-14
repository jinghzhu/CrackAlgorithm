# <center>509 - Fibonacci Number (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Recursion
* Difficulty: Easy
* Link: https://leetcode.com/problems/fibonacci-number/

<br></br>



## Description
----
The Fibonacci numbers, commonly denoted F(n) form a sequence, called the Fibonacci sequence, such that each number is the sum of the two preceding ones, starting from 0 and 1. That is,
- F(0) = 0,   F(1) = 1
- F(N) = F(N - 1) + F(N - 2), for N > 1.

<br></br>



## Example
----
Input: 3
Output: 2
Explanation: F(3) = F(2) + F(1) = 1 + 1 = 2.

<br></br>



## Solution
----
### Python
```python
from functools import lru_cache

class Solution:
    @lru_cache(maxsize=8)
    def fib(self, n: int) -> int:
        if n < 2:
            return n
        return self.fib(n - 1) + self.fib(n - 2)
```