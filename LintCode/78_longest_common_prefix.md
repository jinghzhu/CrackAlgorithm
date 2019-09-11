# <center>78 - Longest Common Prefix (E)</center> 



<br></br>

* Tag: String
* Difficulty: Easy
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Link: https://www.lintcode.com/problem/longest-common-prefix/description

<br></br>



## Description
----
Find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string "".

<br></br>



## Example
----
1. Input: ["flower","flow","flight"] Output: "fl"
2. Input: ["dog","racecar","car"] Output: ""

<br></br>



## Solution
----
### Go
```go
func LongestCommonPrefix(strs []string) string {
	if len(strs) < 1 {
		return ""
	}
	root := lcpNewNode(1, 1)
	for _, v := range strs {
		lcpAdd(root, v)
	}

	return lcpCheckDict(root, len(strs))
}

type lcpDictionaryNode struct {
	key      uint8
	count    int
	children map[uint8]*lcpDictionaryNode
}

func lcpNewNode(k uint8, c int) *lcpDictionaryNode {
	return &lcpDictionaryNode{
		key:      k,
		count:    c,
		children: make(map[uint8]*lcpDictionaryNode),
	}
}

func lcpAdd(root *lcpDictionaryNode, str string) {
	cur := root
	for i := 0; i < len(str); i++ {
		child, ok := cur.children[str[i]]
		if ok {
			child.count++
		} else {
			child = lcpNewNode(str[i], 1)
			cur.children[str[i]] = child
		}
		cur = child
	}
}

func lcpCheckDict(root *lcpDictionaryNode, length int) string {
	curDict := root.children
	result := ""
	flag := true
	for flag && len(curDict) != 0 {
		flag = false
		for k, v := range curDict {
			if v.count == length {
				result += string(k)
				curDict = v.children
				flag = true
				break
			}
		}
	}

	return result
}
```

<br>


### Java
```java
public class LongestCommonPrefix {
	/**
     * @param strs: A list of strings
     * @return: The longest common prefix
     */
    public String longestCommonPrefix(String[] strs) {
        if (strs == null)
        	return "";
        
        lcpDictNode root = new lcpDictNode();
        for (String s : strs)
        	add(s, root);
        
        return check(strs, root);
    }
    
    private void add(String str, lcpDictNode root) {
    	char[] charArr = str.toCharArray();
    	lcpDictNode cur = root;
    	for (char ch : charArr) {
    		if (cur.children.containsKey(ch)) {
    			lcpDictNode child = cur.children.get(ch);
    			child.count++;
    			cur.children.put(ch, child);
    			cur = child;
    		} else {
    			lcpDictNode child = new lcpDictNode(ch, 1);
    			cur.children.put(ch, child);
    			cur = child;
    		}
    	}
    }
    
    private String check(String[] strs, lcpDictNode root) {
    	int len = strs.length;
    	String str = "";
    	lcpDictNode cur = root;
    	boolean flag = true;
    	while (flag && cur.children.size() != 0) {
    		flag = false;
    		for (Map.Entry<Character, lcpDictNode> e : cur.children.entrySet()) {
    			lcpDictNode child = e.getValue();
    			if (child.count == len) {
    				str += child.key;
    				cur = child;
    				flag = true;
    				break;
    			}
    		}
    	}
    	
    	return str;
    }
}

class lcpDictNode {
	public char key;
	public int count;
	public HashMap<Character, lcpDictNode> children;
	
	public lcpDictNode() {
		this.key = ' ';
		this.count = 0;
		this.children = new HashMap<Character, lcpDictNode>();
	}
	
	public lcpDictNode(char k, int c) {
		this.key = k;
		this.count = c;
		this.children = new HashMap<Character, lcpDictNode>();
	}

```