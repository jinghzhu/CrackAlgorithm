# <center>453 - Flatten Binary Tree to Linked List (E)</center> 


<br></br>

* Tag: Binary Tree, Recursion
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Microsoft
* Link: https://www.lintcode.com/problem/flatten-binary-tree-to-linked-list/description

<br></br>



## Description
----
Given a binary tree, flatten it to a linked list in-place.

<br></br>



## Example
----
For example, given the following tree:
```
    1
   / \
  2   5
 / \   \
3   4   6
```

The flattened tree should look like:
```
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
```

<br></br>



## Solution
----
三个算法：
1. 非递归前序遍历，设置一个dummy节点作为虚拟跟节点，然后一个tail节点标志前一个。
2. 非递归前序遍历，每次都把节点放入队列。然后正常出队，就能获得答案。
3. 递归。

<br>


### Go
```go
func Flatten1(root *TreeNode) {
	if root == nil {
		return
	}
	dummyRoot := &TreeNode{}
	n, tail := root, dummyRoot
	st := make([]*TreeNode, 0)
	for n != nil || len(st) > 0 {
		for ; n != nil; n = n.Left {
			if n.Right != nil {
				st = append(st, n.Right)
			}
			tail.Right = n
			tail.Left = nil
			tail = n
		}
		l := len(st)
		if l > 0 {
			n = st[l-1]
			st = st[:l-1]
		}
	}

	root = dummyRoot.Right
}
```

```go
func Flatten2(root *TreeNode) {
	if root == nil {
		return
	}
	dummyRoot := &TreeNode{}
	n := root
	st, q := make([]*TreeNode, 0), make([]*TreeNode, 0)
	for n != nil || len(st) > 0 {
		for ; n != nil; n = n.Left {
			q = append(q, n)
			if n.Right != nil {
				st = append(st, n.Right)
			}
		}
		l := len(st)
		if l > 0 {
			n = st[l-1]
			st = st[:l-1]
		}
	}
	n = dummyRoot
	for _, v := range q {
		v.Left, v.Right = nil, nil
		n.Right = v
		n = v
	}

	root = dummyRoot.Right
}
```

```go
func Flatten3(root *TreeNode) {
	fHelper(root)
}

func fHelper(root *TreeNode) *TreeNode {
	if root == nil {
		return root
	}

	left, right := fHelper(root.Left), fHelper(root.Right)
	if left != nil {
		left.Right = root.Right
		root.Right = root.Left
		root.Left = nil
	}
	if right != nil {
		return right
	}
	if left != nil {
		return left
	}

	return root
}
```

<br>


### Java
```java
public class Convert2LinkedList {
	/**
     * @param root the root of the binary tree
     * @return: nothing
     */
    public void flattenN(TreeNode root) {
        Stack<TreeNode> st = new Stack<TreeNode>();
        TreeNode cur = root, pre = null;
        
        while (!st.isEmpty() || cur != null) {
        	while (cur != null) {
        		if (cur.right != null) {
        			st.push(cur.right);
        			cur.right = null;
        		}
        		if (pre != null) {
            		pre.right = cur;
            		pre.left = null;
            	}
        		pre = cur;
        		cur = cur.left;
        	}
        	
        	if (!st.isEmpty())
        		cur = st.pop();
        	pre.right = cur;
        }
    }
    
 // version 2: Divide & Conquer
    public void flatten(TreeNode root) {
        helper(root);
    }
    
    private TreeNode helper(TreeNode root) {
        if (root == null) 
            return null;
        
        TreeNode leftLast = helper(root.left);
        TreeNode rightLast = helper(root.right);
        
        // connect leftLast to root.right
        if (leftLast != null) {
            leftLast.right = root.right;
            root.right = root.left;
            root.left = null;
        }
        if (rightLast != null) 
            return rightLast;
        if (leftLast != null) 
            return leftLast;
        
        return root;
    }
}
```

<br>


### Python
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def flatten(self, root: TreeNode) -> None:
        if not root:
            return
        tail = dummy_root = TreeNode(0)
        n = root
        st = []
        while len(st) > 0 or n:
            while n:
                if n.right:
                    st.append(n.right)
                tail.right = n
                tail.left = None
                tail = n
                n = n.left
            if len(st) > 0:
                n = st.pop(-1)
        
        return dummy_root.right
```