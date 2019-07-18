# <center>56 - Two Sums (E)</center> 



<br></br>

* Tag: Hash, Sort, Two Pointers, Array
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Apple, LinkedIn, AirBnB, Facebook, Microsoft, Uber, Amazon
* Link: https://www.lintcode.com/problem/two-sum/description

<br></br>



## Description
----
Given an array of integers, find two numbers such that they add up to a specific target number.

The function `twoSum` should return indices of the two numbers such that they add up to the target, where `index1` must be less than `index2`.

<br></br>



## Example
----
Given `nums = [2, 7, 11, 15]`, `target = 9`,

return `[0, 1]`.

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param numbers: An array of Integer
     * @param target: target = numbers[index1] + numbers[index2]
     * @return: [index1, index2] (index1 < index2)
     */
    public int[] twoSum(int[] nums, int target) {
        if (nums == null || nums.length < 2)
			return nums;
		
		int[] result = new int[2];
        HashMap<Integer, ArrayList<Integer>> datas = new HashMap<Integer, ArrayList<Integer>>();
        long target_l = (long)target;

        for (int i =0; i < nums.length; i++) {
        	ArrayList<Integer> l = null;
        	int key = nums[i];
        	if (datas.containsKey(key))
        		l = datas.get(key);
        	else
        		l = new ArrayList<Integer>();
        	l.add(i);
        	datas.put(key,  l);
        }

        for (int i =0; i < nums.length; i++) {
        	int i_val = nums[i];
        	long i_val_l = (long) i_val;
        	long j_val_l = target_l - i_val_l;
        	if (j_val_l < (long)Integer.MIN_VALUE || j_val_l > (long)Integer.MAX_VALUE)
        		continue;
        	int j_val = (int) j_val_l;
        	if (!datas.containsKey(j_val))
        		continue;
        	ArrayList<Integer> values = datas.get(j_val);
        	int j_index = 0;
        	for (; j_index < values.size(); j_index++) {
        		int j = values.get(j_index);
        		if (i == j)
        			continue;
        		result[0] = i;
        		result[1] = j;
        		break;
        	}
        	if (j_index != values.size())
        		break;
        }
        
        if (result[0] > result[1]) {
        	int temp = result[0];
        	result[0] = result[1];
        	result[1] = temp;
        }
        
        return result;
    }
}
```

<br>


### Python
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        d = {}
        for i in range(len(nums)):
            wanted = target - nums[i]
            if wanted in d:
                return [d[wanted], i]
            d[nums[i]] = i
        
        return []
```

<br>


### Go
```go
func twoSum(nums []int, target int) []int {
    if len(nums) < 2 {
		return nums
	}
	result := make([]int, 2, 2)
	sort(nums)
	i, j := 0, len(nums)-1
	for i != j {
		if nums[i]+nums[j] > target {
			j--
			continue
		}
		if nums[i]+nums[j] < target {
			i++
			continue
		}
		result[0], result[1] = i, i
	}

	return result
}

func sort(data []int) {
	if len(data) < 2 {
		return
	}

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
	data[i + 1], data[end] = data[end], data[i + 1] // Swap arr[i+1] and arr[high] (or pivot)

	return i + 1 // Pivot is at i + 1
}
```
