# <center>237 - Delete Node in a Linked List (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List
* Difficulty: Easy
* Company: Apple, Microsoft
* Link: https://leetcode.com/problems/delete-node-in-a-linked-list/

<br></br>



## Description
----
Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.

<br></br>



## Example
----
Input: head = [4,5,1,9], node = 5
Output: [4,1,9]

<br></br>



## Solution
----
### Java
```java
public class DeleteWithoutHead {
	/*
     * @param node: the node in the list should be deleted at.
     * @return: nothing
     */
    public void deleteNode(ListNode node) {
        if (node == null)
            return;
        
        ListNode n = node;
        while (n.next != null) {
            n.val = n.next.val;
            if (n.next.next == null) {
                n.next = null;
                return;
            }
            n = n.next;
        }
    }
}
```

<br>


### Go
```go
func DeleteWithoutHead(node *ListNode) {
	if node == nil {
		return
	}
	for n := node; n.Next != nil; n = n.Next {
		n.Val = n.Next.Val
		if n.Next.Next == nil {
			n.Next = nil
			return
		}
	}
}
```

<br>


### Python
----
```python
class DeleteWithoutHead:
    def solution(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        pre, cur = node, node.next
        while cur.next:
            pre.val, cur.val = cur.val, pre.val
            pre, cur = cur, cur.next
        pre.val, cur.val = cur.val, pre.val
        pre.next = cur.next
```