# <center>112 - Remove Duplicates from Sorted List (E)</center> 



<br></br>

* Tag: Linked List
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/remove-duplicates-from-sorted-list/description

<br></br>



## Description
----
Given a sorted linked list, delete all duplicates such that each element appear only once.

<br></br>



## Example
----
Input: 1->1->2->3->3
Output: 1->2->3

<br></br>



## Solution
----
### Java
```java
public class No112RemoveDuplicatesFromSortedList {
	/**
     * @param head: head is the head of the linked list
     * @return: head of linked list
     */
	public ListNode solution1(ListNode head) {
		ListNode cur = head;
		while(cur != null) {
			ListNode n = cur.next;
			if (n != null && n.val == cur.val) {
				cur.next = n.next;
				n.next = null;
			} else {
			    cur = cur.next;
			}
		}
		
		return head;
    }
	
	/**
     * @param h: head is the head of the linked list
     * @return: head of linked list
     */
	public ListNode solution2(ListNode h) {
		ListNode cur = h;
		while (cur != null) {
			ListNode end = cur;
			while (end.next != null && end.next.val == cur.val)
				end = end.next;
			// Important to save the next node of end. Because it may happen that end == cur.
			// And the order of following statements can't be changed.
			ListNode next = end.next;
			end.next = null;
			cur.next = next;
			cur = cur.next;
		}
		
		return h;
	}
}
```

<br>


### Go
```go
func DeleteDuplicates1(head *ListNode) *ListNode {
	cur := head
	for cur != nil {
		n := cur.Next
		if n != nil && cur.Val == n.Val {
			cur.Next = n.Next
			n.Next = nil
		} else { // else is important!
			cur = cur.Next
		}

	}

	return head
}
```

```go
func DeleteDuplicates2(h *ListNode) *ListNode {
	cur := h
	for cur != nil {
		end := cur
		for end.Next != nil && end.Next.Val == cur.Val {
			end = end.Next
		}
		// Important to save the next node of end. Because it may happen that end == cur.
		// And the order of following statements can't be changed.
		next := end.Next
		end.Next = nil
		cur.Next = next
		cur = next
	}

	return h
}
```

```go
func DeleteDuplicates3(head *ListNode) *ListNode {
	if head == nil {
		return head
	}
	dummyH := &ListNode{Next: head}
	last, n := dummyH.Next, head.Next
	head.Next = nil
	for n != nil {
		next := n.Next
		if last.Val != n.Val {
			last.Next = n
			last = last.Next
		}
		n.Next = nil
		n = next
	}
	head = dummyH.Next
	dummyH = nil

	return head
}
```

<br>


### Python
----
```python
class RemoveDuplicatesFromSortedLinkedList:
    def solution(self, head: ListNode) -> ListNode:
        n = head
        while n:
            m = n.next
            if m and n.val == m.val:
                n.next = m.next
                m.next = None
            else:
                n = n.next
        return head
```