# <center>88 - Lowest Common Ancestor of a Binary Tree (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Medium
* Company: LinkedIn, Facebook, Apple, Amazon, Microsoft
* Link: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

<br></br>



## Description
----
Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.


<br></br>



## Solution
----
算法：
1. 非递归后序遍历。分别找到从跟节点到两个节点的路径，然后从路径中计算LCA。
2. RMQ. When visiting all nodes in post order, for each time of visiting this node, including visiting it from its parent node or from its children nodes, push it into array. For target data a and b, get their last positions in this array. Assume the sub-array starts from s and ends in e, the node with the smallest depth in [s, e] is the LCA node.
3. 利用ResultType。其中有三个域，分别表示其子树中是否存在A或B节点，以及该子树根节点。在递归后序遍历中，分别记录这三个域值，然后返回并汇总即可。

<br>


### Go
```go
func LCA1(root, p, q *TreeNode) *TreeNode {
	l1, l2 := getLCAPath(root, p), getLCAPath(root, q)
	i, j := 0, 0
	for i < len(l1) && j < len(l2) && l1[i] == l2[j] {
		i++
		j++
	}
	if l1[i-1] == l2[j-1] {
		return l1[i-1]
	}
	return nil
}

func getLCAPath(root, t *TreeNode) []*TreeNode {
	st := make([]*TreeNode, 0)
	n := root
	for len(st) > 0 || n != nil {
		for n != nil {
			if n == t {
				return append(st, t)
			}
			st = append(st, n)
			if n.Left != nil {
				n = n.Left
			} else {
				n = n.Right
			}
		}
		n = st[len(st)-1]
		st = st[:len(st)-1]
		for len(st) > 0 && st[len(st)-1].Right == n {
			n = st[len(st)-1]
			st = st[:len(st)-1]
		}
		if len(st) > 0 {
			n = st[len(st)-1].Right
		} else {
			n = nil
		}
	}

	return st
}
```

```go
type lcaResultType struct {
	node   *TreeNode
	pExist bool
	qExist bool
}

// LCA2 finds the LCA of the two nodes.
func LCA2(root, p, q *TreeNode) *TreeNode {
	r := lca2Helper(root, p, q)
	return r.node
}

func lca2Helper(root, p, q *TreeNode) *lcaResultType {
	if root == nil {
		return &lcaResultType{nil, false, false}
	}

	leftR, rightR := lca2Helper(root.Left, p, q), lca2Helper(root.Right, p, q)
	if leftR.node != nil {
		return leftR
	}
	if rightR.node != nil {
		return rightR
	}

	pExist := leftR.pExist || rightR.pExist
	qExist := leftR.qExist || rightR.qExist

	// If root is LCA
	if (pExist && qExist) || (root == p && qExist) || (root == q && pExist) {
		return &lcaResultType{root, true, true}
	}
	// If only node p exists in the subtree rooted at root.
	if pExist || root == p {
		return &lcaResultType{nil, true, false}
	}
	// If only node q exists in the subtree rooted at root.
	if qExist || root == q {
		return &lcaResultType{nil, false, true}
	}
	return leftR
}
```

<br>


### Java
```java
public class LCA {
	public TreeNode solution1(TreeNode root, int a, int b) {
		if(root == null) 
			return null;
		
		Stack<TreeNode> s = new Stack<TreeNode>();
		TreeNode p = root;
		TreeNode[] aPath = null;
		TreeNode[] bPath = null;
		int i = 0;
		
		while(!s.isEmpty() || p != null) {
			while(p != null) {
				s.push(p);
				if(p.val == a) {
					int length = s.size();
					aPath = new TreeNode[length];
					for(i = 0; i < aPath.length; i++) 
						aPath[i] = s.get(i);
				}
				
				if(p.val == b) {
					int length = s.size();
					bPath = new TreeNode[length];
					for(i = 0; i < bPath.length; i++) 
						bPath[i] = s.get(i);
				}
				
				if(aPath != null && bPath != null) 
					break;
				if(p.left != null) 
					p = p.left;
				else 
					p = p.right;
			} // while
			
			if(aPath != null && bPath != null) 
				break;
			
			p = s.pop();
			
			while(!s.isEmpty() && s.peek().right == p) 
				p = s.pop();
			
			if(!s.isEmpty()) 
				p = s.peek().right;
			else
				p = null;
		} // out while
		
		if (aPath == null || bPath == null)
			return null;
		
		for(i = 0; i < aPath.length && i < bPath.length; i++) 
			if(aPath[i].val != bPath[i].val) 
				break;
		
		return aPath[i - 1];	
	}
	
	public TreeNode soluion2RMQ(TreeNode root, int a, int b) {
		if(root == null) 
			return null;
		
		class DNode {
			public int depth;
			public TreeNode node;
			
			public DNode(TreeNode n, int d) {
				depth = d;
				node = n;
			}
		}
		
		ArrayList<DNode> al = new ArrayList<DNode>();
		Stack<TreeNode> s = new Stack<TreeNode>();
		TreeNode p = root;
		int depth = 0;
		
		while(!s.isEmpty() || p != null) {
			while(p != null) {
				s.push(p);
				depth++;
				al.add(new DNode(p, depth));
				if(p.left != null) 
					p = p.left;
				else 
					p = p.right;
			} // inner while
			
			p = s.pop();
			
			while(!s.isEmpty() && s.peek().right == p) {
				p = s.pop();
				al.add(new DNode(p, --depth));
			}
			
			if(!s.isEmpty()) {
				p = s.peek().right;
				al.add(new DNode(s.peek(), --depth));
			} else {
				p = null;
			}
		} // out while
		
		int aPos = 0, bPos = 0, start = 0, end = 0, maxDepth = 0;
		TreeNode result = null;
		for(int i = 0; i < al.size(); i++) {
			TreeNode n = al.get(i).node;
			if(n.val == a) 
				aPos = i;
			if(n.val == b) 
				bPos = i;
		}
		if(aPos < bPos) {
			start = aPos;
			end = bPos;
		} else {
			start = bPos;
			end = aPos;
		}
		
		maxDepth = al.get(start).depth;
		for(int i = start; i < end; i++) {
			if(al.get(i).depth < maxDepth) {
				result = al.get(i).node;
				maxDepth = al.get(i).depth;
			}
		}
		
		return result;
	}
	
	public TreeNode solution3(TreeNode root, TreeNode A, TreeNode B) {
        ResultType rt = helper(root, A, B);
        if (rt.aExist && rt.bExist)
            return rt.node;
        return null;
    }

    private ResultType helper(TreeNode root, TreeNode A, TreeNode B) {
        if (root == null)
            return new ResultType(false, false, null);
            
        ResultType left_rt = helper(root.left, A, B);
        ResultType right_rt = helper(root.right, A, B);
        
        boolean a_exist = left_rt.aExist || right_rt.aExist || root == A;
        boolean b_exist = left_rt.bExist || right_rt.bExist || root == B;
        
        if (root == A || root == B)
            return new ResultType(a_exist, b_exist, root);

        if (left_rt.node != null && right_rt.node != null)
            return new ResultType(a_exist, b_exist, root);
        if (left_rt.node != null)
            return new ResultType(a_exist, b_exist, left_rt.node);
        if (right_rt.node != null)
            return new ResultType(a_exist, b_exist, right_rt.node);

        return new ResultType(a_exist, b_exist, null);
    }
    
    class ResultType {
        public boolean aExist, bExist;
        public TreeNode node;
        
        ResultType(boolean a, boolean b, TreeNode n) {
        	aExist = a;
            bExist = b;
            node = n;
        }
    }
}
```

<br>


### Python
```python
class LCA:
    """
    @param: root: The root of the binary search tree.
    @param: A: A TreeNode in a Binary.
    @param: B: A TreeNode in a Binary.
    @return: Return the least common ancestor(LCA) of the two nodes.
    """
    def solution1(self, root, A, B):
        l1, l2 = self.get_path(root, A), self.get_path(root, B)
        i, j = 0, 0
        while i < len(l1) and j < len(l2) and l1[i] == l2[j]:
            i += 1
            j += 1
        if l1[i - 1] == l2[j - 1]:
            return l1[i - 1]
        return None

    def get_path(self, root, t):
        st = []
        n = root
        while n or len(st) > 0:
            while n:
                st.append(n)
                if n == t:
                    return st
                if n.left:
                    n = n.left
                else:
                    n = n.right
            n = st.pop(-1)
            while len(st) > 0 and st[-1].right == n:
                n = st.pop(-1)
            if len(st) == 0:
                n = None
            else:
                n = st[-1].right

        return st
```