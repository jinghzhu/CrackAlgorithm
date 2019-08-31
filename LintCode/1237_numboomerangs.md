# <center>1237 - Number of Boomerangs (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Google
* Tag: Hash
* Link: https://www.lintcode.com/problem/number-of-boomerangs/description

<br></br>



## Description
----
Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points (i, j, k) such that the distance between i and j equals the distance between i and k (the order of the tuple matters).

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).

<br></br>



## Solution
----
### Java
```java
public class BoomerangNumber {
	private int getDistance(int[] a, int[] b) {
        int dx = a[0] - b[0];
        int dy = a[1] - b[1];
        return dx * dx + dy * dy;
    }
    
    public int getBoomerangNum(int[][] points) {
        if (points == null)
            return 0;
        
        int ans = 0;
        for (int i = 0; i < points.length; i++) {
            Map<Integer, Integer> cache = new HashMap<>();
            for (int j = 0; j < points.length; j++) {
                if (i == j)
                    continue;
                int distance = getDistance(points[i], points[j]);
                int count = cache.getOrDefault(distance, 0);
                cache.put(distance, count + 1);
            }
            for (int count : cache.values()) // important
                ans += count * (count - 1);
        }
        
        return ans;
    }
}
```

<br>


### Python
```python
class BoomerangsNum:
    def getDistance(self, a, b):
        dx = a[0] - b[0]
        dy = a[1] - b[1]
        return dx * dx + dy * dy

    def solution(self, points):
        """
        :type points: List[List[int]]
        :rtype: int
        """
        if points == None:
            return 0
        ans = 0
        for i in range(len(points)):
            disCount = {}
            for j in range(len(points)):
                if i == j:
                    continue
                distance = self.getDistance(points[i], points[j])
                count = disCount.get(distance, 0)
                disCount[distance] = count + 1
            for distance in disCount:  # important
                ans += disCount[distance] * (disCount[distance] - 1)
        return ans
```

<br>


### Go
```go
func NumBoomerangs(points [][]int) int {
	l, result := len(points), 0
	if l < 3 {
		return 0
	}
	for k1, v1 := range points {
		m := make(map[int]int)
		for k2, v2 := range points {
			if k1 == k2 {
				continue
			}
			dis := calDistance(v1, v2)
			count, ok := m[dis]
			if ok {
				count++
			} else {
				count = 1
			}
			m[dis] = count
		}
		for k := range m {
			result += m[k] * (m[k] - 1)
		}
	}

	return result
}

func calDistance(a, b []int) int {
	i, j := a[0]-b[0], a[1]-b[1]
	return i*i + j*j
}
```