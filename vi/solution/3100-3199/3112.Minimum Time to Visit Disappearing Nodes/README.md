---
comments: true
difficulty: Medium
rating: 1756
source: Biweekly Contest 128 Q3
tags:
    - Graph
    - Array
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3112. Minimum Time to Visit Disappearing Nodes](https://leetcode.com/problems/minimum-time-to-visit-disappearing-nodes)

[中文文档](/solution/3100-3199/3112.Minimum%20Time%20to%20Visit%20Disappearing%20Nodes/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị vô hướng gồm <code>n</code> node. Bạn được cung cấp một mảng 2 chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, length<sub>i</sub>]</code> mô tả một cạnh nối node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code>, với thời gian đi qua là <code>length<sub>i</sub></code> đơn vị.</p>

<p>Ngoài ra, bạn được cung cấp một mảng <code>disappear</code>, trong đó <code>disappear[i]</code> là thời điểm node <code>i</code> biến mất khỏi đồ thị và bạn không thể đi đến node đó nữa.</p>

<p><strong>Lưu ý</strong>&nbsp;rằng đồ thị có thể <em>không liên thông</em> và có thể chứa <em>nhiều cạnh trùng nhau</em>.</p>

<p>Hãy trả về mảng <code>answer</code>, trong đó <code>answer[i]</code> là số đơn vị thời gian <strong>ít nhất</strong> cần để đi từ node 0 đến node <code>i</code>. Nếu node <code>i</code> <strong>không thể đến được</strong> từ node 0 thì <code>answer[i]</code> là <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,2],[1,2,1],[0,2,4]], disappear = [1,1,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,-1,4]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3112.Minimum%20Time%20to%20Visit%20Disappearing%20Nodes/images/output-onlinepngtools.png" style="width: 350px; height: 210px;" /></p>

<p>Chúng ta bắt đầu hành trình từ node 0 và cần tìm thời gian nhỏ nhất để đi đến từng node trước khi node đó biến mất.</p>

<ul>
	<li>Với node 0, chúng ta không cần tốn thời gian vì đây là node bắt đầu.</li>
	<li>Để đi đến node 1, chúng ta cần ít nhất 2 đơn vị thời gian để đi qua <code>edges[0]</code>. Tuy nhiên, node này biến mất đúng thời điểm đó nên chúng ta không thể đi đến nó.</li>
	<li>Để đi đến node 2, chúng ta cần ít nhất 4 đơn vị thời gian để đi qua <code>edges[2]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,2],[1,2,1],[0,2,4]], disappear = [1,3,5]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,2,3]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3112.Minimum%20Time%20to%20Visit%20Disappearing%20Nodes/images/output-onlinepngtools-1.png" style="width: 350px; height: 210px;" /></p>

<p>Chúng ta bắt đầu hành trình từ node 0 và cần tìm thời gian nhỏ nhất để đi đến từng node trước khi node đó biến mất.</p>

<ul>
	<li>Với node 0, chúng ta không cần tốn thời gian vì đây là node bắt đầu.</li>
	<li>Để đi đến node 1, chúng ta cần ít nhất 2 đơn vị thời gian để đi qua <code>edges[0]</code>.</li>
	<li>Để đi đến node 2, chúng ta cần ít nhất 3 đơn vị thời gian để đi qua <code>edges[0]</code> và <code>edges[1]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1,1]], disappear = [1,1]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Node 1 biến mất đúng lúc chúng ta đi đến node đó.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, length<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= length<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>disappear.length == n</code></li>
	<li><code>1 &lt;= disappear[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dijkstra tối ưu bằng Heap

<!-- thinking:start -->

> **Tư duy**
>
> Các node biến mất tại những thời điểm đã cho, vì vậy phải đến node trước thời điểm đó. Nếu tìm riêng từ node nguồn đến từng node, chúng ta sẽ lặp lại các bước nới lỏng giống nhau.
>
> Bài toán vẫn là tìm đường đi ngắn nhất từ một nguồn với trọng số không âm, nhưng có thêm ràng buộc $dist[u]+w < \textit{disappear}[v]$ khi nới lỏng $v$.
>
> Hãy xây dựng đồ thị vô hướng và chạy Dijkstra bằng heap từ node $0$. Giữ lại $dist[i]$ nếu nó nhỏ hơn $\textit{disappear}[i]$, nếu không thì trả về $-1$.

<!-- thinking:end -->

Đầu tiên, chúng ta tạo một danh sách kề $\textit{g}$ để lưu các cạnh của đồ thị. Sau đó, chúng ta tạo một mảng $\textit{dist}$ để lưu khoảng cách ngắn nhất từ node $0$ đến các node khác. Khởi tạo $\textit{dist}[0] = 0$, còn khoảng cách đến các node khác được khởi tạo bằng vô cùng.

Tiếp theo, chúng ta sử dụng thuật toán Dijkstra để tính khoảng cách ngắn nhất từ node $0$ đến các node khác. Các bước cụ thể như sau:

1. Tạo một hàng đợi ưu tiên $\textit{pq}$ để lưu khoảng cách và số hiệu node. Ban đầu, thêm node $0$ với khoảng cách $0$ vào hàng đợi.
2. Lấy một node $u$ ra khỏi hàng đợi. Nếu khoảng cách $du$ của $u$ lớn hơn $\textit{dist}[u]$, điều đó có nghĩa là $u$ đã được cập nhật, nên chúng ta bỏ qua node này.
3. Duyệt qua tất cả node kề $v$ của node $u$. Nếu $\textit{dist}[v] > \textit{dist}[u] + w$ và $\textit{dist}[u] + w < \textit{disappear}[v]$, cập nhật $\textit{dist}[v] = \textit{dist}[u] + w$ và thêm node $v$ vào hàng đợi.
4. Lặp lại bước 2 và 3 cho đến khi hàng đợi rỗng.

Cuối cùng, chúng ta duyệt qua mảng $\textit{dist}$. Nếu $\textit{dist}[i] < \textit{disappear}[i]$ thì $\textit{answer}[i] = \textit{dist}[i]$; ngược lại, $\textit{answer}[i] = -1$.

Độ phức tạp thời gian là $O(m \times \log m)$ và độ phức tạp không gian là $O(m)$. Trong đó, $m$ là số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumTime(
        self, n: int, edges: List[List[int]], disappear: List[int]
    ) -> List[int]:
        g = defaultdict(list)
        for u, v, w in edges:
            g[u].append((v, w))
            g[v].append((u, w))
        dist = [inf] * n
        dist[0] = 0
        pq = [(0, 0)]
        while pq:
            du, u = heappop(pq)
            if du > dist[u]:
                continue
            for v, w in g[u]:
                if dist[v] > dist[u] + w and dist[u] + w < disappear[v]:
                    dist[v] = dist[u] + w
                    heappush(pq, (dist[v], v))
        return [a if a < b else -1 for a, b in zip(dist, disappear)]
```

#### Java

```java
class Solution {
    public int[] minimumTime(int n, int[][] edges, int[] disappear) {
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].add(new int[] {v, w});
            g[v].add(new int[] {u, w});
        }
        int[] dist = new int[n];
        Arrays.fill(dist, 1 << 30);
        dist[0] = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, 0});
        while (!pq.isEmpty()) {
            var e = pq.poll();
            int du = e[0], u = e[1];
            if (du > dist[u]) {
                continue;
            }
            for (var nxt : g[u]) {
                int v = nxt[0], w = nxt[1];
                if (dist[v] > dist[u] + w && dist[u] + w < disappear[v]) {
                    dist[v] = dist[u] + w;
                    pq.offer(new int[] {dist[v], v});
                }
            }
        }
        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = dist[i] < disappear[i] ? dist[i] : -1;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> minimumTime(int n, vector<vector<int>>& edges, vector<int>& disappear) {
        vector<vector<pair<int, int>>> g(n);
        for (const auto& e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].push_back({v, w});
            g[v].push_back({u, w});
        }

        vector<int> dist(n, 1 << 30);
        dist[0] = 0;

        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        pq.push({0, 0});

        while (!pq.empty()) {
            auto [du, u] = pq.top();
            pq.pop();

            if (du > dist[u]) {
                continue;
            }

            for (auto [v, w] : g[u]) {
                if (dist[v] > dist[u] + w && dist[u] + w < disappear[v]) {
                    dist[v] = dist[u] + w;
                    pq.push({dist[v], v});
                }
            }
        }

        vector<int> ans(n);
        for (int i = 0; i < n; ++i) {
            ans[i] = dist[i] < disappear[i] ? dist[i] : -1;
        }

        return ans;
    }
};
```

#### Go

```go
func minimumTime(n int, edges [][]int, disappear []int) []int {
	g := make([][]pair, n)
	for _, e := range edges {
		u, v, w := e[0], e[1], e[2]
		g[u] = append(g[u], pair{v, w})
		g[v] = append(g[v], pair{u, w})
	}

	dist := make([]int, n)
	for i := range dist {
		dist[i] = 1 << 30
	}
	dist[0] = 0

	pq := hp{{0, 0}}

	for len(pq) > 0 {
		du, u := pq[0].dis, pq[0].u
		heap.Pop(&pq)

		if du > dist[u] {
			continue
		}

		for _, nxt := range g[u] {
			v, w := nxt.dis, nxt.u
			if dist[v] > dist[u]+w && dist[u]+w < disappear[v] {
				dist[v] = dist[u] + w
				heap.Push(&pq, pair{dist[v], v})
			}
		}
	}

	ans := make([]int, n)
	for i := 0; i < n; i++ {
		if dist[i] < disappear[i] {
			ans[i] = dist[i]
		} else {
			ans[i] = -1
		}
	}

	return ans
}

type pair struct{ dis, u int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].dis < h[j].dis }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

#### TypeScript

```ts
function minimumTime(n: number, edges: number[][], disappear: number[]): number[] {
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (const [u, v, w] of edges) {
        g[u].push([v, w]);
        g[v].push([u, w]);
    }
    const dist = Array.from({ length: n }, () => Infinity);
    dist[0] = 0;
    const pq = new PriorityQueue({
        compare: (a, b) => (a[0] === b[0] ? a[1] - b[1] : a[0] - b[0]),
    });
    pq.enqueue([0, 0]);
    while (pq.size() > 0) {
        const [du, u] = pq.dequeue()!;
        if (du > dist[u]) {
            continue;
        }
        for (const [v, w] of g[u]) {
            if (dist[v] > dist[u] + w && dist[u] + w < disappear[v]) {
                dist[v] = dist[u] + w;
                pq.enqueue([dist[v], v]);
            }
        }
    }
    return dist.map((a, i) => (a < disappear[i] ? a : -1));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
