# <center>448 - Find All Numbers Disappeared in an Array (E)</center>



<br></br>

* Tag: Hash, Array
* Difficulty: Easy
* Company: Google
* Link: https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/

<br></br>



## Description
Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.

Find all the elements of [1, n] inclusive that do not appear in this array.

<br></br>



## Example
Input: [4,3,2,7,8,2,3,1]

Output: [5,6]

<br></br>



## Solution
### Java
```java
public class FindAllNumDisappearedInArr {
	/**
     * @param nums: a list of integers
     * @return: return a list of integers
     */
	public List<Integer> findDisappearedNumbers(int[] nums) {
        ArrayList<Integer> res = new ArrayList<Integer>();
        if (nums == null || nums.length < 1)
            return res;
        int[] visited = new int[nums.length];
        for (int i : nums)
            visited[i - 1] = -1;
        for (int i = 0; i < visited.length; i++)
            if (visited[i] != -1)
                res.add(i + 1);
        return res;
    }
}
```

<br>


### Python
```python
class Solution:
    def findDisappearedNumbers(self, nums: List[int]) -> List[int]:
        return [i for i in range(1, len(nums) + 1) if i not in set(nums)]
```