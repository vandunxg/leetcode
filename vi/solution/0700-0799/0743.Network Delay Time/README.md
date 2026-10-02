---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Shortest Path
    - Dijkstra
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [743. Network Delay Time](https://leetcode.com/problems/network-delay-time)

[中文文档](/solution/0700-0799/0743.Network%20Delay%20Time/README.md)

## Mô tả

<!-- description:start -->

<p>Cho mạng gồm <code>n</code> node, được đánh nhãn từ <code>1</code> đến <code>n</code>. Bạn cũng được cho danh sách <code>times</code> gồm các cạnh có hướng và thời gian di chuyển <code>times[i] = (u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>)</code>, trong đó <code>u<sub>i</sub></code> là node nguồn, <code>v<sub>i</sub></code> là node đích, còn <code>w<sub>i</sub></code> là thời gian để tín hiệu truyền từ nguồn đến đích.</p>

<p>Ta gửi tín hiệu từ node <code>k</code>. Hãy trả về <em>thời gian <strong>ít nhất</strong> để toàn bộ <code>n</code> node nhận được tín hiệu</em>. Nếu không thể truyền tín hiệu đến tất cả <code>n</code> node, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0743.Network%20Delay%20Time/images/931_example_1.png" style="width: 217px; height: 239px;" />
<pre>
<strong>Đầu vào:</strong> times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
<strong>Đầu ra:</strong> 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> times = [[1,2,1]], n = 2, k = 1
<strong>Đầu ra:</strong> 1
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> times = [[1,2,1]], n = 2, k = 2
<strong>Đầu ra:</strong> -1
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= k &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= times.length &lt;= 6000</code></li>
	<li><code>times[i].length == 3</code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>0 &lt;= w<sub>i</sub> &lt;= 100</code></li>
	<li>Mọi cặp <code>(u<sub>i</sub>, v<sub>i</sub>)</code> đều <strong>duy nhất</strong> (tức không có cạnh trùng).</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra cơ bản

<!-- thinking:start -->

> **Tư duy**
>
> Thời gian để tín hiệu từ $k$ đến từng node bằng khoảng cách đường đi ngắn nhất lớn nhất. Với $n\le 100$, có thể dùng Dijkstra cơ bản với độ phức tạp $O(n^2)$.
>
> Liên tục chọn node chưa dùng có khoảng cách tạm thời nhỏ nhất và relax các cạnh đi ra từ nó. Dùng adjacency matrix để lưu trọng số; cạnh không tồn tại có giá trị $+\infty$.
>
> Đáp án là $\max(\textit{dist})$, hoặc $-1$ nếu giá trị đó vẫn là vô cùng.

<!-- thinking:end -->

Định nghĩa $\textit{g}[u][v]$ là trọng số cạnh từ node $u$ đến node $v$. Nếu không có cạnh giữa node $u$ và node $v$, đặt $\textit{g}[u][v] = +\infty$.

Duy trì mảng $\textit{dist}$, trong đó $\textit{dist}[i]$ là độ dài đường đi ngắn nhất từ node $k$ đến node $i$. Ban đầu, đặt mọi $\textit{dist}[i]$ bằng $+\infty$, ngoại trừ $\textit{dist}[k - 1] = 0$. Định nghĩa mảng $\textit{vis}$, trong đó $\textit{vis}[i]$ cho biết node $i$ đã được thăm hay chưa. Ban đầu, mọi $\textit{vis}[i]$ là $\text{false}$.

Mỗi lượt, tìm node chưa thăm $t$ có khoảng cách nhỏ nhất rồi relax các cạnh xuất phát từ $t$. Với mỗi node $j$, nếu $\textit{dist}[j] > \textit{dist}[t] + \textit{g}[t][j]$, cập nhật $\textit{dist}[j] = \textit{dist}[t] + \textit{g}[t][j]$.

Cuối cùng, trả về giá trị lớn nhất trong $\textit{dist}$. Nếu đáp án là $+\infty$, nghĩa là có node không thể tới được, khi đó trả về $-1$.

Độ phức tạp thời gian là $O(n^2 + m)$ và độ phức tạp không gian là $O(n^2)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        g = [[inf] * n for _ in range(n)]
        for u, v, w in times:
            g[u - 1][v - 1] = w
        dist = [inf] * n
        dist[k - 1] = 0
        vis = [False] * n
        for _ in range(n):
            t = -1
            for j in range(n):
                if not vis[j] and (t == -1 or dist[t] > dist[j]):
                    t = j
            vis[t] = True
            for j in range(n):
                dist[j] = min(dist[j], dist[t] + g[t][j])
        ans = max(dist)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        int[][] g = new int[n][n];
        int[] dist = new int[n];
        final int inf = 1 << 29;
        Arrays.fill(dist, inf);
        for (var e : g) {
            Arrays.fill(e, inf);
        }
        for (var e : times) {
            g[e[0] - 1][e[1] - 1] = e[2];
        }
        dist[k - 1] = 0;
        boolean[] vis = new boolean[n];
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
        int ans = 0;
        for (int x : dist) {
            ans = Math.max(ans, x);
        }
        return ans == inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        const int inf = 1 << 29;
        vector<vector<int>> g(n, vector<int>(n, inf));
        for (const auto& e : times) {
            g[e[0] - 1][e[1] - 1] = e[2];
        }
        vector<int> dist(n, inf);
        dist[k - 1] = 0;
        vector<bool> vis(n);
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
        int ans = ranges::max(dist);
        return ans == inf ? -1 : ans;
    }
};
```

#### Go

```go
func networkDelayTime(times [][]int, n int, k int) int {
	const inf = 1 << 29
	g := make([][]int, n)
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = inf
		}
	}
	for _, e := range times {
		g[e[0]-1][e[1]-1] = e[2]
	}

	dist := make([]int, n)
	for i := range dist {
		dist[i] = inf
	}
	dist[k-1] = 0

	vis := make([]bool, n)
	for i := 0; i < n; i++ {
		t := -1
		for j := 0; j < n; j++ {
			if !vis[j] && (t == -1 || dist[t] > dist[j]) {
				t = j
			}
		}
		vis[t] = true
		for j := 0; j < n; j++ {
			dist[j] = min(dist[j], dist[t]+g[t][j])
		}
	}

	if ans := slices.Max(dist); ans != inf {
		return ans
	}
	return -1
}
```

#### TypeScript

```ts
function networkDelayTime(times: number[][], n: number, k: number): number {
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(Infinity));
    for (const [u, v, w] of times) {
        g[u - 1][v - 1] = w;
    }
    const dist: number[] = Array(n).fill(Infinity);
    dist[k - 1] = 0;
    const vis: boolean[] = Array(n).fill(false);
    for (let i = 0; i < n; ++i) {
        let t = -1;
        for (let j = 0; j < n; ++j) {
            if (!vis[j] && (t === -1 || dist[j] < dist[t])) {
                t = j;
            }
        }
        vis[t] = true;
        for (let j = 0; j < n; ++j) {
            dist[j] = Math.min(dist[j], dist[t] + g[t][j]);
        }
    }
    const ans = Math.max(...dist);
    return ans === Infinity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Thuật toán Dijkstra tối ưu bằng heap

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 quét mọi node để chọn node tiếp theo; cách này tốn kém với đồ thị thưa. Heap lấy node tốt nhất hiện tại trong $O(m\log m)$.
>
> Lưu adjacency list. Lấy $(d,u)$ khỏi heap; bỏ qua nếu khoảng cách đã cũ, nếu không thì relax các node kề và thêm chúng vào heap. Đáp án vẫn là khoảng cách hữu hạn lớn nhất.

<!-- thinking:end -->

Có thể dùng priority queue (heap) để tối ưu thuật toán Dijkstra cơ bản.

Định nghĩa $\textit{g}[u]$ là danh sách tất cả cạnh kề với node $u$, còn $\textit{dist}[u]$ là độ dài đường đi ngắn nhất từ node $k$ đến node $u$. Ban đầu, đặt mọi $\textit{dist}[u]$ bằng $+\infty$, ngoại trừ $\textit{dist}[k - 1] = 0$.

We define a priority queue $\textit{pq}$, where each element is $(\textit{d}, u)$, representing the distance $\textit{d}$ from node $u$ to node $k$. Each time, we take out the node $(\textit{d}, u)$ with the smallest distance from $\textit{pq}$. If $\textit{d} > $\textit{dist}[u]$, we skip this node. Otherwise, we traverse all adjacent edges of node $u$. For each adjacent edge $(v, w)$, if $\textit{dist}[v] > \textit{dist}[u] + w$, we update $\textit{dist}[v] = \textit{dist}[u] + w$ and add $(\textit{dist}[v], v)$ to $\textit{pq}$.

Cuối cùng, trả về giá trị lớn nhất trong $\textit{dist}$. Nếu đáp án là $+\infty$, nghĩa là có node không thể tới được, khi đó trả về $-1$.

Độ phức tạp thời gian là $O(m \times \log m + n)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def networkDelayTime(self, times: List[List[int]], n: int, k: int) -> int:
        g = [[] for _ in range(n)]
        for u, v, w in times:
            g[u - 1].append((v - 1, w))
        dist = [inf] * n
        dist[k - 1] = 0
        pq = [(0, k - 1)]
        while pq:
            d, u = heappop(pq)
            if d > dist[u]:
                continue
            for v, w in g[u]:
                if (nd := d + w) < dist[v]:
                    dist[v] = nd
                    heappush(pq, (nd, v))
        ans = max(dist)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {
        final int inf = 1 << 29;
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : times) {
            g[e[0] - 1].add(new int[] {e[1] - 1, e[2]});
        }
        int[] dist = new int[n];
        Arrays.fill(dist, inf);
        dist[k - 1] = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.offer(new int[] {0, k - 1});
        while (!pq.isEmpty()) {
            var p = pq.poll();
            int d = p[0], u = p[1];
            if (d > dist[u]) {
                continue;
            }
            for (var e : g[u]) {
                int v = e[0], w = e[1];
                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                    pq.offer(new int[] {dist[v], v});
                }
            }
        }
        int ans = Arrays.stream(dist).max().getAsInt();
        return ans == inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int networkDelayTime(vector<vector<int>>& times, int n, int k) {
        const int inf = 1 << 29;
        using pii = pair<int, int>;
        vector<vector<pii>> g(n);
        for (auto& edge : times) {
            g[edge[0] - 1].emplace_back(edge[1] - 1, edge[2]);
        }

        vector<int> dist(n, inf);
        dist[k - 1] = 0;
        priority_queue<pii, vector<pii>, greater<>> pq;
        pq.emplace(0, k - 1);

        while (!pq.empty()) {
            auto [d, u] = pq.top();
            pq.pop();
            if (d > dist[u]) {
                continue;
            }
            for (auto& [v, w] : g[u]) {
                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                    pq.emplace(dist[v], v);
                }
            }
        }

        int ans = ranges::max(dist);
        return ans == inf ? -1 : ans;
    }
};
```

#### Go

```go
func networkDelayTime(times [][]int, n int, k int) int {
	g := make([][][2]int, n)
	for _, e := range times {
		u, v, w := e[0]-1, e[1]-1, e[2]
		g[u] = append(g[u], [2]int{v, w})
	}
	dist := make([]int, n)
	const inf int = 1 << 29
	for i := range dist {
		dist[i] = inf
	}
	dist[k-1] = 0
	pq := hp{{0, k - 1}}
	for len(pq) > 0 {
		p := heap.Pop(&pq).(pair)
		d, u := p.x, p.i
		if d > dist[u] {
			continue
		}
		for _, e := range g[u] {
			v, w := e[0], e[1]
			if nd := d + w; nd < dist[v] {
				dist[v] = nd
				heap.Push(&pq, pair{nd, v})
			}
		}
	}
	if ans := slices.Max(dist); ans < inf {
		return ans
	}
	return -1

}

type pair struct{ x, i int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].x < h[j].x }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(x any)        { *h = append(*h, x.(pair)) }
func (h *hp) Pop() (x any)      { a := *h; x = a[len(a)-1]; *h = a[:len(a)-1]; return }
```

#### TypeScript

```ts
function networkDelayTime(times: number[][], n: number, k: number): number {
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (const [u, v, w] of times) {
        g[u - 1].push([v - 1, w]);
    }
    const dist: number[] = Array(n).fill(Infinity);
    dist[k - 1] = 0;
    const pq = new PriorityQueue<number[]>((a, b) => a[0] - b[0]);
    pq.enqueue([0, k - 1]);
    while (!pq.isEmpty()) {
        const [d, u] = pq.dequeue();
        if (d > dist[u]) {
            continue;
        }
        for (const [v, w] of g[u]) {
            if (dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                pq.enqueue([dist[v], v]);
            }
        }
    }
    const ans = Math.max(...dist);
    return ans === Infinity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
