# <center>73 - Construct Binary Tree from Preorder and Inorder Traversal (HM</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Recursion, Binary Tree
* Difficulty: Medium
* Company: Bloomberg, Microsoft
* Link: https://www.lintcode.com/problem/construct-binary-tree-from-preorder-and-inorder-traversal/description

<br></br>



## Description
----
Given preorder and inorder traversal of a tree, construct the binary tree.

<br></br>



## Example
----
For example, given

```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```

Return the following binary tree:

```
    3
   / \
  9  20
    /  \
   15   7
```

<br></br>



## Solution
----
算法：
1. 对每次递归，当前子树根结点就是当然前序遍历数组第一个，假设为preOrder[preStart]。
2. 寻找当前根结点在后续数组中位置，假设为inOrder[pos]。
3. 当前左子树范围为preOrder[preStart + 1, pos - inStart + preStart]，inOrder[inStart, pos - 1]。
4. 当前右子树范围为preOrdeer[pos - inStart + preStart + 1, preEnd]，inOrder[pos + 1, inEnd]。

<br>


### Java
```java
public class CreateByPreIn {
	private int findPosition(int[] arr, int start, int end, int key) {
        int i;
        for (i = start; i <= end; i++) {
            if (arr[i] == key) {
                return i;
            }
        }
        return -1;
    }

    private TreeNode myBuildTree(int[] inorder, int instart, int inend,
            int[] preorder, int prestart, int preend) {
        if (instart > inend) {
            return null;
        }

        TreeNode root = new TreeNode(preorder[prestart]);
        int position = findPosition(inorder, instart, inend, preorder[prestart]);

        root.left = myBuildTree(inorder, instart, position - 1,
                preorder, prestart + 1, prestart + position - instart);
        root.right = myBuildTree(inorder, position + 1, inend,
                preorder, prestart + position - instart + 1, preend);
        return root;
    }

    public TreeNode solution(int[] preorder, int[] inorder) {
        if (inorder.length != preorder.length)
            return null;
        
        return myBuildTree(inorder, 0, inorder.length - 1, preorder, 0, preorder.length - 1);
    }
}
```

<br>


### Go
```go
func CreateByPreIn(preorder []int, inorder []int) *TreeNode {
	l1, l2 := len(preorder), len(inorder)
	if l1 != l2 {
		return &TreeNode{}
	}

	return piHelper(preorder, 0, l1-1, inorder, 0, l2-1)
}

func piHelper(preorder []int, preStart, preEnd int, inorder []int, inStart, inEnd int) *TreeNode {
	l1, l2 := len(preorder), len(inorder)
	if preStart < 0 || preEnd >= l1 || preStart > preEnd || inStart < 0 || inEnd >= l2 || inStart > inEnd {
		return nil
	}
	root := &TreeNode{}
	root.Val = preorder[preStart]
	pos := inStart
	for ; pos <= inEnd; pos++ {
		if root.Val == inorder[pos] {
			break
		}
	}
	if pos > inEnd {
		return nil
	}
	root.Left = piHelper(preorder, preStart+1, pos-inStart+preStart, inorder, inStart, pos-1)
	root.Right = piHelper(preorder, pos-inStart+preStart+1, preEnd, inorder, pos+1, inEnd)

	return root
}
```