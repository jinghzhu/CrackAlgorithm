# <center>183 - Wood Cut (H)</center> 



<br></br>

* Tag: Binary Search
* Difficulty: Hard
* Link: https://www.lintcode.com/problem/wood-cut/description

<br></br>



## Description
----
Given n pieces of wood with length L[i] (integer array). Cut them into small pieces to guarantee you could have equal or more than k pieces with the same length. What is the longest length you can get from the n pieces of wood? Given L & k, return the maximum length of the small pieces.

<br></br>



## Example
----
* Input: L = [232, 124, 456] k = 7
* Output: 114
* Explanation: We can cut it into 7 pieces if any piece is 114cm long, however we can't cut it into 7 pieces if any piece is 115cm long.

<br></br>



## Solution
----
### Java
```java
public class WoodCut {
    /** 
     *@param L: Given n pieces of wood with length L[i]
     *@param k: An integer
     *return: The maximum length of the small pieces.
     */
    public int woodCut(int[] L, int k) {
    	if (L == null || L.length == 0 || k < 1)
    		return 0;
    	
        int max = 0;
        for (int i = 0; i < L.length; i++) 
            max = Math.max(max, L[i]);
        
        // find the largest length that can cut more than k pieces of wood.
        int start = 1, end = max;
        while (start + 1 < end) {
            int mid = start + (end - start) >> 1;
            if (count(L, mid) >= k) 
                start = mid;
            else 
                end = mid;
        }
        
        if (count(L, end) >= k) 
            return end;
        if (count(L, start) >= k) 
            return start;
        
        return 0;
    }
    
    private int count(int[] L, int length) {
        int sum = 0;
        for (int i = 0; i < L.length; i++) 
            sum += L[i] / length;
        
        return sum;
    }
}
```

<br>


### Python
```python
class WoodCut:
    """
    @param L: Given n pieces of wood with length L[i]
    @param k: An integer
    @return: The maximum length of the small pieces
    """
    def solution(self, L, k):
        if not L or len(L) < 1 or k < 1:
            return 0
        max_l = L[0]
        for l in L:
            max_l = max(max_l, l)
        low, high = 1, max_l
        while low + 1 < high:
            mid = (low + high) // 2
            if self.count(L, mid) >= k:
                low = mid
            else:
                high = mid

        if self.count(L, high) >= k:
            return high
        if self.count(L, low) >= k:
            return low
        return 0

    def count(self, L, length):
        num = 0
        for l in L:
            num += l // length

        return num
```

<br>


### Go
```go
func WoodCut(L []int, k int) int {
	if len(L) < 1 || k < 1 {
		return 0
	}
	max := L[0]
	for _, v := range L {
		if v < 1 {
			return 0
		}
		max = maxInt(max, v)
	}
	low, high := 1, max
	for low+1 < high {
		mid := low + (high-low)/2
		if countWC(L, mid) >= k {
			low = mid
		} else {
			high = mid
		}
	}
	if countWC(L, high) >= k {
		return high
	}
	if countWC(L, low) >= k {
		return low
	}

	return 0
}

func countWC(L []int, length int) int {
	count := 0
	for _, v := range L {
		count += v / length
	}

	return count
}
```
