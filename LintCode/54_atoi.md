# <center>8 - Atoi (H)</center> 



<br></br>

* Difficulty: Hard
* Company: Uber, Amazon, Microsoft
* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Link: https://leetcode.com/problems/string-to-integer-atoi/

<br></br>



## Description
----
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

<br></br>



## Example
----
1. Input: "42" Output: 42
2. Input: "   -42" Output: -42
3. Input: "4193 with words" Output: 4193
4. Input: "words and 987" Output: 0
5. Input: "-91283472332" Output: -2147483648

<br></br>



## Solution
----
### Go
```go
func Atoi(str string) int {
    str = strings.TrimSpace(str)
	l := len(str)
	if l == 0 {
		return 0
	}

	intMax, intMin, sign, index := 1, 1, 1, 0
	for i := 0; i < 31; i++ {
		intMax <<= 1
	}
	intMax, intMin = intMax-1, (-1)*intMax
	if str[index] == '-' {
		sign = -1
		index++
	} else if str[index] == '+' {
		index++
	}

	var result int64
	for ; index < l; index++ {
		if str[index] < '0' || str[index] > '9' {
			break
		}
		result = 10*result + int64(str[index]-'0')
		if result > int64(intMax) {
			break
		}
	}
	result = result * int64(sign)
	if result > int64(intMax) {
		return int(intMax)
	}
	if result < int64(intMin) {
		return int(intMin)
	}

	return int(result)
}
```

<br>


### Python
```python
class Atoi:
    def solution(self, s: str) -> int:
        s = s.strip(" ")
        l = len(s)
        if l == 0:
            return 0
        i = 1
        for j in range(31):
            i <<= 1
        max_int, min_int = i - 1, i * (-1)
        i, sign, result = 0, 1, 0
        if s[i] == '-':
            sign = -1
            i += 1
        elif s[i] == '+':
            i += 1
        while i < l:
            if s[i] < '0' or s[i] > '9':
                break
            result = result * 10 + int(s[i])
            if result > max_int:
                break
            i += 1
        result *= sign
        if result > max_int:
            return max_int
        if result < min_int:
            return min_int
        return result
```

<br>


### Java
```java
public class Atoi {
	/**
     * @param str: A string
     * @return: An integer
     */
	public int solution(String str) {
		if(str == null)
            return 0;
		
        str = str.trim();
        if (str.length() == 0)
            return 0;
            
        int sign = 1, index = 0;
    
        if (str.charAt(index) == '+') // important
            index++;
        else if (str.charAt(index) == '-') {
        	sign = -1;
            index++;
        }
            
        long num = 0;
        for (; index < str.length(); index++) {
            if (str.charAt(index) < '0' || str.charAt(index) > '9')
                break;
            num = num * 10 + (str.charAt(index) - '0');
            if (num > Integer.MAX_VALUE )
                break;
        }   
        if (num * sign >= Integer.MAX_VALUE)
            return Integer.MAX_VALUE;
        if (num * sign <= Integer.MIN_VALUE)
            return Integer.MIN_VALUE;
        
        return (int)num * sign;
	}
}
```