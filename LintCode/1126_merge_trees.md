# <center>1126 - Merge Two Binary Trees (E)</center> 



<br></br>

* Tag: BFS, Recursion, Binary Tree
* Difficulty: Easy
* Company: Amazon
* Link: https://www.lintcode.com/problem/merge-two-binary-trees/description

<br></br>



## Description
----
Given two binary trees and imagine that when you put one of them to cover the other, some nodes of the two trees are overlapped while the others are not.

You need to merge them into a new binary tree. The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node. Otherwise, the NOT null node will be used as the node of new tree.

<br></br>



## Example
----
Input: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  

Output:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7

<br></br>



## Solution
----
Each entry in the stackstack strores data in the form [node_{tree1}, node_{tree2}].

We start off by pushing root nodes onto stack. At every step, 
1. Remove a node pair from stack.
2. For every node pair removed, we add the values corresponding to the two nodes and update the value of the corresponding node in the first tree.
3. If the left child of the first tree exists, we push the left child(pair) of both the trees onto the stack.
4. If the left child of the first tree doesn't exist, we append the left child(subtree) of the second tree to the current node of the first tree.
5. Do the same for the right child pair as well.

<br>


### Java
```java
public class MergeTrees {
	public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if(t1 == null)
            return t2;
        if(t2 == null)
            return t1;
        
        TreeNode t0 = new TreeNode(t1.val + t2.val);
        t0.left = mergeTrees(t1.left, t2.left);
        t0.right = mergeTrees(t1.right, t2.right);
        
        return t0;
    }

    public TreeNode mergeTreesN(TreeNode t1, TreeNode t2) {
        if (t1 == null)
            return t2;
        if (t2 == null)
        	return t1;
        
        Stack<TreeNode[]> stack = new Stack<>();
        stack.push(new TreeNode[] {t1, t2});
        while (!stack.isEmpty()) {
            TreeNode[] t = stack.pop();
            t[0].val += t[1].val;
            if (t[0].left != null && t[1].left != null)
                stack.push(new TreeNode[] {t[0].left, t[1].left});
            else if (t[0].left == null)
                t[0].left = t[1].left;
            if (t[0].right != null && t[1].right != null)
                stack.push(new TreeNode[] {t[0].right, t[1].right});
            else if (t[0].right == null)
                t[0].right = t[1].right;
        }
        
        return t1;
    }
}
```

<br>


### Go
```go
func MergeTrees(t1, t2 *TreeNode) *TreeNode {
	if t1 == nil {
		return t2
	}
	if t2 == nil {
		return t1
	}
	t0 := &TreeNode{}
	t0.Val = t1.Val + t2.Val
	t0.Left = MergeTrees(t1.Left, t2.Left)
	t0.Right = MergeTrees(t1.Right, t2.Right)

	return t0
}
```

```go
func MergeTrees(t1 *TreeNode, t2 *TreeNode) *TreeNode {
	if t1 == nil {
		return t2
	}
	if t2 == nil {
		return t1
	}
	st1, st2 := make([]*TreeNode, 0), make([]*TreeNode, 0)
	st1, st2 = append(st1, t1), append(st2, t2)
	for len(st1) > 0 && len(st2) > 0 {
		n1, n2 := st1[len(st1)-1], st2[len(st2)-1]
		st1, st2 = st1[:len(st1)-1], st2[:len(st2)-1]
		n1.Val += n2.Val
		if n1.Left != nil && n2.Left != nil {
			st1, st2 = append(st1, n1.Left), append(st2, n2.Left)
		} else if n1.Left == nil {
			n1.Left = n2.Left
		}
		if n1.Right != nil && n2.Right != nil {
			st1, st2 = append(st1, n1.Right), append(st2, n2.Right)
		} else if n1.Right == nil {
			n1.Right = n2.Right
		}
	}

	return t1
}
```

```go
func MergeTrees(t1, t2 *TreeNode) *TreeNode {
	root := newNode(t1, t2)
	if root == nil {
		return root
	}
	q0, q1, q2 := queue.New(), queue.New(), queue.New()
	q0.Offer(root)
	q1.Offer(t1)
	q2.Offer(t2)

	for !q0.IsEmpty() {
		cur0, cur1, cur2 := q0.Poll().(*TreeNode), q1.Poll().(*TreeNode), q2.Poll().(*TreeNode)
		left, right := newNode(getLeft(cur1), getLeft(cur2)), newNode(getRight(cur1), getRight(cur2))

		if left != nil {
			cur0.Left = left
			q0.Offer(left)
			if cur1 == nil {
				q1.Offer(nil)
				q2.Offer(cur2.Left)
			} else if cur2 == nil {
				q1.Offer(cur1.Left)
				q2.Offer(nil)
			} else {
				q1.Offer(cur1.Left)
				q2.Offer(cur2.Left)
			}
		}

		if right != nil {
			cur0.Right = right
			q0.Offer(right)
			if cur1 == nil {
				q1.Offer(nil)
				q2.Offer(cur2.Right)
			} else if cur2 == nil {
				q1.Offer(cur1.Right)
				q2.Offer(nil)
			} else {
				q1.Offer(cur1.Right)
				q2.Offer(cur2.Right)
			}
		}
	}

	return root
}

func getLeft(n *TreeNode) *TreeNode {
	if n == nil {
		return nil
	}

	return n.Left
}

func getRight(n *TreeNode) *TreeNode {
	if n == nil {
		return nil
	}

	return n.Right
}

func newNode(t1, t2 *TreeNode) *TreeNode {
	if t1 == nil && t2 == nil {
		return nil
	}
	n := &TreeNode{}
	if t1 == nil {
		n.Val = t2.Val
	} else if t2 == nil {
		n.Val = t1.Val
	} else {
		n.Val = t1.Val + t2.Val
	}

	return n
}
```

<br>


### Python
----
```python
class MergeTrees:
    def solution_n(self, t1: TreeNode, t2: TreeNode) -> TreeNode:
        if t1 is None:
            return t2
        if t2 is None:
            return t1
        st1, st2 = [t1], [t2]
        while st1 and st2:
            n1, n2 = st1[-1], st2[-1]
            st1.pop(-1)
            st2.pop(-1)
            n1.val += n2.val
            if n1.left and n2.left:
                st1.append(n1.left)
                st2.append(n2.left)
            elif n1.left is None:
                n1.left = n2.left
            if n1.right and n2.right:
                st1.append(n1.right)
                st2.append(n2.right)
            elif n1.right is None:
                n1.right = n2.right

        return t1

    def solution(self, t1, t2):
        if t1 is None:
            return t2
        if t2 is None:
            return t1
        t = TreeNode(t1.val + t2.val)
        t.left = self.mergeTrees(t1.left, t2.left)
        t.right = self.mergeTrees(t1.right, t2.right)
        return t
```