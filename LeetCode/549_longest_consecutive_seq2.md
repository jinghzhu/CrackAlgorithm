# <center>549 - Binary Tree Longest Consecutive Sequence II (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Medium
* Company: Google
* Link: https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii

<br></br>



## Description
----
Given a binary tree, find the length(number of nodes) of the longest consecutive sequence(Monotonic and adjacent node values differ by 1) path.

The path could be start and end at any node in the tree.

<br></br>



## Example
----
Input: {1,2,0,3}

Output: 4

Explanation: 0-1-2-3
```
    1
   / \
  2   0
 /
3
```

<br></br>



## Solution
----
算法：利用ResultType。利用回溯思想较容易。因为当一个结点没有子结点时，它只需跟父结点比较，这种情况最容易处理。一旦叶结点处理完，可一层层回溯，直到根结点。在遍历过程中不断更新结果即可。
1. 其中有三个域，分别是其子树中最大长度，以当前节点为终点的最长递增序列长度，和以当前节点为终点的最长递减序列长度。
2. 递归后序遍历。终止条件仍旧是空节点。
3. 初始化自己三个域值都是1。因为无论怎样，至少还有自己本身节点。
4. 分别计算如下值，然后求最大值返回：
	1. 当前子树中最长递增和递减序列长度。
	2. 当前子树中经过当前节点的最长递增和递减序列长度。
	3. 左右子树返回的最大序列长度。

<br>


### Go
```go
type lcs2ResultType struct {
	down int
	up   int
	max  int
}

// LongestConsecutive2 finds the length of the longest consecutive sequence path.
func LongestConsecutive2(root *TreeNode) int {
	return helper(root).max
}

func helper(root *TreeNode) *lcs2ResultType {
	if root == nil {
		return &lcs2ResultType{0, 0, 0}
	}

	left, right := helper(root.Left), helper(root.Right)
	r := &lcs2ResultType{1, 1, 1} // 都为1，是因为至少还有自己。
	leftDown, rightDown, leftUp, rightUp := 0, 0, 0, 0
	if root.Left != nil && root.Val-root.Left.Val == 1 {
		leftDown = left.down
	}
	if root.Right != nil && root.Val-root.Right.Val == 1 {
		rightDown = right.down
	}
	r.down = maxInt(r.down, maxInt(leftDown, rightDown)+1)

	if root.Left != nil && root.Val-root.Left.Val == -1 {
		leftUp = left.up
	}
	if root.Right != nil && root.Val-root.Right.Val == -1 {
		rightUp = right.up
	}
	r.up = maxInt(r.up, maxInt(leftUp, rightUp)+1)

	max1 := maxInt(leftDown+rightUp+1, leftUp+rightDown+1)
	max2 := maxInt(r.down, r.up)
	r.max = maxInt(maxInt(max1, max2), maxInt(left.max, right.max))

	return r
}
```

<br>


### Java
```java
public class LongestConsecutiveSeq2 {
	public int soltuion(TreeNode root) {
		return helper(root).max;
	}
	
	class LCS2ResultType {
		int down;
		int up;
		int max;
		
		LCS2ResultType(int d, int u, int m) {
			down = d;
			up = u;
			max = m;
		}
	}
	
	private LCS2ResultType helper(TreeNode root) {
		if (root == null)
			return new LCS2ResultType(0, 0, 0);
		
		LCS2ResultType left = helper(root.left);
		LCS2ResultType right = helper(root.right);
		
		int max = 1, down = 1, up = 1, leftDown = 0, leftUp = 0, rightDown = 0, rightUp = 0;
		if (root.left != null && root.val - root.left.val == 1)
			leftDown = left.down;
		if (root.right != null && root.val - root.right.val == 1)
			rightDown = right.down;
		down = Math.max(leftDown, rightDown) + 1;
		
		if (root.left != null && root.val - root.left.val == -1)
			leftUp = left.up ;
		if (root.right != null && root.val - root.right.val == -1)
			rightUp = right.up;
		up = Math.max(leftUp, rightUp) + 1;
		
		int max1 = Math.max(leftDown + rightUp + 1, leftUp + rightDown + 1);
		int max2 = Math.max(down, up);
		max = Math.max(Math.max(max1, max2), Math.max(left.max, right.max));
		
		return new LCS2ResultType(down, up, max);
	}
}
```

<br>


### Python
```python
class LongestConsecutiveSeq2:
    class ResultType:
        def __init__(self, up, down, longest):
            self.up = up
            self.down = down
            self.longest = longest

    """
    @param root: the root of binary tree
    @return: the length of the longest consecutive sequence path
    """
    def solution(self, root):
        return self.helper(root).longest

    def helper(self, root):
        if not root:
            return self.ResultType(0, 0, 0)

        left, right = self.helper(root.left), self.helper(root.right)

        r = self.ResultType(1, 1, 1)
        left_down, left_up, right_down, right_up = 0, 0, 0, 0

        if root.left and root.val - root.left.val == 1:
            left_down = left.down
        if root.right and root.val - root.right.val == 1:
            right_down = right.down
        r.down = max(left_down, right_down) + 1

        if root.left and root.val - root.left.val == -1:
            left_up = left.up
        if root.right and root.val - root.right.val == -1:
            right_up = right.up
        r.up = max(left_up, right_up) + 1

        max1 = max(left.longest, right.longest)
        max2 = max(left_down + right_up, left_up + right_down) + 1
        max3 = max(r.down, r.up)
        r.longest = max(max1, max(max2, max3))

        return r
```