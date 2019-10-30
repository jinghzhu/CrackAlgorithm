# <center>628 - Maximum Subtree (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Recursion, DFS, Binary Tree
* Company: Amazon
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/maximum-subtree

<br></br>



## Description
----
Given a binary tree, find the subtree with maximum sum. Return the root of the subtree.

<br></br>



## Example
----
Input: {1,-5,2,0,3,-4,-5}

Output: 3

The tree is look like this:

```
     1
   /   \
 -5     2
 / \   /  \
0   3 -4  -5
```

The sum of subtree 3 (only one node) is the maximum. So we return 3.

<br></br>



## Solution
----
### Go
```go
type ResultType struct {
    sum int
    max int
    node *TreeNode
}

func findSubtree (root *TreeNode) *TreeNode {
    r := helper(root)
    return r.node
}

func helper(root *TreeNode) *ResultType {
    if root == nil {
        return &ResultType{0, -2147483648, nil}
    }
    
    left, right := helper(root.Left), helper(root.Right)
    sum := root.Val + left.sum + right.sum
    max := maxInt(sum, left.max, right.max)
    n := root
    if max == left.max {
        n = left.node
    } else if max == right.max {
        n = right.node
    }
    
    return &ResultType{sum, max, n}
}

func maxInt(args ...int) int {
	max := args[0]
	for _, v := range args {
		if v > max {
			max = v
		}
	}

	return max
}
```

<br>
