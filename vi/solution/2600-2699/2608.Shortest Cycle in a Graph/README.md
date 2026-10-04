---
comments: true
difficulty: Hard
rating: 1904
source: Biweekly Contest 101 Q4
tags:
    - Breadth-First Search
    - Graph
---

<!-- problem:start -->

# [2608. Shortest Cycle in a Graph](https://leetcode.com/problems/shortest-cycle-in-a-graph)

[中文文档](/solution/2600-2699/2608.Shortest%20Cycle%20in%20a%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị <strong>vô hướng</strong> gồm <code>n</code> đỉnh, trong đó mỗi đỉnh được gán nhãn từ <code>0</code> đến <code>n - 1</code>. Các cạnh trong đồ thị được biểu diễn bằng một mảng số nguyên 2D <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh nối đỉnh <code>u<sub>i</sub></code> và đỉnh <code>v<sub>i</sub></code>. Mỗi cặp đỉnh có nhiều nhất một cạnh và không đỉnh nào có cạnh nối với chính nó.</p>

<p>Hãy trả về <em>độ dài của <strong>chu trình ngắn nhất </strong>trong đồ thị</em>. Nếu không tồn tại chu trình, trả về <code>-1</code>.</p>

<p>Một chu trình là một đường đi bắt đầu và kết thúc tại cùng một nút, trong đó mỗi cạnh trên đường đi chỉ được sử dụng một lần.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2608.Shortest%20Cycle%20in%20a%20Graph/images/cropped.png" style="width: 387px; height: 331px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[0,1],[1,2],[2,0],[3,4],[4,5],[5,6],[6,3]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Chu trình có độ dài nhỏ nhất là: 0 -&gt; 1 -&gt; 2 -&gt; 0
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2600-2699/2608.Shortest%20Cycle%20in%20a%20Graph/images/croppedagin.png" style="width: 307px; height: 307px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1],[0,2]]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Đồ thị này không có chu trình.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= edges.length &lt;= 1000</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê cạnh + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Có thể tìm chu trình ngắn nhất bằng cách lần lượt xóa từng cạnh và tính đường đi ngắn nhất giữa hai đầu mút của cạnh đó. Với $n,m \le 1000$, BFS có độ phức tạp $O(m(n+m))$ là đủ nhanh.
>
> Mọi chu trình đều sử dụng một cạnh $(u,v)$ nào đó; sau khi xóa cạnh này, $\mathrm{dist}(u,v)+1$ là chu trình ngắn nhất đi qua cạnh đó. Ta lấy giá trị nhỏ nhất trên tất cả các cạnh, hoặc trả về không có chu trình nếu không có cặp đầu mút nào còn được nối với nhau.

<!-- thinking:end -->

Trước hết, chúng ta xây dựng danh sách kề $g$ của đồ thị dựa trên mảng $edges$, trong đó $g[u]$ biểu diễn tất cả các đỉnh kề với đỉnh $u$.

Sau đó, chúng ta liệt kê từng cạnh hai chiều $(u, v)$. Nếu vẫn tồn tại đường đi từ đỉnh $u$ đến đỉnh $v$ sau khi xóa cạnh này, thì độ dài chu trình ngắn nhất chứa cạnh này là $dist[v] + 1$, trong đó $dist[v]$ biểu diễn độ dài đường đi ngắn nhất từ đỉnh $u$ đến đỉnh $v$. Ta lấy giá trị nhỏ nhất trong tất cả các chu trình này.

Độ phức tạp thời gian là $O(m^2)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của mảng $edges$ và số lượng đỉnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findShortestCycle(self, n: int, edges: List[List[int]]) -> int:
        def bfs(u: int, v: int) -> int:
            dist = [inf] * n
            dist[u] = 0
            q = deque([u])
            while q:
                i = q.popleft()
                for j in g[i]:
                    if (i, j) != (u, v) and (j, i) != (u, v) and dist[j] == inf:
                        dist[j] = dist[i] + 1
                        q.append(j)
            return dist[v] + 1

        g = defaultdict(set)
        for u, v in edges:
            g[u].add(v)
            g[v].add(u)
        ans = min(bfs(u, v) for u, v in edges)
        return ans if ans < inf else -1
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private final int inf = 1 << 30;

    public int findShortestCycle(int n, int[][] edges) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        int ans = inf;
        for (var e : edges) {
            int u = e[0], v = e[1];
            ans = Math.min(ans, bfs(u, v));
        }
        return ans < inf ? ans : -1;
    }

    private int bfs(int u, int v) {
        int[] dist = new int[g.length];
        Arrays.fill(dist, inf);
        dist[u] = 0;
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(u);
        while (!q.isEmpty()) {
            int i = q.poll();
            for (int j : g[i]) {
                if ((i == u && j == v) || (i == v && j == u) || dist[j] != inf) {
                    continue;
                }
                dist[j] = dist[i] + 1;
                q.offer(j);
            }
        }
        return dist[v] + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findShortestCycle(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        const int inf = 1 << 30;
        auto bfs = [&](int u, int v) -> int {
            int dist[n];
            fill(dist, dist + n, inf);
            dist[u] = 0;
            queue<int> q{{u}};
            while (!q.empty()) {
                int i = q.front();
                q.pop();
                for (int j : g[i]) {
                    if ((i == u && j == v) || (i == v && j == u) || dist[j] != inf) {
                        continue;
                    }
                    dist[j] = dist[i] + 1;
                    q.push(j);
                }
            }
            return dist[v] + 1;
        };
        int ans = inf;
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            ans = min(ans, bfs(u, v));
        }
        return ans < inf ? ans : -1;
    }
};
```

#### Go

```go
func findShortestCycle(n int, edges [][]int) int {
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	const inf = 1 << 30
	bfs := func(u, v int) int {
		dist := make([]int, n)
		for i := range dist {
			dist[i] = inf
		}
		dist[u] = 0
		q := []int{u}
		for len(q) > 0 {
			i := q[0]
			q = q[1:]
			for _, j := range g[i] {
				if (i == u && j == v) || (i == v && j == u) || dist[j] != inf {
					continue
				}
				dist[j] = dist[i] + 1
				q = append(q, j)
			}
		}
		return dist[v] + 1
	}
	ans := inf
	for _, e := range edges {
		u, v := e[0], e[1]
		ans = min(ans, bfs(u, v))
	}
	if ans < inf {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
function findShortestCycle(n: number, edges: number[][]): number {
    const g: number[][] = new Array(n).fill(0).map(() => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const inf = 1 << 30;
    let ans = inf;
    const bfs = (u: number, v: number) => {
        const dist: number[] = new Array(n).fill(inf);
        dist[u] = 0;
        const q: number[] = [u];
        while (q.length) {
            const i = q.shift()!;
            for (const j of g[i]) {
                if ((i == u && j == v) || (i == v && j == u) || dist[j] != inf) {
                    continue;
                }
                dist[j] = dist[i] + 1;
                q.push(j);
            }
        }
        return 1 + dist[v];
    };
    for (const [u, v] of edges) {
        ans = Math.min(ans, bfs(u, v));
    }
    return ans < inf ? ans : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Liệt kê đỉnh + BFS

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 chạy BFS một lần cho mỗi cạnh. Chỉ cần bắt đầu BFS từ mỗi đỉnh: một lân cận đã được thăm trước đó nhưng không phải đỉnh cha sẽ tạo thành một chu trình có độ dài $\mathrm{dist}(u)+\mathrm{dist}(v)+1$.
>
> Khi đồ thị dày hơn, cách này thực hiện ít lần tìm kiếm hơn, với độ phức tạp $O(n(n+m))$, đồng thời không cần xóa cạnh.

<!-- thinking:end -->

Tương tự Lời giải 1, trước hết chúng ta xây dựng danh sách kề $g$ của đồ thị dựa trên mảng $edges$, trong đó $g[u]$ biểu diễn tất cả các đỉnh kề với đỉnh $u$.

Sau đó, chúng ta liệt kê đỉnh $u$. Nếu có hai đường đi từ đỉnh $u$ đến đỉnh $v$, thì ta tìm được một chu trình; độ dài của chu trình là tổng độ dài của hai đường đi đó. Ta lấy giá trị nhỏ nhất trong tất cả các chu trình này.

Độ phức tạp thời gian là $O(m \times n)$ và độ phức tạp không gian là $O(m + n)$, trong đó $m$ và $n$ lần lượt là độ dài của mảng $edges$ và số lượng đỉnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findShortestCycle(self, n: int, edges: List[List[int]]) -> int:
        def bfs(u: int) -> int:
            dist = [-1] * n
            dist[u] = 0
            q = deque([(u, -1)])
            ans = inf
            while q:
                u, fa = q.popleft()
                for v in g[u]:
                    if dist[v] < 0:
                        dist[v] = dist[u] + 1
                        q.append((v, u))
                    elif v != fa:
                        ans = min(ans, dist[u] + dist[v] + 1)
            return ans

        g = defaultdict(list)
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        ans = min(bfs(i) for i in range(n))
        return ans if ans < inf else -1
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private final int inf = 1 << 30;

    public int findShortestCycle(int n, int[][] edges) {
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        int ans = inf;
        for (int i = 0; i < n; ++i) {
            ans = Math.min(ans, bfs(i));
        }
        return ans < inf ? ans : -1;
    }

    private int bfs(int u) {
        int[] dist = new int[g.length];
        Arrays.fill(dist, -1);
        dist[u] = 0;
        Deque<int[]> q = new ArrayDeque<>();
        q.offer(new int[] {u, -1});
        int ans = inf;
        while (!q.isEmpty()) {
            var p = q.poll();
            u = p[0];
            int fa = p[1];
            for (int v : g[u]) {
                if (dist[v] < 0) {
                    dist[v] = dist[u] + 1;
                    q.offer(new int[] {v, u});
                } else if (v != fa) {
                    ans = Math.min(ans, dist[u] + dist[v] + 1);
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findShortestCycle(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        for (auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        const int inf = 1 << 30;
        auto bfs = [&](int u) -> int {
            int dist[n];
            memset(dist, -1, sizeof(dist));
            dist[u] = 0;
            queue<pair<int, int>> q;
            q.emplace(u, -1);
            int ans = inf;
            while (!q.empty()) {
                auto p = q.front();
                u = p.first;
                int fa = p.second;
                q.pop();
                for (int v : g[u]) {
                    if (dist[v] < 0) {
                        dist[v] = dist[u] + 1;
                        q.emplace(v, u);
                    } else if (v != fa) {
                        ans = min(ans, dist[u] + dist[v] + 1);
                    }
                }
            }
            return ans;
        };
        int ans = inf;
        for (int i = 0; i < n; ++i) {
            ans = min(ans, bfs(i));
        }
        return ans < inf ? ans : -1;
    }
};
```

#### Go

```go
func findShortestCycle(n int, edges [][]int) int {
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	const inf = 1 << 30
	bfs := func(u int) int {
		dist := make([]int, n)
		for i := range dist {
			dist[i] = -1
		}
		dist[u] = 0
		q := [][2]int{{u, -1}}
		ans := inf
		for len(q) > 0 {
			p := q[0]
			u = p[0]
			fa := p[1]
			q = q[1:]
			for _, v := range g[u] {
				if dist[v] < 0 {
					dist[v] = dist[u] + 1
					q = append(q, [2]int{v, u})
				} else if v != fa {
					ans = min(ans, dist[u]+dist[v]+1)
				}
			}
		}
		return ans
	}
	ans := inf
	for i := 0; i < n; i++ {
		ans = min(ans, bfs(i))
	}
	if ans < inf {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
function findShortestCycle(n: number, edges: number[][]): number {
    const g: number[][] = new Array(n).fill(0).map(() => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const inf = 1 << 30;
    let ans = inf;
    const bfs = (u: number) => {
        const dist: number[] = new Array(n).fill(-1);
        dist[u] = 0;
        const q: number[][] = [[u, -1]];
        let ans = inf;
        while (q.length) {
            const p = q.shift()!;
            u = p[0];
            const fa = p[1];
            for (const v of g[u]) {
                if (dist[v] < 0) {
                    dist[v] = dist[u] + 1;
                    q.push([v, u]);
                } else if (v !== fa) {
                    ans = Math.min(ans, dist[u] + dist[v] + 1);
                }
            }
        }
        return ans;
    };
    for (let i = 0; i < n; ++i) {
        ans = Math.min(ans, bfs(i));
    }
    return ans < inf ? ans : -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
