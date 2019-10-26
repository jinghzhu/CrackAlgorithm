# <center>298 - Binary Tree Longest Consecutive Sequence (M)</center> 



<br></br>

* Tag: Binary Tree, DFS, Recursion
* Difficulty: Medium
* Company: Google
* Link: https://leetcode.com/problems/binary-tree-longest-consecutive-sequence

<br></br>



## Description
----
Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (cannot be the reverse).

<br></br>



## Example
----
Input:
```
   1
    \
     3
    / \
   2   4
        \
         5
```

Output:3 Longest consecutive sequence path is 3-4-5, so return 3.

Input:
```
   2
    \
     3
    / 
   2    
  / 
 1
```

Output:2 Longest consecutive sequence path is 2-3,not 3-2-1, so return 2.

<br></br>



## Solution
----
算法：
- 递归先序遍历。
    1. 递归函数有三个参数，分别是当前节点，当前节点的父节点和到父节点为止的最长上升序列长度。
	2. 比较当前节点和父节点是否构成上升序列关系，如果是的话长度+1，否则长度重新计算。
	3. 待左右节点递归计算完毕，返回至当前节点为止最大长度、左子树和右子树最大长度中最大值。
- ResultType
	1. 其中有两个域，分别表示到当前节点为止最大长度和当前节点的子树中最大长度。
	2. 递归后序遍历。终止条件为空节点，此时两个值都是0。
	3. 判断左右子树结果。如果左（右）儿子节点与当前节点构成上升序列关系，那么到当前节点为止最大长度 + 1。然后，再和左右子树中最大长度比较。

<br>


### Go
```go
func LongestConsecutiveSeq11(root *TreeNode) int {
	return lcs11Helper1(root, nil, 0)
}

func lcs11Helper1(root, parent *TreeNode, parentSeqLen int) int {
	if root == nil {
		return parentSeqLen
	}
	rootSeqLen := 1
	if parent != nil && parent.Val+1 == root.Val {
		rootSeqLen = parentSeqLen + 1
	}
	left := lcs11Helper1(root.Left, root, rootSeqLen)
	right := lcs11Helper1(root.Right, root, rootSeqLen)

	return maxInt(rootSeqLen, maxInt(left, right))
}
```

```go
func LongestConsecutiveSeq12(root *TreeNode) int {
	return lcs12Helper(root).maxLenInSubtree
}

type lcs1ResultType struct {
	maxLenInSubtree int
	maxLenFromRoot  int
}

func lcs12Helper(root *TreeNode) *lcs1ResultType {
	if root == nil {
		return &lcs1ResultType{0, 0}
	}

	leftRT, rightRT := lcs12Helper(root.Left), lcs12Helper(root.Right)
	r := &lcs1ResultType{0, 1}
	if root.Left != nil && root.Left.Val-root.Val == 1 {
		r.maxLenFromRoot = maxInt(r.maxLenFromRoot, leftRT.maxLenFromRoot+1)
	}
	if root.Right != nil && root.Right.Val-root.Val == 1 {
		r.maxLenFromRoot = maxInt(r.maxLenFromRoot, rightRT.maxLenFromRoot+1)
	}
	r.maxLenInSubtree = maxInt(r.maxLenFromRoot, maxInt(leftRT.maxLenInSubtree, rightRT.maxLenInSubtree))

	return r
}
```

<br>


### Java
```java
public class LongestConsecutiveSeq1 {
	public int solution1(TreeNode root) {
        return helper1(root, null, 0);
    }
    
	private int helper1(TreeNode root, TreeNode par, int parentSeqLen) {
		if (root == null)
			return 0;
		
		int len = 1;
		if (par != null && par.val + 1 == root.val)
			 len = ++parentSeqLen;
		int left = helper1(root.left, root, len);
		int right = helper1(root.right, root, len);
		
		return Math.max(len, Math.max(left, right));
	}
	
	private class ResultType {
		int maxInSubtree;
		int maxFromRoot;
		
		public ResultType(int maxInSubtree, int maxFromRoot) {
			this.maxInSubtree = maxInSubtree;
			this.maxFromRoot = maxFromRoot;
		}
	}
 
	public int solution2(TreeNode root) {
		return helper2(root).maxInSubtree;
	}
 
	private ResultType helper2(TreeNode root) {
		if (root == null) 
			return new ResultType(0, 0);
     
		ResultType left = helper2(root.left);
		ResultType right = helper2(root.right);
		ResultType result = new ResultType(0, 1); // 1 is the root itself.
     
		if (root.left != null && root.val + 1 == root.left.val) 
			result.maxFromRoot = Math.max(result.maxFromRoot, left.maxFromRoot + 1);
		if (root.right != null && root.val + 1 == root.right.val) 
			result.maxFromRoot = Math.max(result.maxFromRoot, right.maxFromRoot + 1);
     
		result.maxInSubtree = Math.max(result.maxFromRoot, 
				Math.max(left.maxInSubtree, right.maxInSubtree));
     
		return result;
	}
}
```

<br>


### Python
```python
class LongestConsecutiveSeq1:
    """
    @param root: the root of binary tree
    @return: the length of the longest consecutive sequence path
    """
    def soltuion1(self, root):
        return self.soltuion1_helper(root, None, 0)

    def soltuion1_helper(self, root, parent, parent_seq_len):
        if not root:
            return parent_seq_len
        l = 1
        if parent and root.val - 1 == parent.val:
            l = parent_seq_len + 1
        left = self.helper(root.left, root, l)
        right = self.helper(root.right, root, l)

        return max(l, max(left, right))
```