# <center>460 - Find K Closest Elements (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search, Two Pointers
* Company: Google
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/find-k-closest-elements/description

<br></br>



## Description
----
Given a sorted array, two integers k and x, find the k closest elements to x in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred. 

<br></br>



## Example
----
1. Input: [1,2,3,4,5], k=4, x=3 Output: [1,2,3,4]
2. Input: [1,2,3,4,5], k=4, x=-1 Output: [1,2,3,4]

<br></br>



## Solution
----
假设目标值为n，数组为data，需要返回k个元素。
1. 如果n <= data[0]，返回数组前k个元素。
2. 如果n >= data[data.length - 1]，返回数组最后k个元素。
3. 否则：
    1. 先求得离n最接近的数组元素，记为data[m]。
    2. 要求的k个元素必在区间[m - k + 1, m + k - 1]。之所以两边“延伸“k - 1长度，因为首先data[m]是离n最接近的元素，剩下要找的k - 1个元素，要么以下标m为核心两边展开，要么全在data[m]左边，要么全在data[m]右边。
    3. 按双指针法，设left, right := data[m - k + 1], data[m + k - 1]。当然，要检查left和right是否越界。
    4. 然后，按值比较，直至right - left == k - 1。

<br>


### Java
```java
public class KClosestNumbersInSortedArray {
	public int[] kClosestNumbers(int[] A, int target, int k) {
        if (A == null || A.length == 0 || k > A.length) 
            return A;
        
        int[] result = new int[k];
        if (target <= A[0]) {
            for (int i = 0; i < k; i++)
                result[i] = A[i];
            return result;
        }
        if (target >= A[A.length - 1]) {
            for (int i = A.length - k; i < A.length ; i++)
                result[i] = A[i];
            return result;
        }
        
        int index = firstIndex(A, target);
        int start = Math.max(0, index - k + 1), end = Math.min(A.length - 1, index + k - 1);
        for (int i = end - start; i > k - 1; i--) {
        	// Because smaller elements are always preferred. Otherwise, need to change compare direction.
        	if (end >= A.length)
                start++;
            else if (start < 0 || target - A[start] <= A[end] - target)
                end--;
            else
                start++;
        }
        start = Math.max(0, start);
        end = Math.min(end, A.length - 1);
        for (int i = 0; i < k; i++)
            result[i] = A[start + i];
        
        return result;
    }
    
    private int firstIndex(int[] A, int target) {
        int start = 0, end = A.length - 1;
        while (start + 1 < end) {
            int mid = start + (end - start) >> 1;
            if (A[mid] == target) 
                return mid;
            if (A[mid] > target) 
                end = mid;
            else 
                start = mid;
        }
        
        if (target - A[start] <= A[end] - target)
            return start;
        return end;
    }
}
```

<br>


### Python
```python
class GetKClosestElements:
    def solution(self, arr, k: int, x: int):
        l = len(arr)
        if not arr or k < 0 or k > l:
            return arr
        if x <= arr[0]:
            return arr[:k]
        if x >= arr[l - 1]:
            return arr[l - k:]

        index = self.get_index(arr, x)
        left, right = max(0, index - k + 1), min(l - 1, index + k - 1)
        if right - left <= k - 1:
            return arr
        for i in range(right - left, k - 1, -1):
            if right >= l:
                left += 1
            elif left < 0 or x - arr[left] <= arr[right] - x:
                right -= 1
            else:
                left += 1

        left, right = max(0, left), min(right, l - 1)
        return arr[left:right + 1]

    def get_index(self, arr, x):
        low, high = 0, len(arr) - 1
        while low + 1 < high:
            mid = low + (high - low) // 2
            if arr[mid] == x:
                return mid
            if arr[mid] > x:
                high = mid
            else:
                low = mid

        if x - arr[low] <= arr[high] - x:
            return low
        return high
```

<br>


### Go
```go
func GetKClosestElements(arr []int, k, x int) []int {
	l := len(arr)
	if l < 1 || k < 1 || k >= l {
		return make([]int, 0)
	}
	if x <= arr[0] {
		return arr[:k]
	}
	if x >= arr[l-1] {
		return arr[l-k:]
	}
	index := getFirstIndex(arr, x)
	left, right := index-k+1, index+k-1
	if left < 0 {
		left = 0
	}
	if right >= l {
		right = l - 1
	}
	if right-left <= k-1 {
		return arr
	}
	for count := right - left; count > k-1; count-- {
		// Because smaller elements are always preferred. Otherwise, need to change compare direction.
		if right >= l {
			left++
		} else if left < 0 || x-arr[left] <= arr[right]-x {
			right--
		} else {
			left++
		}
	}

	if left < 0 { // Important.
		left = 0
	}
	if right >= l {
		right = l - 1
	}
	return arr[left : right+1]
}

func getFirstIndex(arr []int, x int) int {
	l := len(arr)
	low, high := 0, l-1
	for low+1 < high {
		mid := low + (high-low)/2
		if arr[mid] == x {
			return mid
		}
		if arr[mid] > x {
			high = mid
		} else {
			low = mid
		}
	}
	if arr[high]-x <= x-arr[low] {
		return high
	}
	return low
}
```