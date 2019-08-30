# <center>171 - Excel Sheet Column Number (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Math
* Difficulty: Easy
* Company: Uber, Microsoft
* Link: https://leetcode.com/problems/excel-sheet-column-number/

<br></br>



## Description
----
Given a column title as appear in an Excel sheet, return its corresponding column number.

<br></br>



## Example
----
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...

<br></br>



## Solution
----
### Java
```java
public class ExcelTitle2Num {
	/**
     * @param s: a string
     * @return: return a integer
     */
    public int titleToNumber(String s) {
        int res = 0;
        for (int i = 0; i < s.length() ; i++)
            res += Math.pow (26, s.length() - 1 - i) * (s.charAt (i) - 64);

        return res;
    }
}
```

<br>


### Python
```python
class ExcelTitle2Num:
    def solution(self, s: str) -> int:
        s = s[::-1]
        sum = 0
        for exp, char in enumerate(s):
            sum += (ord(char) - 65 + 1) * (26 ** exp)
        return sum
```

<br>


### Go
```go
func TitleToNumber(s string) int {
	s = strings.ToUpper(s)
	result, tmp := 0, 1
	for i := len(s) - 1; i >= 0; i-- {
		if s[i] < 'A' || s[i] > 'Z' {
			return 0
		}
		result += (int(s[i]-'A') + 1) * tmp
		tmp *= 26
	}

	return result
}
```
