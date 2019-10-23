# <center>378 - Convert Binary Tree to Doubly Linked List (M)</center> 


<br></br>

* Tag: Binary Tree
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/convert-binary-tree-to-doubly-linked-list/description

<br></br>



## Description
----
Convert a binary tree to doubly linked list with in-order traversal.

<br></br>



## Example
----
Input:

```
	    4
	   / \
	  2   5
	 / \
	1   3		
```

Output: 1<->2<->3<->4<->5

<br></br>



## Solution
----
### Python
```python
"""
Definition of Doubly-ListNode
class DoublyListNode(object):
    def __init__(self, val, next=None):
        self.val = val
        self.next = self.prev = nextDefinition of TreeNode:
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left, self.right = None, None
"""

class Solution:
    """
    @param root: The root of tree
    @return: the head of doubly list node
    """
    def bstToDoublyList(self, root):
        tail = dummy_h = DoublyListNode(0)
        if not root:
            return dummy_h
        st = []
        n = root
        while len(st) > 0 or n:
            while n:
                st.append(n)
                n = n.left
            if len(st) > 0:
                n = st.pop(-1)
                d_node = DoublyListNode(n.val)
                tail.next = d_node
                d_node.prev = tail
                tail = tail.next
                n = n.right
        dummy_h = dummy_h.next
        dummy_h.prev = None
        
        return dummy_h
            

```