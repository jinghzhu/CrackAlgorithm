# <center>87 - Remove Node in Binary Search Tree (H)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: BST
* Difficulty: Hard
* Link: http://www.lintcode.com/problem/remove-node-in-binary-search-tree/description

<br></br>



## Description
----
Given a root of Binary Search Tree with unique value for each node. Remove the node with given value. If there is no such a node with given value in the binary search tree, do nothing. You should keep the tree still a binary search tree after removal.

<br></br>



## Example
----
Input: {5,3,6,2,4}, 3

Output: {5,2,6,#,4} or {5,4,6,2}

Given binary search tree:

```
    5
   / \
  3   6
 / \
2   4
```

Remove 3, you can either return:

```
    5
   / \
  2   6
   \
    4
```

or

```
    5
   / \
  4   6
 /
2
```

<br></br>



## Solution
----
算法1：
1. Find the target node and its parent node.
2. Check right child of target node. If the it is nil, it means there is no right subtree rooted at the target node. Then directly deal with the left child of target and its parent.
3. Find the successor node of target node, whose value is the min in right sub tree rooted at target node.
4. Swap the value between successor and target node.
5. Remove the successor node.

算法2：
1. 中序遍历输出为数组。
2. 删除指定元素。
3. 重建BST。

<br>


### Go
```go
func RemoveNode(root *TreeNode, val int) *TreeNode {
	if root == nil {
		return root
	}

	// step 1
	targetNode, targetNodePar := rnGetNode(root, val)
	if targetNode == nil {
		return nil
	}

	// step 2
	if targetNode.Right == nil {
		if targetNodePar == nil {
			root = targetNode.Left
		} else if targetNodePar.Left == targetNode {
			targetNodePar.Left = targetNode.Left
		} else {
			targetNodePar.Right = targetNode.Left
		}

		return root
	}

	// step 3
	successorNode, successorNodePar := rnGetSuccessor(targetNode)
	// step 4
	successorNode.Val, targetNode.Val = targetNode.Val, successorNode.Val
	// step 5
	if successorNodePar == targetNode {
		targetNode.Right = successorNode.Right
	} else {
		successorNodePar.Left = successorNode.Right
	}

	return root
}

func rnGetNode(root *TreeNode, val int) (*TreeNode, *TreeNode) {
	var cur, par *TreeNode
	if root == nil {
		return cur, par
	}

	for cur = root; cur != nil; {
		if cur.Val == val {
			return cur, par
		}
		if cur.Val < val {
			par, cur = cur, cur.Right
		} else {
			par, cur = cur, cur.Left
		}
	}

	return nil, nil
}

func rnGetSuccessor(root *TreeNode) (*TreeNode, *TreeNode) {
	var cur, par *TreeNode
	if root == nil || root.Right == nil {
		return cur, par
	}
	for cur, par = root.Right, root; cur.Left != nil; cur, par = cur.Left, cur {
	}

	return cur, par
}
```

<br>


### Java
```java
public class RemoveNode {
	public TreeNode solution1(TreeNode root, int value) {
        if (root == null)
            return root;
        
        // step 1
        ArrayList<TreeNode> res = getNode(root, value);
        TreeNode targetNode = res.get(0);
        TreeNode targetNodePar = res.get(1);
        if (targetNode == null)
            return root;
        // step 2
        if (targetNode.right == null) {
            if (targetNodePar == null)
                root = targetNode.left;
            else if (targetNodePar.left == targetNode)
                targetNodePar.left = targetNode.left;
            else
                targetNodePar.right = targetNode.left;
            
            return root;
        }
        
        // step 3
        res = getSuccessor(targetNode);
        TreeNode successorNode = res.get(0);
        TreeNode successorNodePar = res.get(1);
        // step 4
        int tmp = successorNode.val;
        successorNode.val = targetNode.val;
        targetNode.val = tmp;
        // step 5
        if (successorNodePar == targetNode)
            targetNode.right = successorNode.right;
        else
            successorNodePar.left = successorNode.right;
        
        
        return root;
    }
    
    private ArrayList<TreeNode> getNode(TreeNode root, int value) {
        ArrayList<TreeNode> res = new ArrayList<TreeNode>();
        TreeNode cur = root;
        TreeNode par = null;
        while (cur != null) {
            if (cur.val == value)
                break;
            par = cur;
            if (cur.val < value)
                cur = cur.right;
            else
                cur = cur.left;
        }
        
        res.add(cur);
        res.add(par);
                
        return res;
    }
    
    private ArrayList<TreeNode> getSuccessor(TreeNode root) {
        ArrayList<TreeNode> res = new ArrayList<TreeNode>();
        if (root == null || root.right == null) {
            res.add(null);
            res.add(null);
            
            return res;
        }
        
        TreeNode cur = root.right;
        TreeNode par = root;
        while (cur.left != null) {
            par = cur;
            cur = cur.left;
        }
        
        res.add(cur);
        res.add(par);
            
        return res;
    }
}
```

<br>


### Python
```python
class RemoveNode:
    """
    @param: root: The root of the binary search tree.
    @param: value: Remove the node with given value.
    @return: The root of the binary search tree after removal.
    """
    def solution1(self, root, value):
        if not root:
            return root

        # step 1
        target_node, target_node_par = self.get_node(root, value)
        if not target_node:
            return root
        # step 2
        if not target_node.right:
            if not target_node_par:
                root = target_node.left
            elif target_node_par.left == target_node:
                target_node_par.left = target_node.left
            else:
                target_node_par.right = target_node.left

            return root

        # step 3
        successor_node, successor_node_par = self.get_successor(target_node)
        # step 4
        successor_node.val, target_node.val = target_node.val, successor_node.val
        # step 5
        if successor_node_par == target_node:
            target_node.right = successor_node.right
        else:
            successor_node_par.left = successor_node.right

        return root

    def get_node(self, root, value):
        if not root:
            return None, None

        cur, par = root, None
        while cur:
            if cur.val == value:
                return cur, par
            if cur.val < value:
                cur, par = cur.right, cur
            else:
                cur, par = cur.left, cur

        return None, None

    def get_successor(self, root):
        if not root or not root.right:
            return None, None

        cur, par = root.right, root
        while cur.left:
            cur, par = cur.left, cur

        return cur, par

    ans = []

    def in_order(self, root, value):
        if root is None:
            return

        self.in_order(root.left, value)
        if root.val != value:
            self.ans.append(root.val)
        self.in_order(root.right, value)

    def build(self, l, r):
        if l == r:
            node = TreeNode(self.ans[l])
            return node

        if l > r:
            return None

        mid = (l + r) / 2
        node = TreeNode(self.ans[mid])
        node.left = self.build(l, mid - 1)
        node.right = self.build(mid + 1, r)
        return node

    def solution(self, root, value):
        self.in_order(root, value)

        return self.build(0, len(self.ans) - 1)
```