# <center>173 - Binary Search Tree Iterator (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: BST, DFS
* Difficulty: Medium
* Company: Microsoft, Facebook, LinkedIn, Google
* Link: https://leetcode.com/problems/binary-search-tree-iterator/

<br></br>



## Description
----
Implement an iterator over a binary search tree (BST). Your iterator will be initialized with the root node of a BST.

Calling `next()` will return the next smallest number in the BST.

<br></br>



## Example
----
```
  7
 /  \
3   15
    / \
   9  20
```

```java
iterator.next();    // return 3
iterator.next();    // return 7
iterator.hasNext(); // return true
iterator.next();    // return 9
iterator.hasNext(); // return true
iterator.next();    // return 15
iterator.hasNext(); // return true
iterator.next();    // return 20
iterator.hasNext(); // return false
```

<br></br>



## Solution
----
### Java
```java
public class BSTIterator {
	private Stack<TreeNode> stack = new Stack<>();
    private TreeNode curt;
    
    // @param root: The root of binary tree.
    public BSTIterator(TreeNode root) {
        curt = root;
    }

    // @return: True if there has next node, or false
    public boolean hasNext() {
        return (curt != null || !stack.isEmpty());
    }
    
    //@return: return next node. If no next node, return null.
    public TreeNode next() {
        while (curt != null) {
            stack.push(curt);
            curt = curt.left;
        }
        
        TreeNode node = null;
        if (!stack.isEmpty()) {
        	curt = stack.pop();
            node = curt;
            curt = curt.right;
        }
        
        return node;
    }
}
```

<br>


### Python
```python
class BSTIterator:
    def __init__(self, root: TreeNode):
        self.visited = False
        self.st = []
        self.cur = root

    def next(self) -> int:
        """
        @return the next smallest number
        """
        l = len(self.st)
        if self.visited and l < 1:
            return -1
        if not self.visited:
            return self.helper()
        
        if l < 1:
            return -1
    
        return self.helper()
    
    def helper(self):
        while self.cur:
            self.st.append(self.cur)
            self.cur = self.cur.left
        self.cur = self.st.pop()
        result = self.cur.val
        self.cur = self.cur.right
        return result

    def hasNext(self) -> bool:
        """
        @return whether we have a next smallest number
        """
        if not self.visited and self.cur:
            return True
        return len(self.st) > 0
```

<br>
