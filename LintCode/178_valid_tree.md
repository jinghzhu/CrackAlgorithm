# <center>178 - Graph Valid Tree (M)</center> 



<br></br>

* Tag: Graph, BFS, Union Find
* Author: Jinghua Zhu <jhzhu@outlook.com>
* Difficulty: Medium
* Company: Google, Facebook
* Link: https://www.lintcode.com/problem/graph-valid-tree/description

<br></br>



## Description
----
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

<br></br>



## Example
----
Input: n = 5 edges = [[0, 1], [0, 2], [0, 3], [1, 4]]
Output: true.

<br></br>



## Solution
----
Steps:
1. Create a graph in adjmap style and record the number of edges.
2. Check if edge number == n - 1.
3. BFS
4. If edge number == n - 1 but connected nodes number (visited.size()) != n, means it is not a valid tree.

<br>



### Java
```java
public class IsTree {
	/**
     * @param n: An integer
     * @param edges: a list of undirected edges
     * @return: true if it's a valid tree, or false
     */
    public boolean validTree(int n, int[][] edges) {
        if (edges == null || n < 1)
            return false;
        if (n == 1 && edges.length < 1)
            return true; // 1 node, 0 edge.
        
        Map<Integer, List<Integer>> adjmap = createGraph(edges);
        if (adjmap.get(-1).get(0) != n - 1)
            return false;
    
        Set<Integer> visited = new HashSet<Integer>();
        visited.add(0);
        Queue<Integer> q = new LinkedList<Integer>();
        q.offer(0);
        
        while (!q.isEmpty()) {
            int id = q.poll();
            for (int i : adjmap.get(id)) {
                if (!visited.contains(i)) {
                    visited.add(i);
                    q.offer(i);
                }
            }
        }
        
        return visited.size() == n;
    }
    
    private Map<Integer, List<Integer>> createGraph(int[][] edges) {
        Map<Integer, List<Integer>> adjmap = new HashMap<Integer, List<Integer>>();
        int edgeNum = 0;
        for (int i = 0; i < edges.length; i++) {
            edgeNum++;
            int start = edges[i][0], end = edges[i][1];
            insert(adjmap, start, end); // Assume no duplicated edge entries in input array.
            insert(adjmap, end, start);
        }
        List<Integer> l = new ArrayList<Integer>();
        l.add(edgeNum);
        adjmap.put(-1, l);
        
        return adjmap;
    }
    
    private void insert(Map<Integer, List<Integer>> adjmap, int s, int e) {
        if (!adjmap.containsKey(s))
            adjmap.put(s, new ArrayList<Integer>());
        List<Integer> l = adjmap.get(s);
        l.add(e);
        adjmap.put(s, l);
    }
}
```

<br>


### Python
```python
class IsTree:
    """
    @param n: An integer
    @param edges: a list of undirected edges
    @return: true if it's a valid tree, or false
    """
    def solution(self, n, edges):
        if not n or n < 1:
            return False
        if n == 1 and len(edges) < 1:  # Important. 1 node and 0 edge.
            return True

        adjmap, edge_num = self.init_graph(edges)
        if edge_num != n - 1:
            return False

        q = [0]
        visited = set()
        visited.add(0)

        while len(q) > 0:
            id = q.pop(0)
            for i in adjmap[id]:
                if i not in visited:
                    visited.add(i)
                    q.append(i)

        return len(visited) == n

    def init_graph(self, edges):
        edge_num = 0
        adjmap = {}
        for e in edges:
            edge_num += 1
            self.insert(adjmap, e[0], e[1])
            self.insert(adjmap, e[1], e[0])
        return adjmap, edge_num

    def insert(self, adjmap, a, b):
        if a not in adjmap:
            adjmap[a] = [b]
        else:
            nodes = adjmap[a]
            nodes.append(b)
            adjmap[a] = nodes
```

<br>


### Go
```go
func IsTree(n int, edges [][]int) bool {
	if n < 1 {
		return false
	}
	if n == 1 && len(edges) < 1 { // Important. 1 node and 0 edge.
		return true
	}

	adjmap, edgeNum := itInitGraph(edges)
	if edgeNum != n-1 {
		return false
	}
	visited := make(map[int]bool)
	visited[0] = true
	q := make([]int, 0)
	q = append(q, 0)

	for len(q) > 0 {
		id := q[0]
		q = q[1:]
		for _, v := range adjmap[id] {
			if _, ok := visited[v]; !ok {
				visited[v] = true
				q = append(q, v)
			}
		}
	}

	return len(visited) == n
}

func itInitGraph(edges [][]int) (map[int][]int, int) {
	adjmap := make(map[int][]int)
	edgeNum := 0
	for _, v := range edges {
		edgeNum++
		itInsert(adjmap, v[0], v[1])
		itInsert(adjmap, v[1], v[0])
	}

	return adjmap, edgeNum
}

func itInsert(m map[int][]int, s, e int) {
	v, ok := m[s]
	if !ok {
		v = make([]int, 0)
	}
	m[s] = append(v, e)
}

```
