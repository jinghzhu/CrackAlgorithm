# <center>425 - Word Squares (H)</center> 



<br></br>

* Tag: Backtracking, Recursion, DFS, String, Trie
* Comapny: Google
* Difficulty: Hard
* Link: https://leetcode.com/problems/word-squares

<br></br>



## Description
----
Given a set of words without duplicates, find all word squares you can build from them.

A sequence of words forms a valid word square if the kth row and column read the exact same string, where 0 ≤ k < max(numRows, numColumns).

For example, the word sequence ["ball","area","lead","lady"] forms a word square because each word reads the same both horizontally and vertically.

```
b a l l
a r e a
l e a d
l a d y
```

<br></br>



## Example
----
- Input: ["area","lead","wall","lady","ball"]
- Output: [["wall","area","lead","lady"],["ball","area","lead","lady"]]

<br></br>



## Solution
----
算法：
1. 使用Trie缓存所有word和prefix关系。
2. 使用DFS逐个尝试所有可能。注意DFS时，前缀“”用以表示开始第一个字符串。对后续的，正常DFS即可。
3. 剪枝（假设第一行为ball，现在放第二行）：
    1. 只考虑a开头的字符串。
    2. 如果放入area，需检查le和la为前缀的字符串是否存在。

<br>


## Go
```go
func GetWordsSquares(words []string) [][]string {
	res := make([][]string, 0)
	l := len(words)
	if l < 1 { // important
		return res
	}
	t := newTrie()
	for _, v := range words {
		t.insert(v)
	}
	path := make([]string, 0)
	gwsDFS(l, &path, &res, t)

	return res
}

func gwsDFS(remains int, path *[]string, res *[][]string, t *trie) {
	if remains == 0 {
		tmp := make([]string, len(*path), len(*path))
		copy(tmp, *path) // deep copy is important
		*res = append(*res, tmp)
		return
	}

	prefix := gwsGetPrefix(*path)
	candidates := t.GetAllStartsWith(prefix)
	for _, word := range candidates {
		if !gwsValid(word, *path, t) {
			continue
		}
		*path = append(*path, word)
		gwsDFS(remains-1, path, res, t)
		*path = (*path)[:len(*path)-1]
	}
}

func gwsValid(word string, path []string, t *trie) bool {
	if word == "" {
		return true
	}
	l := len(path)
	for i := l + 1; i < len(path[0]); i++ {
		s := ""
		for _, v := range path {
			s += string(v[i])
		}
		if !t.search(s) {
			return false
		}
	}

	return true
}

func gwsGetPrefix(path []string) string {
	l := len(path)
	if l < 1 {
		return ""
	}
	s := ""
	for _, v := range path {
		s += string(v[1])
	}

	return s
}

type trieNode struct {
	children         map[int32]*trieNode
	allChildrenWords []string
	end              bool
}

func newTrieNode() *trieNode {
	return &trieNode{
		children:         make(map[int32]*trieNode),
		allChildrenWords: make([]string, 0),
		end:              false,
	}
}

type trie struct {
	root  *trieNode
	words map[string]bool // not necessary
}

func newTrie() *trie {
	return &trie{
		root:  newTrieNode(),
		words: make(map[string]bool), // not necessary
	}
}

func (t *trie) insert(word string) {
	if t.words[word] {
		return
	}

	t.words[word] = true
	cur := t.root
	for _, v := range word {
		cur.allChildrenWords = append(cur.allChildrenWords, word) // not necessary
		if _, ok := cur.children[v]; !ok {
			cur.children[v] = newTrieNode()
		}
		cur = cur.children[v]
	}
	cur.end = true
}

func (t *trie) search(word string) bool {
	n := t.getNode(word)
	return n != nil && n.end
}

func (t *trie) getNode(word string) *trieNode {
	if word == "" {
		return t.root
	}
	cur := t.root
	for _, v := range word {
		if _, ok := cur.children[v]; !ok {
			return nil
		}
		cur = cur.children[v]
	}

	return cur
}

func (t *trie) GetAllStartsWith(prefix string) []string {
	n := t.getNode(prefix)
	if n == nil {
		return make([]string, 0)
	}
	return n.allChildrenWords
}
```

<br>


### Java
```java
public class GetWordSquares {
	class TrieNode {
		boolean end;
		Map<Character, TrieNode> children;
		List<String> allChildren;
		
		public TrieNode() {
			this.children = new HashMap<Character, TrieNode>();
			this.allChildren = new ArrayList<String>();
			this.end = false;
		}
	}
	
	class Trie {
		TrieNode root;
		Set<String> words;
		
		public Trie() {
			this.root = new TrieNode();
			this.words = new HashSet<String>();
		}
		
		public void add(String s) {
			if (s == null || this.words.contains(s))
				return;
			
			this.words.add(s);
			TrieNode n = this.root;
			
			for (Character ch : s.toCharArray()) {
				n.allChildren.add(s);
				if (!n.children.containsKey(ch))
					n.children.put(ch, new TrieNode());
				n = n.children.get(ch);
			}
			n.end = true;
		}
		
		private TrieNode getNode(String s) {
			if (s == null)
				return null;
			
			TrieNode n = this.root;
			for (Character ch : s.toCharArray()) {
				if (n == null)
					return null;
				n = n.children.get(ch);
			}
			
			return n;
		}
		
		public List<String> getStartWith(String s) {
			List<String> words = new ArrayList<String>();
			if (s == null)
				return words;
			if (s.equals("")) // important
				return new ArrayList<String>(this.words);
			
			TrieNode n = this.getNode(s);
			
			if (n == null)
				return words;
			
			return n.allChildren;
		}
		
		public boolean contains(String s) {
			TrieNode n = this.getNode(s);
			return n != null && n.end;
		}
	}
	
	private Trie t;
	
	public List<List<String>> solution(String[] words) {
        List<List<String>> res = new ArrayList<List<String>>();
        if (words == null || words.length < 1)
        	return res;
        
        this.t = new Trie();
        for (String s : words)
        	t.add(s);
        
        this.dfs(words[0].length(), new ArrayList<String>(), res);
        
        return res;
    }
	
	private void dfs(int remains, List<String> path, List<List<String>> res) {
		if (remains == 0) {
			res.add(new ArrayList<>(path)); // Deep copy is very important.
			return;
		}
		
		String prefix = this.getPrefix(path); // 剪枝1
		List<String> words = this.t.getStartWith(prefix);
		for (String w : words) {
			if (!this.isValid(path, w)) // 剪枝2
				continue;
			path.add(w);
			dfs(remains - 1, path, res);
			path.remove(path.size() - 1); // backtracking
		}
	}
	
	private boolean isValid(List<String> path, String word) {
		int l = path.size();
		if (l == 0)
			return true;
		
		String tmp = "";
		for (int i = l + 1; i < path.get(0).length(); i++) {
			tmp = "";
			for (String s : path)
				tmp += s.substring(i, i + 1);
			if (t.getStartWith(tmp).size() < 1)
				return false;
		}
		
		return true;
	}
	
	private String getPrefix(List<String> path) {
		int l = path.size();
		if (l == 0)
			return ""; // important
		
		String prefix = "";
		for (String s : path)
			prefix += s.substring(l, l + 1);
		
		return prefix;
	}	
}
```