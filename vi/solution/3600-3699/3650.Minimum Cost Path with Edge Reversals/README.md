---
comments: true
difficulty: Medium
rating: 1853
source: Biweekly Contest 163 Q3
tags:
    - Graph
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3650. Minimum Cost Path with Edge Reversals](https://leetcode.com/problems/minimum-cost-path-with-edge-reversals)

[中文文档](/solution/3600-3699/3650.Minimum%20Cost%20Path%20with%20Edge%20Reversals/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một đồ thị có hướng, có trọng số với <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>, và một mảng <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu diễn một cạnh có hướng từ node <code>u<sub>i</sub></code> đến node <code>v<sub>i</sub></code> với cost <code>w<sub>i</sub></code>.</p>

<p>Mỗi node <code>u<sub>i</sub></code> có một switch chỉ có thể được sử dụng <strong>nhiều nhất một lần</strong>: khi đến <code>u<sub>i</sub></code> và chưa sử dụng switch của node này, bạn có thể kích hoạt nó trên một trong các cạnh đi vào <code>v<sub>i</sub> &rarr; u<sub>i</sub></code>, đảo ngược cạnh đó thành <code>u<sub>i</sub> &rarr; v<sub>i</sub></code> rồi <strong>ngay lập tức</strong> đi qua cạnh đó.</p>

<p>Việc đảo cạnh chỉ có hiệu lực cho lần di chuyển đơn lẻ đó, và đi qua một cạnh đã đảo có cost <code>2 * w<sub>i</sub></code>.</p>

<p>Trả về tổng cost <strong>nhỏ nhất</strong> để đi từ node 0 đến node <code>n - 1</code>. Nếu không thể đi được, trả về -1.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1,3],[3,1,1],[2,3,4],[0,2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">5</span></p>

<p><strong>Giải thích: </strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3650.Minimum%20Cost%20Path%20with%20Edge%20Reversals/images/e1drawio.png" style="width: 171px; height: 111px;" /></strong></p>

<ul>
	<li>Đi theo đường đi <code>0 &rarr; 1</code> (cost 3).</li>
	<li>Tại node 1, đảo cạnh ban đầu <code>3 &rarr; 1</code> thành <code>1 &rarr; 3</code> và đi qua nó với cost <code>2 * 1 = 2</code>.</li>
	<li>Tổng cost là <code>3 + 2 = 5</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,2,1],[2,1,1],[1,3,1],[2,3,3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Không cần đảo cạnh. Đi theo đường <code>0 &rarr; 2</code> (cost 1), sau đó <code>2 &rarr; 1</code> (cost 1), rồi <code>1 &rarr; 3</code> (cost 1).</li>
	<li>Tổng cost là <code>1 + 1 + 1 = 3</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 1000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra

<!-- thinking:start -->

> **Tư duy**
>
> Đi xuôi có cost $w$, còn đảo cạnh có cost $2w$. Ta cần tìm đường đi có cost nhỏ nhất từ $0$ đến $n-1$. Việc liệt kê các cạnh cần đảo có số trường hợp tăng theo cấp số mũ.
>
> Ta mô hình hóa chiều ngược như một cung bổ sung: giữ $(u,v,w)$ và thêm $(v,u,2w)$. Khi đó, bài toán trở thành bài toán tìm đường đi ngắn nhất thông thường với các trọng số không âm.
>
> Dijkstra dùng heap xuất phát từ $0$ trả về ngay khi $n-1$ được lấy ra; heap rỗng nghĩa là không thể đến đích. Mỗi cạnh ban đầu tạo ra hai cung có hướng, nên độ phức tạp là $m\log m$.

<!-- thinking:end -->

Theo mô tả bài toán, ta có thể xây dựng một đồ thị có hướng $g$, trong đó mỗi cạnh $(u, v)$ cho phép hai kiểu di chuyển:

- Đi theo chiều ban đầu với cost $w$, tương ứng với cạnh $(u, v)$.
- Đi theo chiều ngược với cost $2w$, tương ứng với cạnh $(v, u)$.

Sau đó, ta có thể dùng thuật toán Dijkstra trên đồ thị $g$ để tìm đường đi ngắn nhất từ node $0$ đến node $n-1$, chính là tổng cost nhỏ nhất cần thiết.

Cụ thể, ta định nghĩa một priority queue $pq$, trong đó mỗi phần tử là một tuple $(d, u)$, biểu diễn cost nhỏ nhất hiện tại để đi đến node $u$ là $d$. Ta cũng định nghĩa một mảng $\textit{dist}$, trong đó $\textit{dist}[u]$ biểu diễn cost nhỏ nhất từ node $0$ đến node $u$. Ban đầu, ta đặt $\textit{dist}[0] = 0$, cost để đến mọi node khác là vô cùng, rồi đưa $(0, 0)$ vào queue.

Ở mỗi lần lặp, ta lấy node $(d, u)$ có cost nhỏ nhất khỏi priority queue. Nếu $d$ lớn hơn $\textit{dist}[u]$, ta bỏ qua node này. Nếu không, ta duyệt tất cả neighbor $v$ của node $u$, tính cost mới $nd = d + w$ để đi đến node $v$ qua node $u$. Nếu $nd$ nhỏ hơn $\textit{dist}[v]$, ta cập nhật $\textit{dist}[v] = nd$ và đưa $(nd, v)$ vào queue.

Khi lấy node $n-1$ ra, $d$ hiện tại chính là tổng cost nhỏ nhất từ node $0$ đến node $n-1$. Nếu priority queue trở nên rỗng mà node $n-1$ vẫn chưa được lấy ra, điều đó có nghĩa node $n-1$ không thể đến được, nên ta trả về -1.

Độ phức tạp thời gian là $O(n + m \times \log m)$, và độ phức tạp không gian là $O(n + m)$. Ở đây, $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(self, n: int, edges: List[List[int]]) -> int:
        g = [[] for _ in range(n)]
        for u, v, w in edges:
            g[u].append((v, w))
            g[v].append((u, w * 2))
        pq = [(0, 0)]
        dist = [inf] * n
        dist[0] = 0
        while pq:
            d, u = heappop(pq)
            if d > dist[u]:
                continue
            if u == n - 1:
                return d
            for v, w in g[u]:
                nd = d + w
                if nd < dist[v]:
                    dist[v] = nd
                    heappush(pq, (nd, v))
        return -1
```

#### Java

```java
class Solution {
    public int minCost(int n, int[][] edges) {
        List<int[]>[] g = new ArrayList[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].add(new int[] {v, w});
            g[v].add(new int[] {u, w * 2});
        }

        final int inf = Integer.MAX_VALUE / 2;
        int[] dist = new int[n];
        Arrays.fill(dist, inf);
        dist[0] = 0;

        PriorityQueue<int[]> pq = new PriorityQueue<>(Comparator.comparingInt(a -> a[0]));
        pq.offer(new int[] {0, 0});

        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            int d = cur[0], u = cur[1];
            if (d > dist[u]) {
                continue;
            }
            if (u == n - 1) {
                return d;
            }
            for (int[] nei : g[u]) {
                int v = nei[0], w = nei[1];
                int nd = d + w;
                if (nd < dist[v]) {
                    dist[v] = nd;
                    pq.offer(new int[] {nd, v});
                }
            }
        }
        return -1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minCost(int n, vector<vector<int>>& edges) {
        using pii = pair<int, int>;
        vector<vector<pii>> g(n);
        for (auto& e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].push_back({v, w});
            g[v].push_back({u, w * 2});
        }

        const int inf = INT_MAX / 2;
        vector<int> dist(n, inf);
        dist[0] = 0;

        priority_queue<pii, vector<pii>, greater<pii>> pq;
        pq.push({0, 0});

        while (!pq.empty()) {
            auto [d, u] = pq.top();
            pq.pop();
            if (d > dist[u]) {
                continue;
            }
            if (u == n - 1) {
                return d;
            }

            for (auto& [v, w] : g[u]) {
                int nd = d + w;
                if (nd < dist[v]) {
                    dist[v] = nd;
                    pq.push({nd, v});
                }
            }
        }
        return -1;
    }
};
```

#### Go

```go
func minCost(n int, edges [][]int) int {
	g := make([][][2]int, n)
	for _, e := range edges {
		u, v, w := e[0], e[1], e[2]
		g[u] = append(g[u], [2]int{v, w})
		g[v] = append(g[v], [2]int{u, w * 2})
	}

	inf := math.MaxInt / 2
	dist := make([]int, n)
	for i := range dist {
		dist[i] = inf
	}
	dist[0] = 0

	pq := &hp{}
	heap.Init(pq)
	heap.Push(pq, pair{0, 0})

	for pq.Len() > 0 {
		cur := heap.Pop(pq).(pair)
		d, u := cur.x, cur.i
		if d > dist[u] {
			continue
		}
		if u == n-1 {
			return d
		}
		for _, ne := range g[u] {
			v, w := ne[0], ne[1]
			if nd := d + w; nd < dist[v] {
				dist[v] = nd
				heap.Push(pq, pair{nd, v})
			}
		}
	}
	return -1
}

type pair struct{ x, i int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].x < h[j].x }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(x any)        { *h = append(*h, x.(pair)) }
func (h *hp) Pop() (x any) {
	a := *h
	x = a[len(a)-1]
	*h = a[:len(a)-1]
	return
}
```

#### TypeScript

```ts
function minCost(n: number, edges: number[][]): number {
    const g: number[][][] = Array.from({ length: n }, () => []);
    for (const [u, v, w] of edges) {
        g[u].push([v, w]);
        g[v].push([u, w * 2]);
    }
    const dist: number[] = Array(n).fill(Infinity);
    dist[0] = 0;
    const pq = new PriorityQueue<number[]>((a, b) => a[0] - b[0]);
    pq.enqueue([0, 0]);
    while (!pq.isEmpty()) {
        const [d, u] = pq.dequeue();
        if (d > dist[u]) {
            continue;
        }
        if (u === n - 1) {
            return d;
        }
        for (const [v, w] of g[u]) {
            const nd = d + w;
            if (nd < dist[v]) {
                dist[v] = nd;
                pq.enqueue([nd, v]);
            }
        }
    }
    return -1;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
