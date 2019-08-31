# <center>392 - House Robber (M)</center> 



<br></br>

* Tag: DP
* Company: Linkedin, Airbnb
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/house-robber/description

<br></br>



## Description
----
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.

<br></br>



## Example
----
1. Input: [1,2,3,1] Output: 4
2. Input: [2,7,9,3,1] Output: 12

<br></br>



## Solution
----
https://www.jiuzhang.com/solution/house-robber

设dp[i]表示前i家房子最多收益，答案是 dp[n]，状态转移方程是
dp[i] = max(dp[i-1], dp[i-2] + A[i-1])
考虑dp[i]计算只涉及dp[i-1]和dp[i-2]，可O(1)空间解决。

<br>


### Java
```java
public class HouseRobber1 {
	// Time: O(n)
	public long solution1(int[] A) {
		if (A == null || A.length == 0)
            return 0;
        int n = A.length;
        long []res = new long[n+1];
        
        res[0] = 0;
        res[1] = A[0];
        for (int i = 2; i <= n; i++) // i = 2 and i <= n are important.
            res[i] = Math.max(res[i - 1], res[ i- 2] + A[i - 1]);
            
        return res[n];
    }
	
	// Time: O(1)
	public long solution2(int[] A) {
        if (A == null || A.length == 0)
            return 0;
        int n = A.length;
        long []res = new long[2];
        
        res[0] = 0;
        res[1] = A[0];
        for (int i = 2; i <= n; i++) // i = 2 and i <= n are important.
            res[i % 2] = Math.max(res[(i - 1) % 2], res[(i - 2) % 2] + A[i - 1]);
            
        return res[n % 2];
    }    
}
```

<br>


### Python
```python
class Rob1:
    # Time: O(n)
    def solution1(self, A) -> int:
        if not A:
            return 0
        if len(A) <= 2:
            return max(A)

        f = [0] * len(A)
        f[0], f[1] = A[0], max(A[0], A[1])

        for i in range(2, len(A)):
            f[i] = max(f[i - 1], f[i - 2] + A[i])

        return f[len(A) - 1]

    # Time: O(1)
    def solution2(self, A) -> int:
        if not A:
            return 0
        l = len(A)
        if l <= 2:
            return max(A)
        cache = [0, A[0]]

        for i in range(2, l + 1):
            cache[i % 2] = max(cache[(i - 1) % 2], cache[(i - 2) % 2] + A[i - 1])
        return cache[l % 2]
```

<br>


### Go
```go
func Rob11(nums []int) int {
	l := len(nums)
	if l < 1 {
		return 0
	}
	cache := [2]int{0, nums[0]}
	for i := 2; i <= l; i++ {
		cache[i%2] = mathMaxInt(cache[(i-1)%2], cache[(i-2)%2]+nums[i-1])
	}

	return cache[l%2]
}
```

```go
func Rob12(nums []int) int {
	l := len(nums)
	if l < 1 {
		return 0
	}
	cache := make([]int, l+1, l+1)
	cache[0], cache[1] = 0, nums[0]
	for i := 2; i <= l; i++ {
		cache[i] = mathMaxInt(cache[i-1], cache[i-2]+nums[i-1])
	}

	return cache[l]
}
```