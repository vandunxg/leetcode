---
comments: true
difficulty: Hard
rating: 2873
source: Weekly Contest 346 Q4
tags:
    - Graph
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2699. Modify Graph Edge Weights](https://leetcode.com/problems/modify-graph-edge-weights)

[中文文档](/solution/2600-2699/2699.Modify%20Graph%20Edge%20Weights/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị <strong>vô hướng, có trọng số</strong> và <strong>liên thông</strong> gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>, cùng một mảng số nguyên <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, w<sub>i</sub>]</code> biểu thị rằng có một cạnh nối node <code>a<sub>i</sub></code> và node <code>b<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code>.</p>

<p>Một số cạnh có trọng số bằng <code>-1</code> (<code>w<sub>i</sub> = -1</code>), trong khi các cạnh còn lại có trọng số <strong>dương</strong> (<code>w<sub>i</sub> &gt; 0</code>).</p>

<p>Nhiệm vụ của bạn là sửa đổi <strong>tất cả các cạnh</strong> có trọng số bằng <code>-1</code> bằng cách gán cho chúng các giá trị <strong>nguyên dương</strong> trong khoảng <code>[1, 2 * 10<sup>9</sup>]</code> sao cho <strong>khoảng cách ngắn nhất</strong> giữa node <code>source</code> và <code>destination</code> bằng một số nguyên <code>target</code>. Nếu có <strong>nhiều cách</strong> <strong>sửa đổi</strong> khiến khoảng cách ngắn nhất giữa node <code>source</code> và <code>destination</code> bằng <code>target</code>, thì cách nào cũng được chấp nhận.</p>

<p><em>Hãy trả về một mảng chứa tất cả các cạnh (kể cả những cạnh không được sửa đổi) theo bất kỳ thứ tự nào nếu có thể khiến khoảng cách ngắn nhất từ </em><code>source</code><em> đến </em><code>destination</code><em> bằng </em><code>target</code><em>, hoặc một <strong>mảng rỗng</strong> nếu không thể.</em></p>

<p><strong>Lưu ý:</strong> Không được sửa đổi trọng số của các cạnh ban đầu có trọng số dương.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2699.Modify%20Graph%20Edge%20Weights/images/graph.png" style="width: 300px; height: 300px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[4,1,-1],[2,0,-1],[0,3,-1],[4,3,-1]], source = 0, destination = 1, target = 5
<strong>Đầu ra:</strong> [[4,1,1],[2,0,1],[0,3,3],[4,3,1]]
<strong>Giải thích:</strong> Hình trên biểu diễn một cách sửa đổi các cạnh, khiến khoảng cách từ 0 đến 1 bằng 5.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2699.Modify%20Graph%20Edge%20Weights/images/graph-2.png" style="width: 300px; height: 300px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1,-1],[0,2,5]], source = 0, destination = 2, target = 6
<strong>Đầu ra:</strong> []
<strong>Giải thích:</strong> Đồ thị ban đầu không thể được sửa đổi để khoảng cách từ 0 đến 2 bằng 6 bằng cách sửa cạnh có trọng số -1. Vì vậy, một mảng rỗng được trả về.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong class="example"><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2699.Modify%20Graph%20Edge%20Weights/images/graph-3.png" style="width: 300px; height: 300px;" /></strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[1,0,4],[1,2,3],[2,3,5],[0,3,-1]], source = 0, destination = 2, target = 6
<strong>Đầu ra:</strong> [[1,0,4],[1,2,3],[2,3,5],[0,3,1]]
<strong>Giải thích:</strong> Hình trên biểu diễn một đồ thị đã được sửa đổi, trong đó khoảng cách ngắn nhất từ 0 đến 2 bằng 6.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 100</code></li>
	<li><code><font face="monospace">1 &lt;= edges.length &lt;= n * (n - 1) / 2</font></code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i&nbsp;</sub>&lt;&nbsp;n</code></li>
	<li><code><font face="monospace">w<sub>i</sub>&nbsp;= -1&nbsp;</font></code>hoặc <code><font face="monospace">1 &lt;= w<sub>i&nbsp;</sub>&lt;= 10<sup><span style="font-size: 10.8333px;">7</span></sup></font></code></li>
	<li><code>a<sub>i&nbsp;</sub>!=&nbsp;b<sub>i</sub></code></li>
	<li><code>0 &lt;= source, destination &lt; n</code></li>
	<li><code>source != destination</code></li>
	<li><code><font face="monospace">1 &lt;= target &lt;= 10<sup>9</sup></font></code></li>
	<li>Đồ thị liên thông và không có self-loop hoặc cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Đường đi ngắn nhất (Thuật toán Dijkstra)

<!-- thinking:start -->

> **Tư duy**
>
> Ta phải thay thế các cạnh có trọng số $-1$ bằng các trọng số dương để đường đi ngắn nhất từ $source$ đến $destination$ bằng $target$. Việc thử mọi bộ trọng số là quá lớn; với $n \le 100$, ta có thể chạy Dijkstra nhiều lần.
>
> Trước tiên bỏ qua các cạnh có trọng số $-1$: nếu đường đi ngắn nhất chỉ gồm các cạnh có trọng số dương đã nhỏ hơn $target$ thì không thể thực hiện; nếu bằng target, ta có thể đặt các cạnh còn lại về giá trị giới hạn để không tạo ra đường tắt. Nếu khoảng cách vẫn lớn hơn, lần lượt thử từng cạnh có trọng số $-1$ với trọng số $1$; khi khoảng cách $\le target$, tăng trọng số của cạnh đó để đạt đúng $target$ và đặt các cạnh có trọng số $-1$ còn lại về giá trị giới hạn.
>
> Nếu đường đi không bao giờ đủ ngắn, trả về một mảng rỗng.

<!-- thinking:end -->

Trước hết, ta bỏ qua các cạnh có trọng số bằng $-1$ và dùng thuật toán Dijkstra để tìm khoảng cách ngắn nhất $d$ từ $source$ đến $destination$.

- Nếu $d < target$, điều đó có nghĩa là tồn tại một đường đi ngắn nhất chỉ gồm các cạnh có trọng số dương. Dù sửa đổi các cạnh có trọng số bằng $-1$ thế nào, ta cũng không thể khiến khoảng cách ngắn nhất từ $source$ đến $destination$ bằng $target$. Vì vậy, không có cách sửa đổi nào thỏa mãn đề bài và ta có thể trả về một mảng rỗng.

- Nếu $d = target$, điều đó có nghĩa là tồn tại một đường đi ngắn nhất chỉ gồm các cạnh có trọng số dương và độ dài của nó bằng $target$. Khi đó, ta có thể sửa đổi tất cả các cạnh có trọng số bằng $-1$ thành giá trị lớn nhất $2 \times 10^9$.

- Nếu $d > target$, ta có thể thử thêm một cạnh có trọng số bằng $-1$ vào đồ thị, đặt trọng số của cạnh đó bằng $1$, rồi lại dùng thuật toán Dijkstra để tìm khoảng cách ngắn nhất $d$ từ $source$ đến $destination$.
    - Nếu khoảng cách ngắn nhất $d \leq target$, điều đó có nghĩa là sau khi thêm cạnh này, đường đi ngắn nhất đã được rút ngắn, và đường đi ngắn nhất chắc chắn đi qua cạnh này. Khi đó, ta chỉ cần đổi trọng số của cạnh này thành $target-d+1$ để khoảng cách ngắn nhất bằng $target$. Sau đó, ta có thể sửa đổi các cạnh còn lại có trọng số bằng $-1$ thành giá trị lớn nhất $2 \times 10^9$.
    - Nếu khoảng cách ngắn nhất $d > target$, điều đó có nghĩa là sau khi thêm cạnh này, đường đi ngắn nhất không được rút ngắn. Khi đó, ta giữ nguyên trọng số của cạnh này là $-1$ và tiếp tục thử thêm các cạnh còn lại có trọng số bằng $-1$.

Độ phức tạp thời gian là $O(n^3)$, và độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số node trong đồ thị.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def modifiedGraphEdges(
        self, n: int, edges: List[List[int]], source: int, destination: int, target: int
    ) -> List[List[int]]:
        def dijkstra(edges: List[List[int]]) -> int:
            g = [[inf] * n for _ in range(n)]
            for a, b, w in edges:
                if w == -1:
                    continue
                g[a][b] = g[b][a] = w
            dist = [inf] * n
            dist[source] = 0
            vis = [False] * n
            for _ in range(n):
                k = -1
                for j in range(n):
                    if not vis[j] and (k == -1 or dist[k] > dist[j]):
                        k = j
                vis[k] = True
                for j in range(n):
                    dist[j] = min(dist[j], dist[k] + g[k][j])
            return dist[destination]

        inf = 2 * 10**9
        d = dijkstra(edges)
        if d < target:
            return []
        ok = d == target
        for e in edges:
            if e[2] > 0:
                continue
            if ok:
                e[2] = inf
                continue
            e[2] = 1
            d = dijkstra(edges)
            if d <= target:
                ok = True
                e[2] += target - d
        return edges if ok else []
```

#### Java

```java
class Solution {
    private final int inf = 2000000000;

    public int[][] modifiedGraphEdges(
        int n, int[][] edges, int source, int destination, int target) {
        long d = dijkstra(edges, n, source, destination);
        if (d < target) {
            return new int[0][];
        }
        boolean ok = d == target;
        for (var e : edges) {
            if (e[2] > 0) {
                continue;
            }
            if (ok) {
                e[2] = inf;
                continue;
            }
            e[2] = 1;
            d = dijkstra(edges, n, source, destination);
            if (d <= target) {
                ok = true;
                e[2] += target - d;
            }
        }
        return ok ? edges : new int[0][];
    }

    private long dijkstra(int[][] edges, int n, int src, int dest) {
        int[][] g = new int[n][n];
        long[] dist = new long[n];
        Arrays.fill(dist, inf);
        dist[src] = 0;
        for (var f : g) {
            Arrays.fill(f, inf);
        }
        for (var e : edges) {
            int a = e[0], b = e[1], w = e[2];
            if (w == -1) {
                continue;
            }
            g[a][b] = w;
            g[b][a] = w;
        }
        boolean[] vis = new boolean[n];
        for (int i = 0; i < n; ++i) {
            int k = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (k == -1 || dist[k] > dist[j])) {
                    k = j;
                }
            }
            vis[k] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[k] + g[k][j]);
            }
        }
        return dist[dest];
    }
}
```

#### C++

```cpp
using ll = long long;
const int inf = 2e9;

class Solution {
public:
    vector<vector<int>> modifiedGraphEdges(int n, vector<vector<int>>& edges, int source, int destination, int target) {
        ll d = dijkstra(edges, n, source, destination);
        if (d < target) {
            return {};
        }
        bool ok = d == target;
        for (auto& e : edges) {
            if (e[2] > 0) {
                continue;
            }
            if (ok) {
                e[2] = inf;
                continue;
            }
            e[2] = 1;
            d = dijkstra(edges, n, source, destination);
            if (d <= target) {
                ok = true;
                e[2] += target - d;
            }
        }
        return ok ? edges : vector<vector<int>>{};
    }

    ll dijkstra(vector<vector<int>>& edges, int n, int src, int dest) {
        ll g[n][n];
        ll dist[n];
        bool vis[n];
        for (int i = 0; i < n; ++i) {
            fill(g[i], g[i] + n, inf);
            dist[i] = inf;
            vis[i] = false;
        }
        dist[src] = 0;
        for (auto& e : edges) {
            int a = e[0], b = e[1], w = e[2];
            if (w == -1) {
                continue;
            }
            g[a][b] = w;
            g[b][a] = w;
        }
        for (int i = 0; i < n; ++i) {
            int k = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (k == -1 || dist[j] < dist[k])) {
                    k = j;
                }
            }
            vis[k] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = min(dist[j], dist[k] + g[k][j]);
            }
        }
        return dist[dest];
    }
};
```

#### Go

```go
func modifiedGraphEdges(n int, edges [][]int, source int, destination int, target int) [][]int {
	const inf int = 2e9
	dijkstra := func(edges [][]int) int {
		g := make([][]int, n)
		dist := make([]int, n)
		vis := make([]bool, n)
		for i := range g {
			g[i] = make([]int, n)
			for j := range g[i] {
				g[i][j] = inf
			}
			dist[i] = inf
		}
		dist[source] = 0
		for _, e := range edges {
			a, b, w := e[0], e[1], e[2]
			if w == -1 {
				continue
			}
			g[a][b], g[b][a] = w, w
		}
		for i := 0; i < n; i++ {
			k := -1
			for j := 0; j < n; j++ {
				if !vis[j] && (k == -1 || dist[j] < dist[k]) {
					k = j
				}
			}
			vis[k] = true
			for j := 0; j < n; j++ {
				dist[j] = min(dist[j], dist[k]+g[k][j])
			}
		}
		return dist[destination]
	}
	d := dijkstra(edges)
	if d < target {
		return [][]int{}
	}
	ok := d == target
	for _, e := range edges {
		if e[2] > 0 {
			continue
		}
		if ok {
			e[2] = inf
			continue
		}
		e[2] = 1
		d := dijkstra(edges)
		if d <= target {
			ok = true
			e[2] += target - d
		}
	}
	if ok {
		return edges
	}
	return [][]int{}
}
```

#### TypeScript

```ts
function modifiedGraphEdges(
    n: number,
    edges: number[][],
    source: number,
    destination: number,
    target: number,
): number[][] {
    const inf = 2e9;
    const dijkstra = (edges: number[][]): number => {
        const g: number[][] = Array(n)
            .fill(0)
            .map(() => Array(n).fill(inf));
        const dist: number[] = Array(n).fill(inf);
        const vis: boolean[] = Array(n).fill(false);
        for (const [a, b, w] of edges) {
            if (w === -1) {
                continue;
            }
            g[a][b] = w;
            g[b][a] = w;
        }
        dist[source] = 0;
        for (let i = 0; i < n; ++i) {
            let k = -1;
            for (let j = 0; j < n; ++j) {
                if (!vis[j] && (k === -1 || dist[j] < dist[k])) {
                    k = j;
                }
            }
            vis[k] = true;
            for (let j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[k] + g[k][j]);
            }
        }
        return dist[destination];
    };
    let d = dijkstra(edges);
    if (d < target) {
        return [];
    }
    let ok = d === target;
    for (const e of edges) {
        if (e[2] > 0) {
            continue;
        }
        if (ok) {
            e[2] = inf;
            continue;
        }
        e[2] = 1;
        d = dijkstra(edges);
        if (d <= target) {
            ok = true;
            e[2] += target - d;
        }
    }
    return ok ? edges : [];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
