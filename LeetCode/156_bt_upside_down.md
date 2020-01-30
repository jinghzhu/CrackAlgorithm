# <center>156 - Binary Tree Upside Down (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: BST, Recursion, DFS, Divide and Conquer
* Difficulty: Medium
* Company: LinkedIn
* Link: https://leetcode.com/problems/binary-tree-upside-down

<br></br>



## Description
----
Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

<br></br>



## Example
----
Input: {1,2,3,4,5}

Output: {4,5,2,#,#,3,1}

The input is:

```
    1
   / \
  2   3
 / \
4   5
```

and the output is:

```
   4
  / \
 5   2
    / \
   3   1
```

<br></br>



## Solution
----
算法：基于resultType的分治法。
1. 定义resultType，其表示当前已翻转完的子树，其中root为当前子树根结点，rightChild为最右边的叶子结点。
2. 只按访问左节点顺序进行DFS，对每次返回的resultType，按如下规则翻转：
    1. r.root即为新子树的根结点。
    2. 老root作为新子树的rightChild。
    3. 老root.Left为新子树的的rightChild的兄弟。

<br>


### Go
```go
type udResultType struct {
	root       *TreeNode
	rightChild *TreeNode
}

// GetUpsideDownBT flips it upside down.
func GetUpsideDownBT(root *TreeNode) *TreeNode {
	return udHelper(root).root
}

func udHelper(root *TreeNode) *udResultType {
	if root == nil {
		return &udResultType{}
	}
	if root.Left == nil {
		return &udResultType{root, root}
	}

	r := udHelper(root.Left)
	r.rightChild.Right = root
	r.rightChild.Left = root.Right
	root.Left, root.Right = nil, nil
	r.rightChild = r.rightChild.Right

	return r
}
```

<br>
