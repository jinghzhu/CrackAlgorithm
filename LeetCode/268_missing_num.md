# <center>268 - Missing Number (M)</center> 



<br></br>

* Author: Jianlan Zhou, <jianlanz@outlook.com>
* Tag: Sort, Math, Bit
* Difficulty: Medium
* Company: Microsoft, Facebook
* Link: https://leetcode.com/problems/missing-number/

<br></br>



## Description
----
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.

<br></br>



## Example
----
Input: [3,0,1]
Output: 2

<br></br>



## Solution
----
There are 3 ways:
1. Sort it and find the missing number.
2. Employ the Gauss' Formula.
3. Use the XOR.
4. Hash.

<br>


### Java
```java
public class MissNum {
	/**
     * @param nums: An array of integers
     * @return: An integer
     */
	// 把数组0到n间完整的数组异或，那么相同的数字都变为0，剩下就是少了的那个数字。
	// We can harness the fact that XOR is its own inverse to find the missing
	// element in linear time. Because we know that nums contains nnn numbers and
	// that it is missing exactly one number on the range [0..n−1][0..n-1][0..n−1],
	// we know that nnn definitely replaces the missing number in nums. Therefore,
	// if we initialize an integer to nnn and XOR it with every index and value, we
	// will be left with the missing number. Consider the following example (the values
	// have been sorted for intuitive convenience, but need not be):
	// Index 	0 	1 	2 	3
	// Value 	0 	1 	3 	4
	// missing=4∧(0∧0)∧(1∧1)∧(2∧3)∧(3∧4)
	//	      =(4∧4)∧(0∧0)∧(1∧1)∧(3∧3)∧2
	//	      =0∧0∧0∧0∧2
	//		  =2
	public int solution1(int a[]) { 
        int result = 0; 
        for(int i = 0 ; i < a.length ; i++)
            result ^= (i + 1) ^ a[i];
        
        return result; 
	} 

	/**
     * @param nums: An array of integers
     * @return: An integer
     */
	// 利用高斯求和公式。
	public int solution2(int a[]) {
		int sum = 0;
		for (int i : a)
			sum += i;
		
		return a.length / 2 * (a.length - 1) - sum;
	}
}
```

<br>


### Go
```go
// We can harness the fact that XOR is its own inverse to find the missing
// element in linear time. Because we know that nums contains nnn numbers and
// that it is missing exactly one number on the range [0..n−1][0..n-1][0..n−1],
// we know that nnn definitely replaces the missing number in nums. Therefore,
// if we initialize an integer to nnn and XOR it with every index and value, we
// will be left with the missing number. Consider the following example (the values
// have been sorted for intuitive convenience, but need not be):
// Index 	0 	1 	2 	3
// Value 	0 	1 	3 	4
// missing=4∧(0∧0)∧(1∧1)∧(2∧3)∧(3∧4)
//        =(4∧4)∧(0∧0)∧(1∧1)∧(3∧3)∧2
//		  =0∧0∧0∧0∧2
//	      =2
func FindMissingNum(a []int) int {
	result := 0
	for k, v := range a {
		// 把数组0到n间完整的数组异或，那么相同的数字都变为0，剩下就是少了的那个数字。
		// 之所以要k+1是因为result初始化为0。
		result ^= (k + 1) ^ v
	}

	return result
}
```

```go
func FindMissingNum(a []int) int {
	sum := 0
	for _, v := range a {
		sum += v
	}

	return len(a)/2*(len(a)-1) - sum // 利用高斯求和公式。
}
```

<br>


### Python
----
```python
class MissingNum:
    def solution1(self, nums):
        nums.sort()

        # Ensure that n is at the last index
        if nums[-1] != len(nums):
            return len(nums)
        # Ensure that 0 is at the first index
        elif nums[0] != 0:
            return 0

        # If we get here, then the missing number is on the range (0, n)
        for i in range(1, len(nums)):
            expected_num = nums[i-1] + 1
            if nums[i] != expected_num:
                return expected_num

    def solution2(self, nums):
        num_set = set(nums)
        n = len(nums) + 1
        for number in range(n):
            if number not in num_set:
                return number

    # We can harness the fact that XOR is its own inverse to find the missing element in linear time. Because we know
    # that nums contains n numbers and that it is missing exactly one number on the range [0..n−1], we know that n
    # definitely replaces the missing number in nums. Therefore, if we initialize an integer to n and XOR it with every
    # index and value, we will be left with the missing number.
    # Consider the following example (the values have been sorted for intuitive convenience, but need not be):
    # Index 0  1  2  3
    # Value 0  1  3  4
    # missing = 4∧(0∧0)∧(1∧1)∧(2∧3)∧(3∧4)
    #         =(4∧4)∧(0∧0)∧(1∧1)∧(3∧3)∧2
    #         =0∧0∧0∧0∧2
    #         =2
    def solution3(self, nums):
        missing = len(nums)
        for i, num in enumerate(nums):
            missing ^= i ^ num
        return missing

    def solution4(self, nums):
        expected_sum = len(nums) * (len(nums) + 1) // 2  # Gauss' Formula
        actual_sum = sum(nums)
        return expected_sum - actual_sum
```