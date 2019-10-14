# <center>55 - Jump Game I (M)</center> 



<br></br>

* Tag: DP
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Microsoft
* Link: https://leetcode.com/problems/jump-game/

<br></br>



## Description
----
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index. 

<br></br>



## Example
----
1. Input : [2,3,1,1,4] Output : true
2. Input : [3,2,1,0,4] Output : false

<br></br>



## Solution
----
- 状态定义：f[n]表示能否跳到石头n。
- 转移方程：f[n] = true，如果在[0, n - 1]区间至少存在一个i满足f[i] == ture且i + f[i] >= n。
- 初始化：f[0] = 0
- 计算方向：从小到大。
- Space: O(n^2) Time: O(n)

<br>


### Java
```java
public class JumpGame1 {
    public boolean solution(int[] A) {
        if (A == null || A.length < 1)
            return false;
        
        boolean[] f = new boolean[A.length];
        f[0] = true;
        for (int i = 1; i < A.length; i++) {
            for (int j = 0; j < i; j++) { // 枚举之前的石头。
                if (f[j] && A[j] + j >= i) {
                    f[i] = true;
                    break;
                }
            }
        }
        
        return f[A.length - 1];
    }
}
```

<br>


### Python
```python
class Solution:
    """
    @param A: A list of integers
    @return: A boolean
    """
    def solution(self, nums):
        if not nums:
            return False

        l = len(nums)
        f = [False] * l
        f[0] = True
        for i in range(1, l):
            for j in range(i):  # 枚举之前的石头。
                if f[j] and nums[j] + j >= i:
                    f[i] = True
                    break

        return f[l - 1]
```

<br>


### Go
```go
func CanJump1(nums []int) bool {
	l := len(nums)
	if l < 1 {
		return false
	}

	f := make([]bool, l, l)
	f[0] = true
	for i := 1; i < l; i++ {
		for j := 0; j < i; j++ {
			if f[j] && nums[j]+j >= i {
				f[i] = true
				break
			}
		}
	}

	return f[l-1]
}
```
