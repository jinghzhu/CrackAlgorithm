# <center>441 - Arranging Coins (E)</center> 



<br></br>

* Tag: Math, Binary Search
* Difficulty: Easy
* Link: https://leetcode.com/problems/arranging-coins/

<br></br>



## Description
----
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.

Given n, find the total number of full staircase rows that can be formed.

<br></br>



## Example
----
Example 1:
```
n = 5 The coins can form the following rows:
¤
¤ ¤
¤ ¤

Because the 3rd row is incomplete, we return 2.
```

Example 2:
```
n = 8 The coins can form the following rows:
¤
¤ ¤
¤ ¤ ¤
¤ ¤

Because the 4th row is incomplete, we return 3.
```

<br></br>



## Solution
----
2种解法：
1. 直线思维，直接一个for循环累计，时间复杂度O(n)。
2. 利用等差数列求和与二分查找来逼近最终行数。初始化查找区间为[1, n]，每次求中值后利用等差数列求和计算如果行数为中值是是否符合。

<br>


### Java
```java
public class ArrangeCoins {
	/**
     * @param n: a non-negative integer
     * @return: the total number of full staircase rows that can be formed
     */
    public int arrangeCoins(int n) {
    	if (n <= 1) return n;
    	long low = 1, high = n;
    	while (low < high) {
    		long mid = low + (high - low) / 2;
    		if (mid * (mid + 1) / 2 <= n) // 利用等差数列求和+二分查找来逼近最终行数。
    			low = mid + 1;
    		else
    			high = mid;
    	}
    	
    	return (int) (low - 1);
    }
}
```

<br>


### Python
```python
class ArrangeCoins:
    def solution(self, n: int) -> int:
        if n < 2:
            return n
        low, high = 1, n
        while low < high:
            mid = (high + low) // 2
            if mid * (mid + 1) // 2 <= n:  # 利用等差数列求和+二分查找来逼近最终行数。
                low = mid + 1
            else:
                high = mid

        return low - 1
```

<br>


### Go
```go
func ArrangeCoins(n int) int {
	if n < 2 {
		return n
	}
	var low, high, mid int64
	low, high = 1, int64(n)
	for low < high {
		mid = low + (high-low)/2
		if mid*(mid+1)/2 <= int64(n) { // 利用等差数列求和+二分查找来逼近最终行数。
			low = mid + 1
		} else {
			high = mid
		}
	}

	return int(low + 1)
}
```
