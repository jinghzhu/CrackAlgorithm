# <center>443 - String Compression (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: String
* Company: Snapchat, Microsoft, Lyft
* Difficulty: Easy
* Link: https://leetcode.com/problems/string-compression/

<br></br>



## Description
----
Given an array of characters, compress it in-place.

The length after compression must always be smaller than or equal to the original array.

Every element of the array should be a character (not int) of length 1.

After you are done modifying the input array in-place, return the new length of the array. 

<br></br>



## Example
----
```
Input:
["a","a","b","b","c","c","c"]

Output:
Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]

Explanation:
"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".
```
 
```
Input:
["a"]

Output:
Return 1, and the first 1 characters of the input array should be: ["a"]

Explanation:
Nothing is replaced.
```
 
```
Input:
["a","b","b","b","b","b","b","b","b","b","b","b","b"]

Output:
Return 4, and the first 4 characters of the input array should be: ["a","b","1","2"].

Explanation:
Since the character "a" does not repeat, it is not compressed. "bbbbbbbbbbbb" is replaced by "b12".
Notice each digit has it's own entry in the array.
```

<br></br>



## Solution
----
### Go
```go
func Compress(chars []byte) int {
	start, end, count := 0, 0, 0

	for end < len(chars) {
		chars[start] = chars[end]
		for end < len(chars) && chars[start] == chars[end] {
			count++
			end++
		}
		if count > 1 {
			countStr := strconv.Itoa(count)
			for _, c := range []byte(countStr) {
				start++
				chars[start] = c
			}
		}
		start++
		count = 0
	}

	return start
}
```

<br>


### Java
```java
public class Compression {
	public int compressLeetCode(char[] chars) {
        if (chars == null)
            return 0;
        
        int write = 0, cur = 0, l = chars.length;
        while (cur < l) {
            int count = 1, j = cur + 1;
            char ch = chars[cur];
            while (j < l && chars[j] == ch) {
                j++;
                count++;
            }
            chars[write++] = ch;
            if (count > 1) {
                String countS = String.valueOf(count);
                for (int i = 0; i < countS.length(); i++)
                    chars[write++] = countS.charAt(i);
            }
            cur = j;
        }
        
        return write;
    }
}
```

<br>


### Python
```python
class Compress:
    def solution_leetcode(self, chars) -> int:
        write, cur, l = 0, 0, len(chars)
        while cur < l:
            cur_char = chars[cur]
            j, count = cur + 1, 1
            while j < l and chars[j] == cur_char:
                j, count = j + 1, count + 1
            chars[write] = cur_char
            write += 1
            if count > 1:
                count = str(count)
                for i in range(len(count)):
                    chars[write] = count[i]
                    write += 1
            cur = j
        return write
```