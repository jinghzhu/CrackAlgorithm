# <center>189 - Rotate Array (E)</center> 



<br></br>

* Tag: Array
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/rotate-array/

<br></br>



## Description
----
Given an array, rotate the array to the right by k steps, where k is non-negative.

<br></br>



## Example
----
Input: [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]

<br></br>



## Solution
----
### Java
```java
public class RotateArray {
	/**
     * @param nums: an array
     * @param k: an integer
     * @return: rotate the array to the right by k steps
     */
	// Space: O(1) Time: O(n)
    public int[] rotate(int[] nums, int k) {
        if (nums == null || k < 1 || k % nums.length == 0)
            return nums;
            
        k %= nums.length;
        int ops = 0;
        for (int i = 0; i < k; i++) {
            int lastV = nums[i];
            for (int j = i + k; j != i; j = (j + k) % nums.length) {
                int temp = nums[j];
                nums[j] = lastV;
                lastV = temp;
                ops++;
            }
            nums[i] = lastV;
            ops++;
            if (ops >= nums.length)
                break;
        }
        
        return nums;
    }
    
	// Space is O(n) and time is O(n)
    public void rotate1(int[] nums, int k) {
        if (nums == null || nums.length < 2 || k <= 0 || k % nums.length == 0)
            return;
            
        k %= nums.length;
    	int[] result = new int[nums.length];
    	for(int i=0; i < k; i++)
    		result[i] = nums[nums.length - k + i];
    	int j=0;
    	for(int i=k; i<nums.length; i++){
    		result[i] = nums[j];
    		j++;
    	}
    	System.arraycopy( result, 0, nums, 0, nums.length );
    }
    
    // This solution is like a bubble sort.
    // Space is O(1) and time is O(n*k)
    public void rotate2(int[] arr, int k) {
    	if (arr == null || k < 0)
    	    return;
    	
    	k %= arr.length;
    	for (int i = 0; i < k; i++) {
    		for (int j = arr.length - 1; j > 0; j--) {
    			int temp = arr[j];
    			arr[j] = arr[j - 1];
    			arr[j - 1] = temp;
    		}
    	}
    }
}
```

<br>


## Go
```go
func Rotate(nums []int, k int) {
	l := len(nums)
	if l == 0 || k < 1 || k%l == 0 {
		return
	}

	k %= l
	ops := 0 // Important
	for i := 0; i < k; i++ {
		index, lastV := i+k, nums[i]
		for ; index != i; index = (index + k) % l {
			temp := nums[index]
			nums[index] = lastV
			lastV = temp
			ops++
		}
		nums[i] = lastV // Important
		ops++
		if ops >= l { // Important
			break
		}
	}
}
```

<br>


## Python
```python
class Rotate:
    def rotate(self, nums, k):
        nums_l = len(nums)
        if nums_l == 0 or k % nums_l == 0:
            return
        ops = 0
        k %= nums_l
        for i in range(k):
            last = nums[i]
            index = i + k
            while index != i:
                temp = nums[index]
                nums[index] = last
                last = temp
                ops += 1
                index = (k + index) % nums_l
            nums[i] = last
            ops += 1
            if ops >= nums_l:
                break
```
