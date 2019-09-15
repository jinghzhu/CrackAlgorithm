# <center>60 - Search Insert Position (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/search-insert-position/description

<br></br>



## Description
----
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You may assume no duplicates in the array.

<br></br>



## Example
----
1. Input: [1,3,5,6], 5 Output: 2
2. Input: [1,3,5,6], 2 Output: 1
3. Input: [1,3,5,6], 7 Output: 4

<br></br>



## Solution
----
### Java
```java
class Solution {
    public int searchInsert(int[] A, int target) {
        if (A == null || A.length == 0) 
            return 0;
  
        int start = 0, end = A.length - 1;
        
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if (A[mid] == target) 
                return mid;
            else if (A[mid] < target) 
                start = mid;
            else 
                end = mid;
        }
        
        if (A[start] >= target) 
            return start;
        else if (A[end] >= target) 
            return end;
        else 
            return end + 1;
    }
}
```

<br>
