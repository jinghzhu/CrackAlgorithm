# <center>40 - Implement Queue using Stacks (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Queue, Stack
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/implement-queue-by-two-stacks/description

<br></br>



## Description
----
Implement the following operations of a queue using stacks.

- `push(x)` -- Push element x to the back of queue.
- `pop()` -- Removes the element from in front of queue.
- `peek()` -- Get the front element.
- `empty()` -- Return whether the queue is empty.

<br></br>



## Example
----
```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```

<br></br>



## Solution
----
### Java
```java
public class No232ImplementQueueUsingStacks {
	class MyQueue {
		Stack<Integer> st1 = null;
		Stack<Integer> st2 = null;
		
	    public MyQueue() {
	        st1 = new Stack<Integer>();
	        st2 = new Stack<Integer>();
	    }
	    
	    /** Push element x to the back of queue. */
	    public void push(int x) {
	        st1.push(x);
	    }
	    
	    /** Removes the element from in front of queue and returns that element. */
	    public int pop() {
	        int size = st1.size();
	        for (int i = 0; i < size; i++)
	        	st2.push(st1.pop());
	        int result = st2.pop();
	        size = st2.size();
	        for (int i = 0; i < size; i++)
	        	st1.push(st2.pop());
	        
	        return result;
	    }
	    
	    /** Get the front element. */
	    public int peek() {
	    	int size = st1.size();
	        for (int i = 0; i < size; i++)
	        	st2.push(st1.pop());
	        int result = st2.peek();
	        size = st2.size();
	        for (int i = 0; i < size; i++)
	        	st1.push(st2.pop());
	        
	        return result;
	    }
	    
	    /** Returns whether the queue is empty. */
	    public boolean empty() {
	        return st1.isEmpty();
	    }
	}
}
```

<br>


### Go
```go
type MyQueue struct {
    st1 []int
    st2 []int
}


/** Initialize your data structure here. */
func Constructor() MyQueue {
    return MyQueue{
		st1: make([]int, 0),
        st2: make([]int, 0),
	}
}


/** Push element x to the back of queue. */
func (this *MyQueue) Push(x int)  {
    this.st1 = append(this.st1, x)
}


/** Removes the element from in front of queue and returns that element. */
func (this *MyQueue) Pop() int {
    l := len(this.st1)
    for i := 0; i < l; i++ {
        this.st2 = append(this.st2, this.st1[len(this.st1) - 1])
        this.st1 = this.st1[:len(this.st1) - 1]
    }
    result := this.st2[len(this.st2) - 1]
    this.st2 = this.st2[:len(this.st2) - 1]
    for i := 0; i < l -1; i++ {
        this.st1 = append(this.st1, this.st2[len(this.st2) - 1])
        this.st2 = this.st2[:len(this.st2) - 1]
    }
    
    return result
}


/** Get the front element. */
func (this *MyQueue) Peek() int {
    l := len(this.st1)
    for i := 0; i < l; i++ {
        this.st2 = append(this.st2, this.st1[len(this.st1) - 1])
        this.st1 = this.st1[:len(this.st1) - 1]
    }
    result := this.st2[len(this.st2) - 1]
    for i := 0; i < l; i++ {
        this.st1 = append(this.st1, this.st2[len(this.st2) - 1])
        this.st2 = this.st2[:len(this.st2) - 1]
    }
    return result
}


/** Returns whether the queue is empty. */
func (this *MyQueue) Empty() bool {
    return len(this.st1) == 0
}
```

<br>


### Python
----
```python
class MyQueue:

    def __init__(self):
        self.st1 = []
        self.st2 = []
        

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        self.st1.append(x)
        

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        l = len(self.st1)
        for i in range(l):
            self.st2.append(self.st1.pop())
        result = self.st2.pop()
        for i in range(l - 1):
            self.st1.append(self.st2.pop())
        return result
        

    def top(self) -> int:
        """
        Get the front element.
        """
        l = len(self.st1)
        for i in range(l):
            self.st2.append(self.st1.pop())
        result = self.st2[-1]
        for i in range(l):
            self.st1.append(self.st2.pop())
        return result
        

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return len(self.st1) == 0
```