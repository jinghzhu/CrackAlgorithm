# <center>132 - Word Search II (H)</center> 



<br></br>

* Tag: DFS, Recursion, Backtracking, Trie
* Difficulty: Hard
* Company: Google, Microsoft, Airbnb
* Link: https://www.lintcode.com/problem/word-search-ii/description

<br></br>



## Description
----
Given a 2D board and a list of words from the dictionary, find all words in the board.

Each word must be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.

<br></br>



## Example
----
Input: 

```
board = [
  ['o','a','a','n'],
  ['e','t','a','e'],
  ['i','h','k','r'],
  ['i','f','l','v']
]

words = ["oath","pea","eat","rain"]
```

Output: ["eat","oath"]

<br></br>



## Solution
----
算法：
1. 引入trie索引所有要查找的字符串。
2. 从board每个节点都开始尝试DFS搜索。
3. 每次搜索递归时，传入上层递归时访问的字符对应的在字典树中的节点。
4. 搜索终止情况是当前字符不在字典树当前节点范围中。
5. 搜索成功情况是当前字符在字典树当前节点范围中且存在单词。此时，加入结果集并继续。

<br>


### Go
```go
var (
	gw2DX, gw2DY = []int{1, 0, -1, 0}, []int{0, 1, 0, -1}
)

func GetWords2(board [][]byte, words []string) []string {
	t := &gw2Trie{
		root: newGW2TrieNode(),
	}
	for _, v := range words {
		t.insert(v)
	}
	m := make(map[string]bool)
	for i := 0; i < len(board); i++ {
		for j := 0; j < len(board[0]); j++ {
			gw2DFS(board, i, j, t.root, m)
		}
	}
	res := make([]string, 0, len(m))
	for k := range m {
		res = append(res, k)
	}

	return res
}

func gw2DFS(board [][]byte, x, y int, root *gw2TrieNode, m map[string]bool) {
	childNode := root.children[int32(board[x][y])]
	if childNode == nil {
		return
	}
	if childNode.end {
		m[childNode.word] = true
	}

	b := board[x][y]
	board[x][y] = ','
	for i := 0; i < 4; i++ {
		if valid(board, x+gw2DX[i], y+gw2DY[i]) {
			gw2DFS(board, x+gw2DX[i], y+gw2DY[i], childNode, m)
		}
	}
	board[x][y] = b
}

func valid(board [][]byte, x, y int) bool {
	return x >= 0 && y >= 0 && x < len(board) && y < len(board[0])
}

type gw2Trie struct {
	root *gw2TrieNode
}

type gw2TrieNode struct {
	children map[int32]*gw2TrieNode
	end      bool
	word     string
}

func newGW2TrieNode() *gw2TrieNode {
	return &gw2TrieNode{
		children: make(map[int32]*gw2TrieNode),
		end:      false,
	}
}

func (t *gw2Trie) insert(word string) {
	cur := t.root
	for _, v := range word {
		if _, ok := cur.children[v]; !ok {
			cur.children[v] = newGW2TrieNode()
		}
		cur = cur.children[v]
	}
	cur.end = true
	cur.word = word
}
```

<br>
