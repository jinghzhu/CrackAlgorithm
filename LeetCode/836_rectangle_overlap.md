# <center>836 - Rectangle Overlap (E)</center> 



<br></br>

* Tag: Math
* Company: Amazon
* Difficulty: Easy
* Link: https://leetcode.com/problems/rectangle-overlap/

<br></br>



## Description
----
A rectangle is represented as a list [x1, y1, x2, y2], where (x1, y1) are the coordinates of its bottom-left corner, and (x2, y2) are the coordinates of its top-right corner.

Two rectangles overlap if the area of their intersection is positive.  To be clear, two rectangles that only touch at the corner or edges do not overlap.

Given two (axis-aligned) rectangles, return whether they overlap.

<br></br>



## Example
----
Input: rec1 = [0,0,2,2], rec2 = [1,1,3,3]

Output: true

<br></br>



## Solution
----
### Go
```go
func isRectangleOverlap(rec1 []int, rec2 []int) bool {
    if rec1[0] >= rec2[2] || rec2[0] >= rec1[2] {
        return false
    }
    if rec1[1] >= rec2[3] || rec2[1] >= rec1[3] {
        return false
    }
    return true
}
```

<br>
