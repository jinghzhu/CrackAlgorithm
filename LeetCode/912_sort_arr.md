# <center>912- Sort an Array (M)</center> 



<br></br>

* Tag: Sort
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Link: https://leetcode.com/problems/sort-an-array/

<br></br>



## Description
----
Given an array of integers nums, sort the array in ascending order.

<br></br>



## Example
----
Input: [5,2,3,1]
Output: [1,2,3,5]

<br></br>



## Solution
----
### Java
```java
public class Quick {
	/**
	 * @desc: Sort array with recursion quick sort.
     * @param A: Int data array
     */
	public void sort(int[] A) {
		if (A != null && A.length > 1)
			sort(A, 0, A.length - 1);
	}
	
	private void sort(int[] A, int start, int end) {
        if (A == null || start >= end || end < 0 || end >= A.length)
            return;
        
        int left = start, right = end;
        // 1: pivot is the value, not the index.
        int pivot = A[start + (end - start) / 2];

        // 2: every time compare left & right, it is left <= right NOT left < right.
        while (left <= right) {
            // 3-1: A[left] < pivot, NOT A[left] <= pivot.
            while (left <= right && A[left] < pivot)
                left++;
            // 3-2: A[right] > pivot, NOT A[right] >= pivot.
            while (left <= right && A[right] > pivot)
                right--;
            if (left <= right) {
                swap(A, left, right);
                // 4: Don't forget to update index here.
                left++;
                right--;
            }
        }
        
        sort(A, start, right);
        sort(A, left, end);
    }
	
	/**
	 * @desc: Sort array with partition quick sort.
     * @param A: Int data array
     */
	public void sortPartition(int[] A) {
		if (A != null && A.length > 1)
			sortPartition(A, 0, A.length - 1);
	}
	
	private void sortPartition(int[] A, int start, int end) {
		if (A != null && start < end && start > -1 && end < A.length) {
			int partition = getPartition(A, start, end);
			sortPartition(A, start, partition - 1);
			sortPartition(A, partition + 1, end);
		}
	}
	
	private int getPartition(int[] A, int start, int end) {
		int i = start - 1, pivot = A[end];
		for (int j = start; j < end; j++) {
			if (A[j] <= pivot) {
				i++;
				if (i != j)
					swap(A, i, j);
			}	
		}
		swap(A, i + 1, end);
		
		return i + 1; // Pivot is at i + 1.
	}
	
	/**
	 * @desc: Sort array with non-recursion quick sort.
     * @param A: Int data array
     */
	public void sortN(int[] A) {
		if (A == null || A.length < 2)
			return;
		
		Stack<Integer> leftSt = new Stack<Integer>();
		Stack<Integer> rightSt = new Stack<Integer>();
		leftSt.push(0);
		rightSt.push(A.length - 1);
		
		while (!leftSt.isEmpty() && !rightSt.isEmpty()) {
			// 1: Remember the position of start and end.
			int start = leftSt.pop(), end = rightSt.pop();
			// 2: In case of infinite loop. 
			if (start > end)
				continue;
			
			int left = start, right = end, pivot = A[start + (end - start) / 2];
			
			while (left <= right) {
				while (left <= right && A[left] < pivot)
					left++;
				while (left <= right && A[right] > pivot)
					right--;
	            if (left <= right) {
	                swap(A, left, right);
	                // 3: Don't forget to update index here.
	                left++;
	                right--;
	            }
			} // while (left <= right)
			
			// 4: Push start and end.
			leftSt.push(start);
			rightSt.push(right);
			leftSt.push(left);
			rightSt.push(end);
		}
	}
	
	private void swap(int[] data, int left, int right) {
		int temp = data[left];
		data[left] = data[right];
		data[right] = temp;
	}
}
```

<br>


### Go
```go
func QuickSort(data []int) {
	quickSort(data, 0, len(data)-1)
}

func quickSort(data []int, start, end int) {
	if start < 0 || end <= start || end >= len(data) {
		return
	}

	left, right, pivot := start, end, data[start+(end-start)/2]

	for left <= right {
		for (left <= right) && data[left] < pivot {
			left++
		}
		for (left <= right) && data[right] > pivot {
			right--
		}
		if left <= right {
			data[left], data[right] = data[right], data[left]
			left++
			right--
		}
	}

	quickSort(data, start, right)
	quickSort(data, left, end)
}
```

```go
import (
	stack "github.com/jinghzhu/DataStructure/Go/stack/slice"
)

func QuickSortN(data []int) {
	if len(data) < 2 {
		return
	}

	start, end := 0, len(data)-1
	leftStc := stack.New()
	rightStc := stack.New()
	leftStc.Push(start)
	rightStc.Push(end)

	for !leftStc.IsEmpty() && !rightStc.IsEmpty() {
		start, _ := leftStc.Pop().(int)
		end, _ := rightStc.Pop().(int)
		if start > end { // imporant
			continue
		}
		left, right, pivot := start, end, data[start+(end-start)/2]
		for left <= right {
			for left <= right && data[left] < pivot {
				left++
			}
			for left <= right && data[right] > pivot {
				right--
			}
			if left <= right {
				data[left], data[right] = data[right], data[left]
				left++
				right--
			}
		}
		leftStc.Push(start)
		rightStc.Push(right)
		leftStc.Push(left)
		rightStc.Push(end)
	}
}
```

```go
func QuickSortPartition(data []int) {
	quickSortPartition(data, 0, len(data)-1)
}

func quickSortPartition(data []int, start, end int) {
	if start < end && end < len(data) && start > -1 {
		p := getPartition(data, start, end)
		quickSortPartition(data, start, p-1)
		quickSortPartition(data, p+1, end)
	}
}

func getPartition(data []int, start, end int) int {
	if data == nil || start < 0 || end >= len(data) || start > end {
		return -1
	}
	i, pivot := start-1, data[end]
	for j := start; j < end; j++ {
		if data[j] <= pivot {
			i++
			if i != j {
				data[i], data[j] = data[j], data[i]
			}
		}
	}
	data[i+1], data[end] = data[end], data[i+1]

	return i + 1 // Pivot is at i + 1
}
```

<br>


### Python
```python
class QuickSort:
    def sort(self, nums):
        self.sort(nums, 0, len(nums) - 1)

    def sort(self, nums, low, high):
        if low < 0 or high >= len(nums) or high <= low:
            return

        left, right, pivot = low, high, nums[low + (high - low) // 2]
        while left <= right:
            while left <= right and nums[left] < pivot:
                left += 1
            while left <= right and nums[right] > pivot:
                right -= 1
            if left <= right:
                nums[left], nums[right] = nums[right], nums[left]
                left += 1
                right -= 1

        self.sort(nums, low, right)
        self.sort(nums, left, high)

    def sort_p(self, nums):
        self.sort_partition(nums, 0, len(nums) - 1)

    def sort_partition(self, nums, low, high):
        if low >= 0 and high < len(nums) and high > low:
            p = self.get_partition(nums, low, high)
            self.sort_partition(nums, low, p - 1)
            self.sort_partition(nums, p + 1, high)

    def get_partition(self, nums, low, high):
        i, pivot = low - 1, nums[high]
        for j in range(low, high):
            if nums[j] < pivot:
                i += 1
                if i != j:
                    nums[i], nums[j] = nums[j], nums[i]

        nums[i + 1], nums[high] = nums[high], nums[i + 1]

        return i + 1
```