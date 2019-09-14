# <center>102 - Linked List Cycle (M)</center> 



<br></br>

* Tag: Linked List
* Difficulty: Medium
* Company: Microsoft, Amazon
* Link: https://www.lintcode.com/problem/linked-list-cycle/description

<br></br>



## Description
----
Given a linked list, determine if it has a cycle in it.

<br></br>



## Solution
----
证明：一个走1步，一个走2步必定能相遇。

设慢指针为S，快指针为F，假设相遇时在k点，此时S走过x＋d，F走过x ＋ ny ＋ d ＝ 2(x＋d)。对y取余，(x + ny + d) % y = (x + d) % y

<br>


### Java
```java
public class Cycle1 {
	/**
     * @param head: The first node of linked list.
     * @return: True if it has a cycle, or false
     */
	public boolean HasCycle(ListNode head) {
		ListNode slow = head, fast = head;
		while (fast != null && fast.next != null) {
			slow = slow.next;
			fast = fast.next.next;
			if (slow == fast)
				return true;
		}
		
		return false;
	}
}
```

<br>


### Go
```go
func HasCycle(head *ListNode) bool {
	if head == nil {
		return false
	}
	slow, fast := head, head
	for fast != nil && fast.Next != nil {
		slow = slow.Next
		fast = fast.Next.Next
		if slow == fast {
			return true
		}
	}

	return false
}
```

<br>


### Python
----
```python
class HasCycle:
    def solution(self, head: ListNode) -> bool:
        if not head:
            return False
        slow = fast = head
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
            if slow == fast:
                return True
        return False
```