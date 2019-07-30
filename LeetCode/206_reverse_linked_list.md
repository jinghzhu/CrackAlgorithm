# <center>206 - Reverse Linked List (E)</center> 



<br></br>

* Tag: Linked List
* Difficulty: Easy
* Company: Apple, Twitter, Amazon, Facebook, Snapchat, Microsoft, Uber
* Link: https://leetcode.com/problems/reverse-linked-list/

<br></br>



## Description
----
Reverse a singly linked list.

<br></br>



## Example
----
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL

<br></br>



## Solution
----
### Java
```java
public class Reverse {
	public ListNode reverseListN(ListNode head) {
        ListNode dummy1 = new ListNode(0);
        ListNode dummy2 = new ListNode(1);
        dummy1.next = head;
        while (dummy1.next != null) {
            ListNode n = dummy1.next;
            dummy1.next = n.next;
            n.next = dummy2.next;
            dummy2.next = n;
        }
        head = dummy2.next;
        dummy1.next = dummy2.next = null;
        
        return head;
    }
	
	public ListNode reverseList(ListNode head) {
	    if (head == null || head.next == null) return head;
	    ListNode p = reverseList(head.next);
	    head.next.next = head;
	    head.next = null;
	    return p;
	}
}
```

<br>


### Go
```go
func Reverse(head *ListNode) *ListNode {
	if head == nil || head.Next == nil {
		return head
	}
	p := Reverse(head.Next)
	head.Next.Next = head
	head.Next = nil
	return p
}
```

```go
func ReverseN(head *ListNode) *ListNode {
	dummyH := &ListNode{}
	for head != nil {
		n := head.Next
		head.Next = dummyH.Next
		dummyH.Next = head
		head = n
	}
	head = dummyH.Next
	dummyH.Next = nil

	return head
}
```

<br>


### Python
----
```python
class ReverseLinkedList:

    def reverse1(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        p = self.reverseList(head.next)
        head.next.next = head
        head.next = None
        return p

    def reverse2(self, head):
        dummy1, dummy2 = ListNode(0), ListNode(1)
        dummy1.next = head
        while dummy1.next:
            n = dummy1.next
            dummy1.next = n.next
            n.next = dummy2.next
            dummy2.next = n
        head = dummy2.next
        dummy1.next, dummy2.next = None, None
        return head
```