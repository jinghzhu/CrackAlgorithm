# <center>21 - Merge Two Sorted Lists (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List
* Difficulty: Easy
* Company: Microsoft, Apple, LinkedIn, Amazon
* Link: https://leetcode.com/problems/merge-two-sorted-lists/

<br></br>



## Description
----
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

<br></br>



## Example
----
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4

<br></br>



## Solution
----
### Java
```java
public class MergeTwoSortedLists {
	/**
     * @param l1: ListNode l1 is the head of the linked list
     * @param l2: ListNode l2 is the head of the linked list
     * @return: ListNode head of linked list
     */
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
    	ListNode dummy = new ListNode();
    	ListNode lastNode = dummy;
        
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                lastNode.next = l1;
                l1 = l1.next;
            } else {
                lastNode.next = l2;
                l2 = l2.next;
            }
            lastNode = lastNode.next;
        }
        
        if (l1 != null)
            lastNode.next = l1;
        else // No need to check l2 != null.
            lastNode.next = l2;
        l1 = dummy.next;
        dummy.next = null;
        
        return l1;
    }
}
```

<br>


### Go
```go
func MergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
	dummyH, n1, n2 := &ListNode{}, l1, l2
	last := dummyH
	for n1 != nil && n2 != nil {
		if n1.Val < n2.Val {
			last.Next = n1
			n1 = n1.Next
		} else {
			last.Next = n2
			n2 = n2.Next
		}
		last = last.Next
		last.Next = nil
	}
	if n1 != nil {
		last.Next = n1
	} else {
		last.Next = n2
	}
	l1 = dummyH.Next
	dummyH = nil

	return l1
}
```

<br>


### Python
----
```python
class MergeTwoSortedLists:
    def solution(self, l1: ListNode, l2: ListNode) -> ListNode:
        n1, n2, h = l1, l2, ListNode(0)
        pre = h
        while n1 and n2:
            if n1.val <= n2.val:
                pre.next = n1
                pre = n1
                n1 = n1.next
            else:
                pre.next = n2
                pre = n2
                n2 = n2.next
        if n1:
            pre.next = n1
        else:
            pre.next = n2

        return h.next
```