# <center>420 - Count and Say</center> 


* Tag: String
* Company: acebook

https://www.lintcode.com/problem/count-and-say/description

<br></br>



## Description
----
The count-and-say sequence is the sequence of integers with the first five terms as following:

```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.

Given an integer n where 1 ≤ n ≤ 30, generate the nth term of the count-and-say sequence.

<br></br>



## Example
----
1: Input: 1 Output: "1"
2. Input: 4 Output: "1211"

<br></br>



## Solution
----
### C++
```cpp
class Solution {
public:
    string int_to_string(int j) {
        stringstream in;
        in << j;
        string temp;
        in >> temp;
        return temp;    
    }
    
    string genate(string s) {
        string now;
        int j = 0;
        for(int i = 0; i < s.size(); i += j) {
            for(j = 0; j + i < s.size(); j++) {
                if(s[i] != s[i+j]) {
                    break;
                } 
            }
            now = now + int_to_string(j) + s[i];
        }
        return now;
    }
    
    string countAndSay(int n) {
        string s("1");
        
        while(--n) {
            s = genate(s);
        }
        return s;
    }
};
```

<br>


### Java
```java
public class Solution {
    public String countAndSay(int n) {
        String oldString = "1";
        while (--n > 0) {
            StringBuilder sb = new StringBuilder();
            char [] oldChars = oldString.toCharArray();

            for (int i = 0; i < oldChars.length; i++) {
                int count = 1;
                while ((i+1) < oldChars.length && oldChars[i] == oldChars[i+1]) {
                    count++;
                    i++;
                }
                sb.append(String.valueOf(count) + String.valueOf(oldChars[i]));
            }
            oldString = sb.toString();
        }

        return oldString;
    }
}
```