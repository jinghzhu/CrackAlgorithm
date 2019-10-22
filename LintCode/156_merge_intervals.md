# <center>156 - Merge Intervals (E)</center> 


<br></br>

* Tag: Sort, Array
* Company: LinkedIn, Google, Facebook, Microsoft, Twitter
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/merge-intervals/description

<br></br>



## Description
----
Given a collection of intervals, merge all overlapping intervals.

<br></br>



## Example
----
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]

<br></br>



## Solution
----
算法：先按start位进行排序，然后merge。

<br>


### Go
```go
func merge (intervals []*Interval) []*Interval {
    l, i := len(intervals), 0
    if l < 2 {
        return intervals // important
    }
    sort(intervals, 0, l - 1)
    for j := 1; j < l; j++ {
        if intervals[i].End < intervals[j].Start {
            i++
            intervals[i] = intervals[j]
        } else {
            intervals[i].End = maxInt(intervals[i].End, intervals[j].End)
        }
    }
    
    return intervals[:i + 1] // If slice is empty, here error bounds out of range.
}

func maxInt(a, b int) int {
    if a >= b {
        return a
    }
    return b
}

func sort(intervals []*Interval, start, end int) {
    if start < 0 || end >= len(intervals) || start >= end {
        return
    }
    p := getPartition(intervals, start, end)
    sort(intervals, start, p - 1)
    sort(intervals, p + 1, end)
}

func getPartition(intervals []*Interval, start, end int) int {
    if start < 0 || end >= len(intervals) || start >= end {
        return -1
    }
    i, pivot := start - 1, intervals[end]
    for j := start; j < end; j++ {
        if intervals[j].Start <= pivot.Start {
            i++
            intervals[i], intervals[j] = intervals[j], intervals[i]
        }
    }
    intervals[i + 1], intervals[end] = intervals[end], intervals[i + 1]
    
    return i + 1
}
```

<br>


### Python
```python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        l = len(intervals)
        self.sort(intervals, 0, l - 1)
        i = 0
        for j in range(1, l):
            if intervals[i][1] < intervals[j][0]:
                i += 1
                intervals[i] = intervals[j]
            else:
                intervals[i][1] = max(intervals[i][1], intervals[j][1])
        
        return intervals[:i + 1]
    
    def sort(self, intervals, start, end):
        if not intervals or start < 0 or end >= len(intervals) or start >= end:
            return
        p = self.get_partition(intervals, start, end)
        self.sort(intervals, start, p - 1)
        self.sort(intervals, p + 1, end)
    
    def get_partition(self, intervals, start, end):
        if not intervals or start < 0 or end >= len(intervals) or start >= end:
            return -1
        i, pivot = start - 1, intervals[end]
        for j in range(start, end):
            if intervals[j][0] <= pivot[0]:
                i += 1
                intervals[i], intervals[j] = intervals[j], intervals[i]
        intervals[i + 1], intervals[end] = intervals[end], intervals[i + 1]
        
        return i + 1
```