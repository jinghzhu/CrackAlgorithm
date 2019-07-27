# <center>174 - Remove Nth Node From End of List (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Two Pointers, Linked List
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/remove-nth-node-from-end-of-list/description

<br></br>



## Description
----
Given a linked list, remove the n-th node from the end of list and return its head.

<br></br>



## Example
----
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.

<br></br>



## Solution
----
### Java
```java
public class RemoveNthFromEnd {
	/**
     * @param h: The first node of linked list.
     * @param n: An integer
     * @return: The head of linked list.
     */
	public ListNode solution2(ListNode h, int n) {
		if (n < 1 || h == null)
			return null;
		ListNode dummyH = new ListNode();
		dummyH.next = h;
		ListNode pre = dummyH, end = h;
		for (int i = 0; i < n - 1 && end != null; i++)
			end = end.next;
		if (end == null)
			return null;
		while (end.next != null) {
			end = end.next;
			pre = pre.next;
		}
		ListNode target = pre.next;
		pre.next = target.next;
		target.next = null;
		h = dummyH.next;
		dummyH.next = null;
		
		return h;
	}
}
```

<br>


### Go
```go
func RemoveNthFromEnd(head *ListNode, n int) *ListNode {
	if head == nil || n < 1 {
		return head
	}
	dummyH := &ListNode{Next: head}
	pre, cur, pivot := dummyH, head, head
	for i := 0; pivot != nil && i < n-1; i++ {
		pivot = pivot.Next
	}
	if pivot == nil {
		return head
	}
	for ; pivot.Next != nil; pivot = pivot.Next {
		pre = cur
		cur = cur.Next
	}
	pre.Next = cur.Next
	cur.Next = nil
	head = dummyH.Next
	dummyH = nil

	return head
}
```

<br>


### Python
----
```python
class remove_nth_from_end:
    def solution(self, head, n):
        if n < 1:
            return head
        dummy = ListNode(0)
        dummy.next = head
        pre, cur, tail = dummy, head, dummy
        for i in range(n):
            if tail is None:
                return head
            tail = tail.next
        while tail.next:
            tail = tail.next
            pre = cur
            cur = cur.next
        pre.next = cur.next
        cur.next = None
        head = dummy.next
        dummy.next = None
        return head
```