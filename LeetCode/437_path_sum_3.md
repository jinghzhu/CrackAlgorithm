# <center>437 - Path Sum III</center> 


* Tag: DFS, Binarytree, Recursion

https://leetcode.com/problems/path-sum-iii/

<br></br>



## Description
----
Given a binary tree in which each node contains an integer value, find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

<br></br>



## Example
----
Given a binary tree:
```
      10
     /  \
    5   -3
   / \    \
  3   2   11
 / \   \
3  -2   1
```

Return 3. The paths that sum to 8 are:
1.  5 -> 3
2.  5 -> 2 -> 1
3. -3 -> 11

<br></br>



## Solution
----
### Go

```go
func PathSum3(root *TreeNode, sum int) int {
	if root == nil {
		return 0
	}

	return pathSum3Helper(root, sum) + PathSum3(root.Left, sum) + PathSum3(root.Right, sum)
}

func pathSum3Helper(root *TreeNode, sum int) int {
	if root == nil {
		return 0
	}
	count := 0
	if root.Val == sum {
		count++
	}

	return count + pathSum3Helper(root.Left, sum-root.Val) + pathSum3Helper(root.Right, sum-root.Val)
}
```


### Java
```java
public class PathSum3 {
	public int solution1(TreeNode root, int sum) {
        if (root == null)
        	return 0;

        return helper1(root, sum) + solution1(root.left, sum) + solution1(root.right, sum);
    }

    private int helper1(TreeNode root, int sum) {
        if (root == null)
        	return 0;
        int res = 0;
        if (root.val == sum)
        	res++;
        
        return res + helper1(root.left, sum - root.val) + helper1(root.right, sum - root.val);
    }
}
```