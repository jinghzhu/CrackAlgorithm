# <center>366 - Find Leaves of Binary Tree (M)</center> 



<br></br>

* Tag: BST, Recursion, DFS
* Difficulty: Medium
* Company: LinkedIn
* Link: https://leetcode.com/problems/find-leaves-of-binary-tree

<br></br>



## Description
----
Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

<br></br>



## Example
----
Input: {1,2,3,4,5}

Output: [[4, 5, 3], [2], [1]].

Explanation:
```
    1
   / \
  2   3
 / \     
4   5    
```

<br></br>



## Solution
----
算法：求出每个Node的height就是它在list中相对应的位置

<br>


### Go
```go
func GetLeaves(root *TreeNode) [][]int {
	m := make(map[int][]int)
	maxDepth := glDFS(root, m)
	res := make([][]int, 0)
	for i := 0; i < maxDepth; i++ {
		res = append(res, m[i])
	}

	return res
}

func glDFS(root *TreeNode, m map[int][]int) int {
	if root == nil {
		return 0
	}

	d := maxInt(glDFS(root.Left, m), glDFS(root.Right, m)) + 1
	list, ok := m[d]
	if !ok {
		list = make([]int, 0)
	}
	list = append(list, root.Val)
	m[d] = list

	return d
}
```

<br>
