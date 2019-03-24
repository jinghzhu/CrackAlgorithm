# <center>572 - Subtree of Another Tree</center> 


* Tag: BFS, Binarytree, Recursion
* Company: Amazon, eBay

https://leetcode.com/problems/subtree-of-another-tree/

<br></br>



## Description
----
Given two non-empty binary trees s and t, check whether tree t has exactly the same structure and node values with a subtree of s. A subtree of s is a tree consists of a node in s and all of this node's descendants. The tree s could also be considered as a subtree of itself.

<br></br>



## Example
----
Given tree s:
```
     3
    / \
   4   5
  / \
 1   2
```

Given tree t:
```
   4 
  / \
 1   2
```
Return true, because t has the same structure and node values with a subtree of s.

Given tree s:
```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

Given tree t:
```
   4
  / \
 1   2
```

Return false.

<br></br>



## Solution
----
### Go
* Author: Jinghua Zhu jhzhu@outlook.com

```go
func IsSubtree(s, t *TreeNode) bool {
	if s == nil && t == nil {
		return true
	}
	if s == nil || t == nil {
		return false
	}
	if s.Val == t.Val && isSubtreeHelper(s, t) {
		return true
	}

	return IsSubtree(s.Left, t) || IsSubtree(s.Right, t)
}

func isSubtreeHelper(s, t *TreeNode) bool {
	if s == nil && t == nil {
		return true
	}
	if s == nil || t == nil || s.Val != t.Val {
		return false
	}

	return isSubtreeHelper(s.Left, t.Left) && isSubtreeHelper(s.Right, t.Right)
}

```


### Java
```java
public class IsSubtree {
	/**
     * @param s: the root
     * @param t: the root
     * @return: whether tree t has exactly the same structure and node values with a subtree of s
     */
    public boolean solution1(TreeNode s, TreeNode t) {
        if(s == null)
        	return t == null;
        if(s.val == t.val && compare(s,t) )
        	return true;
        
        return solution1(s.left, t) || solution1(s.right, t);
    }
    
    boolean compare(TreeNode s, TreeNode t) {
        if(s == null)
        	return t == null;
        if(t == null || s.val != t.val)
        	return false;
        
        return compare(s.left, t.left) && compare(s.right, t.right);
    }
}
```