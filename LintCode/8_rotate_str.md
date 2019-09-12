# <center>8 - Rotate String (E)</center> 



<br></br>

* Difficulty: Easy
* Tag: String
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Link: https://www.lintcode.com/problem/rotate-string/description

<br></br>



## Description
----
Given a string and an offset, rotate the string by offset in place. (rotate from left to right).

<br></br>



## Example
----
1. Input: str="abcdefg", offset = 3 Output: str = "efgabcd"	
2. Input: str="abcdefg", offset = 0 Output: str = "abcdefg"	

<br></br>



## Solution
----
### Java
```java
public class RotateString1 {
	public char[] RotateString1(char[] str, int offset) {
        if (str == null || str.length <= 1 || offset <= 0 || offset % str.length == 0)
            return str;
        
        offset %= str.length;
        char[] newStr = new char[str.length];
        for (int i = 0; i < newStr.length - offset; i++)
            newStr[offset + i] = str[i];
        for (int i = str.length - offset, j = 0; i < str.length; i++, j++)
            newStr[j] = str[i];
        
        return newStr;
    }

    // space complexity: O(1)
    public char[] RotateString2(char[] str, int offset) {
        if (str == null || str.length <= 1 || offset <= 0 || offset % str.length == 0)
            return str;
        
        offset %= str.length;
        reverse(str, 0, str.length - offset - 1);
        reverse(str, str.length - offset, str.length - 1);
        reverse(str, 0, str.length - 1);
        
        return str;
    }
    
    private void reverse(char[] str, int start, int end) {
    	if(str == null || end > str.length || start > end || start < 0 || end < 0)
    		return;

    	for(int i = start, j = end; i < j; i++, j--) {
    		char temp = str[i];
    		str[i] = str[j];
    		str[j] = temp;
    	}
    }
}
```

<br>


### Go
```go
func RotateStringRight(s string, offset int) string {
	if len(s) == 0 || offset <= 0 || offset%len(s) == 0 {
		return s
	}

	newS := make([]byte, len(s))
	offset %= len(s)
	i, j := 0, 0
	for i < len(s)-offset {
		newS[i+offset] = s[i]
		i++
	}
	i = len(s) - offset
	for i < len(s) {
		newS[j] = s[i]
		i++
		j++
	}

	return string(newS)
}
```

```go
// Space Complexity: O(1)
func RotateStringRight(s string, offset int) string {
	if len(s) == 0 || offset <= 0 || offset%len(s) == 0 {
		return s
	}

	offset %= len(s)
	s = rsrReverse(s, 0, len(s)-offset-1)
	s = rsrReverse(s, len(s)-offset, len(s)-1)
	s = rsrReverse(s, 0, len(s)-1)

	return s
}

func rsrReverse(s string, start, end int) string {
	if len(s) <= 1 || end <= start || end > len(s) || start < 0 || end < 0 {
		return s
	}
	sArr := []rune(s)
	i, j := start, end

	for i < j {
		temp := sArr[i]
		sArr[i] = sArr[j]
		sArr[j] = temp
		i++
		j--
	}

	return string(sArr)
}
```

<br>


### Python
```python
class RotateString:
    def solution1(self, nums):
        if nums is None or len(nums) == 0:
            return
        i = self.find_min(nums)
        if i == 0:
            return
        self.reverse(nums, 0, i - 1)
        self.reverse(nums, i, len(nums) - 1)
        self.reverse(nums, 0, len(nums) - 1)

    def find_min(self, nums):
        x = float('inf')
        ans = -1
        for i in range(len(nums)):
            if nums[i] < x:
                x = nums[i]
                ans = i
        return ans

    def reverse(self, nums, lo, hi):
        while lo < hi:
            nums[lo] , nums[hi] = nums[hi], nums[lo]
            lo += 1
            hi -= 1
```