# <center>285 - Inorder Successor in BST (M)</center> 



<br></br>

* Tag: BST, Recursion
* Difficulty: Medium
* Company: Microsoft, Facebook
* Link: https://leetcode.com/problems/inorder-successor-in-bst

<br></br>



## Description
----
Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

If the given node has no in-order successor in the tree, return null.

<br></br>



## Example
----
Input: {1,#,2}, node with value 1 Output: 2
      1
       \
        2

Input: {2,1,3}, node with value 1 Output: 2
        2
       / \
      1   3


<br></br>



## Solution
----
https://www.jiuzhang.com/solution/inorder-successor-in-bst

首先确定中序遍历的后继：
1. 如果该节点有右子节点，后继是其右子节点子树中最左端节点。
2. 如果该节点没右子节点，后继是离它最近的祖先，该节点在这个祖先的左子树内。

循环实现：
1. 查找该节点，并在过程中维护上述性质的祖先节点。
2. 找到后，如果该节点有右节点，后继在右子树；否则后继是维护的祖先节点。

递归实现：
1. 如果根节点小于或等于查找的节点,，进入右子树递归。
2. 如果根节点大于查找的节点，暂存左子树递归查找的结果。如果是null，返回当前根节点；反之返回左子树递归查找的结果。
3. 递归中，暂存左子树递归查找的结果相当于循环实现中维护的祖先节点。

<br>


### Java
```java
public class GetInorderSuccessor {
	public TreeNode solution1(TreeNode root, TreeNode p) {
		if (root == null || p == null)
			return null;
		
        TreeNode successor = null;
        while (root != null && root.val != p.val) {
            if (root.val > p.val) {
            	// 维护的祖先节点。如果目标节点有右子树，容易在右子树获得。没有的话，
            	// 得在其所在的子树中查找最近的祖先节点。successor就是维护祖先信息。
                successor = root;
                root = root.left;
            } else {
                root = root.right;
            }
        }
        
        if (root == null) 
            return null;
        if (root.right == null) 
            return successor;
        
        root = root.right;
        while (root.left != null) 
            root = root.left;
        
        return root;
    }
	
	public TreeNode solution2(TreeNode root, TreeNode p) {
        TreeNode cand = null;
        
        while (root != null) {
            if (p.val >= root.val) {
                root = root.right;
            } else {
                cand = root;
                root = root.left;
            }
        }
        
        return cand;
    }
	
	public TreeNode solution3(TreeNode root, TreeNode p) {
        if (root == null || p == null)
            return null;
        
        // node的successor在右边。
        if (root.val <= p.val) 
            return solution3(root.right, p);
        
        // successor在左边或root本身。从root.left开始找，找得到返回值，找不到返回root。
        TreeNode left = solution3(root.left, p);
        return left != null ? left : root;
    }
}
```

<br>


### Python
```python
class GetInorderSuccessor:
    """
    @param: root: The root of the BST.
    @param: p: You need find the successor node of p.
    @return: Successor of p.
    """
    def solution(self, root, p):
        if not root or not p:
            return None

        successor, n = None, root
        while n and n != p:
            if p.val < n.val:
                # 维护的祖先节点。如果目标节点有右子树，容易在右子树获得。没有的话，得在其所在的子树中查找最近的祖先节点。successor
                # 就是维护这个祖先信息。
                successor, n = n, n.left
            else:
                n = n.right

        if not n:
            return None
        if not n.right:
            return successor

        n = n.right
        while n.left:
            n = n.left
        return n

    """
    @param: root: The root of the BST.
    @param: p: You need find the successor node of p.
    @return: Successor of p.
    """
    def solution2(self, root, p):
        if not root or not p:
            return None

        successor = None
        while root and root != p:
            if root.val >= p.val:
                successor, root = root, root.left
            else:
                root = root.right

        return successor

    """
    @param: root: The root of the BST.
    @param: p: You need find the successor node of p.
    @return: Successor of p.
    """
    def solution3(self, root, p):
        if not root:
            return None

        # node的successor在右边。
        if root.val <= p.val:
            return self.solution3(root.right, p)

        # successor在左边或root本身。从root.left开始找，找得到返回值，找不到返回root。
        left = self.solution3(root.left, p)
        if left:
            return left
        return root
```

<br>


### Go
----
```go
func GetInorderSuccessor1(root, p *TreeNode) *TreeNode {
	if root == nil || p == nil {
		return nil
	}
	var successor *TreeNode
	for root != nil && root != p {
		if root.Val > p.Val {
			successor, root = root, root.Left
		} else {
			root = root.Right
		}
	}
	if root == nil {
		return nil
	}
	if root.Right == nil {
		return successor
	}

	for root = root.Right; root.Left != nil; root = root.Left {
	}
	return root
}
```

```go
func GetInorderSuccessor2(root, p *TreeNode) *TreeNode {
	if root == nil || p == nil {
		return nil
	}

	var successor *TreeNode
	for root != nil && root != p {
		if root.Val >= p.Val {
			successor, root = root, root.Left
		} else {
			root = root.Right
		}
	}

	return successor
}
```

```go
func GetInorderSuccessor3(root, p *TreeNode) *TreeNode {
	if root == nil || p == nil {
		return nil
	}

	// node的successor在右边。
	if root.Val <= p.Val {
		return GetInorderSuccessor3(root.Right, p)
	}

	// successor在左边或root本身。从root.left开始找，找得到返回值，找不到返回root。
	left := GetInorderSuccessor3(root.Left, p)
	if left == nil {
		return root
	}
	return left
}
```