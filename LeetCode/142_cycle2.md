# <center>142 - Linked List Cycle II (M)</center> 



<br></br>

* Tag: Linked List
* Difficulty: Medium
* Company: Microsoft
* Link: https://leetcode.com/problems/linked-list-cycle-ii/

<br></br>



## Description
----
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

<br></br>



## Solution
----
证明：设链起点到环入口点距离为x，环入口点到fast与low重合点距离为y，又设在fast与low重合时fast已绕环n周，且此时low移动总长度为s，fast移动总长度为2s，环长度为r。则
1 - s + nr = 2s, n > 0
2 - s = x + y

由1式得s = nr。代入2式得
3 - nr = x + y => x = nr - y

现让一指针p1从链表起点开始遍历，指针p2从相遇处开始遍历，且p1和p2移动步长均为1。当p1移动x步即到达环入口点。由3式可知，p2也移动x步即nr-y步。由于p2是从相遇处开始移动，故p2移动nr步是移回到了相遇处，再退y步则是到环入口点。即，当p1移动x步第一次到达环的入口点时，p2也到达入口点。

<br>


### Java
```java
public class Cycle2 {
	/**
     * @param head: The first node of linked list.
     * @return: The node where the cycle begins. if there is no cycle, return null
     */
    public ListNode detectCycle(ListNode head) {
    	if (head == null)
            return null;

        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow)
            	break;
        }
        if(fast == null || fast.next == null) // if no cycle
            return null;
        while (head != slow) {
            head = head.next;
            slow = slow.next;
        }
        
        return head;
    }
	
	public int getCycleLength(ListNode head) {
		if (head == null)
			return 0;

		ListNode entry = detectCycle(head);
		if (entry == null)
			return 0;
		int count = 1;
		for (ListNode n = entry.next; n != entry; n = n.next)
			count++;
		
		return count;
	}
}
```

<br>


### Go
```go
func GetCycleEntry(head *ListNode) *ListNode {
	if head == nil {
		return nil
	}
	slow, fast := head, head
	for fast != nil && fast.Next != nil {
		slow, fast = slow.Next, fast.Next.Next
		if slow == fast {
			break
		}
	}
	if fast == nil || fast.Next == nil { // no cycle case
		return nil
	}
	for head != slow {
		head = head.Next
		slow = slow.Next
	}

	return head
}
```

```go
func GetCycleLength(head *ListNode) int {
	if head == nil {
		return 0
	}
	entry := GetCycleEntry(head)
	if entry == nil {
		return 0
	}
	count := 1
	for p := entry.Next; p != entry; p = p.Next {
		count++
	}

	return count
}
```

<br>


### Python
----
```python
class Cycle2:
    def detect_cycle(self, head: ListNode) -> ListNode:
        if not head:
            return None
        slow = fast = head
        while fast and fast.next:
            slow, fast = slow.next, fast.next.next
            if slow == fast:
                break
        if not fast or not fast.next:
            return None
        while head != slow:
            head, slow = head.next, slow.next
        return head


    def get_cycle_length(self, head: ListNode):
        entry = self.detect_cycle(head)
        if not entry:
            return 0
        p = entry.next
        count = 1
        while p != entry:
            count += 1
            p = p.next
        return count
```