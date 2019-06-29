# <center>548 - Intersection of Two Arrays (E)</center> 



<br></br>

* Tag: Array, Two Pointers, Sort, Hashmap
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Facebook
* Link: https://www.lintcode.com/problem/intersection-of-two-arrays-ii/description

<br></br>



## Description
----
Given two arrays, write a function to compute their intersection.

<br></br>



## Example
----
Input: nums1 = [1, 2, 2, 1], nums2 = [2, 2], 
Output: [2,2].

<br></br>



## Solution
----
### Java
```java
class Solution {
    public int[] intersect(int[] nums1, int[] nums2) {
        if (nums1 == null || nums2 == null)
            return new int[]{};
        
        HashMap<Integer, Integer> cache = new HashMap<Integer, Integer>();
        for (int i : nums1) {
            int num = 1;
            if (cache.containsKey(i))
                num = cache.get(i) + 1;
            cache.put(i, num);
        }
        ArrayList<Integer> result = new ArrayList<Integer>();
        for (int i : nums2) {
            if (!cache.containsKey(i))
                continue;
            result.add(i);
            int num = cache.get(i);
            if (num == 1)
                cache.remove(i);
            else
                cache.put(i, --num);
        }
        
        int[] arr = new int[result.size()];
        for (int i = 0; i < arr.length; i++)
            arr[i] = result.get(i);
        
        return arr;
    }
}
```

<br>


### Go
```go
func intersect(nums1 []int, nums2 []int) []int {
    result := make([]int, 0)
    cache := make(map[int]int)
    for _, v := range nums1 {
        count, ok := cache[v]
        if !ok {
            cache[v] = 1
        } else {
            cache[v] = count + 1
        }
    }
    for _, v := range nums2 {
        if count, ok := cache[v]; ok {
            result = append(result, v)
            if count == 1 {
                delete(cache, v)
            } else {
                cache[v] = count - 1
            }
        }
    }
    
    return result
}
```
