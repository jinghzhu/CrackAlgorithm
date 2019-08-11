# <center>405 - Convert a Number to Hexadecimal (E)</center> 



<br></br>

* Tag: Math, Bit
* Difficulty: Easy
* Link: https://leetcode.com/problems/convert-a-number-to-hexadecimal/

<br></br>



## Description
----
Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

Note:
1. All letters in hexadecimal (a-f) must be in lowercase.
2. The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
3. The given number is guaranteed to fit within the range of a 32-bit signed integer.
4. You must not use any method provided by the library which converts/formats the number to hex directly.

<br></br>



## Example
----
Input: 26 Output: "1a"
Input: -1 Output: "ffffffff"

<br></br>



## Solution
----
### Java
```java
public class Dec2Hex {
	/**
     * @param num: an integer
     * @return: convert the integer to hexadecimal
     */
	public String solution1(int num) {
		if (num == 0) 
            return "0";
		
        char[] refer = {'0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f'};
        String result = "";
        for (int i = 0; i < 8; i++) {
            result = refer[num & 15] + result;
            num >>>= 4;
        }
        
        return result.replaceFirst("^0*", "");
	}
	
	// 负数的存储方式是补码的形式，负数的补码等于其正数反码+1
	// 2的反码表示形式就是1101，所以-2的表示形式就是其反码+1，是1110
	public String solution2(int num) {
		if (num == 0)
			return "0";
		
		long n = num > 0 ? num : -num;
		int[] bit = new int[10];
		int len = 0, leader0 = 1; // leader0 is to local first 0 in hex expression
		String ans = "";
		for (int v : bit)
			v = 0;
		
		while (n > 0) {
			bit[len++] = (int)n % 16;
			n /= 16;
		}
		
		if (num < 0) { // translate its true form into complement form 
			for (int i = 0; i < 8; i++)
				bit[i] = 15 - bit[i];
			int pos = 0;
			while (bit[pos] == 15) 
				bit[pos++] = 0;
			bit[pos]++;
		}
		
		for (int i = 7; i >= 0; i--) {
			if (bit[i] != 0)
				leader0 = 0;
			if (leader0 == 1)
				continue;
			if (bit[i] < 10)
				ans += (char)('0' + bit[i]);
			else
				ans += (char)('a' + bit[i] - 10);
		}
		
		return ans;
	}	
}
```

<br>


### Go
```go
func Dec2Hex(num int) string {
	if num == 0 {
		return "0"
	}

	var s string
	refer := []string{"0", "1", "2", "3", "4", "5", "6", "7", "8", "9", "a", "b", "c", "d", "e", "f"}
	for i := 0; i < 8; i++ {
		s = refer[num&15] + s
		num >>= 4
	}

	return strings.TrimLeft(s, "0")
}
```

<br>


### Python
```python
class Solution:
    def toHex(self, num: int) -> str:
        if num == 0:
            return '0'
        data = ['0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f']
        result = ""
        for i in range(8):
            result = data[num & 15] + result
            num >>= 4
        return result.lstrip("0")
```