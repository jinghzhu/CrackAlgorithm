# <center>128 - Longest Consecutive Sequence (H)</center> 



<br></br>

* Tag: Hash
* Company: Google, Facebook, LinkedIn
* Difficulty: Hard
* Link: https://leetcode.com/problems/longest-consecutive-sequence/

<br></br>



## Description
----
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

<br></br>



## Example
----
- Input: [100, 4, 200, 1, 3, 2]
- Output: 4
- Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.

<br></br>



## Solution
----
算法：
1. 把所有元素放入哈希表。
2. 遍历哈希表，对每个元素依次值加（减）一，并检查是否在哈希表中。如果存在，移出哈希表。

<br>


### Java
```java
public class GetLongestConsecutiveSeq {
	public int solution(int[] nums) {
        HashSet<Integer> set = new HashSet<>();
        for (int i = 0; i < nums.length; i++) {
            set.add(nums[i]);
        }
        
        int longest = 0;
        for (int i = 0; i < nums.length; i++) {
            int down = nums[i] - 1;
            while (set.contains(down)) {
                set.remove(down);
                down--;
            }
            
            int up = nums[i] + 1;
            while (set.contains(up)) {
                set.remove(up);
                up++;
            }
            
            longest = Math.max(longest, up - down - 1);
        }
        
        return longest;
    }
}
```

<br>


### Go
```go
func GetLongestConsecutiveSeq(nums []int) int {
	m := make(map[int]bool)
	for _, v := range nums {
		m[v] = true
	}
	res, down, up := 1, 0, 0
	for k := range m {
		for i := k + 1; m[i]; i++ {
			up++
			delete(m, i)
		}
		for i := k - 1; m[i]; i-- {
			down++
			delete(m, i)
		}
		delete(m, k)
		if 1+up+down > res {
			res = 1 + up + down
		}
		down, up = 0, 0 // important
	}

	return res
}
```

<br>


### Python
```python
class GetLongestConsecutiveSeq:
    """
    @param num, a list of integer
    @return an integer
    """
    def solution(self, num):
        dict = {}

        for x in num:
            dict[x] = 1

        ans = 0

        for x in num:
            if x in dict:
                len = 1
                del dict[x]
                l = x - 1
                r = x + 1
                while l in dict:
                    del dict[l]
                    l -= 1
                    len += 1
                while r in dict:
                    del dict[r]
                    r += 1
                    len += 1
                if ans < len:
                    ans = len

        return ans
```