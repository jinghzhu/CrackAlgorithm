# <center>145 - Binary Tree Postorder Traversal (H)</center> 



<br></br>

* Tag: DFS, Binary Tree, Recursion
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Company: Microsoft
* Difficulty: Hard
* Link: https://leetcode.com/problems/binary-tree-postorder-traversal/

<br></br>



## Description
----
Given a binary tree, return the postorder traversal of its nodes' values.

<br></br>



## Example
----
Input: `{1,2,3}`.
```
   1
  / \
 2   3
```
Output: `[2,3,1]`

<br></br>



## Solution
----
### Java
```java
public class Traversal {
	public ArrayList<Integer> PostOrderN(TreeNode root) {
		ArrayList<Integer> result = new ArrayList<Integer>();
        TreeNode cur = root;
        Stack<TreeNode> st = new Stack<TreeNode>();
        
        while (cur != null || !st.isEmpty()) {
            while (cur != null) {
                st.push(cur);
                if (cur.left != null)
                    cur = cur.left;
                else
                    cur = cur.right;
            }
            cur = st.pop();
            result.add(cur.val);
            
            // WHILE is important, in case there is only one node.			
            while (!st.isEmpty() && st.peek().right == cur) {
                cur = st.pop();
                result.add(cur.val);
            }
            
            // IF is important, in case there is only one node.
            if (!st.isEmpty())
                cur = st.peek().right;
            else
                cur = null;
        }
        
        return result;
	}
	
	public void PostOrderPrint(TreeNode root) {
		if(root != null) {
			PostOrderPrint(root.getLeft());
			PostOrderPrint(root.getRight());
			System.out.println(root.getVal());
		}
	}
}
```

<br>


### Python
```python
class Solution:
        def post_order_print_helper(self, root):
        if root:
            self.post_order_print_helper(root.left)
            self.post_order_print_helper(root.right)
            print(root.val)

    def post_order(self, root):
        st, result = [], []
        cur = root
        while cur or st:
            while cur:
                st.append(cur)
                if cur.left:
                    cur = cur.left
                else:
                    cur = cur.right
            cur = st[-1]
            st.pop(-1)
            result.append(cur.val)
            while st and st[-1].right is cur:
                cur = st[-1]
                st.pop(-1)
                result.append(cur.val)

            if st:
                cur = st[-1].right
            else:
                cur = None
```

<br>


### Go
```go
func PostOrderPrint(node *TreeNode) {
	if node != nil {
		PostOrderPrint(node.Left)
		PostOrderPrint(node.Right)
		fmt.Printf("%+v ", node.Val)
	}
}
```

```go
func PostOrderPrintN(root *TreeNode) {
	n := root
	st := make([]*TreeNode, 0)

	for len(st) != 0 || n != nil {
		for n != nil {
			st = append(st, n)
			if n.Left != nil {
				n = n.Left
			} else {
				n = n.Right
			}
		}
		n = st[len(st)-1]
		st = st[:len(st)-1]
		fmt.Printf("%+v ", n.Val)

		// Important to check if stack is empty.
		for len(st) != 0 && st[len(st)-1].Right == n {
			n = st[len(st)-1]
			st = st[:len(st)-1]
			fmt.Printf("%+v ", n.Val)
		}

		if len(st) != 0 {
			n = st[len(st)-1].Right
		} else {
			n = nil
		}
	}
}
```

```go
func PostOrderN(root *TreeNode) []interface{} {
	result := make([]interface{}, 0)
	st := make([]*TreeNode, 0)
	cur := root

	for len(st) != 0 || cur != nil {
		for cur != nil {
			st = append(st, cur)
			if cur.Left != nil {
				cur = cur.Left
			} else {
				cur = cur.Right
			}
		}
		cur = st[len(st)-1]
		st = st[:len(st)-1]
		result = append(result, cur.Val)
		for len(st) != 0 && st[len(st)-1].Right == cur {
			cur = st[len(st)-1]
			st = st[:len(st)-1]
			result = append(result, cur.Val)
		}
		if len(st) != 0 {
			cur = st[len(st)-1].Right
		} else {
			cur = nil
		}
	}

	return result
}
```