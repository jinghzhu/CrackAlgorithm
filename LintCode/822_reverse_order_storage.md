# <center>822 - Reverse Order Storage (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Stack
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/reverse-order-storage/description

<br></br>



## Description
----
Give a linked list, and store the values of linked list in reverse order into an array.

<br></br>



## Example
----
Input: 1 -> 2 -> 3 -> null
Output: [3,2,1]

<br></br>



## Solution
----
### Java
```java
public class ReverseOrderStorage {
	/**
     * @param head: the given linked list
     * @return: the array that store the values in reverse order 
     */
    public List<Integer> reverseStore(ListNode head) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        if (head == null)
            return result;
        
        Stack<Integer> st = new Stack<Integer>();
        for (ListNode n = head; n != null; n = n.next)
            st.push(n.val);
        while (!st.isEmpty())
            result.add(st.pop());
            
        return result;
    }
}
```
