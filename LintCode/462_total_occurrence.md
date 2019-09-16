# <center>462 - Total Occurrence of Target (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/total-occurrence-of-target/

<br></br>



## Description
----
Given a target number and an integer array sorted in ascending order. Find the total number of occurrences of target in the array.

<br></br>



## Example
----
Given [1, 3, 3, 4, 5] and target = 3, return 2.

<br></br>



## Solution
----
### Java
```java
public class GetTotalOccurrence {
	/**
     * @param A an integer array sorted in ascending order
     * @param target an integer
     * @return an integer
     */
    public int totalOccurrence(int[] A, int target) {
        if (A.length == 0 || A[0] > target || A[A.length - 1] < target)
            return 0;
        
        int low = 0, high = A.length - 1, mid = 0;
        while (low + 1 < high) {
            mid = (high - low) / 2 + low;
            if (target == A[mid])
                break;
            if (target > A[mid])
                low = mid;
            else 
                high = mid;
        }
        if (A[mid] != target)
        	return 0;
        
        int start = mid, mid1 = 0;
        low = 0;
        high = mid;
        while (low + 1 < high) {
        	mid1 = (high + low) >> 1;
        	if (A[mid1] < target) {
        		low = mid1;
        	} else {
        		high = mid1;
        		start = mid1;
        	}
        }
        if (A[low] == target) // important because it is low + 1 < high, not low <= high
        	start = low;
        
        int end = mid, mid2 = 0;
        low = mid;
        high = A.length - 1;
        while (low + 1 < high) {
        	mid2 = (low + high) >> 1;
        	if (A[mid2] > target) {
        		high = mid2;
        	} else {
        		low = mid2;
        		end = mid2;
        	}
        }
        if (A[high] == target) 
        	end = high;
        
        return end - start + 1;
    }
}
```

<br>


### Python
```python
class Solution:
    # @param {int[]} A an integer array sorted in ascending order
    # @param {int} target an integer
    # @return {int} an integer
    def totalOccurrence(self, A, target):
        if not A:
            return 0

        length = len(A)
        start = 0
        end = length - 1

        while start + 1 < end:
            mid = start + (end - start) // 2
            if A[mid] < target:
                start = mid
            else:
                end = mid

        if A[start] == target:
            head = start
        elif A[end] == target:
            head = end
        else:
            head = -1

        start = 0
        end = length - 1

        while start + 1 < end:
            mid = start + (end - start) // 2
            if A[mid] <= target:
                start = mid
            else:
                end = mid
                
        if A[end] == target:
            tail = end
        elif A[start] == target:
            tail = start
        else:
            tail = -1

        if head >= 0 and tail >= 0:
            return tail - head + 1
        return 0
```