---
comments: true
difficulty: Hard
rating: 1810
source: Biweekly Contest 102 Q4
tags:
    - Graph
    - Design
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2642. Design Graph With Shortest Path Calculator](https://leetcode.com/problems/design-graph-with-shortest-path-calculator)

[中文文档](/solution/2600-2699/2642.Design%20Graph%20With%20Shortest%20Path%20Calculator/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị <strong>có hướng, có trọng số</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Các cạnh của đồ thị ban đầu được biểu diễn bởi mảng <code>edges</code>, trong đó <code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>, edgeCost<sub>i</sub>]</code> nghĩa là có một cạnh từ <code>from<sub>i</sub></code> đến <code>to<sub>i</sub></code> với chi phí <code>edgeCost<sub>i</sub></code>.</p>

<p>Hãy cài đặt class <code>Graph</code>:</p>

<ul>
	<li><code>Graph(int n, int[][] edges)</code> khởi tạo đối tượng với <code>n</code> node và các cạnh đã cho.</li>
	<li><code>addEdge(int[] edge)</code> thêm một cạnh vào danh sách cạnh, trong đó <code>edge = [from, to, edgeCost]</code>. Đảm bảo rằng trước khi thêm cạnh này không có cạnh nào giữa hai node.</li>
	<li><code>int shortestPath(int node1, int node2)</code> trả về chi phí <strong>nhỏ nhất</strong> của một đường đi từ <code>node1</code> đến <code>node2</code>. Nếu không tồn tại đường đi, trả về <code>-1</code>. Chi phí của một đường đi là tổng chi phí các cạnh trên đường đi.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2642.Design%20Graph%20With%20Shortest%20Path%20Calculator/images/graph3drawio-2.png" style="width: 621px; height: 191px;" />
<pre>
<strong>Đầu vào</strong>
[&quot;Graph&quot;, &quot;shortestPath&quot;, &quot;shortestPath&quot;, &quot;addEdge&quot;, &quot;shortestPath&quot;]
[[4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]], [3, 2], [0, 3], [[1, 3, 4]], [0, 3]]
<strong>Đầu ra</strong>
[null, 6, -1, null, 6]

<strong>Giải thích</strong>
Graph g = new Graph(4, [[0, 2, 5], [0, 1, 2], [1, 2, 1], [3, 0, 3]]);
g.shortestPath(3, 2); // return 6. The shortest path from 3 to 2 in the first diagram above is 3 -&gt; 0 -&gt; 1 -&gt; 2 with a total cost of 3 + 2 + 1 = 6.
g.shortestPath(0, 3); // return -1. There is no path from 0 to 3.
g.addEdge([1, 3, 4]); // We add an edge from node 1 to node 3, and we get the second diagram above.
g.shortestPath(0, 3); // return 6. The shortest path from 0 to 3 now is 0 -&gt; 1 -&gt; 3 with a total cost of 2 + 4 = 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= edges.length &lt;= n * (n - 1)</code></li>
	<li><code>edges[i].length == edge.length == 3</code></li>
	<li><code>0 &lt;= from<sub>i</sub>, to<sub>i</sub>, from, to, node1, node2 &lt;= n - 1</code></li>
	<li><code>1 &lt;= edgeCost<sub>i</sub>, edgeCost &lt;= 10<sup>6</sup></code></li>
	<li>Tại mọi thời điểm, đồ thị không có cạnh trùng lặp và không có self-loop.</li>
	<li>Sẽ có nhiều nhất <code>100</code> lần gọi <code>addEdge</code>.</li>
	<li>Sẽ có nhiều nhất <code>100</code> lần gọi <code>shortestPath</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijsktra

<!-- thinking:start -->

> **Tư duy**
>
> Các cạnh được thêm động và có nhiều truy vấn khoảng cách giữa các cặp node. Vì $n \le 100$, Dijkstra dùng heap là không cần thiết; Dijkstra với độ phức tạp $O(n^2)$ trên đồ thị dày đặc sẽ đơn giản hơn.
>
> Dùng ma trận kề để lưu trọng số, trong đó các cạnh không tồn tại được biểu diễn bằng $\infty$. `addEdge` ghi vào một ô; `shortestPath` thực hiện $n$ lần chọn node nhỏ nhất và nới lỏng. Các cặp node không thể đi tới sẽ trả về $-1$.

<!-- thinking:end -->

Trong hàm khởi tạo, trước tiên ta dùng ma trận kề $g$ để lưu trọng số các cạnh của đồ thị, trong đó $g_{ij}$ biểu diễn trọng số của cạnh từ node $i$ đến node $j$. Nếu không có cạnh giữa $i$ và $j$, giá trị của $g_{ij}$ là $\infty$.

Trong hàm `addEdge`, ta cập nhật giá trị của $g_{ij}$ thành $edge[2]$.

Trong hàm `shortestPath`, ta dùng thuật toán Dijsktra để tìm đường đi ngắn nhất từ node $node1$ đến node $node2$. Ở đây, $dist[i]$ biểu diễn đường đi ngắn nhất từ node $node1$ đến node $i$, còn $vis[i]$ cho biết node $i$ đã được duyệt hay chưa. Ta khởi tạo $dist[node1]$ bằng $0$, còn các $dist[i]$ khác đều bằng $\infty$. Sau đó, ta lặp $n$ lần; mỗi lần tìm node chưa được duyệt hiện tại $t$ sao cho $dist[t]$ nhỏ nhất. Tiếp theo, ta đánh dấu node $t$ đã được duyệt, rồi cập nhật giá trị của $dist[i]$ thành $min(dist[i], dist[t] + g_{ti})$. Cuối cùng, ta trả về $dist[node2]$. Nếu $dist[node2]$ bằng $\infty$, nghĩa là không có đường đi từ node $node1$ đến node $node2$, nên ta trả về $-1$.

Độ phức tạp thời gian là $O(n^2 \times q)$, độ phức tạp không gian là $O(n^2)$. Trong đó $n$ là số node, còn $q$ là số lần gọi hàm `shortestPath`.

<!-- tabs:start -->

#### Python3

```python
class Graph:
    def __init__(self, n: int, edges: List[List[int]]):
        self.n = n
        self.g = [[inf] * n for _ in range(n)]
        for f, t, c in edges:
            self.g[f][t] = c

    def addEdge(self, edge: List[int]) -> None:
        f, t, c = edge
        self.g[f][t] = c

    def shortestPath(self, node1: int, node2: int) -> int:
        dist = [inf] * self.n
        dist[node1] = 0
        vis = [False] * self.n
        for _ in range(self.n):
            t = -1
            for j in range(self.n):
                if not vis[j] and (t == -1 or dist[t] > dist[j]):
                    t = j
            vis[t] = True
            for j in range(self.n):
                dist[j] = min(dist[j], dist[t] + self.g[t][j])
        return -1 if dist[node2] == inf else dist[node2]


# Your Graph object will be instantiated and called as such:
# obj = Graph(n, edges)
# obj.addEdge(edge)
# param_2 = obj.shortestPath(node1,node2)
```

#### Java

```java
class Graph {
    private int n;
    private int[][] g;
    private final int inf = 1 << 29;

    public Graph(int n, int[][] edges) {
        this.n = n;
        g = new int[n][n];
        for (var f : g) {
            Arrays.fill(f, inf);
        }
        for (int[] e : edges) {
            int f = e[0], t = e[1], c = e[2];
            g[f][t] = c;
        }
    }

    public void addEdge(int[] edge) {
        int f = edge[0], t = edge[1], c = edge[2];
        g[f][t] = c;
    }

    public int shortestPath(int node1, int node2) {
        int[] dist = new int[n];
        boolean[] vis = new boolean[n];
        Arrays.fill(dist, inf);
        dist[node1] = 0;
        for (int i = 0; i < n; ++i) {
            int t = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[t] + g[t][j]);
            }
        }
        return dist[node2] >= inf ? -1 : dist[node2];
    }
}

/**
 * Your Graph object will be instantiated and called as such:
 * Graph obj = new Graph(n, edges);
 * obj.addEdge(edge);
 * int param_2 = obj.shortestPath(node1,node2);
 */
```

#### C++

```cpp
class Graph {
public:
    Graph(int n, vector<vector<int>>& edges) {
        this->n = n;
        g = vector<vector<int>>(n, vector<int>(n, inf));
        for (auto& e : edges) {
            int f = e[0], t = e[1], c = e[2];
            g[f][t] = c;
        }
    }

    void addEdge(vector<int> edge) {
        int f = edge[0], t = edge[1], c = edge[2];
        g[f][t] = c;
    }

    int shortestPath(int node1, int node2) {
        vector<bool> vis(n);
        vector<int> dist(n, inf);
        dist[node1] = 0;
        for (int i = 0; i < n; ++i) {
            int t = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = min(dist[j], dist[t] + g[t][j]);
            }
        }
        return dist[node2] >= inf ? -1 : dist[node2];
    }

private:
    vector<vector<int>> g;
    int n;
    const int inf = 1 << 29;
};

/**
 * Your Graph object will be instantiated and called as such:
 * Graph* obj = new Graph(n, edges);
 * obj->addEdge(edge);
 * int param_2 = obj->shortestPath(node1,node2);
 */
```

#### Go

```go
const inf = 1 << 29

type Graph struct {
	g [][]int
}

func Constructor(n int, edges [][]int) Graph {
	g := make([][]int, n)
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = inf
		}
	}
	for _, e := range edges {
		f, t, c := e[0], e[1], e[2]
		g[f][t] = c
	}
	return Graph{g}
}

func (this *Graph) AddEdge(edge []int) {
	f, t, c := edge[0], edge[1], edge[2]
	this.g[f][t] = c
}

func (this *Graph) ShortestPath(node1 int, node2 int) int {
	n := len(this.g)
	dist := make([]int, n)
	for i := range dist {
		dist[i] = inf
	}
	vis := make([]bool, n)
	dist[node1] = 0
	for i := 0; i < n; i++ {
		t := -1
		for j := 0; j < n; j++ {
			if !vis[j] && (t == -1 || dist[t] > dist[j]) {
				t = j
			}
		}
		vis[t] = true
		for j := 0; j < n; j++ {
			dist[j] = min(dist[j], dist[t]+this.g[t][j])
		}
	}
	if dist[node2] >= inf {
		return -1
	}
	return dist[node2]
}

/**
 * Your Graph object will be instantiated and called as such:
 * obj := Constructor(n, edges);
 * obj.AddEdge(edge);
 * param_2 := obj.ShortestPath(node1,node2);
 */
```

#### TypeScript

```ts
class Graph {
    private g: number[][] = [];
    private inf: number = 1 << 29;

    constructor(n: number, edges: number[][]) {
        this.g = Array.from({ length: n }, () => Array(n).fill(this.inf));
        for (const [f, t, c] of edges) {
            this.g[f][t] = c;
        }
    }

    addEdge(edge: number[]): void {
        const [f, t, c] = edge;
        this.g[f][t] = c;
    }

    shortestPath(node1: number, node2: number): number {
        const n = this.g.length;
        const dist: number[] = new Array(n).fill(this.inf);
        dist[node1] = 0;
        const vis: boolean[] = new Array(n).fill(false);
        for (let i = 0; i < n; ++i) {
            let t = -1;
            for (let j = 0; j < n; ++j) {
                if (!vis[j] && (t === -1 || dist[j] < dist[t])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (let j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[t] + this.g[t][j]);
            }
        }
        return dist[node2] >= this.inf ? -1 : dist[node2];
    }
}

/**
 * Your Graph object will be instantiated and called as such:
 * var obj = new Graph(n, edges)
 * obj.addEdge(edge)
 * var param_2 = obj.shortestPath(node1,node2)
 */
```

#### C#

```cs
public class Graph {
    private int n;
    private int[,] g;
    private readonly int inf = 1 << 29;

    public Graph(int n, int[][] edges) {
        this.n = n;
        g = new int[n, n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                g[i, j] = inf;
            }
        }
        foreach (var e in edges) {
            int f = e[0], t = e[1], c = e[2];
            g[f, t] = c;
        }
    }

    public void AddEdge(int[] edge) {
        int f = edge[0], t = edge[1], c = edge[2];
        g[f, t] = c;
    }

    public int ShortestPath(int node1, int node2) {
        int[] dist = new int[n];
        bool[] vis = new bool[n];
        Array.Fill(dist, inf);
        dist[node1] = 0;
        for (int i = 0; i < n; ++i) {
            int t = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = Math.Min(dist[j], dist[t] + g[t, j]);
            }
        }
        return dist[node2] >= inf ? -1 : dist[node2];
    }
}

/**
 * Your Graph object will be instantiated and called as such:
 * Graph obj = new Graph(n, edges);
 * obj.AddEdge(edge);
 * int param_2 = obj.ShortestPath(node1,node2);
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
