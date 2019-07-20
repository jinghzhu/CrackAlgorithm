# <center>155 - Min Stack (M)</center> 



<br></br>

* Tag: Stack
* Difficulty: Medium
* Company: Snapchat, Uber, Google, Facebook, Amazon
* Link: https://leetcode.com/problems/min-stack/

<br></br>



## Description
----
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
- `push(x)` -- Push element `x` onto stack.
- `pop()` -- Removes the element on top of the stack.
- `top()` -- Get the top element.
- `getMin()` -- Retrieve the minimum element in the stack.

<br></br>



## Example
----
```
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

<br></br>



## Solution
----
### Java
```java
public class MinStack {
	private Stack<Integer> st;
    private Stack<Integer> minSt;
    
    public MinStack() {
        st = new Stack<Integer>();
        minSt= new Stack<Integer>();
    }

    /*
     * @param number: An integer
     * @return: nothing
     */
    public void push(int number) {
        st.push(number);
        if (minSt.isEmpty())
            minSt.push(number);
        else
            minSt.push(Math.min(number, minSt.peek()));
    }

    /*
     * @return: An integer
     */
    public int pop() {
        minSt.pop();
        
        return st.pop();
    }

    /*
     * @return: An integer
     */
    public int min() {
        return minSt.peek();
    }
}

```

<br>


### Python
```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, x: int) -> None:
        self.stack.append(x)
        if len(self.min_stack) == 0 or self.min_stack[-1] > x:
            self.min_stack.append(x)
        else:
            y = self.min_stack[-1]
            self.min_stack.append(y)

    def pop(self) -> None:
        if len(self.stack) > 0:
            self.stack.pop(-1)
            self.min_stack.pop(-1)

    def top(self) -> int:
        return self.stack[-1]

    def get_min(self) -> int:
        return self.min_stack[-1]
```

<br>


### Go
```go
// minStack is the stack with a GetMin() method.
type minStack struct {
	stack []minStackItem
}

type minStackItem struct {
	min, x int
}

// Constructor returns a MinStack.
func Constructor() minStack {
	return minStack{}
}

// Push puts data into stack.
func (this *minStack) Push(x int) {
	min := x
	if len(this.stack) > 0 && this.GetMin() < x {
		min = this.GetMin()
	}
	this.stack = append(this.stack, minStackItem{min: min, x: x})
}

// Pop removes the data.
func (this *minStack) Pop() {
	this.stack = this.stack[:len(this.stack)-1]
}

// Top returns the data which was last put in.
func (this *minStack) Top() int {
	return this.stack[len(this.stack)-1].x
}

// GetMin returns the min value in the stack.
func (this *minStack) GetMin() int {
	return this.stack[len(this.stack)-1].min
}
```
