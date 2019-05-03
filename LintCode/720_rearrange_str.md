# <center>720 - Rearrange a String With Integers</center> 


* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Facebook
* Link: https://www.lintcode.com/problem/rearrange-a-string-with-integers/description

<br></br>



## Description
----
Given a string containing uppercase alphabets and integer digits (from 0 to 9), write a function to return the alphabets in the order followed by the sum of digits.

<br></br>



## Example
----
Input : str = "AC2BEW3"
Output : "ABCEW5"

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param str: a string containing uppercase alphabets and integer digits
     * @return: the alphabets in the order followed by the sum of digits
     */
    public String rearrange(String str) {
        if (str == null)
            return str;
        
        Stack<Integer> st = new Stack<Integer>();
        int[] count = new int[26];
        
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);
            if (ch >= 'A' && ch <= 'Z') {
                count[ch - 'A'] += 1;
            } else if (ch >= '0' && ch <= '9') {
                int data = ch - '0';
                if (st.isEmpty())
                    st.push(data);
                else
                    st.push(st.pop() + data);
            } else {
                return str;
            }
            
        }
        
        if (st.isEmpty())
            return str;
            
        String s = "";
        for (int i = 0; i < count.length; i++) {
            char ch = (char) ('A' + i);
            String temp = String.valueOf(ch);
            for (int j = 0; j < count[i]; j++)
                s += temp;
        }
        
        return s + String.valueOf(st.pop());
    }
}
```

<br>


## Go
```go
import "fmt"

func rearrange (str string) string {
    data := [26]int{}
    var sum int32 = -1 // error if sum := 1
    for _, v := range str {
        if v >= 'A' && v <= 'Z' {
            data[v - 'A'] += 1
        } else if v >= '0' && v <= '9' {
            if sum == -1 {
                sum = v - '0'
            } else {
                sum += v - '0'
            }
        } else {
            return str
        }
    }
    
    if sum == -1 {
        return str
    }
    s := ""
    for k, v := range data {
        temp := string(k + 'A')
        for i := 0; i < v; i++ {
            s += temp
        }
    }
    
    return s + fmt.Sprintf("%d", sum)
}
```

<br>


## Python
```python
class Solution:
    """
    @param str: a string containing uppercase alphabets and integer digits
    @return: the alphabets in the order followed by the sum of digits
    """
    def rearrange(self, s0):
        sum = -1
        l = [0] * 26
        for ch in s0:
            if ch >= 'A' and ch <= 'Z':
                l[ord(ch) - ord('A')] += 1
            elif ch >= '0' and ch <= '9':
                if sum == -1:
                    sum = int(ch)
                else:
                    sum += int(ch)
            else:
                return s0
        
        if sum == -1:
            return s0
            
        s = ""
        for i in range(26):
            temp = chr(ord('A') + i)
            for j in range(l[i]):
                s += temp
        
        return s + str(sum)   
```
