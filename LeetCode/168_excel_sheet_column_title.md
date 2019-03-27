# <center>168 - Excel Sheet Column Title</center> 


* Tag: String, Math
* Author: Jinghua Zhu jhzhu@outlook.com

https://leetcode.com/problems/excel-sheet-column-title/

<br></br>



## Description
----
Given a positive integer, return its corresponding column title as appear in an Excel sheet.

<br></br>



## Example
----
1 -> A
2 -> B
3 -> C
...
26 -> Z
27 -> AA
28 -> AB
...

Input: 1  	Output: "A"
Input: 28 	Output: "AB"
Input: 701	Output: "ZY"

<br></br>



## Solution
----
### Go
```go
func ConvertExcelTitle(n int) string {
	str := ""
	if n < 1 {
		return str
	}
	for n != 0 {
		n-- // Important. Image such case that n = 26, what will happen for n/= 26?
		i := n % 26
		str = string(65+i) + str
		n /= 26
	}

	return str
}
```


### Java
```java
public class ExcelSheetColumnTitle {
    public String solution1(int n) {
    	String str = "";
    	if (n < 1)
    		return str;
    	String[] letters = new String[]{"A", "B", "C", "D", "E", "F", "G", "H", "I", "J", "K", "L", "M", "N", "O", "P", "Q", "R", "S", "T", "U", "V", "W", "X", "Y", "Z"};
        while (n != 0) {
            n--; // Important. Image case for n = 26, n/26 = 1.
            int index = n % 26;
            str = letters[index] + str;
            n /= 26;
        }
        
        return str;
    }
    
    public String solution2(int n) {
        StringBuilder str = new StringBuilder();
        while (n > 0) {
            n--;
            str.append ( (char) ( (n % 26) + 'A'));
            n /= 26;
        }
        
        return str.reverse().toString();
    }
}

```