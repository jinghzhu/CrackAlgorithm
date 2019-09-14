# <center>876 - Middle of the Linked List (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List, Two Pointers
* Difficulty: Easy
* Link: https://leetcode.com/problems/middle-of-the-linked-list/

<br></br>



## Description
----
Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

<br></br>



## Example
----
1. Input: [1,2,3,4,5] Output: Node 3
2. Input: [1,2,3,4,5,6] Output: Node 4

<br></br>



## Solution
----
### Java
```java
public class GetMiddle {
	public ListNode solution(ListNode head) {
		ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        
        return slow;
	}
}
```

<br>


### Go
```go
func GetMiddleNode(head *ListNode) *ListNode {
	slow, fast := head, head
	for fast != nil && fast.Next != nil {
		slow, fast = slow.Next, fast.Next.Next
	}

	return slow
}
```

<br>


### Python
----
```python
class GetMiddle(object):
    def solution(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

        return slow
```