# <center>98 - Validate Binary Search Tree (M)</center> 



<br></br>

* Tag: Recursion, Binary Search Tree
* Difficulty: Medium
* Company: Facebook, Microsoft, Amazon
* Link: https://leetcode.com/problems/validate-binary-search-tree/

<br></br>



## Description
----
Given a binary tree, determine if it is a valid binary search tree (BST).

<br></br>



## Solution
----
There are 2 ways:
1. Recursion - Just follow the pattern of BST to valid each node value.
2. In-order - The in-order sequence should be an sorted array.

<br>


### Java
```java
public class IsBST {
	/**
     * @param root: The root of binary tree.
     * @return: True if the binary tree is BST, or false
     */
	public boolean isBST1(TreeNode cur)  {
		return helper1(cur, Integer.MIN_VALUE, Integer.MAX_VALUE);
	}
	 
	    
	private boolean helper1(TreeNode node, int min, int max){
	    if (node == null)
	    	return true;
	 
	    if (node.val < min || node.val > max)
	    	return false;
		
	    return helper1(node.left, min, node.val - 1) && helper1(node.right, node.val + 1, max);
	}
	
	/**
     * @param root: The root of binary tree.
     * @return: True if the binary tree is BST, or false
     */
	// Time: O(n) Space: O(n)
	public boolean isBST2(TreeNode root) {
	    return helper2(root, null, null);
	 }
	
	private boolean helper2(TreeNode node, Integer lower, Integer upper) {
	    if (node == null)
	    	return true;
	    if (lower != null && node.val <= lower)
	    	return false;
	    if (upper != null && node.val >= upper)
	    	return false;
	    if (!helper2(node.right, node.val, upper) || !helper2(node.left, lower, node.val))
	    	return false;
	    
	    return true;
	  }
	
	/**
     * @param root: The root of binary tree.
     * @return: True if the binary tree is BST, or false
     */
	// Time: O(n) Space: O(n)
	public boolean isBSTByInOrder(TreeNode cur) {
		ArrayList<TreeNode> al = new ArrayList<TreeNode>();
		nodesToList(cur, al);
		for(int i = 1; i < al.size(); i++)
			if(al.get(i).val <= al.get(i - 1).val) // Should be <=, not <
				return false;

		return true;
	}
	
	private void nodesToList(TreeNode cur, ArrayList<TreeNode> al) {
		if(cur != null) {
			nodesToList(cur.left, al);
			al.add(cur);
			nodesToList(cur.right, al);
		}
	}
}
```

<br>


### Python
```python
class IsBST:
    def solution1(self, root):  # Time: O(n) Space: O(n)
        """
        :type root: TreeNode
        :rtype: bool
        """
        def helper(node, lower=float('-inf'), upper=float('inf')):
            if not node:
                return True
            if node.val <= lower or node.val >= upper:
                return False
            if not helper(node.right, node.val, upper):
                return False
            if not helper(node.left, lower, node.val):
                return False

            return True

        return helper(root)

    def solution2(self, root):  # Time: O(n) Space: O(n)
        """
        :type root: TreeNode
        :rtype: bool
        """
        stack, inorder = [], float('-inf')

        while stack or root:
            while root:
                stack.append(root)
                root = root.left
            root = stack.pop()
            if root.val <= inorder:
                return False
            inorder = root.val
            root = root.right

        return True
```

<br>


### Go
```go
func IsBST(root *TreeNode) bool {
	if root == nil {
		return false
	}

	result := make([]int, 0)
	st := make([]*TreeNode, 0)
	n := root

	for n != nil || len(st) != 0 {
		for ; n != nil; n = n.Left {
			st = append(st, n)
		}
		if len(st) != 0 {
			n = st[len(st)-1]
			if len(result) != 0 && n.Val <= result[len(result)-1] {
				return false
			}
			st = st[:len(st)-1]
			result = append(result, n.Val)
		}
	}

	return true
}
```

```go
const (
	integerMinIsBST int64 = -21474836480
	integerMaxIsBST int64 = 21474836470
)

func IsBST(root *TreeNode) bool {
	return helper(root, integerMinIsBST, integerMaxIsBST)
}

func helper(root *TreeNode, lower, higher int64) bool {
	if root == nil {
		return true
	}
	v := int64(root.Val)
	if lower >= v {
		return false
	}
	if higher <= v {
		return false
	}
	if !helper(root.Right, v, higher) || !helper(root.Left, lower, v) {
		return false
	}

	return true
}
```

```go
const (
	integerMinIsBST int64 = -21474836480
	integerMaxIsBST int64 = 21474836470
)

func IsBST(root *TreeNode) bool {
	return isBSTHelper(root, integerMinIsBST, integerMaxIsBST)
}

func isBSTHelper(n *TreeNode, min, max int64) bool {
	if n == nil {
		return true
	}
	v := int64(n.Val)
	if v < min || v > max {
		return false
	}

	return isBSTHelper(n.Left, min, v-1) && isBSTHelper(n.Right, v+1, max)
}
```