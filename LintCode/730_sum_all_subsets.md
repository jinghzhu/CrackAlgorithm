# <center>730 - Sum of All Subsets (E)</center> 



<br></br>

* Tag: Math
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/sum-of-all-subsets/description

<br></br>



## Description
----
Given a number n, we need to find the sum of all the elements from all possible subsets of a set formed by first n natural numbers.

<br></br>



## Example
----
Input: n = 2

Output: 6

Explanation:  Possible subsets are {{1}, {2}, {1, 2}}. Sum of elements in subsets is 1 + 2 + 1 + 2 = 6

<br></br>



## Solution
----
通过找规律可以发现，每个数出现2^(n - 1)次。

<br>


### Python
```python
class Solution:
    """
    @param n: the given number
    @return: Sum of elements in subsets
    """
    def subSum(self, n):
        # write your code here
        sum = 0
        for i in range(1, n + 1):
            sum += i
        return sum * (int)(math.pow(2, n - 1))
```

<br>
