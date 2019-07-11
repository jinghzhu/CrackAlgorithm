# <center>97 - Maximum Depth of Binary Tree (E)</center> 



<br></br>

* Tag: Divide and Conquer, Recursion, Binary Tree, DFS, BFS
* Company: Uber, LinkedIn, Apple
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/maximum-depth-of-binary-tree/description

<br></br>



## Description
----
The max depth is the number of nodes along the longest path from root node down to the farthest leaf node.

<br></br>



## Example
----
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```

return its depth = 3.

<br></br>



## Solution
----
### Java
```java
public class MaxDepth {
	public int getDeepth(TreeNode root) {
		if (root == null) 
			return 0;
		
		return 1 + Math.max(getDeepth(root.left), getDeepth(root.right));
	}
	
    /*
     * We can also call LevelOrderTraversal to do it.
     * The key is to create a dummyNode to help us detect different level.
     *      1  
           / \  
          2   3  
         / \   \  
        4   5   6
     * 
     * In queue： 1, dummy, 2, 3, dummy, 4, 5, 5, dummy  
     * 
    */  
	public int getDeepthByLevelOrder(TreeNode root) {
		int deepth = 0;
		if (root == null) 
			return deepth;
		
		Queue<TreeNode> q = new LinkedList<TreeNode>();
		TreeNode dummy = new TreeNode(), cur;
		q.offer(root);
		q.offer(dummy);
		
		while (!q.isEmpty()) {
			cur = q.poll();
			if (cur == dummy) { // use dummy node to detect the last node of each level
				deepth++;
				if (!q.isEmpty())  // finish BFS, need to exist
					q.offer(dummy);
			} 
			if (cur.left != null) 
				q.offer(cur.left);
			if (cur.right != null) 
				q.offer(cur.right);
		}
		
		return deepth;
	}
	
	public int maxDepthByInOrder(TreeNode root) {
        Stack<TreeNode> st = new Stack<TreeNode>();
        int depth = 0;
        TreeNode n = root;
        while (n != null || !st.isEmpty()) {
            while (n != null) {
                st.push(n);
                if (n.left != null)
                    n = n.left;
                else
                    n = n.right;
            }
            if (st.size() > depth)
                depth = st.size();
            n = st.pop();
            while (!st.isEmpty() && st.peek().right == n)
                n = st.pop();
            if (st.isEmpty())
                n = null;
            else
                n = st.peek().right;
        }
    
        return depth;
    }
}
```

<br>


### Go
```go
import (
	stack "github.com/jinghzhu/DataStructure/Go/stack/slice"
)

func GetMaxDepthByInOrder(root *TreeNode) int {
	depth := 0
	st := stack.New()
	n := root
	for n != nil || !st.IsEmpty() {
		for n != nil {
			st.Push(n)
			if n.Left != nil {
				n = n.Left
			} else {
				n = n.Right
			}
		}
		if st.Size() > depth {
			depth = st.Size()
		}
		n = st.Pop().(*TreeNode)
		for !st.IsEmpty() && st.Peek().(*TreeNode).Right == n {
			n = st.Pop().(*TreeNode)
		}
		if st.IsEmpty() {
			n = nil
		} else {
			n = st.Peek().(*TreeNode).Right
		}
	}

	return depth
}
```

```go
func GetMaxDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}

	return 1 + max(GetMaxDepth(root.Left), GetMaxDepth(root.Right))
}

func max(a, b int) int {
	if a > b {
		return a
	}

	return b
}
```

<br>


### Python
```python
class MaxDepth:
    def solution1(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```