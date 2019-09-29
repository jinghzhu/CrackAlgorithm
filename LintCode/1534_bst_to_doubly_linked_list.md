# <center>1534 - Convert Binary Search Tree to Sorted Doubly Linked List (M)</center> 



<br></br>

* Difficulty: Medium
* Tag: BST, Linked List
* Company: Google, Amazon, Facebook, Microsoft
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Link: https://www.lintcode.com/problem/convert-binary-search-tree-to-sorted-doubly-linked-list/description

<br></br>



## Description
----
Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

<br></br>



## Example
----
Given a binary search tree:
```
 	    4
 	   / \
 	  2   5
 	 / \
 	1   3
```

Return `1<->2<->3<->4<->5`.

<br></br>



## Solution
----
### Go
```go
func BSTToDoublyList(root *TreeNode) *TreeNode {
	dummyH := &TreeNode{}
	last, cur := dummyH, root
	st := make([]*TreeNode, 0)
	for len(st) != 0 || cur != nil {
		for ; cur != nil; cur = cur.Left {
			st = append(st, cur)
		}
		if len(st) == 0 {
			continue
		}
		cur = st[len(st)-1]
		st = st[:len(st)-1]
		tmp := cur.Right
		cur.Left, cur.Right = last, dummyH
		dummyH.Left, last.Right = cur, cur
		last = cur
		cur = tmp
	}

	last.Right, dummyH.Right.Left = dummyH.Right, last

	return last.Right
}
```

<br>


### Java
```java
public class BST2DoublyLinkedList {
	/**
     * @param root: root of a tree
     * @return: head node of a doubly linked list
     */
    public TreeNode solution(TreeNode root) {
        if (root == null)
            return root;
        
        TreeNode dummyH = new TreeNode(0), cur = root;
        TreeNode last = dummyH;
        Stack<TreeNode> st = new Stack<TreeNode>();
        while (!st.isEmpty() || cur != null) {
            for (; cur != null; cur = cur.left)
                st.push(cur);
            if (st.isEmpty())
                continue;
            cur = st.pop();
            TreeNode tmp = cur.right;
            cur.left = last;
            cur.right = dummyH;
            last.right = cur;
            dummyH.left = cur;
            last = cur;
            cur = tmp;
        }
        
        last.right = dummyH.right;
        dummyH.right.left = dummyH.left;
        
        return last.right;
    }
}
```

<br>


### Python
```python
class BST2DoublyLinkedList:
    """
    @param root: root of a tree
    @return: head node of a doubly linked list
    """
    def solution(self, root):
        if not root:
            return root
        dummyH, cur = TreeNode(0), root
        last = dummyH
        st = []
        while not st or not cur:
            while not cur:
                st.append(cur)
                cur = cur.left
            if not st:
                continue
            cur = st.pop()
            tmp = cur.right
            cur.left, cur.right = last, dummyH
            last.right, dummyH.left = cur, cur
            last = cur
            cur = tmp

        dummyH.right.left, last.right = dummyH.left, dummyH.right

        return last.right
```