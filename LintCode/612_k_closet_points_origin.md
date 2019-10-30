# <center>612 - K Closest Points to Origin (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Hash, Heap
* Difficulty: Medium
* Company: Amazon, LinkedIn, Microsoft
* Link: https://www.lintcode.com/problem/k-closest-points

<br></br>



## Description
----
Given some points and an origin point in two-dimensional space, find k points which are nearest to the origin.
Return these points sorted by distance, if they are same in distance, sorted by the x-axis, and if they are same in the x-axis, sorted by y-axis.

<br></br>



## Example
----
Input: points = [[4,6],[4,7],[4,4],[2,5],[1,1]], origin = [0, 0], k = 3 
Output: [[1,1],[2,5],[4,4]]

<br></br>



## Solution
----
### Go
```go
import (
    "sort"
)

func kClosest (points []*Point, origin *Point, k int) []*Point {
    result := make([]*Point, 0, k)
    if len(points) < 1 || k < 1 {
        return result
    }
    m := calculate(points, origin)
    keys := make([]int, 0)
    for k1 := range m {
        keys = append(keys, k1)
    }
    sort.Ints(keys)
    i := 0
    for j := 0; j < len(keys) && i < k; j++ {
        points := m[keys[j]]
        sort1(points, 0, len(points) - 1)
        for m := 0; m < len(points) && i < k; m++ {
            result = append(result, points[m])
            i++
        }
    }
    
    return result
}

func calculate(points []*Point, origin *Point) map[int][]*Point {
    m := make(map[int][]*Point)
    for _, p := range points {
        d := abs(p.X - origin.X) *abs(p.X - origin.X)  + abs(p.Y - origin.Y) * abs(p.Y - origin.Y)
        v, ok := m[d]
        if !ok {
            v = make([]*Point, 0)
        }
        m[d] = append(v, p)
    }
    
    return m
}

func sort1(points []*Point, start, end int) {
    if start < 0 || end >= len(points) || start >= end {
        return
    }
    p := getPartition(points, start, end)
    sort1(points, start, p - 1)
    sort1(points, p + 1, end)
}

func getPartition(points []*Point, start, end int) int {
    if start < 0 || end >= len(points) || start >= end {
        return -1
    }
    i := start - 1
    pivot := points[end]
    for j := start; j < end; j++ {
        if points[j].X < pivot.X || (points[j].X == pivot.X && points[j].Y <= pivot.Y) {
            i++
            points[i], points[j] = points[j], points[i]
        }
    }
    points[i + 1], points[end] = points[end], points[i + 1]
    
    return i + 1
}

func abs(a int) int {
    if a < 0 {
        a *= -1
    }
    return a
}
```

<br>


### Java
```java
public class Solution {
    /**
     * @param points a list of points
     * @param origin a point
     * @param k an integer
     * @return the k closest points
     */
    private Point global_origin = null;
    public Point[] kClosest(Point[] points, Point origin, int k) {
        // Write your code here
        global_origin = origin;
        PriorityQueue<Point> pq = new PriorityQueue<Point> (k, new Comparator<Point> () {
            @Override
            public int compare(Point a, Point b) {
                int diff = getDistance(b, global_origin) - getDistance(a, global_origin);
                if (diff == 0)
                    diff = b.x - a.x;
                if (diff == 0)
                    diff = b.y - a.y;
                return diff;
            }
        });

        for (int i = 0; i < points.length; i++) {
            pq.offer(points[i]);
            if (pq.size() > k)
                pq.poll();
        }
        
        k = pq.size();
        Point[] ret = new Point[k];  
        while (!pq.isEmpty())
            ret[--k] = pq.poll();
        return ret;
    }
    
    private int getDistance(Point a, Point b) {
        return (a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y);
    }
}
```