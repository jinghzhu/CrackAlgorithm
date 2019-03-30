# <center>67 - Add Binary</center> 


* Tag: String
* Author: Jinghua Zhu jhzhu@outlook.com

https://www.lintcode.com/problem/add-binary/description

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
 
func addBinary (a, b string) string {
    lenA, lenB := len(a), len(b)
    i, j := lenA - 1, lenB - 1
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
        result = fmt.Sprintf("%d", tmp % 2) + result
        i--
        j--
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
        result = fmt.Sprintf("%d", tmp % 2) + result
    }
    for ; j >= 0; j-- {
        tmp := 0
        if flag {
            tmp++
            flag = false
        }
        if b[j] == '1' {
            tmp++
        }
        if tmp > 1 {
            flag = true
        }
        result = fmt.Sprintf("%d", tmp % 2) + result
    }
    
    if result == "" {
        return "0"
    } else if flag {
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
        if (a == null || b == null || (a.length() == 0 && b.length()== 0))
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
        for (; i >= 0; i--) {
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
        for (; j >= 0; j--) {
            int tmp = 0;
            if (flag) {
                tmp++;
                flag = false;
            }
            if (b.charAt(j) == '1')
                tmp++;
            if (tmp > 1)
                flag = true;
            tmp %= 2;
            result = String.valueOf(tmp) + result;
        }
        if (flag) {
            result = "1" + result;
        } else if (result.equals("")) {
            return "0";
        }
        
        return result;
    }
}
```