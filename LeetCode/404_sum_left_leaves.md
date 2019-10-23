# <center>404 - Sum of Left Leaves (E)</center> 


<br></br>

* Tag: Binary Tree, Recursion, DFS
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Company: Facebook
* Link: https://leetcode.com/problems/sum-of-left-leaves/

<br></br>



## Description
----
Find the sum of all left leaves in a given binary tree.

<br></br>



## Example
----
There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

```
  3
   / \
  9  20
    /  \
   15   7
```

<br></br>



## Solution
----
算法：
- 递归：递归遍历二叉树的节点，判断当前节点是否有左儿子，进而判断左儿子是否为叶子节点。
- 非递归中序遍历：每次出栈后，检查此时栈定元素的左儿子是不是刚出栈的节点。

<br>


### Go
```go
func SumLeftLeaves1(root *TreeNode) int {
	count := 0
	st := make([]*TreeNode, 0)
	n := root

	for n != nil || len(st) > 0 {
		for ; n != nil; n = n.Left {
			st = append(st, n)
		}
		l := len(st)
		if l > 0 {
			n = st[l-1]
			st = st[:l-1]
			if len(st) > 0 && n.Left == nil && n.Right == nil && st[len(st)-1].Left == n {
				count += n.Val
			}
			n = n.Right
		}
	}

	return count
}
```

```go
func SumLeftLeaves2(root *TreeNode) int {
	if root == nil {
		return 0
	}

	sum := 0
	if root.Left != nil {
		left := root.Left
		if left.Left == nil && left.Right == nil { // 如果左儿子为叶子节点
			sum += left.Val
		} else { // 否则继续递归遍历左子树
			sum += SumLeftLeaves2(left)
		}
	}
	// 递归遍历右子树
	if root.Right != nil {
		right := root.Right
		sum += SumLeftLeaves2(right)
	}

	return sum
}
```

<br>


### Java
```java
public class SumLeftLeaves {
	// 递归遍历二叉树的节点，判断当前节点是否有左儿子，进而判断左儿子是否为叶子节点。
	public int solution1(TreeNode root) {
        if(root == null)
            return 0;

        int sum = 0;
        if (root.left != null) {
            TreeNode left = root.left;
            
            if(left.left == null && left.right == null) // 如果左儿子为叶子节点
                sum += left.val;
            else // 否则继续递归遍历左子树
                sum += solution1(left);
        }
        // 递归遍历右子树
        if(root.right != null) {
            TreeNode right = root.right;
            sum += solution1(right);
        }
        
        return sum;
    }
}
```

<br>


### Python
```python
class SumLeftLeaves:
    # 递归遍历二叉树的节点，判断当前节点是否有左儿子，进而判断左儿子是否为叶子节点。
    def solution(self, root: TreeNode) -> int:
        if not root:
            return 0
        count = 0
        if root.left:
            left = root.left
            if not left.left and not left.right:
                count += left.val
            else:
                count += self.solution(left)
        if root.right:
            count += self.solution(root.right)

        return count
```