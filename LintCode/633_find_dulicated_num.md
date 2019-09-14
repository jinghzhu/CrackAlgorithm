# <center>633 - Find the Duplicate Number (H)</center> 



<br></br>

* Tag: Linked List, Two Pointers, Sort, Math
* Difficulty: Hard
* Link: https://www.lintcode.com/problem/find-the-duplicate-number/description

<br></br>



## Description
----
Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

<br></br>



## Example
----
1. Input: [1,3,4,2,2] Output: 2
2. Input: [2,2,2] Output: 2

<br></br>



## Solution
----
3种解法：
1. 排序后查找。
2. 用哈希表。
3. 利用单链表求环入口法。
    如果看做Linked List，第i个位置值代表第i个点的下一个点是什么。能画出从0出发，共有n + 1个点的Linked List。这个Linked List一定存在环。因为无环的Linked List里非空next数目和节点的数目关系是差一个
    
    那么，证明了这是一个带环链表，要找的重复数，是两个点都指向同一个点作为next的那个点，即环入口。
	
    快慢指针算法：
	* 从起点出发，慢指针走一步，快指针走两步。因为有环，所以一定会相遇。
	* 相遇后，把其中一根指针拉回起点，重新走，这回快慢指针都各走一步。他们仍然会再次相遇，且相遇点为环的入口。

<br>


### Java
```java
public class GetDuplicateNum {
	public int solution1(int[] nums) {
        Arrays.sort(nums);
        for (int i = 1; i < nums.length; i++)
            if (nums[i] == nums[i-1])
                return nums[i];

        return -1;
    }
	
	public int solution2(int[] nums) {
        Set<Integer> seen = new HashSet<Integer>();
        for (int num : nums) {
            if (seen.contains(num))
                return num;
            seen.add(num);
        }

        return -1;
    }

	public int solution3(int[] nums) {
        if (nums.length <= 1)
            return -1;

        int slow = nums[0], fast = nums[nums[0]];
        while (slow != fast) {
            slow = nums[slow];
            fast = nums[nums[fast]];
        }

        fast = 0;
        while (fast != slow) {
            fast = nums[fast];
            slow = nums[slow];
        }
        
        return slow;
    }
}
```

<br>


### Go
```go
func FindDuplicate(nums []int) int {
	slow, fast := nums[0], nums[nums[0]]
	for slow != fast {
		slow, fast = nums[slow], nums[nums[fast]]
	}
	fast = 0
	for slow != fast {
		slow, fast = nums[slow], nums[fast]
	}

	return slow
}
```

<br>


### Python
----
```python
class GetDuplicatedNum:
    def solution1(self, nums):
        if len(nums) <= 1:
            return -1

        slow = nums[0]
        fast = nums[nums[0]]
        while slow != fast:
            slow = nums[slow]
            fast = nums[nums[fast]]

        fast = 0;
        while fast != slow:
            fast = nums[fast]
            slow = nums[slow]

        return slow

    def solution2(self, nums):
        nums.sort()
        for i in range(1, len(nums)):
            if nums[i] == nums[i - 1]:
                return nums[i]

    def solution3(self, nums):
        seen = set()
        for num in nums:
            if num in seen:
                return num
            seen.add(num)
```