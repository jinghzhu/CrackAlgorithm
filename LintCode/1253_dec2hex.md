# <center>1253 - Convert a Number to Hexadecimal (E)</center> 



<br></br>

* Tag: Math, Bit
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/convert-a-number-to-hexadecimal/description

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
	
	/*
	 *  整型数在计算机内部是二进制形式存储，那么不管的是正数还是负数，其实计算机已有了完整的存储，
	 *  那么其实不需要再去计算反码补码，只需对其进行2进制转换成16进制就可以了。
	 *  
	 *  因为二进制一位表示的是2,16进制一位表示的是16，那么16进制下一位表示的是2进制下的4位(16 = 2^4)。
	 *  在转换过程中可以每四位转换成一位，这里有个小技巧：用到位运算&和>>来提高运行速度，&15相当于%16
	 */
	public String solution2(int num) {
		if (num == 0)
			return "0";
		
		String ans = "";
		int len = 0;
		while (num != 0 && len < 8) {
			int bit = num & 15;
			if (bit < 10)
				ans = (char)(bit + '0') + ans;
			else
				ans = (char)(bit - 10 + 'a') + ans;
			num >>= 4;
			len++;
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

```go
func Dec2Hex2(num int) string {
	if num == 0 {
		return "0"
	}
	s := ""
	for i := 0; num != 0 && i < 8; i++ {
		tmp := num & 15
		if tmp < 10 {
			s = string('0'+tmp) + s
		} else {
			s = string('a'+tmp-10) + s
		}
		num >>= 4
	}

	return s
}
```

<br>


### Python
```python
class Solution:
    def toHex1(self, num: int) -> str:
        if num == 0:
            return '0'
        data = ['0','1','2','3','4','5','6','7','8','9','a','b','c','d','e','f']
        result = ""
        for i in range(8):
            result = data[num & 15] + result
            num >>= 4
        return result.lstrip("0")

	def toHex2(self, num: int) -> str:
        if num == 0:
            return "0"
        s = ""
        for i in range(8):
            if num == 0:
                break
            tmp = num & 15
            if tmp < 10:
                s = str(tmp) + s
            else:
                s = chr(tmp - 10 + ord('a')) + s
            num >>= 4
        
        return s
```