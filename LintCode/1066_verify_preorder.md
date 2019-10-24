# <center>1066 - Verify Preorder Serialization of a Binary Tree (M)</center> 



<br></br>

* Tag: Binary Tree, String
* Difficulty: Medium
* Company: Google
* Link: https://www.lintcode.com/problem/verify-preorder-serialization-of-a-binary-tree/description

<br></br>



## Description
----
One way to serialize a binary tree is to use pre-order traversal. When we encounter a non-null node, we record the node's value. If it is a null node, we record using a sentinel value such as #.

<br></br>



## Example
----
1. Input: "9,3,4,#,#,1,#,#,2,#,6,#,#" Output: true
2. Input: "1,#" Output: false

<br></br>



## Solution
----
算法：二叉树去掉叶节点后仍是二叉树。在字符序列中，叶节点后跟两个空节点＃。通过不断把(number,#,#)子串缩成空节点#，即把（number,#,#）替换为#。如果最后字符序列缩短到只有一个字符＃，是合法先序遍历

<br>


### Go
```go
func IsValidPreorderSerialization(s string) bool {
	if s == "" {
		return true
	}
	for len(s) > 1 {
		i := strings.Index(s, ",#,#")
		if i < 0 {
			return false
		}
		j := i
		if s[j-1] == '#' {
			return false
		}
		// 前一个可能是多个数字，比如100。
		for ; j > 0 && s[j-1] != ','; j-- {
		}
		// 务必如此拼接，不能s[:j] + ",#" + s[i + 5:]。否则可能
		// 越界或无法处理最后的情况，"9,#,#"
		s = s[:j] + s[i+3:]
	}

	return s == "#"
}
```

<br>


### Java
```java
public class IsValidPreord {
	public boolean isValidPreOrdSeq(String seq) {
		if (seq == null)
			return true;
		
		String s = seq;
		while (s.length() > 1) {
			int index = s.indexOf(",#,#");
			if (index < 0)
				return false;
			int start = index;
			while (start > 0 && s.charAt(start - 1) != ',')
				start--;
			if (s.charAt(start) == '#') // Important
				return false;
			s = s.substring(0, start) + s.substring(index + 3);
		}
		
		return s.equals("#");
	}
}
```

<br>


### Python
```python
class Solution:
    def isValidSerialization(self, s: str) -> bool:
        if not s:
            return True
        
        while len(s) > 1:
            j = i = s.find(",#,#")
            if i == -1:
                return False
            while j > 0 and s[j - 1] != ',':
                j -= 1
            if s[j] == '#':
                return False
            s = s[:j] + s[i + 3:]
        
        return s == "#"
```