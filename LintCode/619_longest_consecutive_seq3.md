# <center>619 - Binary Tree Longest Consecutive Sequence III (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Binary Tree, DFS, Recursion
* Difficulty: Medium
* Company: Google
* Link: https://www.lintcode.com/problem/binary-tree-longest-consecutive-sequence-iii

<br></br>



## Description
----
Given a k-ary tree, find the length of the longest consecutive sequence path. The path could be start and end at any node in the tree

<br></br>



## Example
----
```
       5
     /   \
    6     4
   /|\   /|\
  7 5 8 3 5 3
```

Return 5, which is 3-4-5-6-7.

<br></br>



## Solution
----
仍旧和LongestConsecutiveSeq2一致，忽略它不是二叉树。因为，对任何节点，无论有多少个儿子节点，其必然最多只有一条最长up序列和最长一条down序列，且它们不会是经过同一个儿子节点。所以，仍旧按照“后序遍历”思想，进行递归DFS。依次计算每个子树的up、down和max值，然后和自己比较。最后，再从max和up+down-1中获得最大值，即自己的max。

<br>


### Go
```go
type lcs3ResultType struct {
	down int
	up   int
	max  int
}

type lcs3TreeNode struct {
	val      int
	children []*lcs3TreeNode
}

// LongestConsecutiveSeq3 finds the length of longest consecutive sequence path.
func LongestConsecutiveSeq3(root *lcs3TreeNode) int {
	return lcs3Helper(root).max
}

func lcs3Helper(root *lcs3TreeNode) *lcs3ResultType {
	if root == nil {
		return &lcs3ResultType{0, 0, 0}
	}

	up, down, max := 0, 0, 0
	for _, v := range root.children {
		resultTmp := lcs3Helper(v)
		if root.val-v.val == 1 {
			down = maxInt(down, resultTmp.down+1)
		} else if root.val-v.val == -1 {
			up = maxInt(up, resultTmp.up+1)
		}
		max = maxInt(max, resultTmp.max)
	}
	max = maxInt(max, down+up-1)

	return &lcs3ResultType{down, up, max}
}
```

<br>


### Java
```java
public class LongestConsecutiveSeq3 {
	class MultiTreeNode {
		int val;
		List<MultiTreeNode> children;
		
		MultiTreeNode(int x) {
			val = x;
		}
	}
	
	class ResultType {
	    int down;
	    int up;
	    int max;
	    
	    ResultType(int d, int u, int m) {
	        down = d;
	        up = u;
	        max = m;
	    }
	}
	
    public int solution(MultiTreeNode root) {
        return helper(root).max;
    }
    
    private ResultType helper(MultiTreeNode root) {
        if (root == null)
            return new ResultType(0, 0, 0);
        
        int down = 1, up = 1, max = 1;
        
        if (root.children != null) {
            for (int i = 0; i < root.children.size(); i ++) {
                MultiTreeNode child = root.children.get(i);
                ResultType temp = helper(child);
                if (root.val - child.val == 1 && temp.down + 1> down) 
                    down = temp.down + 1;
                else if (root.val - child.val == -1 && temp.up + 1> up) 
                    up = temp.up + 1;
                max = Math.max(max, temp.max);
            }
        }
        
        max = Math.max(max, down + up - 1); // -1因为前面计算up和down是都+1，避免重复。
            
        return new ResultType(down, up, max);
    }
}
```

<br>


### Python
```python
class LongestConsecutiveSeq3:
    class MultiTreeNode:
        def __init__(self, val):
            self.val = val
            self.children = []

    class ResultType:
        def __int__(self, down, up, longest):
            self.down = down
            self.up = up
            self.longest = longest

    def solution(self, root: MultiTreeNode):
        return self.helper(root).longest

    def helper(self, root: MultiTreeNode):
        if not root:
            return self.ResultType(1, 1, 1)

        down, up, longest = 0, 0, 0
        for child in root.children:
            result_tmp = self.helper(child)
            if root.val - child.val == 1:
                down = max(down, result_tmp.down + 1)
            elif root.val - child.val == -1:
                up = max(up, result_tmp.up + 1)
            longest = max(longest, result_tmp.longest)
        longest = max(longest, down + up - 1)  # -1因为前面计算up和down是都+1，避免重复。

        return self.ResultType(down, up, longest)
```