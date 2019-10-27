# <center>596 - Minimum Subtree (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Easy
* Company: Microsoft, Yelp
* Link: https://www.lintcode.com/problem/minimum-subtree/

<br></br>



## Description
----
Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

<br></br>



## Example
----
Input:
```
     1
   /   \
 -5     2
 / \   /  \
0   2 -4  -5
```

Output:1

<br></br>



## Solution
----
### Java
```java
public class MinSubtree {
	private static int MIN_VALUE = Integer.MAX_VALUE;
	private static TreeNode SUB_ROOT = null;
	
	/**
	 * @author jhzhu@outlook.com
     * @param root the root of binary tree
     * @return the root of the minimum subtree
     */
	public TreeNode findSubtree(TreeNode root) {
		helper(root);
		return SUB_ROOT;
	}
	
	private int helper(TreeNode root) {
		if (root == null)
			return 0;
		
		int left = helper(root.left);
		int right = helper(root.right);
		int sum = left + right + root.val;
		if (sum < MIN_VALUE) {
			MIN_VALUE = sum;
			SUB_ROOT = root;
		}
		
		return sum;
	}
	
	class ResultType {
		int val;
		TreeNode subRoot;
		
		public ResultType(int v, TreeNode n) {
			val = v;
			subRoot = n;
		}
	}
}
```

Version 2: Pure divide conquer
```java
class MinSubtreeSolution2 {
	class ResultType {
		 public TreeNode minSubtree;
		 public int sum, minSum;
		 public ResultType(TreeNode minSubtree, int minSum, int sum) {
		     this.minSubtree = minSubtree;
		     this.minSum = minSum;
		     this.sum = sum;
		 }
	}
	
	public TreeNode findSubtree(TreeNode root) {
		ResultType result = helper(root);
		
		return result.minSubtree;
	}
 
	public ResultType helper(TreeNode node) {
		if (node == null)
			return new ResultType(null, Integer.MAX_VALUE, 0);
     
		ResultType leftResult = helper(node.left);
		ResultType rightResult = helper(node.right);
		ResultType result = new ResultType(
				node,
				leftResult.sum + rightResult.sum + node.val,
				leftResult.sum + rightResult.sum + node.val
		);
     
		if (leftResult.minSum < result.minSum) {
			result.minSum = leftResult.minSum;
			result.minSubtree = leftResult.minSubtree;
		}
		if (rightResult.minSum < result.minSum) {
			result.minSum = rightResult.minSum;
			result.minSubtree = rightResult.minSubtree;
		}
     
		return result;
	}
}
```

<br>
