# <center>1784 - Decrease To Be Palindrome (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String, Two Pointers
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/decrease-to-be-palindrome

<br></br>



## Description
----
Given a string s with a-z. We want to change s into a palindrome with following operations:

```
1. change 'z' to 'y';
2. change 'y' to 'x';
3. change 'x' to 'w';
................
24. change 'c' to 'b';
25. change 'b' to 'a';
```

<br></br>



## Example
----
Input: "abc"

Output: 2

Explanation: 
1. Change 'c' to 'b': "abb"
2. Change the last 'b' to 'a': "aba"

<br></br>



## Solution
----
### Go
```go
/**
 * @param s: the string s
 * @return: the number of operations at least
 */
func numberOfOperations (s string) int {
    count, i, j := 0, 0, len(s) - 1
    for i < j {
        count += absInt(int(s[i]) - int(s[j]))
        i++
        j--
    }
    
    return count
}

func absInt(i int) int {
    if i < 0 {
        return -1 * i
    }
    return i
}
```

<br>
