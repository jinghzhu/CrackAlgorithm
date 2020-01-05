# <center>641 - Missing Ranges (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Array
* Company: Google
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/missing-ranges

<br></br>



## Description
----
Given a sorted integer array where the range of elements are in the inclusive range [lower, upper], return its missing ranges.

<br></br>



## Example
----
Input: nums = [0, 1, 3, 50, 75], lower = 0 and upper = 99
Output: ["2", "4->49", "51->74", "76->99"]
Explanation: in range[0,99],the missing range includes:range[2,2],range[4,49],range[51,74] and range[76,99]

<br></br>



## Solution
----
算法：
1. 异常判断。
2. 如果nums为空，判断lower和upper区间。
3. 判断lower和nums[0]之间区间。
4. 对nums中任意紧邻的两个元素，判断它们之间的区间：
5. 判断nums[l - 1]和upper之间区间。

<br>


### Python
```python
class Solution:
    """
    @param: nums: a sorted integer array
    @param: lower: An integer
    @param: upper: An integer
    @return: a list of its missing ranges
    """
    def findMissingRanges(self, nums, lower, upper):
        res = []
        if lower > upper:
            return res
        l = len(nums)
        if l == 0:
            res.append(self.attach(lower - 1, upper + 1))
            return res
        if nums[0] > lower:
            res.append(self.attach(lower - 1, nums[0]))
        for i in range(l - 1):
            j = i + 1
            if nums[j] - nums[i] > 1:
                res.append(self.attach(nums[i], nums[j]))
        if upper > nums[l - 1]:
            res.append(self.attach(nums[l - 1], upper + 1))
            
        return res

    def attach(self, start, end):
        print(start)
        print(end)
        if end - start == 2:
            return str(end - 1)
        return str(start + 1) + "->" + str(end - 1)
```