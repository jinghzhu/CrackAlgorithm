# <center>112 - Path Sum I (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Easy
* Company: Microsoft
* Link: https://leetcode.com/problems/path-sum/

<br></br>



## Description
----
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.

<br></br>



## Example
----
Given the below binary tree and sum = 22,

```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```

return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.

<br></br>



## Solution
----
两种算法：
1. 非递归后序遍历。每次出栈时检查是否为叶子节点。如果是，计算栈中元素和是否符合要求。
2. 递归先序遍历。终止条件为空节点和叶子节点。

<br>


### Java
```java
public class PathSum1 {
	public boolean solution(TreeNode root, int sum) {
        if (root == null)
            return false;
        if (root.left == null && root.right == null)
            return sum == root.val;
        return solution (root.left, sum - root.val) || solution(root.right, sum - root.val);
    }
}
```

<br>


### Python
```python
class PathSum1:
    def solution(self, root: TreeNode, sum: int) -> bool:
        st = []
        n = root

        while n or len(st) > 0:
            while n:
                st.append(n)
                if n.left:
                    n = n.left
                else:
                    n = n.right
            if self.is_leaf(st[-1]) and self.is_valid(st, sum):
                return True
            n = st.pop(-1)
            while len(st) > 0 and st[-1].right == n:
                n = st.pop(-1)
            if len(st) < 1:
                n = None
            else:
                n = st[-1].right

        return False

    def is_leaf(self, n):
        return n.left == None and n.right == None

    def is_valid(self, st, sum):
        i = 0
        for j in st:
            i += j.val
        return i == sum
```

<br>


### Go
```go
func PathSum1(root *TreeNode, sum int) bool {
	if root == nil {
		return false
	}
	if root.Left == nil && root.Right == nil {
		return root.Val == sum
	}
	return PathSum1(root.Left, sum-root.Val) || PathSum1(root.Right, sum-root.Val)
}
```