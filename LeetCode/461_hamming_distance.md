# <center>461 - Hamming Distance (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Bit
* Company: Facebook, Google
* Difficulty: Easy
* Link: https://leetcode.com/problems/hamming-distance/

<br></br>



## Description
----
The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

<br></br>



## Example
----
Input: x = 1, y = 4
Output: 2
```
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
```
The above arrows point to positions where the corresponding bits are different.

<br></br>



## Solution
----
### Java
```java
public class No835HammingDistance {
	/**
     * @param x: an integer
     * @param y: an integer
     * @return: return an integer, denote Hamming Distance between two integers.
     */
	public int solution1(int x, int y) {
        int distance=0;
        
        while ( x != 0 || y != 0 ) {
            if ( x % 2 != y % 2 )
                distance++;
            x /= 2;
            y /= 2;
        }
        
        return distance;
    }
	
	/**
     * @param x: an integer
     * @param y: an integer
     * @return: return an integer, denote Hamming Distance between two integers.
     */
	public int solution2(int x, int y) {
        int count = 0;
        for (int z = x ^ y; z != 0; z = z >> 1) 
            if ((z & 1) == 1)
                count++;
        
        return count;
	}
}
```

<br>


### Python
```python
class HammingDistance:
    def solution1(self, x: int, y: int) -> int:
        """
        @param x: an integer
        @param y: an integer
        @return: return an integer, denote the Hamming Distance between two integers
        """
        count, z = 0, x ^ y
        while z != 0:
            if z & 1 == 1:
                count += 1
            z = z >> 1

        return count

    def solution2(self, x, y):
        """
        @param x: an integer
        @param y: an integer
        @return: return an integer, denote the Hamming Distance between two integers
        """
        count = 0
        while x != 0 or y != 0:
            if (x % 2) != (y % 2):
                count += 1
            x, y = x >> 1, y >> 1

        return count
```

<br>


### Go
```go
func HammingDistance(x, y int) int {
	result := 0
	for x != 0 || y != 0 {
		if x%2 != y%2 {
			result++
		}
		x /= 2
		y /= 2
	}

	return result
}
```

```go
func HammingDistance(x, y int) int {
    count := 0
    for z := x ^ y; z != 0; z = z >> 1 {
        if z & 1 == 1 {
            count++
        }
    }
    
    return count
}
```
