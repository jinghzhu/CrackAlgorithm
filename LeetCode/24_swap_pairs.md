# <center>24 - Swap Nodes in Pairs (M)</center> 



<br></br>

* Author: Jianlan Zhou, <jianlan_z@126.com>, Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List
* Difficulty: Medium
* Company: Uber, Microsoft
* Link: https://leetcode.com/problems/swap-nodes-in-pairs/

<br></br>



## Description
----
Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed.

<br></br>



## Example
----
Given 1->2->3->4, you should return the list as 2->1->4->3.

<br></br>



## Solution
----
### Java
```java
public class SwapNodesInPairs {
	/**
     * @param head: a ListNode
     * @return: a ListNode
     */
    public ListNode swapPairs(ListNode head) {
       ListNode n = head;
       while (n != null && n.next != null) {
           int tmp = n.val;
           n.val = n.next.val;
           n.next.val = tmp;
           n = n.next.next;
       }
       
       return head;
    }
}
```

<br>


### Go
```go
func SwapPairs(head *ListNode) *ListNode {
	n := head
	for ; n != nil && n.Next != nil; n = n.Next.Next {
		n.Val, n.Next.Val = n.Next.Val, n.Val
	}

	return head
}
```

<br>


### Python
----
```python
class SwapParis(object):
    def solution(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        n = head
        while n and n.next:
            n.val, n.next.val = n.next.val, n.val
            n = n.next.next
        return head
```