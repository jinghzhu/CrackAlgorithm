# <center>480 - Binary Tree Paths (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Easy
* Company: Apple, Facebook, Google
* Link: https://www.lintcode.com/problem/binary-tree-paths/description

<br></br>



## Description
----
Given a binary tree, return all root-to-leaf paths.

<br></br>



## Example
----
Input：{1,2,3,#,5}

Output：["1->2->5","1->3"]

```
   1
 /   \
2     3
 \
  5
```

<br></br>



## Solution
----
### Java
```java
public class PathLeaf2Root {
	// version 1: non-recursion
	public static ArrayList<List<Integer>> getPathFromLeafToRoot1(TreeNode root) {
		ArrayList<List<Integer>> results = new ArrayList<List<Integer>>();
		Stack<TreeNode> s = new Stack<TreeNode>();
		TreeNode p = root;
		
		while(!s.isEmpty() || p != null) {
			while(p != null) {
				s.push(p);
				if(p.left != null) 
					p = p.left;
				else 
					p = p.right;
			} // while
			
			p = s.pop();
			if(p.left == null && p.right == null) {
				List<Integer> path = new ArrayList<Integer>();
				Iterator<TreeNode> it = s.iterator();
				while(it.hasNext()) {
					TreeNode n = it.next();
					path.add(n.val);
				}
				path.add(p.val);
				results.add(path);
			}		
			
			while(!s.isEmpty() && s.peek().right == p) 
				p = s.pop();
			
			if(!s.isEmpty()) 
				p = s.peek().right;
			else
				p = null;
		}
		
		return results;
	}
	
	// version 2: Divide and Conquer
	public List<String> getPathFromLeafToRoot2(TreeNode root) {
        List<String> paths = new ArrayList<>();
        if (root == null)
            return paths;
        
        List<String> leftPaths = getPathFromLeafToRoot2(root.left);
        List<String> rightPaths = getPathFromLeafToRoot2(root.right);
        for (String path : leftPaths)
            paths.add(root.val + "->" + path);
        for (String path : rightPaths)
            paths.add(root.val + "->" + path);
        
        // root is a leaf
        if (paths.size() == 0)
            paths.add("" + root.val);
        
        return paths;
    }
	
	// version 3: traverse
	public List<String> getPathFromLeafToRoot3(TreeNode root) {
        List<String> result = new ArrayList<String>();
        if (root == null)
            return result;
        helper(root, String.valueOf(root.val), result);
        return result;
    }
    
    private void helper(TreeNode root, String path, List<String> result) {
        if (root == null)
            return;
        
        if (root.left == null && root.right == null) {
            result.add(path);
            return;
        }
        
        if (root.left != null)
            helper(root.left, path + "->" + String.valueOf(root.left.val), result);
        if (root.right != null)
            helper(root.right, path + "->" + String.valueOf(root.right.val), result);
    }
}
```

<br>
