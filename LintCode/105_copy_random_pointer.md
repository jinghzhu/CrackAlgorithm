# <center>105 - Copy List with Random Pointer (M)</center> 



<br></br>

* Tag: Linked List
* Comapny: Bloomberg, Uber, Microsoft, Amazon
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/copy-list-with-random-pointer/description

<br></br>



## Description
----
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

<br></br>



## Solution
----
https://www.cnblogs.com/TenosDoIt/p/3387000.html

<br>


## Go
```go
type randomListNode struct {
	next   *randomListNode
	random *randomListNode
	val    int
}

// CopyWithRandomPointer returns a deep copy of the list.
func CopyWithRandomPointer(h *randomListNode) *randomListNode {
	randomPointerCopy(h)
	randomPointerAddRandom(h)
	return randomPointerGetNewH(h)
}

func randomPointerCopy(h *randomListNode) {
	for cur := h; cur != nil; {
		newNode := &randomListNode{
			next: cur.next,
			val:  cur.val,
		}
		cur, cur.next = newNode.next, newNode
	}
}

func randomPointerAddRandom(h *randomListNode) {
	for cur := h; cur != nil; cur = cur.next.next {
		newNode := cur.next
		if cur.random != nil {
			newNode.random = cur.random.next
		}
	}
}

func randomPointerGetNewH(h *randomListNode) *randomListNode {
	dummyH := &randomListNode{}
	for cur, tail := h, dummyH; cur != nil; tail, cur = tail.next, cur.next {
		newNode := cur.next
		tail.next, cur.next = newNode, newNode.next
	}

	return dummyH.next
}
```

<br>


### Python
```python
class RandomListNode:
    def __init__(self, x):
        self.label = x
        self.next = None
        self.random = None


class CopyRandomList:
    def copyRandomList(self, head):
        self.duplicate(head)
        self.add_random(head)

        return self.get_new_head(head)

    def duplicate(self, h):
        cur = h
        while cur:
            new_node = RandomListNode(cur.label)
            new_node.next = cur.next
            cur.next = new_node
            cur = new_node.next

    def add_random(self, h):
        cur = h
        while cur:
            new_node = cur.next
            if cur.random:
                new_node.random = cur.random.next
            cur = new_node.next

    def get_new_head(self, h):
        dummy_h = RandomListNode(0)
        cur, tail = h, dummy_h
        while cur:
            new_node = cur.next
            cur.next, tail.next = new_node.next, new_node
            cur, tail = cur.next, tail.next

        return dummy_h.next
```