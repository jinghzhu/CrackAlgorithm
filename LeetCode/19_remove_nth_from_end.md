# <center>19 - Remove Nth Node From End of List (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List
* Difficulty: Medium
* Link: https://leetcode.com/problems/remove-nth-node-from-end-of-list/

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
	public ListNode solution1(ListNode h, int n) {
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
	
	public ListNode solution2(ListNode head, int n) {
        if (head == null || n < 1)
            return head;
        ListNode pre = null, cur = head, end = head;
        int i = 0;
        for (; end != null && i < n; i++)
            end = end.next;
        if (end == null && i < n)
            return head;
        for (; end != null; end = end.next) {
            pre = cur;
            cur = cur.next;
        }
        if (pre == null) {
            cur = head.next;
            head.next = null;
            head = cur;
        } else {
            pre.next = cur.next;
            cur.next = null;
        }
        
        return head;
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

```go
func RemoveNthFromEnd(head *ListNode, n int) *ListNode {
	if n < 1 || head == nil {
		return head
	}
	var pre *ListNode
	cur, end := head, head
	i := 0
	for ; i < n && end != nil; i++ {
		end = end.Next
	}
	if end == nil && i < n {
		return head
	}
	for ; end != nil; end = end.Next {
		pre, cur = cur, cur.Next
	}
	if pre == nil {
		cur = head.Next
		head.Next, head = nil, cur
	} else {
		pre.Next, cur.Next = cur.Next, nil
	}

	return head
}
```

<br>


### Python
----
```python
class remove_nth_from_end:
        def solution1(self, head, n):
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

    def solution2(self, head: ListNode, n: int) -> ListNode:
        if not head or n < 1:
            return head
        pre, cur, end = None, head, head
        i = 0
        while i < n and end:
            end = end.next
            i += 1
        if not end and i < n:
            return head
        while end:
            pre, cur, end = cur, cur.next, end.next
        if not pre:
            head.next, cur = None, head.next
            head = cur
        else:
            pre.next, cur.next = cur.next, None

        return head
```