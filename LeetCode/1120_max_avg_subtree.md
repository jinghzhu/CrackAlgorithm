# <center>1120 - Maximum Average Subtree (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Medium
* Link: https://leetcode.com/problems/maximum-average-subtree

<br></br>



## Description
----
Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

<br></br>



## Example
----
Input：{1,-5,11,1,2,4,-2}

Output：11

The tree is look like this:

```
     1
   /   \
 -5     11
 / \   /  \
1   2 4    -2 
```

The average of subtree of 11 is 4.3333, is the maximun.

<br></br>



## Solution
----
### Java
```java
public class MaxAvgSubtree {
	private class ResultType {
        public int sum, size;
        public ResultType(int sum, int size) {
            this.sum = sum;
            this.size = size;
        }
    }
	
	private TreeNode subtree = null;
    private ResultType subtreeResult = null;
 
	
	/**
     * @param root the root of binary tree
     * @return the root of the maximum average of subtree
     */
    public TreeNode findSubtree(TreeNode root) {
    	helper(root);
        return subtree;
    }
    
    private ResultType helper(TreeNode root) {
        if (root == null) {
            return new ResultType(0, 0);
        }
        
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        ResultType result = new ResultType(
            left.sum + right.sum + root.val,
            left.size + right.size + 1
        );
        
        // 乘法比除法的鲁棒性更好
        if (subtree == null ||
            subtreeResult.sum * result.size < result.sum * subtreeResult.size
        ) {
            subtree = root;
            subtreeResult = result;
        }
        return result;
    }
}
```

<br>
