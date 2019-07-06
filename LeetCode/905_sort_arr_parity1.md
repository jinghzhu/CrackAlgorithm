# <center>905 - Sort Array By Parity I (E)</center> 



<br></br>

* Tag: Array, Two Pointers
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/sort-array-by-parity/

<br></br>



## Description
----
Given an array A of non-negative integers, return an array consisting of all the even elements of A, followed by all the odd elements of A.

<br></br>



## Example
----
Input: [3,1,2,4]
Output: [2,4,3,1]
The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.

<br></br>



## Solution
----
### Java
```java
class Solution {
    public int[] sortArrayByParity(int[] A) {
        if (A == null)
            return A;
        int i = 0, j = A.length - 1;
        while (i <= j) {
            while (i <= j && A[i] % 2 == 0)
                i++;
            while (i <= j && A[j] % 2 != 0)
                j--;
            if (i <= j) {
                int temp = A[i];
                A[i] = A[j];
                A[j] = temp;
                i++;
                j--;
            }
        }
        
        return A;
    }
}
```

<br>


### Python
```python
class Solution_0:
    def sortArrayByParity(self, A):
        """
        :type A: List[int]
        :rtype: List[int]
        """
        if not A or len(A) == 1:
            return A

        left, right = 0, len(A) - 1
        while left < right:
            if A[left] % 2 > A[right] % 2:
                A[left], A[right] = A[right], A[left]

            if A[left] % 2 == 0:
                left += 1

            if A[right] % 2 != 0:
                right -= 1

        return A


class Solution_1:
    def sortArrayByParity(self, A):
        """
        :type A: List[int]
        :rtype: List[int]
        """
        if not A or len(A) == 1:
            return A

        left = 0
        right = len(A) - 1
        while left <= right:
            while left < right and A[left] % 2 == 0:
                left += 1

            while right > left and A[right] % 2 != 0:
                right -= 1

            if left == right:
                break
            else:
                temp = A[left]
                A[left] = A[right]
                A[right] = temp
                left += 1
                right -= 1

        return A
```
