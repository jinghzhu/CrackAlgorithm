# <center>66 - Binary Tree Preorder Traversal (E)</center> 



<br></br>

* Tag: DFS, Binary Tree, Recursion
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/binary-tree-preorder-traversal/description

<br></br>



## Description
----
Given a binary tree, return the preorder traversal of its nodes' values.

<br></br>



## Solution
----
### Java
```java
public class Traversal {
	public ArrayList<Integer> PreOrderN(TreeNode root) {
		ArrayList<Integer> result = new ArrayList<Integer>();
		Stack<TreeNode> s = new Stack<TreeNode>();
		TreeNode p = root;
		while(!s.isEmpty() || p != null) {
			while(p != null) {
				result.add(p.val);
				if(p.getRight() != null) 
					s.push(p.getRight());
				p = p.getLeft();
			} // while
			if(!s.isEmpty()) // important
				p = s.pop(); 
		}
		
		return result;
	}
	
	public void PreOrderPrint(TreeNode root) {
		if(root != null) {
			System.out.print(root.getVal());
			PreOrderPrint(root.getLeft());
			PreOrderPrint(root.getRight());
		}
	}
	
	public List<Integer> PreOrder(TreeNode root) {
        ArrayList<Integer> result = new ArrayList<Integer>();
        preHelper(root, result);
        
        return result;
    }
    
    private void preHelper(TreeNode root, ArrayList<Integer> result) {
        if (root != null) {
            result.add(root.val);
            preHelper(root.left, result);
            preHelper(root.right, result);
        }
    }
}
```

<br>


### Python
```python
class BTreeTraversal:
    def pre_order_print(self, root):
        if root is None:
            print("Empty tree")
            return
        self.pre_order_print_helper(root)

    def pre_order_print_helper(self, root):
        if root is not None:
            print(root.val)
            self.pre_order_print_helper(root.left)
            self.pre_order_print_helper(root.right)

    def pre_order(self, root):
        result, st = [], []
        cur = root
        while cur is not None and len(st) > 0:
            while cur is not None:
                result.append(cur.val)
                if cur.right is not None:
                    st.append(cur.right)
                cur = cur.left
            if len(st) > 0:
                cur = st[-1]
                result.append(cur.val)
                st.pop(-1)

        return result
```

<br>


### Go
```go
func PreOrderPrint(node *TreeNode) {
	if node != nil {
		fmt.Printf("%+v ", node.Val)
		PreOrderPrint(node.Left)
		PreOrderPrint(node.Right)
	}
}
```

```go
func PreOrderPrintN(root *TreeNode) {
	n := root
	st := make([]*TreeNode, 0)
	for len(st) != 0 || n != nil {
		for ; n != nil; n = n.Left {
			fmt.Printf("%+v ", n.Val)
			if n.Right != nil {
				st = append(st, n.Right)
			}
		}
		if len(st) != 0 { // Important.
			n = st[len(st)-1]
			st = st[:len(st)-1]
		}
	}
}
```

```go
func PreOrderN(root *TreeNode) []interface{} {
	result := make([]interface{}, 0)
	st := make([]*TreeNode, 0)
	cur := root

	for cur != nil || len(st) != 0 {
		for ; cur != nil; cur = cur.Left {
			result = append(result, cur.Val)
			if cur.Right != nil {
				st = append(st, cur.Right)
			}

		}
		if len(st) != 0 { // Important
			cur = st[len(st)-1]
			st = st[:len(st)-1]
		}
	}

	return result
}
```