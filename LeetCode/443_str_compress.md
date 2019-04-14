# <center>443 - String Compression</center> 


* Tag: String
* Company: Snapchat, Microsoft, Yelp, Lyft, Bloomberg

https://leetcode.com/problems/string-compression/

<br></br>



## Description
----
Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3.

If the "compressed" string would not become smaller than the original string, your method should return the original string.

You can assume the string has only upper and lower case letters (a-z).

<br></br>



## Example
----
1. Input: str = "aabcccccaaa" Output: "a2b1c5a3"
2. Input: str = "aabbcc" Output: "aabbcc"

<br></br>



## Solution
----
### Go
```go
func Compress(chars []byte) int {
	start := 0
	end := 0
	count := 0

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
	/**
     * @param originalString: a string
     * @return: a compressed string
     */
    public String compress(String originalString) {
        if (originalString == null || originalString.length() < 2)
            return originalString;
        
        char[] originalChar = originalString.toCharArray();
        String result = String.valueOf(originalChar[0]);
        int count = 0, i = 1;
        char last = originalChar[0];
        
        while (i < originalChar.length) {
            if (originalChar[i] == last) {
                count++;
            } else {
                result += String.valueOf(count+1);
                count = 0;
                result += String.valueOf(originalChar[i]);
            }
            last = originalChar[i++];
        }
        
        result += String.valueOf(count+1); // Important.
        
        if (result.length() < originalString.length())
            return result;
        
        return originalString;
    }
}
```