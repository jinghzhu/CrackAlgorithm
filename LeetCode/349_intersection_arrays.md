# <center>349 - Intersection of Two Arrays (E)</center> 



<br></br>

* Tag: Array, Two Pointers, Sort, Hashmap
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Uber
* Link: https://leetcode.com/problems/intersection-of-two-arrays/

<br></br>



## Description
----
Given two arrays, write a function to compute their intersection.

<br></br>



## Example
----
Input: nums1 = [1, 2, 2, 1], nums2 = [2, 2], 
Output: [2].

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param nums1 an integer array
     * @param nums2 an integer array
     * @return an integer array
     */
    public int[] intersection1(int[] nums1, int[] nums2) {
        if (nums1 == null || nums2 == null)
            return null;
        
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        
        int i = 0, j = 0, index = 0;
        int[] temp = new int[nums1.length];

        while (i < nums1.length && j < nums2.length) {
            if (nums1[i] == nums2[j]) {
                if (index == 0 || temp[index - 1] != nums1[i])
                    temp[index++] = nums1[i];
                i++;
                j++;
            } else if (nums1[i] < nums2[j])
                i++;
            else
                j++;
        }
        
        int[] result = new int[index];
        for (int k = 0; k < index; k++)
            result[k] = temp[k];
        
        return result;
    }

    public int[] intersection2(int[] nums1, int[] nums2) {
        if (nums1 == null || nums2 == null)
            return null;
        
        HashSet<Integer> hash = new HashSet<>();
        for (int i = 0; i < nums1.length; i++)
            hash.add(nums1[i]);
        
        HashSet<Integer> resultHash = new HashSet<>();
        for (int i = 0; i < nums2.length; i++)
            if (hash.contains(nums2[i]) && !resultHash.contains(nums2[i]))
                resultHash.add(nums2[i]);
        
        int index = 0;
        int[] result = new int[resultHash.size()];
        for (Integer num : resultHash)
            result[index++] = num;
        
        return result;
    }
}
```

<br>


### Go
```go
func intersection(nums1 []int, nums2 []int) []int {
    result := make([]int, 0)
    cache, visited := make(map[int]int), make(map[int]int)
    for _, v := range nums1 {
        cache[v] = 1
    }
    for _, v := range nums2 {
        if _, ok := cache[v]; ok {
            if _, ok2 := visited[v]; !ok2 {
                result = append(result, v)
                visited[v] = 1
            }
        }
    }
    
    return result
}
```
