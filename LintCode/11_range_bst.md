# <center>11- Search Range in Binary Search Tree (M)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: BST, Recursion
* Difficulty: Medium
* Link: https://www.lintcode.com/problem/search-range-in-binary-search-tree/description

<br></br>



## Description
----
Given a binary search tree and a range [k1, k2], return node values within a given range in ascending order.

<br></br>



## Example
----
If k1 = 10 and k2 = 22, then should return [12, 20, 22].
```
 	    20
 	   /  \
 	  8   22
 	 / \
 	4   12
```

<br></br>



## Solution
----
可利用BST中序有序规则减少遍历的节点：
- 如果是非递归，可利用小于`k1`，搜索右子树（大于`k2`，搜索左子树）减少压栈范围。
- 如果是递归，不能像非递归那样，否则会绕过去。比如上述例子，如果`k1 = 7`且`k2 = 13`，8在[7, 12]中，不能简单的比较`n.val < k1`或`n.val > k2`。

<br>


### Java
```java
public class GetRangeInBST {
	private ArrayList<Integer> results;
	
    /**
     * @param root: The root of the binary search tree.
     * @param k1 and k2: range k1 to k2.
     * @return: Return all keys that k1<=key<=k2 in increasing order.
     */
    public ArrayList<Integer> solution1(TreeNode root, int k1, int k2) {
        results = new ArrayList<Integer>();
        if (k1 > k2)
        	return results;
        helper1(root, k1, k2);
        
        return results;
    }
    
    private void helper1(TreeNode root, int k1, int k2) {
        if (root == null) 
            return;
        if (root.val > k1) 
        	helper1(root.left, k1, k2);
        if (root.val >= k1 && root.val <= k2) 
            results.add(root.val);
        if (root.val < k2) 
        	helper1(root.right, k1, k2);
    }
    
    public List<Integer> solution2(TreeNode root, int k1, int k2) {
        List<Integer> result=new ArrayList<Integer>();
        if(root == null)
            return result;
        if(root.val >= k1) // 先搜左子树符合要求的节点。
            result.addAll(solution2(root.left, k1, k2));
        if(root.val >= k1 && root.val<=k2) // 判断当前节点是否在范围内，这个在递归解法是必须。
            result.add(root.val);
        if(root.val <= k2) // 再搜索右子树符合要求的节点。
            result.addAll(solution2(root.right, k1, k2));
        
        return result; // 因为搜索时是左->根->右，遍历结果对搜索二叉树来说是有序的。
    }
    
    public List<Integer> solution3(TreeNode root, int k1, int k2) {
    	ArrayList<Integer> result = new ArrayList<Integer>();
        if (root == null || k2 < k1)
            return result;
        
        Stack<TreeNode> st = new Stack<TreeNode>();
        TreeNode n = root;
        while (n != null || !st.isEmpty()) {
            while (n != null) {
                if (n.val < k1) {
                    n = n.right;
                } else if (n.val > k2) {
                    n = n.left;
                } else {
                    st.push(n);
                    n = n.left;
                }
            }
            if (st.isEmpty())
                break;
            n = st.pop();
            results.add(n.val);
            n = n.right;
        }
        
        return result;
    }
}
```

<br>


### Go
```go
func GetRange1(root *TreeNode, k1 int, k2 int) []int {
	result := make([]int, 0)
	if root == nil || k2 < k1 {
		return result
	}

	n := root
	st := make([]*TreeNode, 0)
	for n != nil || len(st) > 0 {
		for n != nil {
			if n.Val < k1 {
				n = n.Right
			} else if n.Val > k2 {
				n = n.Left
			} else {
				st = append(st, n)
				n = n.Left
			}
		}
		l := len(st)
		if l < 1 {
			break
		}
		n = st[l-1]
		st = st[:l-1]
		result = append(result, n.Val)
		n = n.Right
	}

	return result
}
```

```go
func GetRange2(root *TreeNode, k1 int, k2 int) []int {
	result := make([]int, 0)
	if root == nil {
		return result
	}
	if root.Val >= k1 { // 先搜左子树符合要求的节点。
		result = append(result, GetRange2(root.Left, k1, k2)...)
	}
	if root.Val >= k1 && root.Val <= k2 { // 判断当前节点是否在范围内，这个在递归解法是必须。
		result = append(result, root.Val)
	}
	if root.Val <= k2 { // 再搜索右子树符合要求的节点。
		result = append(result, GetRange2(root.Right, k1, k2)...)
	}

	return result // 因为搜索时是左->根->右，遍历结果对搜索二叉树来说是有序的。
}
```

<br>


### Python
----
```python
class GetRange:
    """
    @param root: param root: The root of the binary search tree
    @param k1: An integer
    @param k2: An integer
    @return: return: Return all keys that k1<=key<=k2 in ascending order
    """
    def solution1(self, root, k1, k2):
        result = []
        if not root or k2 < k1:
            return result
        if root.val >= k1:  # 先搜左子树符合要求的节点。
            result += self.searchRange(root.left, k1, k2)
        if k1 <= root.val <= k2:  # 判断当前节点是否在范围内，这个在递归解法是必须。
            result.append(root.val)
        if root.val <= k2:  # 再搜索右子树符合要求的节点。
            result += self.searchRange(root.right, k1, k2)

        return result  # 因为搜索时是左->根->右，遍历结果对搜索二叉树来说是有序的。

    """
    @param root: param root: The root of the binary search tree
    @param k1: An integer
    @param k2: An integer
    @return: return: Return all keys that k1<=key<=k2 in ascending order
    """
    def solution2(self, root, k1, k2):
        result = []
        if not root or k2 < k1:
            return result
        st = []
        n = root
        while st or n:
            while n:
                if n.val < k1:
                    n = n.right
                elif n.val > k2:
                    n = n.left
                else:
                    st.append(n)
                    n = n.left
            if not st:
                break
            n = st.pop()
            result.append(n.val)
            n = n.right

        return result
```