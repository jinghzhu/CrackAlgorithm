# <center>111 - Climb Stairs (E)</center> 



<br></br>

* Tag: DP
* Company: HP, eBay, Amazon
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/climbing-stairs/description

<br></br>



## Description
----
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

<br></br>



## Example
----
Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

<br></br>



## Solution
----
https://www.jiuzhang.com/solution/climbing-stairs/

考虑最后一步走1阶还是走2阶。方案数Dp[n] = 最后一步走1阶的方案数 + 最后一步走2阶的方案数。因为无轮前者还是后者，最后走到n阶，都只需再走1步或2步，即都没有新的可能，所以可最终确定。

<br>


### Java
```java
public class ClimbStairs {
	int[] result = null;

    void f(int X) {
         if (result[X] != -1) return;                                                 
         if (X == 0 || X == 1) {
            result[X] = 1;
            return;
         }
         
         f(X - 1);
         f(X - 2);
         result[X] = result[X - 1] + result[X - 2];
    }

    public int climbStairs(int n) {
        if (n == 0)
            return 0;
        
        result  = new int[n + 1]; // n + 1 is important.
        for (int i = 0; i <= n; ++i)
            result[i] = -1;
        f(n);
        
        return result[n];
    }
}
```

<br>


### Python
```python
class ClimbStairs:
    result = []

    """
    @param n: An integer
    @return: An integer
    """
    def solution(self, n):
        if n < 2:
            return n
        self.result = [0] * (n + 1)  # n + 1 is important.
        self.f(n)
        return self.result[n]

    def f(self, n):
        if self.result[n] != 0:
            return
        if n == 0 or n == 1:
            self.result[n] = 1
            return
        self.f(n - 1)
        self.f(n - 2)
        self.result[n] = self.result[n - 1] + self.result[n - 2]
```

<br>


### Go
```go
var resultCS []int

func ClimbStairs(n int) int {
	if n < 1 {
		return 0
	}
	resultCS = make([]int, n+1, n+1)
	helperCS(n)

	return resultCS[n]
}

func helperCS(n int) {
	if resultCS[n] != 0 {
		return
	}
	if n < 2 {
		resultCS[n] = 1
		return
	}
	helperCS(n - 1)
	helperCS(n - 2)
	resultCS[n] = resultCS[n-1] + resultCS[n-2]
}
```