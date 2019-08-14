# <center>96 - Partition List (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/partition-list/description

<br></br>



## Description
----
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

<br></br>



## Example
----
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5

<br></br>



## Solution
----
### Java
```java
public class Solution {
    /**
     * @param head: The first node of linked list
     * @param x: An integer
     * @return: A ListNode
     */
    public ListNode partition(ListNode head, int x) {
        ListNode smallH = new ListNode(0), bigH = new ListNode(0);
        bigH.next = head;
        ListNode curS = smallH, curB = bigH;
        while (curB.next != null) {
            ListNode n = curB.next;
            if (n.val >= x) {
                curB = n;
                continue;
            }
            curB.next = n.next;
            curS.next = n;
            curS = n;
        }
        if (smallH.next != null) {
            curS.next = bigH.next;
            head = smallH.next;
        }
        smallH.next = null;
        bigH.next = null;
        
        return head;
    }
}
```

<br>


### Python
```python
class Solution:
    def partition(self, head: ListNode, x: int) -> ListNode:
        small_h, big_h = ListNode(0), ListNode(0)
        cur_s, cur_b, big_h.next = small_h, big_h, head
        while cur_b.next:
            n = cur_b.next
            if n.val >= x:
                cur_b = n
                continue
            cur_b.next = n.next
            cur_s.next = n
            cur_s = n
        if small_h.next:
            cur_s.next = big_h.next
            head = small_h.next
        big_h.next = small_h.next = None
        return head
```

<br>


### Go
```go
func Partition(head *linkedlist.Node, x int) *linkedlist.Node {
	if head == nil {
		return head
	}
	head1, head2 := &linkedlist.Node{Next: nil}, &linkedlist.Node{Next: nil}
	end1, end2 := head1, head2
	for cur := head; cur != nil; cur = cur.Next {
		if cur.Val.(int) < x {
			end1.Next = cur
			end1 = end1.Next
		} else {
			end2.Next = cur
			end2 = end2.Next
		}
	}

	end1.Next = head2.Next
	head = head1.Next
	// Important to set end2.Next = nil. Because its next may pointer to other node before partition.
	end2.Next, head1.Next, head2.Next = nil, nil, nil

	return head
}
```
