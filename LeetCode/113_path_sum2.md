# <center>113 - Path Sum II (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Medium
* Company: LinkedIn, Quora, Bloomberg, Amazon
* Link: https://leetcode.com/problems/path-sum-ii/

<br></br>



## Description
----
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

<br></br>



## Example
----
Input: root = {5,4,8,11,#,13,4,7,2,#,#,5,1}, sum = 22

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
```

Output: [[5,4,11,2],[5,8,4,5]]

<br></br>



## Solution
----
两种算法：
1. 非递归后序遍历。每次出栈时检查是否为叶子节点。如果是，计算栈中元素和是否符合要求。
2. 递归先序遍历。终止条件为空节点和叶子节点。

<br>


### Java
```java
public class PathSum2 {
	public List<List<Integer>> solution(TreeNode root, int sum) {
        List<List<Integer>> rst = new ArrayList<List<Integer>>();
        List<Integer> solution = new ArrayList<Integer>();

        helper(rst, solution, root, sum);
        return rst;
    }

    private void helper(
    		List<List<Integer>> result,
    		List<Integer> solution,
    		TreeNode root,
    		int sum){
        if (root == null)
            return;

        if (root.left == null && root.right == null) {
            if (sum == root.val){
                solution.add(root.val);
                result.add(new ArrayList<Integer>(solution));
                solution.remove(solution.size()-1); // important
            }
            return;
        }

        solution.add(root.val);
        helper(result, solution, root.left, sum - root.val);
        helper(result, solution, root.right, sum - root.val);
        solution.remove(solution.size()-1); // important
    }
}
```

<br>


### Python
```python
class PathSum2:
    """
    @param root: a binary tree
    @param sum: the sum
    @return: the scheme
    """
    def soltuion(self, root: TreeNode, sum: int):
        # sum is over writer, so new function my sum ....
        def mysum(nums):
            count = 0
            for n in nums:
                count += n
            return count

        # dfs find each path
        def dfs(root, path):
            if not root:
                return

            if root.left is None and root.right is None:
                if mysum(path + [root.val]) == sum:
                    all_path.append([t for t in path + [root.val]])
                return

            dfs(root.left, path + [root.val])
            dfs(root.right, path + [root.val])

        all_path = []
        dfs(root, [])

        return all_path

```

<br>


### Go
```go
var (
	ps2Paths [][]int
	ps2Path  []int
)

// PathSum2 finds all root-to-leaf paths where each path's sum equals the given sum.
func PathSum2(root *TreeNode, sum int) [][]int {
	ps2Paths = make([][]int, 0)
	ps2Path = make([]int, 0)
	ps2Helper(root, sum)

	return ps2Paths
}

func ps2Helper(root *TreeNode, sum int) {
	if root == nil {
		return
	}
	if root.Left == nil && root.Right == nil {
		if root.Val == sum {
			p := make([]int, 0)
			for _, v := range ps2Path {
				p = append(p, v)
			}
			p = append(p, root.Val)
			ps2Paths = append(ps2Paths, p)
		}
		return
	}
	ps2Path = append(ps2Path, root.Val)
	ps2Helper(root.Left, sum-root.Val)
	ps2Helper(root.Right, sum-root.Val)
	ps2Path = ps2Path[:len(ps2Path)-1] // important
}
```