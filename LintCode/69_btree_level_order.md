# <center>69 - Binary Tree Level Order Traversal (E)</center> 



<br></br>

* Tag: Binary Tree, BFS
* Company: LinkedIn, Microsoft, Apple, Amazon, Uber
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/binary-tree-level-order-traversal/description

<br></br>



## Description
----
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

<br></br>



## Solution
----
### Java
```java
public class No102BinaryTreeLevelOrderTraversal {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        if (root == null)
            return result;
        
        Queue<TreeNode> q = new LinkedList<TreeNode>();
        q.offer(root);
        while (!q.isEmpty()) {
            List<Integer> temp = new ArrayList<Integer>();
            int len = q.size();
            for (int i = 0; i < len; i++) {
                TreeNode n = q.poll();
                temp.add(n.val);
                if (n.left != null)
                    q.offer(n.left);
                if (n.right != null)
                    q.offer(n.right);
            }
            result.add(temp);
        }
        
        return result;
    }
}
```

<br>


### Go
```go
func LevelOrder(root *TreeNode) [][]int {
	result := make([][]int, 0)
	if root == nil {
		return result
	}

	q := make([]*TreeNode, 0)
	q = append(q, root)
	level := 0
	for len(q) != 0 {
		length := len(q)
		result = append(result, make([]int, length, length))
		for i := 0; i < length; i++ {
            cur := q[0]
            q = q[1:]
			result[level] = append(result[level], cur.Val)
			if cur.Left != nil {
				q = append(q, cur.Left)
			}
			if cur.Right != nil {
				q = append(q, cur.Right)
			}
		}
		level++
	}

	return result
}
```

<br>


### Python
```python
class Solution:
    """
    @param root: A Tree
    @return: Level order a list of lists of integer
    """
    def levelOrder(self, root):
        q, result = [], []
        if root is None:
            return result
        q.append(root)
        while q:
            l = len(q)
            tmp =[]
            for i in range(l):
                n = q[0]
                q.pop(0)
                tmp.append(n.val)
                if n.left:
                    q.append(n.left)
                if n.right:
                    q.append(n.right)
            result.append(tmp)

        return result
```