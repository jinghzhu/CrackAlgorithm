# <center>1294 - Power of Three (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/power-of-three/description

<br></br>



## Description
----
Given an integer, write a function to determine if it is a power of three.

<br></br>



## Example
----
1. Input: n = 0 Output: False
2. Input: n = 9 Output: True


<br></br>



## Solution
----
For other solutions, please check https://leetcode.com/problems/power-of-three/solution/

<br>


### Java
```java
public class Solution {
    /**
     * @param n: an integer
     * @return: if n is a power of three
     */
    public boolean isPowerOfThree(int n) {
        if (n < 1) {
            return false;
        }

        while (n % 3 == 0) {
            n /= 3;
        }

        return n == 1;
    }
}
```
