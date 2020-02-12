# <center>51 - N-Queens (H)</center> 



<br></br>

* Author: Jinghua Zhu <jhzhu@outlook.com>
* Tag: DFS, Backtracking, Recursion, String
* Difficulty: Hard
* Link: https://leetcode.com/problems/n-queens/

<br></br>



## Description
----
The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.

Given an integer n, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where 'Q' and '.' both indicate a queen and an empty space respectively.

<br></br>



## Example
----
Input: 4

Output: 

```
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

<br></br>



## Solution
----
算法：
1. DFS结束条件，当前检查深度为n。
2. 引入visited数组，用以标记哪一列已被访问。
3. 引入path数组，用以标记每行的哪一列已被访问。
4. DFS递归方式：
    1. 如果当前列已被访问，跳过。
    2. 判断是否对角线存在皇后，两个点在斜线上判断用等式abs(x1 - x2) == abs(y1 - y2)即可。

<br>


### Go
```go
func NQueens1(n int) [][]string {
	res := make([][]string, 0)
	path := make([]int, 0)

	nq1DFS(n, make([]bool, n, n), &path, &res)

	return res
}

func nq1DFS(n int, visited []bool, path *[]int, res *[][]string) {
	l := len(*path)
	if l == n {
		nq1Add(res, *path)
		return
	}

	for i := 0; i < n; i++ {
		if visited[i] || !nq1Valid(*path, i) {
			continue
		}
		visited[i] = true
		*path = append(*path, i)
		nq1DFS(n, visited, path, res)
		visited[i] = false
		*path = (*path)[:len(*path)-1]
	}
}

func nq1Add(res *[][]string, path []int) {
	l := len(path)
	tmp := make([]string, 0, l)
	for _, v := range path {
		s := ""
		for i := 0; i < l; i++ {
			if i == v {
				s += "Q"
			} else {
				s += "."
			}
		}
		tmp = append(tmp, s)
	}
	*res = append(*res, tmp)
}

func nq1Valid(path []int, n int) bool {
	l := len(path)
	for k, v := range path {
		if absInt(l-k) == absInt(n-v) {
			return false
		}
	}

	return true
}

func absInt(a int) int {
	if a < 0 {
		return -1 * a
	}
	return a
}
```

<br>


### Java
```java
public class NQueens1 {
	public List<List<String>> solution(int n) {
        List<List<String>> results = new ArrayList<>();
        dfs(n, new boolean[n + 1], new ArrayList<>(), results);
        
        return results;
    }
    
    private void dfs(int n, boolean[] visited, List<Integer> subset, List<List<String>> results){
        if(subset.size() == n){
            results.add(makeBoard(subset));
            return;
        }
        
        for (int i = 1; i <= n; i++){
            if (visited[i] || !isValid(i, subset))
                continue;
            visited[i] = true;
            subset.add(i);
            dfs(n, visited, subset, results);
            subset.remove(subset.size() - 1);
            visited[i] = false;
        }
    }
    
    private List<String> makeBoard(List<Integer> subset){
        List<String> board = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        
        for(int i = 0; i < subset.size(); i++)
            sb.append(".");
        
        for (int i = 0; i < subset.size(); i++){
            int pos = subset.get(i);
            sb.replace(pos - 1, pos, "Q");
            board.add(sb.toString());
            sb.replace(pos - 1, pos, ".");
        }
        
        return board;
    }
    
    private boolean isValid(int n, List<Integer> subset){
        int pos = subset.size();
        for(int i = 0; i < subset.size(); i++)
            if(Math.abs(pos - i) == Math.abs(n - subset.get(i)))
                return false;
        
        return true;
    }
}
```