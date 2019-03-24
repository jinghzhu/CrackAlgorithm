# <center>1181 - Diameter of Binary Treee</center> 


* Tag: DFS, Binarytree, Recursion
* Company: Facebook, Google
* Author: Jinghua Zhu jhzhu@outlook.com

https://www.lintcode.com/problem/diameter-of-binary-tree/description

<br></br>



## Description
----
Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

<br></br>



## Example
----
Given a binary tree:
```
          1
         / \
        2   3
       / \     
      4   5  
```

Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].

<br></br>



## Solution
----
### Go

```go
func Diameter(root *TreeNode) int {
	if root == nil { // Important
		return 0
	}
	a, b := diameterHelper(root)

	return maxInt(a, b) - 1 // - 1 is important.
}

func diameterHelper(root *TreeNode) (int, int) {
	if root == nil {
		return 0, 0
	}
	// a represents the length of the longest path which ends at the current node.
	// b represents the length of the longest path in the subtree rooted at current node.
	a1, b1 := diameterHelper(root.Left)
	a2, b2 := diameterHelper(root.Right)

	return maxInt(a1, a2) + 1, maxInt(a1+a2+1, maxInt(b1, b2))
}
```


### Java
```java
public class Solution {
	/**
     * @param root: a root of binary tree
     * @return: return a integer
     */
    public int diameterOfBinaryTree(TreeNode root) {
        if (root == null)
            return 0;
        ArrayList<Integer> result = helper(root);
        
        return max(result.get(0), result.get(1)) - 1;
    }
    
    // result.get(0) represents the length of the longest path which ends at the current node.
    // result.get(1) represents the length of the longest path in the subtree rooted at the
    // current node.
    private ArrayList<Integer> helper(TreeNode root) {
        if (root == null) {
            ArrayList<Integer> result = new ArrayList<Integer>();
            result.add(0);
            result.add(0);
            
            return result;
        }
        
        ArrayList<Integer> r_left = helper(root.left);
        ArrayList<Integer> r_right = helper(root.right);
        
        int a = max(r_left.get(0), r_right.get(0)) + 1;
        int b = max(max(r_left.get(1), r_right.get(1)), r_left.get(0) + r_right.get(0) + 1);
        r_left.set(0, a);
        r_left.set(1, b);
        
        return r_left;
    }
    
    private int max(int a, int b) {
        if (a >= b)
            return a;
            
        return b;
    }
}
```