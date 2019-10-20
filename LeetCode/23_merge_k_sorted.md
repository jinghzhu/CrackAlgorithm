# <center>104 - Merge K Sorted Lists (H)</center> 


<br></br>

* Tag: Linked List, Heap
* Company: Twitter, LinkedIn, Airbnb, Facebook, Amazon, Microsoft, Uber, Google
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Hard
* Link: https://www.lintcode.com/problem/merge-k-sorted-lists/description

<br></br>



## Description
----
Merge k sorted linked lists and return it as one sorted list.

<br></br>



## Example
----
Input:   [2->4->null,null,-1->null]
Output:  -1->2->4->null

<br></br>



## Solution
----
3种解法：
1. Merge one by one，一个个合并。没有有效利用每个已有序这个特点。时间负载度O(KN)，K为列表个数。
2. 利用堆。把每个list种所有节点逐一加入堆，最终一个个出堆即可。时间复杂度O(NlogK)，k为列表个数。
3. Merge two by two，两两合并，分治算法。时间负载度O(NlogK)。

<br>


### Go
```go
func MergeKListsOneByOne(lists []*ListNode) *ListNode {
	dummyH := &ListNode{}

	for len(lists) > 0 {
		h := lists[0]
		lists = lists[1:]
		mkloboHelper(dummyH, h)
	}

	return dummyH.Next
}

func mkloboHelper(dummyH, h *ListNode) {
	if dummyH == nil || h == nil {
		return
	}
	pre, cur := dummyH, dummyH.Next
	for cur != nil && h != nil {
		if h.Val > cur.Val {
			pre, cur = cur, cur.Next
			continue
		}
		tmp := h.Next
		h.Next = cur
		pre.Next = h
		pre = h
		h = tmp
	}
	if h != nil {
		pre.Next = h
	}
}
```

```go
import (
	"container/heap"
)

func MergeKListsHeap(lists []*ListNode) *ListNode {
	ln := new(listNodes)
	heap.Init(ln)
	for _, v := range lists {
		for n := v; n != nil; n = n.Next {
			heap.Push(ln, n)
		}
	}
	dummyH := &ListNode{}
	var tail *ListNode
	for ln.Len() > 0 {
		n := heap.Pop(ln).(*ListNode)
		if dummyH.Next == nil {
			dummyH.Next, tail = n, n
		} else {
			tail.Next, tail = n, n
		}
	}
	if tail != nil { // important
		tail.Next = nil
	}

	return dummyH.Next
}

type listNodes []*ListNode

func (l listNodes) Len() int {
	return len(l)
}

func (l listNodes) Less(i, j int) bool {
	return l[i].Val < l[j].Val
}

func (l listNodes) Swap(i, j int) {
	l[i], l[j] = l[j], l[i]
}

func (l *listNodes) Pop() interface{} {
	old := *l
	n := len(old)
	x := old[n-1]
	*l = old[:n-1]
	return x
}

func (l *listNodes) Push(x interface{}) {
	*l = append(*l, x.(*ListNode))
}
```

```go
func MergeKListsTwoByTwo1(lists []*ListNode) *ListNode {
	l := len(lists)
	if l == 0 {
		return nil
	}

	return mkltbt1Helper(lists, 0, l-1)
}

func mkltbt1Helper(lists []*ListNode, start, end int) *ListNode {
	if start == end {
		return lists[start]
	}

	mid := start + (end-start)/2
	left := mkltbt1Helper(lists, start, mid)
	right := mkltbt1Helper(lists, mid+1, end)

	return mkltbtMerge(left, right)
}

func mkltbtMerge(l1, l2 *ListNode) *ListNode {
	dummyH := &ListNode{}
	tail := dummyH

	for l1 != nil && l2 != nil {
		if l1.Val <= l2.Val {
			tail.Next = l1
			l1 = l1.Next
		} else {
			tail.Next = l2
			l2 = l2.Next
		}
		tail = tail.Next
	}
	if l1 != nil {
		tail.Next = l1
	} else if l2 != nil {
		tail.Next = l2
	}

	return dummyH.Next
}
```

```go
func MergeKListsTwoByTwo2(lists []*ListNode) *ListNode {
	if len(lists) < 1 {
		return nil
	}

	for len(lists) > 1 {
		l := len(lists)
		if l%2 != 0 {
			l--
		}
		for i := 0; i < l; i += 2 {
			l1, l2 := lists[0], lists[1]
			lists = lists[2:]
			lists = append(lists, mkltbtMerge(l1, l2))
		}
	}

	return lists[0]
}

func mkltbtMerge(l1, l2 *ListNode) *ListNode {
	dummyH := &ListNode{}
	tail := dummyH

	for l1 != nil && l2 != nil {
		if l1.Val <= l2.Val {
			tail.Next = l1
			l1 = l1.Next
		} else {
			tail.Next = l2
			l2 = l2.Next
		}
		tail = tail.Next
	}
	if l1 != nil {
		tail.Next = l1
	} else if l2 != nil {
		tail.Next = l2
	}

	return dummyH.Next
}
```

<br>


### Java
```java
public class MergeKSorted {
	public ListNode solutionNative(List<ListNode> lists) {  
        ListNode dummyH = new ListNode(0);
        for (ListNode l : lists)
        	solutionNativeHelper(dummyH, l);
        
        return dummyH.next;
    }
    
    private void solutionNativeHelper(ListNode dummyH, ListNode l) {
        if (dummyH == null || l == null)
            return;
        
        ListNode pre = dummyH, cur = dummyH.next;
        while (cur != null && l != null) {
            if (cur.val < l.val) {
                pre = cur;
                cur = cur.next;
                continue;
            }
            ListNode tmp = l.next;
            l.next = cur;
            pre.next = l;
            pre = l;
            l = tmp;
        }
        if (l != null)
            pre.next = l;
    }
}
```

<br>


### Python
```python
class MergeKSorted:
    def solution_heap(self, lists) -> ListNode:
        heap = []
        heapify(heap)
        for l in lists:
            n = l
            while n:
                heappush(heap, n)
                n = n.next
        dummy_h = tail = ListNode(0)
        while heap:
            tail.next = heappop(heap)
            tail = tail.next
        if tail:  # important
            tail.next = None

        return dummy_h.next

    def solution_native(self, lists) -> ListNode:
        dummy_h = ListNode(0)

        for l in lists:
            self.solution_native_merge(dummy_h, l)

        return dummy_h.next

    def solution_native_merge(self, dummy_h, l):
        pre, cur = dummy_h, dummy_h.next
        while cur and l:
            if cur.val < l.val:
                pre, cur = cur, cur.next
            else:
                tmp = l.next
                l.next = cur
                pre.next = l
                pre = l
                l = tmp
        if l:
            pre.next = l
```