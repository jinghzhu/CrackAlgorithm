# <center>973 - K Closest Points to Origin (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Hash, Heap
* Difficulty: Medium
* Company: Amazon, LinkedIn, Microsoft
* Link: https://leetcode.com/problems/k-closest-points-to-origin/

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

func kClosest(points [][]int, k int) [][]int {
    result := make([][]int, 0, k)
    if len(points) < 1 || k < 1 {
        return result
    }
    m := calculate(points)
    keys := make([]int, 0)
    for k1 := range m {
        keys = append(keys, k1)
    }
    sort.Ints(keys)
    i := 0
    for j := 0; j < len(keys) && i < k; j++ {
        points := m[keys[j]]
        sort1(points, 0, len(points) - 1)
        fmt.Println("Print points")
        for m := 0; m < len(points) && i < k; m++ {
            result = append(result, points[m])
            i++
        }
    }
    
    return result
}

func calculate(points [][]int) map[int][][]int {
    m := make(map[int][][]int)
    for _, p := range points {
        d := p[0] * p[0]  + p[1] * p[1]
        v, ok := m[d]
        if !ok {
            v = make([][]int, 0)
        }
        m[d] = append(v, p)
    }
    
    return m
}

func sort1(points [][]int, start, end int) {
    if start < 0 || end >= len(points) || start >= end {
        return
    }
    p := getPartition(points, start, end)
    sort1(points, start, p - 1)
    sort1(points, p + 1, end)
}

func getPartition(points [][]int, start, end int) int {
    if start < 0 || end >= len(points) || start >= end {
        return -1
    }
    i := start - 1
    pivot := points[end]
    for j := start; j < end; j++ {
        if points[j][0] < pivot[0] || (points[j][0] == pivot[0] && points[j][1] <= pivot[1]) {
            i++
            points[i], points[j] = points[j], points[i]
        }
    }
    points[i + 1], points[end] = points[end], points[i + 1]
    
    return i + 1
}

```

<br>
