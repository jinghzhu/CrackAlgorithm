# <center>452 - Remove Linked List Elements (E)</center> 



<br></br>

* Tag: Linked List
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/remove-linked-list-elements/description

<br></br>



## Description
----
Remove all elements from a linked list of integers that have value val.

<br></br>



## Example
----
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5

<br></br>



## Solution
----
### Java
```java
public class No203RemoveLinkedListElements {
	/**
     * @param head: a ListNode
     * @param val: An integer
     * @return: a ListNode
     */
    public ListNode removeElements(ListNode head, int val) {
        if (head == null)
            return head;
        
        ListNode dummyH = new ListNode(), cur = head, last = dummyH, next;
        while (cur != null) {
            next = cur.next;
            if (cur.val != val) {
            	last.next = cur;
                last = last.next;
            }
            cur.next = null; // Important to set next as null.
            cur = next;
        }
        
        return dummyH.next;
    }
}
```

<br>


### Go
```go
func RemoveElements(head *ListNode, val int) *ListNode {
	if head == nil {
		return head
	}
	dummyH := &ListNode{}
	cur, last := head, dummyH
	for cur != nil {
		next := cur.Next
		if cur.Val != val {
			last.Next = cur
			last = last.Next
		}
		cur.Next = nil
		cur = next
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
class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        pre, cur = dummy, head
        while cur:
            if cur.val is val:
                pre.next = cur.next
                n = cur.next
                cur.next = None
                cur = n
            else:
                pre = cur
                cur = cur.next
        head = dummy.next
        dummy.next = None
        return head
```