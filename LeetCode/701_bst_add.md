# <center>701 - Insert into a Binary Search Tree (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: BST, Recursion
* Difficulty: Medium
* Link: https://leetcode.com/problems/insert-into-a-binary-search-tree/

<br></br>



## Description
----
Given the root node of a binary search tree (BST) and a value to be inserted into the tree, insert the value into the BST. Return the root node of the BST after the insertion. It is guaranteed that the new value does not exist in the original BST.

Note that there may exist multiple valid ways for the insertion, as long as the tree remains a BST after insertion. You can return any of them.

<br></br>



## Example
----
Given the tree:

```
        4
       / \
      2   7
     / \
    1   3
```

And the value to insert: 5

You can return this binary search tree:

```
         4
       /   \
      2     7
     / \   /
    1   3 5
```

This tree is also valid:

```
         5
       /   \
      2     7
     / \   
    1   3
         \
          4
```

<br></br>



## Solution
----
### Go
```go
func InsertIntoBST1(root *TreeNode, val int) *TreeNode {
	target := &TreeNode{
		Val: val,
	}
	return iiBST1Helper(root, target)
}

func iiBST1Helper(root, target *TreeNode) *TreeNode {
	if root == nil {
		return target
	}
	if root.Val > target.Val {
		root.Left = iiBST1Helper(root.Left, target)
	} else {
		root.Right = iiBST1Helper(root.Right, target)
	}

	return root
}
```

```go
func InsertIntoBST2(root *TreeNode, val int) *TreeNode {
	target := &TreeNode{
		Val: val,
	}
	targetPar := iiBST2Helper(root, val)
	if targetPar == nil {
		root = target
	} else if targetPar.Val > val {
		targetPar.Left = target
	} else {
		targetPar.Right = target
	}

	return root
}

func iiBST2Helper(root *TreeNode, val int) *TreeNode {
	var par, cur *TreeNode = nil, root
	for cur != nil {
		par = cur
		if cur.Val > val {
			cur = cur.Left
		} else {
			cur = cur.Right
		}
	}

	return par
}
```

<br>


### Java
```java
public class AddNode {
	public TreeNode solution1(TreeNode root, TreeNode node) {
        if (root == null) {
            root = node;
            return root;
        }
        TreeNode tmp = root;
        TreeNode last = null;
        while (tmp != null) {
            last = tmp;
            if (tmp.val > node.val) {
                tmp = tmp.left;
            } else {
                tmp = tmp.right;
            }
        }
        if (last != null) {
            if (last.val > node.val) {
                last.left = node;
            } else {
                last.right = node;
            }
        }
        return root;
    }
	
	public TreeNode solution2(TreeNode root, TreeNode node) {
        if (root == null)
            return node;
 
        if (root.val > node.val)
            root.left = solution2(root.left, node);
        else
            root.right = solution2(root.right, node);
   
        return root;
    }
}
```

<br>


### Python
```python
class AddNode:
    def solution(self, root, node):
        if not root:
            return node
        if root.val > node.val:
            root.left = self.insertNode(root.left, node)
        else:
            root.right = self.insertNode(root.right, node)

        return root
```