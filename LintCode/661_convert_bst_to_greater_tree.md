# <center>661 - Convert BST to Greater Tree (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: DFS, BST, Recursion
* Company: Amazon
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/convert-bst-to-greater-tree/description

<br></br>



## Description
----
Given a Binary Search Tree (BST), convert it to a Greater Tree such that every key of the original BST is changed to the original key plus sum of all keys greater than the original key in BST.

<br></br>



## Example
----
Input:
```
              5
            /   \
           2     13
```

Output:
```
             18
            /   \
          20     13
```

<br></br>



## Solution
----
The key is that the list of in-order traversal of BST records the nodes from smaller value to larger one.


### Go
```go
var sumGreaterTree int

func ConvertBST2GreaterTree(root *TreeNode) *TreeNode {
	if root != nil {
		convertBST(root.Right)
		root.Val += sumGreaterTree
		sumGreaterTree = root.Val
		convertBST(root.Left)
	}

	return root
}
```

```go
func ConvertBST2GreaterTreeN(root *TreeNode) *TreeNode {
	n := root
	st := make([]*TreeNode, 0)
	sum := 0
	for n != nil || len(st) != 0 {
		for ; n != nil; n = n.Right {
			st = append(st, n)
		}
		if len(st) != 0 {
			n = st[len(st)-1]
			st = st[:len(st)-1]
			n.Val += sum
			sum = n.Val
			n = n.Left
		}
	}

	return root
}
```


### Java
```java
public class BST2GreaterTree {
	private int sum = 0;
	
	public TreeNode solution(TreeNode root) {
        if (root != null) {
        	solution(root.right);
            root.val += sum;
            sum = root.val;
            solution(root.left);
        }
        
        return root;
    }
	
	public TreeNode solutionN(TreeNode root) {
        Stack<TreeNode> st = new Stack<TreeNode>();
        TreeNode n = root;
        int sumN = 0;
        while (n != null || !st.isEmpty()) {
            while (n != null) {
                st.push(n);
                n = n.right;
            }
            if (!st.isEmpty()) {
                n = st.pop();
                n.val += sumN;
                sumN = n.val;
                n = n.left;
            }
        }
        
        return root;
    }
}
```

<br>


### Python
```python
class Convert2GreaterTree:
    def __init__(self):
        self.val_tmp = 0

    """
    @param root: the root of binary tree
    @return: the new root
    """
    def solution1(self, root):
        if root:
            self.convertBST(root.right)
            root.val += self.val_tmp
            self.val_tmp = root.val
            self.convertBST(root.left)
        return root

    """
    @param root: the root of binary tree
    @return: the new root
    """
    def solution2(self, root):
        n = root
        st = []
        val = 0
        while len(st) > 0 or n:
            while n:
                st.append(n)
                n = n.right
            if len(st) > 0:
                n = st.pop()
                n.val += val
                val = n.val
                n = n.left

        return root
```