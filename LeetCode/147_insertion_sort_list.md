# <center>147 - Insertion Sort List (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Sort, Linked List
* Difficulty: Medium
* Link: https://leetcode.com/problems/insertion-sort-list/

<br></br>



## Description
----
Sort a linked list using insertion sort.

<br></br>



## Example
----
Input: 4->2->1->3
Output: 1->2->3->4

<br></br>



## Solution
----
### Java
```java
public class LinkedList {
	/**
     * @param head: The head of linked list.
     * @return: Head of the sorted linked list.
     */
	public ListNode sort(ListNode head) {
		ListNode dummy1 = new ListNode(1);
        ListNode dummy2 = new ListNode(2);
        dummy1.next = head;
        while (dummy1.next != null) {
            ListNode n = dummy1.next;
            dummy1.next = n.next;
            ListNode pre = dummy2;
            ListNode cur = dummy2.next;
            while (cur != null && cur.val < n.val) {
                pre = cur;
                cur = cur.next;
            }
            n.next = cur;
            pre.next = n;
        }
        
        head = dummy2.next;
        dummy2.next = null; // For GC.
        
        return head;
	}
}
```

<br>


### Go
```go
func LinkedListSort(head *ListNode) *ListNode {
	dummy1, dummy2 := &ListNode{Next: head}, &ListNode{}
	for dummy1.Next != nil {
		n := dummy1.Next
		dummy1.Next = n.Next
		pre, cur := dummy2, dummy2.Next
		for cur != nil && cur.Val < n.Val {
			pre = cur
			cur = cur.Next
		}
		pre.Next = n
		n.Next = cur
	}
	head = dummy2.Next
	dummy2.Next = nil

	return head
}
```

<br>


### Python
----
```python
class InsertionSortList:
    def solution(self, head):
        dummy1, dummy2 = ListNode(1), ListNode(2)
        dummy1.next = head
        while dummy1.next:
            n = dummy1.next
            dummy1.next = n.next
            pre, cur = dummy2, dummy2.next
            while cur and cur.val < n.val:
                pre = cur
                cur = cur.next
            n.next = cur
            pre.next = n

        head = dummy2.next
        dummy2.next = None  # For GC.
        return head
```