# <center>1157 - Shortest Unsorted Continuous Subarray (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Array, Sort, Two Pointers
* Difficulty: Easy
* Company: Google
* Link: https://www.lintcode.com/problem/shortest-unsorted-continuous-subarray/description

<br></br>



## Description
----
Given an integer array, you need to find one continuous subarray that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the shortest such subarray and output its length.

<br></br>



## Example
----
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.

<br></br>



## Solution
----
There are 2 ways to solve it:
1. Sort - Time: O(nlog) Space: O(n)

    Steps:
	1. Copy to an new array.
	2. Sort the new array.
	3. Count all different elements at same position between these 2 arrays.

2. Two pointers - Time: O(n) Space: O(1)

    Steps:
    1. Use two pointers to mark the start and end position of the unsorted subarray.
    2. For each time it encounters an unsorted pair:
        1. It sets the start position. Remember if it may need to update the start position. For example, [80,90,93,100,92,99,85,110,70,120]. The first unsorted pair is 90-100, its start postion is element 90. For unsorted pair 110-70, it needs to update the start position at element 80.
        2. When it encounters an unsorted pair, this means it enters an unsorted subarray. Assume the unsorted pair is at [i-1,i]. The candidate end position is the element whose value is >= nums[i-1].
    
<br>


### Java
```java
public class ShortestUnsortedContinuousSubarr {
	public int solution1(int[] nums) {
        int[] tmp = Arrays.copyOf(nums, nums.length);
        Arrays.sort(tmp);
        int start = -1, end = -1;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != tmp[i]) {
                if (start == -1) // Only need to set start once.
                    start = i;
                // Important: end should be the LAST position that the element value is
                // different between 2 arrays.
                // Image that [1,3,2,4,6,5] vs [1,2,3,4,5,6], the element 4 is at the
                // correct position, but the end position should be the last element.
                for (end = i + 1; end < nums.length && nums[end] != tmp[end]; end++){}
            }
        }
        
        return end - start;
    }
	
	public int solution2(int[] nums) {
        if (nums == null)
            return 0;
        
        int start = -1;
        int end = -1;
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] >= nums[i - 1])
                continue;
            start = getStart(nums, start, i);
            for (end = i; end < nums.length && nums[end] < nums[i - 1]; end++)
                start = getStart(nums, start, end);
            i = end;
        }
        
        return end - start;
    }
    
    private int getStart(int[] nums, int start, int end) {
        if (start == -1)
            start = end;
        for (; start > 0 && nums[start -1] > nums[end]; start--) {}
        
        return start;
    }
}
```

<br>


### Go
```go
func FindUnsortedSubarray(nums []int) int {
	if len(nums) < 2 {
		return 0
	}
	tmp := make([]int, len(nums), len(nums))
	copy(tmp, nums)
	sort.Ints(tmp)
	start, end := -1, -1
	for k, v := range tmp {
		if v != nums[k] {
			end = k
			if start == -1 {
				start = k
			}
		}
	}

	if start == -1 {
		return 0
	}

	return end - start + 1
}
```

```go
func FindUnsortedSubarray(nums []int) int {
	start, end, i := -1, -1, 1
	for i < len(nums) {
		if nums[i] >= nums[i-1] {
			i++
			continue
		}
		start = getStart(nums, start, i)
		for end = i; end < len(nums); end++ {
			if nums[end] >= nums[i-1] {
				break
			}
			start = getStart(nums, start, end)
		}
		i = end
	}
	return end - start
}

func getStart(nums []int, start, end int) int {
	if start == -1 {
		start = end
	}
	for ; start > 0 && nums[start-1] > nums[end]; start-- {
	}
	return start
}
```

<br>


### Python
----
```python
class ShortestUnsortedSubarr:
    def solution1(self, nums) -> int:
        tmp = sorted(nums)
        start, end = -1, -1
        for i in range(len(nums)):
            if nums[i] != tmp[i]:
                end = i
                if start == -1:
                    start = i
        if start == -1:
            return 0
        return end - start + 1

    def solution2(self, nums):
        start, end, i = -1, -1, 1
        while i < len(nums):
            if nums[i] >= nums[i - 1]:
                i += 1
                continue
            start = self.getStart(nums, start, i)
            end = i
            while end < len(nums) and nums[end] < nums[i - 1]:
                start = self.getStart(nums, start, end)
                end += 1
            i = end
        return end - start

    def getStart(self, nums, start, end):
        if start == -1:
            start = end
        while start > 0 and nums[start - 1] > nums[end]:
            start -= 1
        return start
```