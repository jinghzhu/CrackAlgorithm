# <center>1360 - Symmetric Tree (E)</center> 



<br></br>

* Tag: Binary Tree, Recursion, BFS, DFS
* Company: LinkedIn, Microsoft
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/symmetric-tree/description

<br></br>



## Description
----
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

<br></br>



## Example
----
This binary tree [1,2,2,3,4,4,3] is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

But the following [1,2,2,null,3,null,3] is not:
```
    1
   / \
  2   2
   \   \
   3    3
```

<br></br>



## Solution
----
### Java
```java
public class IsSymmetricTree {
	public boolean isSymmetric(TreeNode root) {
        if(root == null) 
        	return true;
        
        return helper(root.left,root.right);
    }
	
    public boolean helper(TreeNode left,TreeNode right){
    	if (left == null && right == null) 
    		return true;
    	
    	if (left == null || right == null || left.val != right.val) 
    		return false;
    	
    	return helper(left.right,right.left) && helper(left.left,right.right);
    }
    
    public boolean isSymmetricN1(TreeNode root) {
        if (root==null)  
            return true;  
        
        Queue<TreeNode> ql=new LinkedList<TreeNode>(), qr=new LinkedList<TreeNode>();  
        ql.offer(root.left);  
        qr.offer(root.right);  
        while (!ql.isEmpty()){  
        	TreeNode left=ql.poll(), right=qr.poll();  
            if (left==null && right==null)
                continue;  
            if (left==null || right==null || left.val != right.val) 
                return false;  
            
            ql.offer(left.left);  
            ql.offer(left.right);  
            qr.offer(right.right);  
            qr.offer(right.left);  
        }  
            
        return true;  
    }
    
    public boolean isSymmetricN2(TreeNode root) {
    	ArrayList<TreeNode> a1 = new ArrayList<TreeNode>(), a2 = new ArrayList<TreeNode>();
        Stack<TreeNode> st = new Stack<TreeNode>();
        TreeNode cur = root;
        
        while (cur != null || !st.isEmpty()) {
            while (cur != null) {
                a1.add(cur);
                if (cur.right != null)
                    st.push(cur.right);
                cur = cur.left;
            }
            a1.add(null);
            if (!st.isEmpty()) // important
                cur = st.pop();
        }
        
        st.clear();
        cur = root;
        
        while (cur != null || !st.isEmpty()) {
            while (cur != null) {
                a2.add(cur);
                if (cur.left != null)
                    st.push(cur.left);
                cur = cur.right;
            }
            a2.add(null);
            if (!st.isEmpty()) // important
                cur = st.pop();
        }
        
        for (int i = 0; i < a1.size(); i++) {
            TreeNode v1 = a1.get(i), v2 = a2.get(i);
            if (v1 == null && v2 == null)
                continue;
            if (v1 == null || v2 == null || v1.val != v2.val)
                return false;
        }
        
        return true;
    }
}
```

<br>


### Go
```go
import (
	queuel "github.com/jinghzhu/DataStructure/Go/queue/linkedlist"
)

func IsSymmetric(root *TreeNode) bool {
	if root == nil {
		return true
	}

	return isSymHelper(root.Left, root.Right)
}

func isSymHelper(left, right *TreeNode) bool {
	if left == nil && right == nil {
		return true
	}
	if left == nil || right == nil || left.Val != right.Val {
		return false
	}

	return isSymHelper(left.Left, right.Right) && isSymHelper(left.Right, right.Left)
}
```

```go
import (
	queuel "github.com/jinghzhu/DataStructure/Go/queue/linkedlist"
)

func IsSymmetricN1(root *TreeNode) bool {
	if root == nil {
		return true
	}
	leftQ, rightQ := queuel.New(), queuel.New()
	leftQ.Offer(root.Left)
	rightQ.Offer(root.Right)

	for !leftQ.IsEmpty() {
		left, right := leftQ.Poll().(*TreeNode), rightQ.Poll().(*TreeNode)
		if left == nil && right == nil {
			continue
		}
		if left == nil || right == nil || left.Val != right.Val {
			return false
		}
		leftQ.Offer(left.Left)
		leftQ.Offer(left.Right)
		rightQ.Offer(right.Right)
		rightQ.Offer(right.Left)
	}

	return true
}
```

```go
import (
	queuel "github.com/jinghzhu/DataStructure/Go/queue/linkedlist"
)

func isSymmetricN2(root *TreeNode) bool {
	a1, a2 := make([]*TreeNode, 0), make([]*TreeNode, 0)
	cur := root
	st := make([]*TreeNode, 0)

	for len(st) != 0 || cur != nil {
		for cur != nil {
			a1 = append(a1, cur)
			if cur.Right != nil {
				st = append(st, cur.Right)
			}
			cur = cur.Left
		}
		a1 = append(a1, nil)
		if len(st) != 0 {
			cur = st[len(st)-1]
			st = st[:len(st)-1]
		}
	}

	st = nil
	cur = root

	for len(st) != 0 || cur != nil {
		for cur != nil {
			a2 = append(a2, cur)
			if cur.Left != nil {
				st = append(st, cur.Left)
			}
			cur = cur.Right
		}
		a2 = append(a2, nil)
		if len(st) != 0 {
			cur = st[len(st)-1]
			st = st[:len(st)-1]
		}
	}

	for i := 0; i < len(a1); i++ {
		v1, v2 := a1[i], a2[i]
		if v1 == nil && v2 == nil {
			continue
		}
		if v1 == nil || v2 == nil || v1.Val != v2.Val {
			return false
		}
	}

	return true
}
```

<br>


### Python
```python
class IsSymmetric:
    def is_symmetric_n1(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        if (not root) or (not root.left and not root.right):
            return True
        return self.is_symmetric_n1_helper(root)

    def is_symmetric_n1_helper(self, root):
        left = [root.left]
        right = [root.right]
        while left and right:
            new_left = []
            new_right = []
            while left and right:
                item_left = left.pop(0)
                val_left = None
                if item_left:
                    val_left = item_left.val
                    new_left.extend([item_left.left, item_left.right])
                item_right = right.pop(0)
                val_right = None
                if item_right:
                    val_right = item_right.val
                    new_right.extend([item_right.right, item_right.left])
                if val_left != val_right:
                    return False
            left = new_left
            right = new_right
        return True

    def is_symmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        def is_symmetric_helper(root1, root2):
            if not root1 and not root2:
                return True
            elif not root1:
                return False
            elif not root2:
                return False
            else:  # root1 and root2 both exist
                if root1.val != root2.val:
                    return False
                else:
                    return is_symmetric_helper(root1.left, root2.right) and is_symmetric_helper(root1.right, root2.left)

        if not root:
            return True
        return is_symmetric_helper(root.left, root.right)
```
