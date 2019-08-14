# <center>177 - Convert Sorted Array to Binary Search Tree (E)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: Recursion, BST
* Difficulty: Easy
* Link: https://www.lintcode.com/problem/convert-sorted-array-to-binary-search-tree-with-minimal-height/description

<br></br>



## Description
----
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

<br></br>



## Example
----
Given the sorted array: [-10,-3,0,5,9],

One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:
```
      0
     / \
   -3   9
   /   /
 -10  5
```

<br></br>



## Solution
----
### Java
```java
public class SortedArray2BST {
	public TreeNode sortedArrayToBST(int[] num) {
        if (num == null)
            return null;
        
        return buildTree(num, 0, num.length - 1);
    }
    
    private TreeNode buildTree(int[] num, int start, int end) {
        if (start > end)
            return null;

        int mid = start + (end - start) / 2;
        TreeNode node = new TreeNode(num[mid]);
        node.left = buildTree(num, start, mid - 1);
        node.right = buildTree(num, mid + 1, end);
        
        return node;
    }
}
```

<br>


### Go
```go
func SortedArray2BST(nums []int) *TreeNode {
	if nums == nil {
		return nil
	}

	return sortedArray2BST(nums, 0, len(nums)-1)
}

func sortedArray2BST(nums []int, start, end int) *TreeNode {
	if start > end || start < 0 || end >= len(nums) {
		return nil
	}
	mid := start + (end-start)/2

	return &TreeNode{
		Val:   nums[mid],
		Left:  sortedArray2BST(nums, start, mid-1),
		Right: sortedArray2BST(nums, mid+1, end),
	}
}
```

<br>


### Python
```python
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None


class SortedArr2BST:
    def solution1(self, nums):
        """
        :type nums: List[int]
        :rtype: TreeNode
        """
        if not nums:
            return
        left = 0
        right = len(nums) - 1
        return self.solution1_helper(left, right, nums)

    def solution1_helper(self, left, right, nums):
        if left > right:
            return None

        if left == right:
            return TreeNode(nums[left])

        mid = left + (right-left)//2
        node = TreeNode(nums[mid])
        node.left = self.solution1_helper(left, mid-1, nums)
        node.right = self.solution1_helper(mid+1, right, nums)
        return node
```