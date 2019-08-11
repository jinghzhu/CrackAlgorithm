# <center>2 - Add Two Numbers (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Linked List
* Difficulty: Medium
* Company: Microsoft, Airbnb, Amazon
* Link: https://leetcode.com/problems/add-two-numbers/

<br></br>



## Description
----
You have two numbers represented by a linked list, where each node contains a single digit. The digits are stored in reverse order, such that the 1's digit is at the head of the list. Write a function that adds the two numbers and returns the sum as a linked list.

<br></br>



## Example
----
Input: 7->1->6->null, 5->9->2->null
Output: 2->1->9->null	
Explanation: 617 + 295 = 912, 912 to list:  2->1->9->null

Input:  3->1->5->null, 5->9->2->null
Output: 8->0->8->null	
Explanation: 513 + 295 = 808, 808 to list: 8->0->8->null

<br></br>



## Solution
----
Caution thar there are many corner cases.

<br>


### Java
```java
public class AddTwoNum {
	/**
     * @param l1: the first list
     * @param l2: the second list
     * @return: the sum list of l1 and l2 
     */
    public ListNode addLists(ListNode l1, ListNode l2) {
        if (l1 == null || l2 == null)
        	return l1;

        ListNode dummyH = new ListNode(0);
        ListNode last = dummyH, n1 = l1, n2 = l2;
        boolean flag = false;
        while (n1 != null && n2 != null) {
        	int num = n1.val + n2.val;
        	if (flag)
        		num++;
        	flag = false;
        	if (num / 10 != 0) {
        		flag = true;
        		num %= 10;
        	}
        	last.next = new ListNode(num);
        	last = last.next;
        	n1 = n1.next;
        	n2 = n2.next;
        }
        
        if (n1 != null)
        	last.next = n1;
        else
        	last.next = n2;
        while (flag && last.next != null) {
        	last = last.next;
        	last.val++;
        	flag = false;
        	if (last.val / 10 != 0) {
        		flag = true;
        		last.val %= 10;
        	}
        }
        if (flag) // important
        	last.next = new ListNode(1);
        
        ListNode h = dummyH.next;
        dummyH = null;
        
        return h;
    }
}
```

<br>


### Go
```go
func AddTwoNum(l1 *ListNode, l2 *ListNode) *ListNode {
	if l1 == nil || l2 == nil {
		return nil
	}

	dummyH := &ListNode{}
	n1, n2, last := l1, l2, dummyH
	flag := false
	// We can't convert data into int64. Because the number may be too large.
	for n1 != nil && n2 != nil {
		num := n1.Val + n2.Val
		if flag {
			num++
			flag = false
		}
		if num/10 != 0 {
			flag = true
			num %= 10
		}
		n := &ListNode{Val: num}
		last.Next = n
		last, n1, n2 = last.Next, n1.Next, n2.Next
	}
	// Need to check the remaining of n1 and n2.
	if n1 != nil {
		last.Next = n1
	} else {
		last.Next = n2
	}
	// There are four cases here:
	// 1. n1 or n2 is not empty and flag is false.
	// 2. n1 or n2 is not empty and flag is true.
	// 3. both n1 and n2 are empty and flag is ture.
	// 4. both n1 and n2 are empty and flag is false.
	for flag && last.Next != nil {
		last = last.Next
		last.Val++
		if last.Val/10 == 0 {
			flag = false
		} else {
			last.Val %= 10
		}
	}
	// If it is still > 10...
	if flag {
		last.Next = &ListNode{Val: 1}
	}

	h := dummyH.Next
	dummyH = nil

	return h
}
```

<br>


### Python
----
```python
class AddTwoNum:
    def solution(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1 or not l2:
            return l1
        n1, n2 = l1, l2
        flag = False
        dummy_h = ListNode(0)
        pre = dummy_h
        while n1 and n2:
            n = ListNode(n1.val + n2.val)
            pre.next = n
            pre = n
            if flag:
                n.val += 1
                flag = False
            if n.val >= 10:
                flag = True
                n.val -= 10
            n1, n2 = n1.next, n2.next
        pre, flag = self.add(pre, n1, flag)
        pre, flag = self.add(pre, n2, flag)
        if flag:
            pre.next = ListNode(1)

        h = dummy_h.next
        dummy_h.next = None
        return h

    def add(self, pre: ListNode, n: ListNode, flag: bool) -> (ListNode, bool):
        while n:
            cur = ListNode(n.val)
            pre.next = cur
            pre = cur
            if flag:
                cur.val += 1
                flag = False
            if cur.val >= 10:
                flag = True
                cur.val -= 10
            n = n.next
        return pre, flag
```