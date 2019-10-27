# <center>94 - Binary Tree Maximum Path Sum (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Medium
* Company: Microsoft, Baidu
* Link: https://www.lintcode.com/problem/binary-tree-maximum-path-sum/description

<br></br>



## Description
----
Given a binary tree, find the maximum path sum.

The path may start and end at any node in the tree.

<br></br>



## Example
----
Input: [-10,9,20,null,null,15,7]
```
   -10
   / \
  9  20
    /  \
   15   7
```

Output: 42

<br></br>



## Solution
----
1. 仍旧利用ResultType，其中有两个域，分别是当前子树中最大值和，到当前节点为止最大值。
2. 以递归后序遍历。终止条件是空节点。
3. 回溯时，对到当前节点为止最大值，比较如下值：
    1. 当前节点值；
    2. 到左（右）儿子节点最大值 + 当前节点值。
4. 对当前子树最大值，比较：
    1. 到当前节点为止最大值；
    2. 左（右）子树最大值；
    3. 到左儿子节点最大值 + 当前节点值 + 到右儿子节点最大值。

<br>


### Go
```go
func MaxPathSum(root *TreeNode) int {
	return mpsHelper(root).maxInSubTree
}

type mpsResultType struct {
	maxInSubTree int
	maxInPath    int
}

func mpsHelper(root *TreeNode) *mpsResultType {
	if root == nil {
		return &mpsResultType{IntegerMin, IntegerMin}
	}

	// Divide
	left, right := mpsHelper(root.Left), mpsHelper(root.Right)
	// Conquer
	maxInPath := maxInt(root.Val, left.maxInPath+root.Val, right.maxInPath+root.Val)
	maxInSubTree := maxInt(root.Val, left.maxInSubTree, right.maxInSubTree, maxInPath, left.maxInPath+root.Val+right.maxInPath)

	return &mpsResultType{maxInSubTree, maxInPath}
}
```

<br>


### Java
```java
public class MaxPathSum {
	public int solution(TreeNode root) {
        ResultType2 result = helper2(root);
        return result.maxPath;
    }
	
	private class ResultType2 {
        // singlePath: 从root往下走到任意点的最大路径，这条路径可以不包含任何点
        // maxPath: 从树中任意到任意点的最大路径，这条路径至少包含一个点
        int singlePath, maxPath; 
        ResultType2(int singlePath, int maxPath) {
            this.singlePath = singlePath;
            this.maxPath = maxPath;
        }
    }

    private ResultType2 helper2(TreeNode root) {
        if (root == null) 
            return new ResultType2(0, Integer.MIN_VALUE);
        
        // Divide
        ResultType2 left = helper2(root.left);
        ResultType2 right = helper2(root.right);

        // Conquer
        int singlePath = Math.max(left.singlePath, right.singlePath) + root.val;
        singlePath = Math.max(singlePath, root.val);
        int maxPath = Math.max(left.maxPath, right.maxPath);
        maxPath = Math.max(root.val, maxPath);
        maxPath = Math.max(Math.max(maxPath, singlePath), left.singlePath + right.singlePath + root.val);

        return new ResultType2(singlePath, maxPath);
    }
}
```

<br>


### Python
```python
class MaxPathSum:
    class ResultType:
        def __init__(self, max_subtree, max_path):
            self.max_subtree = max_subtree
            self.max_path = max_path

    def maxPathSum(self, root: TreeNode) -> int:
        return self.helper(root).max_subtree

    def helper(self, root: TreeNode) -> ResultType:
        if not root:
            return self.ResultType(-2147483648, -2147483648)

        # divide
        left, right = self.helper(root.left), self.helper(root.right)

        # conquer
        max_path = max(root.val, max(left.max_path, right.max_path) + root.val)
        max_subtree = max(left.max_subtree, right.max_subtree)
        max_subtree = max(left.max_path + root.val + right.max_path, max(max_path, max_subtree))

        return self.ResultType(max_subtree, max_path)
```