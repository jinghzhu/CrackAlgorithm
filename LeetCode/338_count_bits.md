# <center>338 - Count Bits (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Bit, DP
* Difficulty: Medium
* Link: https://leetcode.com/problems/counting-bits/

<br></br>



## Description
----
Given a non negative integer number num. For every numbers i in the range 0 ≤ i ≤ num calculate the number of 1's in their binary representation and return them as an array.

<br></br>



## Example
----
Input: 5

Output: [0,1,1,2,1,2]

The binary representation of 0~5 is:
```
000
001
010
011
100
101
```

The count of "1" in each number is: 0,1,1,2,1,2

<br></br>



## Solution
----
对任意一个数n，设最接近n的且为2的指数倍的值为m，容易发现其1的个数等于1 + (n - m)的1的个数。

举例，n为12，则m应该为8，n - m = 4，即n的1的个数= 1（8只有1个1）+ 1（4只有1个1）。

或者换个思路理解，n的1的个数 = m的1的个数 + (n ^ m)后1的个数。

<br>


### Go
```go
func countBits (num int) []int {
    result := make([]int, num + 1, num + 1)
    result[0] = 0
    last := 1
    for i := 1; i <= num; i++ {
        if i == 1 {
            result[1] = 1
        } else if last * 2 == i {
            result[i] = 1
            last = i
        } else {
            result[i] = 1 + result[i - last]
        }
    }
    
    return result
}
```

<br>


### Java
```java
public class CountBits {
	public int[] solution(int num) {
        int[] f = new int[num+1];
        int i;
        f[0] = 0;
        for (i=1; i<=num; ++i)
            f[i] = f[i&(i-1)] + 1;
        
        return f;
    }
}
```