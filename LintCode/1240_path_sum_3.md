# <center>437 - Path Sum III (E)</center> 



<br></br>

* Tag: DFS, Binary Tree, Recursion
* Company: Facebook, Quora, Uber, Amazon
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/path-sum-iii/description

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
可以转化为PathSum其它问题。虽然要求各种可能路径，但仍旧可归纳为3部分：
1. 在左子树但不以root开头的路径。
2. 在右子树但不以root开头的路径。
3. 必须以root开头的路径。

所以可转化为三个递归：
1. 以本节点开头的路径。
2. 左子树结果。
3. 右子树结果。

<br>

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

<br>


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

<br>


### Python
```python
class PathSum3:
    """
    @param root:
    @param sum:
    @return: nothing
    """
    def dfs(self, root, su, tmp):
        if not root:
            return 0

        flag = 0
        if su == tmp + root.val:
            flag = 1
        return flag + self.dfs(root.left, su, tmp + root.val) + self.dfs(root.right, su, tmp + root.val)

    def solution(self, root, su):
        if not root:
            return 0
        return self.dfs(root, su, 0) + self.solution(root.left, su) + self.solution(root.right, su)
```