# <center>111 - Minimum Depth of Binary Tree (E)</center> 



<br></br>

* Tag: Recursion, Binary Tree, DFS, BFS
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/minimum-depth-of-binary-tree/

<br></br>



## Description
----
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

<br></br>



## Example
----
1. Input: {} Output: 0
2. Input:  {1,#,2,3} Output: 3	

<br></br>



## Solution
----
### Java
```java
public class MinDepth {
	/**
     * @param root: The root of binary tree.
     * @return: An integer.
     */
	public int minDepth(TreeNode root) {
        // Corner case. For root = NULL
        if (root == null)
            return 0;

        // Base case: leaf node. This accounts for height = 1.
        if (root.left == null && root.right == null)
            return 1;

        // If left subtree is NULL, repeat for right subtree.
        if (root.left == null)
            return minDepth(root.right) + 1;

        // If right subtree is NULL, repeat for right subtree.
        if (root.right == null)
            return minDepth(root.left) + 1;

        return Math.min(minDepth(root.left), minDepth(root.right)) + 1;
    }
	
	public int minDepthByLevelOrder(TreeNode root) {
        if (root == null)
            return 0;
        
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(root);
        int level = 0;
        
        while (!q.isEmpty()) {
            level++;
            int l = q.size();
            for (int i = 0; i < l; i++) {
                TreeNode n = q.poll();
                if (n.left == null && n.right == null)
                    return level;
                if (n.left != null)
                    q.offer(n.left);
                if (n.right != null)
                    q.offer(n.right);
            }
        }
        
        return level;
    }
	
	public int minDepthByPostOrder(TreeNode root) {
        int height = Integer.MAX_VALUE;
        if(root == null) 
            return 0;
        
        TreeNode n = root;
		Stack<TreeNode> st = new Stack<TreeNode>();
		
		while (n != null || !st.isEmpty()) {
			while (n != null) {
				st.push(n);
				if(n.left != null) 
					n = n.left;
				else 
					n = n.right;
			} // inner while
			
			if (st.peek().isLeaf() && st.size() < height) // Important to check if it is a leaf node.
				height = st.size();
			
			n = st.pop();
			
			while (!st.isEmpty() && st.peek().right == n) 
				n = st.pop();
			
			if(!st.empty()) 
				n = st.peek().right;
			else 
				n = null;
		} // out while
		
		return height;
    }
}

```

<br>


### Go
```go
func GetMinDepthByPostOrder(root *TreeNode) int {
	if root == nil {
		return 0
	}
	min := -1
	cur := root
	s := make([]*TreeNode, 0)

	for len(s) != 0 || cur != nil {
		for cur != nil {
			s = append(s, cur)
			if cur.Left != nil {
				cur = cur.Left
			} else {
				cur = cur.Right
			}
		}
		// Need to check s.Peek(). Because s.Peek().Right may set it to a non-leaf node.
		if s[len(s)-1].Left == nil && s[len(s)-1].Right == nil {
			if len(s) < min || min == -1 {
				min = len(s)
			}
		}
		cur = s[len(s)-1]
		s = s[:len(s)-1]
		for len(s) != 0 && s[len(s)-1].Right == cur {
			cur = s[len(s)-1]
			s = s[:len(s)-1]
		}
		if len(s) == 0 {
			cur = nil
		} else {
			cur = s[len(s)-1].Right
		}
	}

	return min
}
```

```go
func GetMinDepth(root *TreeNode) int {
	if root == nil {
		return 0
	}
	if root.Left == nil && root.Right == nil {
		return 1
	}
	if root.Left == nil {
		return 1 + GetMinDepth(root.Right)
	}
	if root.Right == nil {
		return 1 + GetMinDepth(root.Left)
	}

	return min(GetMinDepth(root.Left), GetMinDepth(root.Right)) + 1
}

func min(a, b int) int {
	if a <= b {
		return a
	}

	return b
}
```