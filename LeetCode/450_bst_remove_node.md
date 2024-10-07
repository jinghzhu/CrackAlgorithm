<h1 style="text-align: center;"><strong>450 - Delete Node in BST</strong></h1>

- [Description](#description)
- [Example](#example)
- [Solution](#solution)
  - [Go](#go)
  - [Python](#python)
  - [Java](#java)

<br></br>



# Description
Given root node reference of BST and key, delete node with given key in BST. Each node has a unique value. Return root node reference (possibly updated) of BST.

* Tag: Tree, Binary Tree, BST, Divide and Conquer
* Difficulty: Difficulty
* Link: https://leetcode.com/problems/delete-node-in-a-bst/description/

<br></br>



# Example
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



# Solution
Solution 1：
1. Find target node and its parent node.
2. If the target node doesn't exist or the tree is empty, return the root directly.
3. If target node is leaf node, delete it directly.
4. If target node has only one child, delete it and connect its child to its parent.
5. Now, target node mush have two children. Find the next node in in-order traversal and replace the target node with it.

Solution 2:
1. Output BST into array in in-order traversal.
2. Delete target node from array.
3. Rebuild BST from array.

<br>


## Go
```go
func deleteNode(root *TreeNode, key int) *TreeNode {
    par, tar := getTarAndPar(root, key)
    if tar == nil {
        return root
    }
    if tar.Left == nil && tar.Right == nil {
        return deleteCase2(root, par, tar)
    }
    if tar.Left == nil || tar.Right == nil {
        return deleteCase2(root, par, tar)
    }
    return deleteCase3(root, tar)
}

func deleteCase3(root, tar *TreeNode) *TreeNode {
    candidatePar, candidate := getCandidate(tar)
    tar.Val = candidate.Val
    if candidatePar == tar {
        tar.Right = candidate.Right
    } else {
        candidatePar.Left = candidate.Right
    }
    candidate.Right = nil

    return root
}

func getCandidate(root *TreeNode) (*TreeNode, *TreeNode) {
    par, cur := root, root.Right
    for cur.Left != nil {
        par, cur = cur, cur.Left
    }

    return par, cur
}

func deleteCase2(root, par, tar *TreeNode) *TreeNode {
    var child *TreeNode
    if tar.Left != nil {
        child = tar.Left
    } else {
        child = tar.Right
    }

    if par == nil {
        root = child
    } else if par.Left == tar {
        par.Left = child
    } else {
        par.Right = child
    }
    tar.Left, tar.Right = nil, nil

    return root
}

func deleteCase1(root, par, tar *TreeNode) *TreeNode {
    if par == nil {
        return nil
    }

    if par.Left == tar {
        par.Left = nil
    } else {
        par.Right = nil
    }

    return root
}

func getTarAndPar(root *TreeNode, key int) (*TreeNode, *TreeNode) {
    var par, cur *TreeNode
    for cur = root; cur != nil && cur.Val != key; {
        par = cur
        if cur.Val > key {
            cur = cur.Left
        } else {
            cur = cur.Right
        }
    }

    return par, cur
}
```

<br>


## Python
```python
    def removeNode(self, root, value):
        if not root:
            return root

        target_node, target_node_par = self.get_node(root, value)
        if not target_node:
            return root
        if not target_node.right or not target_node.left:
            child = target_node.left
            if not target_node.left:
                child = target_node.right
            if not target_node_par:
                root = child
            elif target_node_par.left == target_node:
                target_node_par.left = child
            else:
                target_node_par.right = child

            return root

        successor_node, successor_node_par = self.get_successor(target_node)
        successor_node.val, target_node.val = target_node.val, successor_node.val

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
```

<br>


## Java
```java
    public TreeNode removeNode(TreeNode root, int value) {
        if (root == null)
            return root;

        ArrayList<TreeNode> res = getNode(root, value);
        TreeNode targetNode = res.get(0);
        TreeNode targetNodePar = res.get(1);
        if (targetNode == null)
            return root;
        if (targetNode.right == null) {
            if (targetNodePar == null)
                root = targetNode.left;
            else if (targetNodePar.left == targetNode)
                targetNodePar.left = targetNode.left;
            else
                targetNodePar.right = targetNode.left;

            return root;
        }

        res = getSuccessor(targetNode);
        TreeNode successorNode = res.get(0);
        TreeNode successorNodePar = res.get(1);
        int tmp = successorNode.val;
        successorNode.val = targetNode.val;
        targetNode.val = tmp;
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
```