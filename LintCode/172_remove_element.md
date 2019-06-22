# <center>172 - Remove Element (E)</center> 



<br></br>

* Tag: Array, Two Pointer
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/remove-element/description

<br></br>



## Description
----
Given an array and a value, remove all occurrences of that value in place and return the new length.

The order of elements can be changed, and the elements after the new length don't matter.

<br></br>



## Example
----
1. Input: [], value = 0 Output: 0
2. Input:  [0,4,4,0,0,2,4,4], value = 4 Output: 4

<br></br>



## Solution
----
### Java
```java
public class RemoveElement {
    public int removeElement1(int[] A, int elem) {
    	if (A == null)
    		return 0;
    	
        int i = 0, pointer = A.length - 1;
        while(i <= pointer)
            if(A[i] == elem)
                A[i] = A[pointer--];
            else
                i++;
        
        return pointer + 1;
    }
	
    public int removeElement2(int[] nums, int val) {
        if (nums == null || nums.length == 0)
            return 0;
        
        int low = 0, high = nums.length - 1, count = 0;;
        while (low <= high) {
            while (low < nums.length && nums[low] != val) {
            	count++;
                low++;
            }
            while (high >= 0 && nums[high] == val) {
                high--;
            }
            if (low <= high) {
                int temp = nums[low];
                nums[low] = nums[high];
                nums[high] = temp;
            }
        }
        
        return count;
    }
}
```

<br>


## Go
```go
func RemoveElement(nums []int, val int) int {
	if len(nums) == 0 {
		return 0
	}

	i, j := 0, 1
	for j < len(nums) {
		if nums[i] != val {
			i++
			j++
			continue
		}
		for ; j < len(nums) && nums[j] == nums[i]; j++ {
		}
		if j == len(nums) {
			break
		}
		nums[i], nums[j] = nums[j], nums[i]
		i++
		j++
	}

	if nums[i] == val {
		return i
	}

	return i + 1
}
```

```go
func RemoveElement(nums []int, val int) int {
	i, j := 0, len(nums) - 1
	for i <= j {
		if nums[i] == val {
			nums[i] = nums[j--]
		} else {
			i++
		}
	}

	return j + 1
}
```

<br>


## Python
```python
class RemoveDup:
    def remove_element(self, nums, val: int) -> int:
        i, j = 0, len(nums) - 1
        while i <= j:
            if nums[i] == val:
                nums[i] = nums[j]
                j -= 1
            else:
                i += 1

        return j + 1
```
