# <center>91 - Decode Ways (M)</center> 



<br></br>

* Tag: DP
* Difficulty: Medium
* Company: Microsoft, Facebook, Uber, Google
* Link: https://leetcode.com/problems/decode-ways/

<br></br>



## Description
----
A message containing letters from A-Z is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

Given a non-empty string containing only digits, determine the total number of ways to decode it.

<br></br>



## Example
----
Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).

Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

<br></br>



## Solution
----
设定状态：`f[i]`表示字符串前i位有多少种解码方案。

状态转移方程:
1. 初始化f数组为0。
2. 若`s[i]`表示的阿拉伯数字在1~9, `f[i] += f[i-1]`
3. 若`s[i-1]`和`s[i]`连起来表示的数字在10~26, `f[i] += f[i-2]` (若 `i==1`，则`f[i] += 1`)

边界: `f[0] = 1`

特判:
1. 如果字符串以0开头，则返回0。
2. 如果`f[i] == 0`，则说明此处无法解码，直接返回0.

<br>


### Java
```java
public class DecodeWays {
	public int solution(String s) {
        if (s == null || s.length() == 0)
            return 0;
        
        char[] sc = s.toCharArray();
        int[] f = new int[s.length() + 1];
        f[0] = 1;
        f[1] = sc[0] != '0' ? 1 : 0;
        for (int i = 2; i <= s.length(); i++) {
            if (sc[i -1] != '0')
                f[i] = f[i - 1];
            int tmp = (sc[i - 2] - '0') * 10 + sc[i - 1] - '0';
            if (tmp >= 10 && tmp <= 26)
                f[i] += f[i - 2];
        }
        
        return f[s.length()];
    }
}
```

<br>


### Python
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        if s is None or len(s) < 1:
            return 0
        
        l, base = len(s), ord('0')
        f = [0] * (l + 1)
        f[0] = 1
        if s[0] != '0':
            f[1] = 1
        for i in range(2, l + 1):
            if s[i - 1] != '0':
                f[i] = f[i - 1]
            tmp = (ord(s[i - 2]) - base) * 10 + ord(s[i - 1]) - base
            if 10 <= tmp <= 26:
                f[i] += f[i - 2]
        
        return f[l]  
```

<br>


### Go
```go
func DecodeWays(s string) int {
	l := len(s)
	if l < 1 {
		return 0
	}
	f := make([]int, l+1, l+1)
	f[0], f[1] = 1, 1
	if s[0] == '0' {
		f[1] = 0
	}
	for i := 2; i <= l; i++ {
		if s[i-1] != '0' {
			f[i] = f[i-1]
		}
		tmp := (s[i-2]-'0')*10 + s[i-1] - '0'
		if tmp >= 10 && tmp <= 26 {
			f[i] += f[i-2]
		}
	}

	return f[l]
}
```
