# <center>166 - Nth to Last Node in List (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/nth-to-last-node-in-list/description

<br></br>



## Description
----
Find the nth to last element of a singly linked list. 

The minimum number of nodes in list is n.

<br></br>



## Example
----
1. Input: list = 3->2->1->5->null, n = 2 Output: 1
2. Input: list  = 1->2->3->null, n = 3 Output: 1

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /*
     * @param head: The first node of linked list.
     * @param n: An integer
     * @return: Nth to last node of a singly linked list. 
     */
    public ListNode nthToLast(ListNode head, int n) {
        if (head == null || n < 1)
            return head;
        ListNode start = head, end = head;
        for (int i = 0; i < n && end != null; i++)
            end = end.next;
        if (end == null)
            return head;
        while (end != null) {
            end = end.next;
            start = start.next;
        }
        
        return start;
    }
}
```

<br>


### Go
```go
func GetNthFromEnd(h *linkedlist.Node, n int) interface{} {
	if h == nil || n < 0 {
		panic("Incorrect parameter")
	}
	start, end := h, h
	for i := 0; end != nil && i < n; i++ {
		end = end.Next
	}
	if end == nil {
		panic("The parameter n exceeds the length")
	}
	for end.Next != nil {
		end = end.Next
		start = start.Next
	}

	return start.Val
}
```

<br>


### Python
----
```python
class Solution:
    """
    @param head: The first node of linked list.
    @param n: An integer.
    @return: Nth to last node of a singly linked list. 
    """
    def nthToLast(self, head, n):
        if head is None or n < 1:
            return None
        cur = head.next
        while cur is not None:
            cur.pre = head
            cur = cur.next
            head = head.next
        n -= 1
        while n > 0:
            head = head.pre
            n -= 1
        return head
```