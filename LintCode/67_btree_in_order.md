# <center>67 - Binary Tree Inorder Traversal (E)</center> 



<br></br>

* Tag: DFS, Binary Tree, Recursion
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/binary-tree-inorder-traversal/description

<br></br>



## Description
----
Given a binary tree, return the inorder traversal of its nodes' values.

<br></br>



## Example
----
Input: `[1,null,2,3]`.
```
   1
    \
     2
    /
   3
```
Output: `[1,3,2]`

<br></br>



## Solution
----
### Java
```java
public class Traversal {
	public ArrayList<Integer> InOrderN(TreeNode root) {
		ArrayList<Integer> result = new ArrayList<Integer>();
		Stack<TreeNode> s = new Stack<TreeNode>();
		TreeNode p = root;
		
		while(!s.isEmpty() || p != null) {
			while(p != null) {
				s.push(p);
				p = p.getLeft();
			}
			if(!s.isEmpty()) { // important
				p = s.pop();
				result.add(p.val);
				p = p.getRight();
			}
		}
		
		return result;
	}
	
    public ArrayList<Integer> InOrder(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        inHelper(root, result);
        
        return result;
    }
    
    private void inHelper(TreeNode root, ArrayList<Integer> result) {
        if (root != null) {
            inHelper(root.left, result);
            result.add(root.val);
            inHelper(root.right, result);
        }
    }
}
```

<br>


### Python
```python
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        result, st = [], []
        cur = root
        while cur or st:
            while cur:
                st.append(cur)
                cur = cur.left
            if st:
                cur = st[-1]
                st.pop(-1)
                result.append(cur.val)
                cur = cur.right
        
        return result
```

<br>


### Go
```go
func InOrderPrint(node *TreeNode) {
	if node != nil {
		InOrderPrint(node.Left)
		fmt.Printf("%+v ", node.Val)
		InOrderPrint(node.Right)
	}
}
```

```go
func InOrderPrintN(root *TreeNode) {
	n := root
	st := make([]*TreeNode, 0)
	for len(st) != 0 || n != nil {
		for ; n != nil; n = n.Left {
			st = append(st, n)
		}
		if len(st) != 0 { // Important
			n = st[len(st)-1]
			st = st[:len(st)-1]
			fmt.Printf("%+v ", n.Val)
			n = n.Right
		}
	}
}
```

```go
func InOrderN(root *TreeNode) []interface{} {
	result := make([]interface{}, 0)
	st := make([]*TreeNode, 0)
	cur := root
	for cur != nil || len(st) != 0 {
		for ; cur != nil; cur = cur.Left {
			st = append(st, cur)
		}
		if len(st) != 0 { // Important
			cur = st[len(st)-1]
			st = st[:len(st)-1]
			result = append(result, cur.Val)
			cur = cur.Right
		}
	}

	return result
}

```