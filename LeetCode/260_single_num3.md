# <center>260 - Single Number III (M)</center> 



<br></br>

* Tag: Bit
* Difficulty: Medium
* Link: https://leetcode.com/problems/single-number-iii/

<br></br>



## Description
----
Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

<br></br>



## Example
----
Input:  [1,2,1,3,2,5]
Output: [3,5]

<br></br>



## Solution
----
算法：
1. 需排除其他出现两次的数。可将所有数进行一遍异或，结果是两个出现一次的数的异或。
2. 因为两个单一出现的数不同，所以结果二进制中至少有一个1。说明在该位上，这两个不同的数有一个为1，一个为0。
3. 将数组在该位上是否为1分为两部分，第一个子数组中每个数字该位都为1，第二个子数组每个数字该位都为0。
4. 两子数组异或，结果是这两个单独的数。注意不是直接分组，而是按照条件查找，边查找编异或。

<br>


### Java
```java
public class SingleNum3 {
	public List<Integer> getNums(int[] data) {
		List<Integer> result = new ArrayList<Integer>();
		if (data == null || data.length % 2 != 0)
			return result;
		
		int mask = 0, num1 = 0, num2 = 0;
		for (int i = 0; i < data.length; i++)
			mask ^= data[i];
		int index = getMagicNum(mask);
		for (int i = 0; i < data.length; i++)
			if (check(data[i], index))
				num1 ^= data[i];
			else
				num2 ^= data[i];

		result.add(num1);
		result.add(num2);
		
		return result;
	}
	
	private boolean check(int data, int mask) {
		return (data & mask) == mask;
	}
	
	/*
	 * int型的最大值：2^{31} - 1 = 2147483647    16进制：0x7FFF FFFF
	 * int型的最小值：-2^{31} = -2147483648   16进制：0x8000 0000
	 */
	private int getMagicNum(int num) {
		if (num == 0)
			return 0;
		
		int indexBit = 1 << 30;
		while ((num & indexBit) == 0)
			indexBit >>= 1;
		
		return indexBit;
	}
}
```

<br>


### Python
```python
class SingleNum3:
    def solution(self, nums):
        result = []
        if not nums or len(nums) % 2 != 0:
            return result

        mask, a, b = 0, 0, 0
        for n in nums:
            mask ^= n
        mask = self.get_magic_num(mask)
        for n in nums:
            if self.check(n, mask):
                a ^= n
            else:
                b ^= n
        result.append(a)
        result.append(b)

        return result

    def check(self, num, mask):
        return (num & mask) == mask

    def get_magic_num(self, num):
        if num == 0:
            return num

        mask = 1 << 30
        while (mask & num) == 0:
            mask >>= 1

        return mask
```

<br>


### Go
```go
func SingleNum3(nums []int) (int, int) {
	num1, num2 := 0, 0
	if len(nums)%2 != 0 {
		return num1, num2
	}

	mask := 0
	for _, v := range nums {
		mask ^= v
	}
	index := getPos(mask)
	for _, v := range nums {
		if v^index == index {
			num1 ^= v
		} else {
			num2 ^= v
		}
	}

	return num1, num2
}

func getPos(mask int) int {
	if mask == 0 {
		return mask
	}
	index := 1 << 30
	for mask&index == 0 {
		index >>= 1
	}

	return index
}
```
