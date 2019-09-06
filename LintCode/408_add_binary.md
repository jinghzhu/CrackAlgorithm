# <center>67 - Add Binary (E)</center> 



<br></br>

* Difficulty: Easy
* Company: Facebook
* Tag: String, Matn
* Author: Jinghua Zhu jhzhu@outlook.com
* Link: https://www.lintcode.com/problem/add-binary/description

<br></br>



## Description
----
Given two binary strings, return their sum (also a binary string). The input strings are both non-empty and contains only characters 1 or 0.

<br></br>



## Example
----
1. Input: a = "11", b = "1"; Output: "100"
2. Input: a = "1010", b = "1011"; Output: "10101"

<br></br>



## Solution
----
### Go
```go
import "fmt"
 
func AddBinary(a, b string) string {
	i, j := len(a)-1, len(b)-1
	flag := false
	result := ""
	for i >= 0 && j >= 0 {
		tmp := 0
		if flag {
			tmp++
			flag = false
		}
		if a[i] == '1' {
			tmp++
		}
		if b[j] == '1' {
			tmp++
		}
		if tmp > 1 {
			flag = true
		}
		result = fmt.Sprintf("%d", tmp%2) + result
		i--
		j--
	}
	if result == "" { // Important. Image a = b = 0
		result = "0"
	}

	if j >= 0 {
		i = j
		a = b
	}
	for ; i >= 0; i-- {
		tmp := 0
		if flag {
			tmp++
			flag = false
		}
		if a[i] == '1' {
			tmp++
		}
		if tmp > 1 {
			flag = true
		}
		result = fmt.Sprintf("%d", tmp%2) + result
	}
	if flag { // Important. Image a = b = 1.
		result = "1" + result
	}

	return result
}
```

<br>


### Java
```java
public class AddBinary {
	public String addBinary(String a, String b) {
        if (a == null || b == null)
             return "0";
        
        int i = a.length() - 1, j = b.length() - 1;
        boolean flag = false;
        String result = "";
        while (i >= 0 && j >= 0) {
            int tmp = 0;
            if (flag) {
                tmp++;
                flag = false;
            }
            if (a.charAt(i) == '1')
                tmp++;
            if (b.charAt(j) == '1')
                tmp++;
            if (tmp > 1)
                flag = true;
            tmp %= 2;
            result = String.valueOf(tmp) + result;
            i--;
            j--;
        }
        if (result.equals("")) // Important.
            return "0";
        if (j >= 0) {
        	i = j;
        	a = b;
        }
        for (; i >= 0; i--) { // Important.
            int tmp = 0;
            if (flag) {
                tmp++;
                flag = false;
            }
            if (a.charAt(i) == '1')
                tmp++;
            if (tmp > 1)
                flag = true;
            tmp %= 2;
            result = String.valueOf(tmp) + result;
        }
        if (flag) // Important
            result = "1" + result;
        
        return result;
    }
}
```

<br>


### Python
```python
class AddBinary:
    def solution(self, a: str, b: str) -> str:
        result = ""
        flag = False
        i, j = len(a) - 1, len(b) - 1
        while i >= 0 and j >= 0:
            tmp = 0
            if a[i] == '1':
                tmp += 1
            if b[j] == '1':
                tmp += 1
            if flag:
                tmp += 1
                flag = False
            if tmp > 1:
                flag = True
                tmp %= 2
            result = str(tmp) + result
            i, j = i - 1, j - 1
        if result == "":  # Important
            return "0"
        if j >= 0:
            i = j
            a = b
        while i >= 0:  # Important
            tmp = 0
            if flag:
                tmp = 1
                flag = False
            if a[i] == '1':
                tmp += 1
            if tmp > 1:
                flag = True
                tmp %= 2
            result = str(tmp) + result
            i -= 1
        if flag:  # Important
            result = "1" + result
        return result
```