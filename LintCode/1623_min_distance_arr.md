# <center>1623 - Minimal Distance In The Array (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Search
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/minimal-distance-in-the-array/description

<br></br>



## Description
----
Given two integer arrays a and b,please find the number in a with the minimal distance between corresponding value in b (the distance between two numbers means the absolute value of two numbers), if there are several numbers in a have same distance between b[i],just output the smallest number.

Finally, you should output an array of length b.length to represent the answer.

<br></br>



## Example
----
Example 1:
```
Input: a = [5,1,2,3], b = [2,4,2]
Output: [2,3,2]
Explanation: 
b[0]=2,2 is the number who has the minimal distance to 2
b[1]=4,3 and 5 have the same distance to 4,output 3 because 3 is smaller
b[2]=2,as well as b[0]
```

Example 2:
```
Input: a = [5,3,1,-1,6], b = [4,8,1,1]
Output: [3,6,1,1]
Explanation: 
b[0]=4, 3 and 5 have the same distance to 4,output 3 because 3 is smaller
b[1]=8, 6 is the number who has the minimal distance to 8
b[2]=1, 1 is the number who has the minimal distance to 1
```

<br></br>



## Solution
----
### Java
```java
public class GetMinDistanceInArray {
	/**
     * @param a: array a
     * @param b: the query array
     * @return: Output an array of length `b.length` to represent the answer
     */
    public int[] minimalDistance(int[] a, int[] b) {
        if (a == null || b == null || a.length == 0)
        	return null;
        int[] result = new int[b.length];
        Arrays.sort(a);
        for (int i = 0; i < b.length; i++)
        	result[i] = getMinDistance(a, b[i]);
        
        return result;
    }
    
    private int getMinDistance(int[] a, int target) {
    	int low = 0, high = a.length - 1;
    	while (low + 1 < high) {
    		int mid = (high - low) / 2 + low;
    		if (a[mid] == target)
    			return a[mid];
    		if (a[mid] > target)
    			high = mid;
    		else
    			low = mid;
    	}
    	int d1 = target - a[low], d2 = high - a[target];
    	if (d1 < 0)
    		d1 = d1 * (-1);
    	if (d2 < 0)
    		d2 = d2 * (-1);
    	
    	return d1 <= d2 ? a[low] : a[high]; // <= not <
    }
}
```

<br>
