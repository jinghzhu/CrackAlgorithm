# <center>72 - Construct Binary Tree from Inorder and Postorder Traversal (M)</center> 


<br></br>

* Tag: Binary Tree, Recursion
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Microsoft
* Link: https://www.lintcode.com/problem/construct-binary-tree-from-inorder-and-postorder-traversal/description

<br></br>



## Description
----
Given inorder and postorder traversal of a tree, construct the binary tree.

<br></br>



## Example
----
For example, given
- inorder = [9,3,15,20,7]
- postorder = [9,15,7,20,3]

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
### Go
```go
func buildTree(inorder []int, postorder []int) *TreeNode {
    l1, l2 := len(inorder), len(postorder)
    if l1 != l2 {
        return nil
    }
    
    return helper(inorder, 0, l1 - 1, postorder, 0, l2 - 1)
}

func helper(inorder []int ,inStart, inEnd int, postorder []int, postStart, postEnd int) *TreeNode {
    if inStart < 0 || inEnd >= len(inorder) || inStart > inEnd || postStart < 0 || postEnd >= len(postorder) || postStart > postEnd {
        return nil
    }
    
    root := &TreeNode{
        Val: postorder[postEnd],
    }
    i := inStart
    for ; i <= inEnd; i++ {
        if inorder[i] == postorder[postEnd] {
            break
        }
    }
    root.Left = helper(inorder, inStart, i - 1, postorder, postStart, postStart + i - inStart - 1)
    root.Right = helper(inorder, i + 1, inEnd, postorder, postStart + i - inStart, postEnd - 1)
    
    return root
}
```

<br>


### Java
```java
public class CreateByInPost {
	public TreeNode solution1(int[] inorder, int[] postorder) {
        if (inorder == null || postorder == null || inorder.length != postorder.length)
            return null;
            
        return solution1Helper(inorder, 0, inorder.length - 1, postorder, 0, postorder.length - 1);
    }
    
    private TreeNode solution1Helper(int[] in, int inSta, int inEnd, int[] post, int postSta, int postEnd) {
		if (in == null || inSta > inEnd || post == null || postSta > postEnd )
			return null;
		
		int i = inSta;
		TreeNode bn = new TreeNode(post[postEnd]);
		
		for (; i <= inEnd; i++ )
			if(in[i] == post[postEnd])
				break;
		
		bn.left = solution1Helper(in, inSta, i - 1, post, postSta, postSta + i - inSta - 1);
		bn.right = solution1Helper(in, i + 1, inEnd, post, postSta + i - inSta, postEnd - 1);
		
		return bn;
	}
}
```

<br>


### Python
```python
class CreateByInPost:
    # def solution(self, inorder: List[int], postorder: List[int]) -> TreeNode:
    def solution(self, inorder, postorder) -> TreeNode:
        if not inorder or not postorder or len(inorder) != len(postorder):
            return None

        return self.helper(inorder, 0, len(inorder) - 1, postorder, 0, len(postorder) - 1)

    def helper(self, inorder, in_start, in_end, postorder, post_start, post_end):
        if in_start < 0 or in_end >= len(inorder) or in_start > in_end or post_start < 0 or post_end >= len(
                postorder) or post_end < post_start:
            return None

        root = TreeNode(postorder[post_end])
        in_pos = in_start
        while in_pos <= in_end:
            if inorder[in_pos] == postorder[post_end]:
                break
            in_pos += 1
        root.left = self.helper(inorder, in_start, in_pos - 1, postorder, post_start,
                                post_start + in_pos - in_start - 1)
        root.right = self.helper(inorder, in_pos + 1, in_end, postorder, post_start + in_pos - in_start, post_end - 1)

        return root
```