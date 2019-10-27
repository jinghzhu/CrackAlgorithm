# <center>199 - Binary Tree Right Side View (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: DFS, Binary Tree, Recursion, BFS
* Company: Amazon
* Difficulty: Medium
* Link: https://leetcode.com/problems/binary-tree-right-side-view/

<br></br>



## Description
----
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom

<br></br>



## Example
----
Input: {1,2,3,#,5,#,4}

Output: [1,3,4]

```
   1            
 /   \
2     3         
 \     \
  5     4     
```

<br></br>



## Solution
----
三种算法：
1. 非递归BFS。右视角节点就是每层最后一个。
2. 递归先序遍历 + 哈希表。每次递归时，深度 + 1，以深度为key，当前节点为value加入哈希表。由先序遍历性质可得，每层最右边节点必然是当前层次在先序遍历中最后一个访问的。
3. 递归先序遍历。每次递归时，深度 + 1，利用栈长度和深度是否相等，确定是否第一次访问当前层。此外，先访问右节点再访问左节点。由先序遍历性质可得，每层最右边节点必然是当前先序遍历顺序中，第一个访问的。

<br>

### Go
```go
func GetRightView1(root *TreeNode) []int {
	m := make(map[int]int)
	grvHelper1(root, m, 1)
	notDone := true
	result := make([]int, 0)
	for d := 1; notDone; d++ {
		if v, ok := m[d]; ok {
			result = append(result, v)
		} else {
			notDone = false
		}
	}

	return result
}

func grvHelper1(root *TreeNode, m map[int]int, depth int) {
	if root == nil {
		return
	}
	m[depth] = root.Val
	grvHelper1(root.Left, m, depth+1)
	grvHelper1(root.Right, m, depth+1)
}
```

```go
func GetRightView2(root *TreeNode) []int {
	l := make([]int, 0)
	grv2Helper(root, &l, 0)

	return l
}

func grv2Helper(root *TreeNode, l *[]int, depth int) {
	if root == nil {
		return
	}
	if len(*l) == depth {
		*l = append(*l, root.Val)
	}
	grv2Helper(root.Right, l, depth+1)
	grv2Helper(root.Left, l, depth+1)
}
```

<br>


### Java
```java
public class RightView {
	public String solution1(TreeNode root) {
		if(root == null)
			return null;
		
		Queue<TreeNode> qu = new LinkedList<TreeNode>();
		StringBuilder st = new StringBuilder();
		TreeNode n = root;
		qu.add(n);
		
		while(!qu.isEmpty()) {
			int size = qu.size();
			while(size > 0) {
				n = qu.poll();
				if(n.left != null ) 
					qu.add(n.left);
				if(n.right != null) 
					qu.add(n.right);
				if(size == 1) 
					st.append(n.val);
				size--;
			}
		}
		
		return st.toString();
	}
    
    public List<Integer> solution2(TreeNode root) {
        HashMap<Integer, Integer> depthToValue = new HashMap<Integer, Integer>();
        helper2(depthToValue, root, 1);
        
        int depth = 1;
        List<Integer> result = new ArrayList<Integer>();
        while (depthToValue.containsKey(depth)) {
            result.add(depthToValue.get(depth));
            depth++;
        }
        
        return result;
    }
    
    private void helper2(HashMap<Integer, Integer> depthToValue, TreeNode node, int depth) {
        if (node == null)
            return;
        
        depthToValue.put(depth, node.val);
        helper2(depthToValue, node.left, depth + 1);
        helper2(depthToValue, node.right, depth + 1);
    }
	
	public List<Integer> solution3(TreeNode root) {
		List<Integer> l = new ArrayList<Integer>();
		helper3(root, l, 0);
		
		return l;
	}
	
	private void helper3(TreeNode root, List<Integer> l, int level) {
		if(root == null || level < 0) 
			return;
		
		if(level == l.size()) 
			l.add(root.val);
		
		helper3(root.right, l, level + 1);
		helper3(root.left, l, level + 1);
	}
}
```

<br>


### Python
```python
class GetRightView:
    """
    @param root: the root of the given tree
    @return: the values of the nodes you can see ordered from top to bottom
    """
    def rightSideView(self, root):
        def collect(node, depth):
            if node:
                if depth == len(view):
                    view.append(node.val)
                collect(node.right, depth + 1)
                collect(node.left, depth + 1)
        view = []
        collect(root, 0)
        return view
```