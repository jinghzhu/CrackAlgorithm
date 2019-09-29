# <center>442 - Trie (M)</center> 



<br></br>

* Difficulty: Medium
* Tag: Hash, Trie
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Link: https://www.lintcode.com/problem/implement-trie-prefix-tree/description
* Company: Twitter, Facebook, Google, Amazon, Uber, Microsoft

<br></br>



## Description
----
Implement a trie with insert, search, and startsWith methods.

<br></br>



## Example
----
```go
trie.insert("apple");
trie.search("apple");   // returns true
trie.search("app");     // returns false
trie.startsWith("app"); // returns true
trie.insert("app");   
trie.search("app");     // returns true
```

<br></br>



## Solution
----
### Java
```java
public class TypeAHead_Trie {
	class TrieNode {
        private HashMap<Character, TrieNode> children;
        private boolean isEnd;
        
        public TrieNode() {
            this.isEnd = false;
            this.children = new HashMap<Character, TrieNode>();
        }
        
        public boolean isEnd() {
        	return this.isEnd;
        }
        
        public void setEnd(boolean flag) {
        	this.isEnd = flag;
        }
        
        public HashMap<Character, TrieNode> getChildren() {
        	return this.children;
        }
        
        public void addChild(char ch, TrieNode n) {
        	this.children.put(ch, n);
        }
        
        public boolean containsChild(char ch) {
        	return this.children.containsKey(ch);
        }
        
        public TrieNode getChild(char ch) {
        	return this.children.get(ch);
        }
    }
    
    private TrieNode root;
    
    public TypeAHead_Trie() {
        this.root = new TrieNode();
    }

    /*
     * @param word: a word
     * @return: nothing
     */
    public void insert(String word) {
        if (word == null)
        	return;
        
        TrieNode cur = this.root;
        for (char ch : word.toCharArray()) {
        	if (!cur.containsChild(ch))
        		cur.addChild(ch, new TrieNode());
        	cur = cur.getChild(ch);
        }
        cur.setEnd(true);
    }

    /*
     * @param word: A string
     * @return: if the word is in the trie.
     */
    public boolean search(String word) {
        TrieNode n = searchHelper(word);
        return n != null && n.isEnd();
    }
    
    private TrieNode searchHelper(String word) {
    	if (word == null)
    		return null;
    	
    	TrieNode cur = this.root;
    	for (char ch : word.toCharArray()) {
    		if (!cur.containsChild(ch))
    			return null;
    		cur = cur.getChild(ch);
    	}
    	
    	return cur;
    }

    /*
     * @param prefix: A string
     * @return: if there is any word in the trie that starts with the given prefix.
     */
    public boolean startsWith(String prefix) {
        return searchHelper(prefix) != null;
    }
}
```

<br>


### Go
```go
// Trie is for the prefix tree.
type Trie struct {
	root *trieNode
}

type trieNode struct {
	children map[int32]*trieNode
	isEnd    bool
}

func newTrieNode() *trieNode {
	return &trieNode{
		children: make(map[int32]*trieNode),
		isEnd:    false,
	}
}

// NewTrie returns a trie.
func NewTrie() Trie {
	return Trie{newTrieNode()}
}

// Insert a word into the trie.
func (t *Trie) Insert(word string) {
	cur := t.root
	for _, v := range word {
		if _, ok := cur.children[v]; !ok {
			cur.children[v] = newTrieNode()
		}
		cur = cur.children[v]
	}
	cur.isEnd = true
}

// Search returns if the word is in the trie.
func (t *Trie) Search(word string) bool {
	n := t.searchHelper(word)
	return n != nil && n.isEnd
}

func (t *Trie) searchHelper(word string) *trieNode {
	cur := t.root
	for _, v := range word {
		if _, ok := cur.children[v]; !ok {
			return nil
		}
		cur = cur.children[v]
	}

	return cur
}

// StartsWith returns if there is any word in the trie that starts with the given prefix.
func (t *Trie) StartsWith(prefix string) bool {
	return t.searchHelper(prefix) != nil
}
```

<br>


### Python
```python
class Trie:
    class TrieNode:
        def __init__(self, end=False, children={}):
            self.end = end
            self.children = children

    def __init__(self):
        self.root = self.TrieNode()

    def insert(self, word):
        """
        @param: word: a word
        @return: nothing
        """
        cur = self.root
        for ch in word:
            if ch not in cur.children:
                cur.children[ch] = self.TrieNode(False, {})
            cur = cur.children[ch]
        cur.end = True

    def search(self, word):
        """
        @param: word: A string
        @return: if the word is in the trie.
        """
        n = self.search_helper(word)
        return n is not None and n.end

    def search_helper(self, word):
        cur = self.root
        for ch in word:
            if ch not in cur.children:
                return None
            cur = cur.children[ch]

        return cur

    def starts_with(self, prefix):
        """
        @param: prefix: A string
        @return: if there is any word in the trie that starts with the given prefix.
        """
        return self.search_helper(prefix) is not None
```