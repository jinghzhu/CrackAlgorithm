# <center>905 - Sort Array By Parity II (E)</center> 



<br></br>

* Tag: Array, Two Pointers
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/sort-array-by-parity-ii/

<br></br>



## Description
----
Given an array A of non-negative integers, half of the integers in A are odd, and half of the integers are even.

Sort the array so that whenever A[i] is odd, i is odd; and whenever A[i] is even, i is even.

You may return any answer array that satisfies this condition.

<br></br>



## Example
----
Input: [4,2,5,7]
Output: [4,5,2,7]
Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.

<br></br>



## Solution
----
### Java
```java
public class SortArrByParity2 {
	 public int[] sortArrayByParityII(int[] A) {
	        if (A == null || A.length < 2)
	            return A;
	        
	        int i = 0, j = 1;
	        while (i < A.length && j < A.length) {
	            while (i < A.length && A[i] % 2 == 0)
	                i += 2;
	            while (j < A.length && A[j] % 2 != 0)
	                j += 2;
	            if (i < A.length && j < A.length) {
	                int temp = A[i];
	                A[i] = A[j];
	                A[j] = temp;
	                i += 2;
	                j += 2;
	            }
	        }
	        
	        return A;
	    }
}
```

<br>


### Python
```python
class SortArrByParity2:
    def solution1(self, data):
        i, j, l = 0, 1, len(data)
        while i < l and j < l:
            while i < l and data[i] % 2 == 0:
                i += 2
            while j < l and data[j] % 2 != 0:
                j += 2
            if i < l and j < l:
                data[i], data[j] = data[j], data[i]
                i += 2
                j += 2

        return A

    def solution2(self, data):
        n = len(data)
        ans = [None] * n

        ans[::2] = [x for x in data if x % 2 == 0]
        ans[1::2] = [x for x in data if x % 2 != 0]

        return ans
```

<br>


### Go
```go
func SortArrByParity2(A []int) []int {
	i, j, l := 0, 1, len(A)
	for i < l && j < l {
		for i < l && A[i]%2 == 0 {
			i += 2
		}
		for j < l && A[j]%2 != 0 {
			j += 2
		}
		if i < l && j < l {
			A[i], A[j] = A[j], A[i]
			i += 2
			j += 2
		}
	}

	return A
}
```