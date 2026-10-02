---
comments: true
difficulty: Medium
rating: 1846
source: Weekly Contest 197 Q3
tags:
    - Graph
    - Array
    - Shortest Path
    - Dijkstra
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [1514. Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability)

[中文文档](/solution/1500-1599/1514.Path%20with%20Maximum%20Probability/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một đồ thị vô hướng có trọng số gồm&nbsp;<code>n</code>&nbsp;node (đánh chỉ số từ 0), được biểu diễn bởi danh sách cạnh, trong đó&nbsp;<code>edges[i] = [a, b]</code>&nbsp;là cạnh vô hướng nối node&nbsp;<code>a</code>&nbsp;và&nbsp;<code>b</code>&nbsp;với xác suất đi qua cạnh thành công là&nbsp;<code>succProb[i]</code>.</p>

<p>Với hai node&nbsp;<code>start</code>&nbsp;và&nbsp;<code>end</code>, hãy tìm đường đi từ&nbsp;<code>start</code>&nbsp;đến&nbsp;<code>end</code>&nbsp;có xác suất thành công lớn nhất và trả về xác suất đó.</p>

<p>Nếu không có đường đi từ&nbsp;<code>start</code>&nbsp;đến&nbsp;<code>end</code>, <strong>trả về&nbsp;0</strong>. Đáp án được chấp nhận nếu sai khác không quá <strong>1e-5</strong> so với đáp án đúng.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1514.Path%20with%20Maximum%20Probability/images/1558_ex1.png" style="width: 187px; height: 186px;" /></strong></p>

<pre>
<strong>Input:</strong> n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
<strong>Output:</strong> 0.25000
<strong>Explanation:</strong>&nbsp;Có hai đường đi từ start đến end, một đường có xác suất thành công = 0.2 và đường kia có xác suất 0.5 * 0.5 = 0.25.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1514.Path%20with%20Maximum%20Probability/images/1558_ex2.png" style="width: 189px; height: 186px;" /></strong></p>

<pre>
<strong>Input:</strong> n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.3], start = 0, end = 2
<strong>Output:</strong> 0.30000
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><strong><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1500-1599/1514.Path%20with%20Maximum%20Probability/images/1558_ex3.png" style="width: 215px; height: 191px;" /></strong></p>

<pre>
<strong>Input:</strong> n = 3, edges = [[0,1]], succProb = [0.5], start = 0, end = 2
<strong>Output:</strong> 0.00000
<strong>Explanation:</strong>&nbsp;Không có đường đi giữa 0 và 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10^4</code></li>
	<li><code>0 &lt;= start, end &lt; n</code></li>
	<li><code>start != end</code></li>
	<li><code>0 &lt;= a, b &lt; n</code></li>
	<li><code>a != b</code></li>
	<li><code>0 &lt;= succProb.length == edges.length &lt;= 2*10^4</code></li>
	<li><code>0 &lt;= succProb[i] &lt;= 1</code></li>
	<li>Giữa mỗi cặp node có nhiều nhất một cạnh.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra tối ưu bằng Heap

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần xác suất thành công lớn nhất từ start đến end trên đồ thị vô hướng, trong đó trọng số cạnh được nhân với nhau. Vì $n\le 10^4$ và $m\le 2\times 10^4$, không thể liệt kê các đường đi đơn.
>
> Nếu xem trọng số là nghịch đảo của chi phí, bài toán tương đương với bài toán đường đi ngắn nhất: các xác suất được nhân và nằm trong $(0,1]$, nên nghiệm tối ưu có cấu trúc tối ưu. Dijkstra dùng max-heap cập nhật các node kề bằng tích của xác suất hiện tại và xác suất cạnh; lưu các giá trị âm để mô phỏng max-heap. Lần đầu node đích được xử lý chính là đáp án.

<!-- thinking:end -->

Ta có thể dùng thuật toán Dijkstra để tìm đường đi ngắn nhất, nhưng ở đây ta điều chỉnh một chút để tìm đường đi có xác suất lớn nhất.

Ta dùng priority queue (max-heap) $\textit{pq}$ để lưu xác suất từ điểm bắt đầu đến mỗi node cùng định danh của node đó. Ban đầu, đặt xác suất của điểm bắt đầu là $1$ và xác suất của các node khác là $0$, sau đó thêm điểm bắt đầu vào $\textit{pq}$.

Trong mỗi lần lặp, ta lấy node $a$ có xác suất cao nhất khỏi $\textit{pq}$ cùng xác suất $w$ của nó. Nếu xác suất của node $a$ đã lớn hơn $w$, ta bỏ qua node này. Ngược lại, ta duyệt tất cả cạnh kề $(a, b)$ của $a$. Nếu xác suất của $b$ nhỏ hơn xác suất của $a$ nhân với xác suất của $(a, b)$, ta cập nhật xác suất của $b$ và thêm $b$ vào $\textit{pq}$.

Cuối cùng, ta thu được xác suất lớn nhất từ điểm bắt đầu đến điểm đích.

Độ phức tạp thời gian là $O(m \times \log m)$, còn độ phức tạp không gian là $O(m)$, trong đó $m$ là số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maxProbability(
        self,
        n: int,
        edges: List[List[int]],
        succProb: List[float],
        start_node: int,
        end_node: int,
    ) -> float:
        g: List[List[Tuple[int, float]]] = [[] for _ in range(n)]
        for (a, b), p in zip(edges, succProb):
            g[a].append((b, p))
            g[b].append((a, p))
        pq = [(-1, start_node)]
        dist = [0] * n
        dist[start_node] = 1
        while pq:
            w, a = heappop(pq)
            w = -w
            if dist[a] > w:
                continue
            for b, p in g[a]:
                if (t := w * p) > dist[b]:
                    dist[b] = t
                    heappush(pq, (-t, b))
        return dist[end_node]
```

#### Java

```java
class Solution {
    public double maxProbability(
        int n, int[][] edges, double[] succProb, int start_node, int end_node) {
        List<Pair<Integer, Double>>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (int i = 0; i < edges.length; ++i) {
            var e = edges[i];
            int a = e[0], b = e[1];
            double p = succProb[i];
            g[a].add(new Pair<>(b, p));
            g[b].add(new Pair<>(a, p));
        }
        double[] dist = new double[n];
        dist[start_node] = 1;
        PriorityQueue<Pair<Integer, Double>> pq
            = new PriorityQueue<>(Comparator.comparingDouble(p -> - p.getValue()));
        pq.offer(new Pair<>(start_node, 1.0));
        while (!pq.isEmpty()) {
            var p = pq.poll();
            int a = p.getKey();
            double w = p.getValue();
            if (dist[a] > w) {
                continue;
            }
            for (var e : g[a]) {
                int b = e.getKey();
                double pab = e.getValue();
                double wab = w * pab;
                if (wab > dist[b]) {
                    dist[b] = wab;
                    pq.offer(new Pair<>(b, wab));
                }
            }
        }
        return dist[end_node];
    }
}
```

#### C++

```cpp
class Solution {
public:
    double maxProbability(int n, vector<vector<int>>& edges, vector<double>& succProb, int start_node, int end_node) {
        using pdi = pair<double, int>;
        vector<pdi> g[n];
        for (int i = 0; i < edges.size(); ++i) {
            int a = edges[i][0], b = edges[i][1];
            double p = succProb[i];
            g[a].emplace_back(p, b);
            g[b].emplace_back(p, a);
        }
        vector<double> dist(n);
        dist[start_node] = 1;
        priority_queue<pdi> pq;
        pq.emplace(1, start_node);
        while (!pq.empty()) {
            auto [w, a] = pq.top();
            pq.pop();
            if (dist[a] > w) {
                continue;
            }
            for (auto [p, b] : g[a]) {
                auto nw = w * p;
                if (nw > dist[b]) {
                    dist[b] = nw;
                    pq.emplace(nw, b);
                }
            }
        }
        return dist[end_node];
    }
};
```

#### Go

```go
func maxProbability(n int, edges [][]int, succProb []float64, start_node int, end_node int) float64 {
	g := make([][]pair, n)
	for i, e := range edges {
		a, b := e[0], e[1]
		p := succProb[i]
		g[a] = append(g[a], pair{p, b})
		g[b] = append(g[b], pair{p, a})
	}
	pq := hp{{1, start_node}}
	dist := make([]float64, n)
	dist[start_node] = 1
	for len(pq) > 0 {
		p := heap.Pop(&pq).(pair)
		w, a := p.p, p.a
		if dist[a] > w {
			continue
		}
		for _, e := range g[a] {
			b, p := e.a, e.p
			if nw := w * p; nw > dist[b] {
				dist[b] = nw
				heap.Push(&pq, pair{nw, b})
			}
		}
	}
	return dist[end_node]
}

type pair struct {
	p float64
	a int
}
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].p > h[j].p }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(x any)        { *h = append(*h, x.(pair)) }
func (h *hp) Pop() (x any)      { a := *h; x = a[len(a)-1]; *h = a[:len(a)-1]; return }
```

#### TypeScript

```ts
function maxProbability(
    n: number,
    edges: number[][],
    succProb: number[],
    start_node: number,
    end_node: number,
): number {
    const pq = new PriorityQueue<number[]>((a, b) => b[0] - a[0]);
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (let i = 0; i < edges.length; ++i) {
        const [a, b] = edges[i];
        g[a].push([b, succProb[i]]);
        g[b].push([a, succProb[i]]);
    }
    const dist = Array.from({ length: n }, () => 0);
    dist[start_node] = 1;
    pq.enqueue([1, start_node]);
    while (!pq.isEmpty()) {
        const [w, a] = pq.dequeue();
        if (dist[a] > w) {
            continue;
        }
        for (const [b, p] of g[a]) {
            const nw = w * p;
            if (nw > dist[b]) {
                dist[b] = nw;
                pq.enqueue([nw, b]);
            }
        }
    }
    return dist[end_node];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
