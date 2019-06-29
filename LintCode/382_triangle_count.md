# <center>382 - Triangle Count (M)</center> 



<br></br>

* Tag: Array, Two Pointers
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/triangle-count/description

<br></br>



## Description
----
Given an array of integers, how many three numbers can be found in the array, so that we can build an triangle whose three edges length is the three numbers that we find?

<br></br>



## Example
----
Input: [3, 4, 6, 7] Output: 3, (3, 4, 6), (3, 6, 7), (4, 6, 7).
Input: [4, 4, 4, 4] Output: 4

<br></br>



## Solution
----
### Java
```java
public class TriangleCount {
	/*
     * @param: A list of integers
     * @return: An integer
     */
    public int solution(int[] data) {
    	int count = 0;
    	
    	if (data == null || data.length < 3)
    		return count;
    	
        Arrays.sort(data);
        
        for(int i = 2; i < data.length; i++) {
            int left = 0, right = i - 1;
            while(left < right) {
                if(data[left] + data[right] > data[i]) {
                    count += right - left;
                    right--;
                } else {
                    left++;
                }
            }
        }
        
        return count;
    }
}
```

<br>


## Go
```go
import (
	"sort"
)

func TriangleCount(data []int) int {
	count := 0
	sort.Ints(data)
	for i := 2; i < len(data); i++ {
		left, right := 0, i-1
		for left < right {
			if data[left]+data[right] > data[i] {
				count += right - left
				right--
			} else {
				left++
			}
		}
	}

	return count
}
```

<br>


## Python
```python
class TriangleCount:
    """
    @param S: A list of integers
    @return: An integer
    """
    def solution(self, data):
        count = 0
        data.sort()
        i = 2
        while i < len(data):
            left, right = 0, i - 1
            while left < right:
                if data[left] + data[right] > data[i]:
                    count += right - left
                    right -= 1
                else:
                    left += 1
            i += 1

        return count
```
