# <center>110 - Balanced Binary Tree (E)</center> 


<br></br>

* Tag: Binary Tree, Recursion, DFS
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://leetcode.com/problems/balanced-binary-tree/

<br></br>



## Description
----
Given a binary tree, determine if it is height-balanced.

<br></br>



## Solution
----
算法：
1. 后序遍历递归，每次检查左右子树深度。
2. 利用ResultType。

<br>


### Go
```go
type ibResultType struct {
	isBalanced bool
	depth      int
}

// IsBalanced1 determines if it is height-balanced.
func IsBalanced1(root *TreeNode) bool {
	return ibHelper1(root).isBalanced
}

func ibNewResultType(ib bool, de int) *ibResultType {
	return &ibResultType{
		isBalanced: ib,
		depth:      de,
	}
}

func ibHelper1(root *TreeNode) *ibResultType {
	if root == nil {
		return ibNewResultType(true, 0)
	}

	left, right := ibHelper1(root.Left), ibHelper1(root.Right)
	if !left.isBalanced || !right.isBalanced || absInt(left.depth-right.depth) > 1 {
		return ibNewResultType(false, -1)
	}
	return ibNewResultType(true, maxInt(left.depth, right.depth)+1)
}
```

```go
func IsBalanced2(root *TreeNode) bool {
	return ibHelper2(root) != -1
}

func ibHelper2(root *TreeNode) int {
	if root == nil {
		return 0
	}
	left, right := ibHelper2(root.Left), ibHelper2(root.Right)
	if left == -1 || right == -1 || absInt(left-right) > 1 {
		return -1
	}

	return maxInt(left, right) + 1
}
```

<br>


### Java
```java
public class IsBalancedTree {
	// Version 1: using ResultType
    public boolean isBalanced1(TreeNode root) {
        return helper(root).isBalanced;
    }
    
    private ResultType helper(TreeNode root) {
        if (root == null) 
            return new ResultType(true, 0);
        
        ResultType left = helper(root.left);
        ResultType right = helper(root.right);
        
        if (!left.isBalanced || !right.isBalanced || Math.abs(left.maxDepth - right.maxDepth) > 1) 
            return new ResultType(false, -1);
        
        return new ResultType(true, Math.max(left.maxDepth, right.maxDepth) + 1);
    }
    
    class ResultType {
        public boolean isBalanced;
        public int maxDepth;
        public ResultType(boolean isBalanced, int maxDepth) {
            this.isBalanced = isBalanced;
            this.maxDepth = maxDepth;
        }
    }
    
    /**
     * @param root: The root of binary tree.
     * @return: True if this Binary tree is Balanced, or false.
     */
    // Version 2: without ResultType
    public boolean isBalanced2(TreeNode root) {
        return maxDepth(root) != -1;
    }

    private int maxDepth(TreeNode root) {
        if (root == null) 
            return 0;

        int left = maxDepth(root.left), right = maxDepth(root.right);
        if (left == -1 || right == -1 || Math.abs(left-right) > 1) 
            return -1;
        
        return Math.max(left, right) + 1;
    }
}
```

<br>


### Python
```python
class IsBalanced:
    def solution(self, root: TreeNode) -> bool:
        return self.dfs(root) != -1

    def dfs(self, root):
        if not root:
            return 0
        left, right = self.dfs(root.left), self.dfs(root.right)
        if left == -1 or right == -1 or abs(left - right) > 1:
            return -1

        return max(left, right) + 1
```