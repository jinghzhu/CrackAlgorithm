# <center>491 - Palindrome Number</center> 


* Tag: String
* Author: Jinghua Zhu jhzhu@outlook.com

https://www.lintcode.com/problem/palindrome-number/description

<br></br>



## Description
----
Check a positive number is a palindrome or not. A palindrome number is that if you reverse the whole number you will get exactly the same number.

<br></br>



## Example
----
* Input:11 Output:true
* Input:1232 Output:false

<br></br>



## Solution
----
### Go

```go
import "fmt"

func isPalindrome (num int) bool {
    str := fmt.Sprintf("%d", num)
    i, j := 0, len(str) - 1
    for i < j {
        if str[i] != str[j] {
            return false
        }
        i++
        j--
    }
    
    return true
}
```

<br>


### Java
```java
class Solution {
    public boolean isPalindrome(int x) {
        if(x < 0)
            return false;
        
        return x == reverse(x);
    }
    
    public int reverse(int data) {
		int r_data = 0, count = 0;
		while(data != 0) {
			int i = data % 10;
			r_data = r_data * 10 + i;
			data /= 10;
			count++;
		}
		
		    return r_data;
	}
}
```