# <center>148 - Sort List (M)</center> 



<br></br>

* Tag: Sort, Linked List
* Difficulty: Medium
* Link: https://leetcode.com/problems/sort-list/

<br></br>



## Description
----
Sort a linked list in O(n log n) time using constant space complexity.

<br></br>



## Solution
----
算法：
1. 插入排序。时间O(n^2)，空间O(1)。
2. 自上至下归并排序。时间O(n lgn)，空间O(lgn)，因为使用递归。
3. 自下至上归并排序。时间O(n lgn)，空间O(1)。
4. 快速排序。时间O(n lgn)，空间O(1)。

<br>


## Go
```go
func LinkedListSort(head *ListNode) *ListNode {
	if head == nil {
		return head
	}

	q := llsInitQ(head)

	for len(q) > 1 {
		l := len(q)
		if l%2 != 0 {
			l--
		}
		for i := 0; i < l/2; i++ {
			h1, h2 := q[0], q[1]
			q = q[2:]
			q = append(q, llsMerge(h1, h2))
		}
	}

	return q[0]
}

func llsInitQ(h *ListNode) []*ListNode {
	q := make([]*ListNode, 0)

	for h != nil {
		n := h.Next
		h.Next = nil
		q = append(q, h)
		h = n
	}

	return q
}

func llsMerge(h1, h2 *ListNode) *ListNode {
	dummyH := &ListNode{}
	tail := dummyH

	for h1 != nil && h2 != nil {
		if h1.Val <= h2.Val {
			tail.Next = h1
			h1 = h1.Next
		} else {
			tail.Next = h2
			h2 = h2.Next
		}
		tail = tail.Next
	}

	if h1 != nil {
		tail.Next = h1
	} else {
		tail.Next = h2
	}

	return dummyH.Next
}
```
