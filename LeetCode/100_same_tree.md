# <center>100 - Same Tree (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, BFS, Recursion
* Difficulty: Easy
* Link: https://leetcode.com/problems/same-tree/

<br></br>



## Description
----
Check if two binary trees are identical. Identical means the two binary trees have the same structure and every identical position has the same value.

<br></br>



## Solution
----
### Go
```go
func IsSameBTree1(p, q *TreeNode) bool {
	n1, n2 := p, q
	st1, st2 := make([]*TreeNode, 0), make([]*TreeNode, 0)
	for (len(st1) > 0 || n1 != nil) && (len(st2) > 0 || n2 != nil) {
		for n1 != nil && n2 != nil {
			if n1.Val != n2.Val {
				return false
			}
			if n1.Right != nil {
				st1 = append(st1, n1.Right)
			}
			if n2.Right != nil {
				st2 = append(st2, n2.Right)
			}
			if len(st1) != len(st2) {
				return false
			}
			n1, n2 = n1.Left, n2.Left
		}
		if n1 != n2 {
			return false
		}
		if len(st1) > 0 {
			n1, n2 = st1[len(st1)-1], st2[len(st2)-1]
			st1, st2 = st1[:len(st1)-1], st2[:len(st2)-1]
		}
	}

	return n1 == nil && n1 == n2 && len(st1) == 0 && len(st2) == 0
}
```

```go
func IsSameBTree2(a, b *TreeNode) bool {
	if a == nil && b == nil {
		return true
	}
	if a == nil || b == nil || a.Val != b.Val {
		return false
	}

	return IsSameBTree2(a.Left, b.Left) && IsSameBTree2(a.Right, b.Right)
}
```

<br>


### Java
```java
public class IsSameBTree {
	public boolean isSame(TreeNode root1, TreeNode root2) {
		if (root1 == null && root2 == null) 
			return true;
		if (root1 == null || root2 == null || root1.val != root2.val) 
			return false;
		
		return isSame(root1.left, root2.left) && isSame(root1.right, root2.right);
	}
	
	public boolean isSameN(TreeNode root1, TreeNode root2) {
		Stack<TreeNode> st1 = new Stack<TreeNode>();
		Stack<TreeNode> st2 = new Stack<TreeNode>();
		TreeNode cur1 = root1, cur2 = root2;
		
		while ((cur1 != null || !st1.isEmpty()) && (cur2 != null || !st2.isEmpty())) {
			while (cur1 != null && cur2 != null) {
				if (cur1.val != cur2.val) 
					return false;
				if (cur1.right != null) 
					st1.push(cur1.right);
				if (cur2.right != null) 
					st2.push(cur2.right);
				if (st1.size() != st2.size())  // Important
					return false;
				cur1 = cur1.left;
				cur2 = cur2.left;
			}
			if (cur1 != null || cur2 != null)  // Important
				return false;
			if (!st1.isEmpty() && !st2.isEmpty()) {
				cur1 = st1.pop();
				cur2 = st2.pop();
			}
		}
		
		return cur1 == null && cur2 == null && st1.isEmpty() && st2.isEmpty(); // Important
	}
}
```

<br>


### Python
```python
class IsSameBTree:
    def soltuion(self, p: TreeNode, q: TreeNode) -> bool:
        st1, st2 = [], []
        n1, n2 = p, q
        while (len(st1) > 0 or n1) and (len(st2) > 0 or n2):
            while n1 and n2:
                if n1.val != n2.val:
                    return False
                if n1.right:
                    st1.append(n1.right)
                if n2.right:
                    st2.append(n2.right)
                if len(st1) != len(st2):  # important
                    return False
                n1, n2 = n1.left, n2.left
            if n1 != n2:  # important
                return False
            if len(st1) > 0:
                n1, n2 = st1.pop(-1), st2.pop(-1)

        return not n1 and not n2 and len(st1) == 0 and len(st2) == 0
```