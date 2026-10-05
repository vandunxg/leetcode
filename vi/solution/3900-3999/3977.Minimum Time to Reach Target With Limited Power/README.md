---
comments: true
difficulty: Hard
rating: 2102
source: Weekly Contest 508 Q4
tags:
    - Graph
    - Array
    - Dynamic Programming
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3977. Minimum Time to Reach Target With Limited Power](https://leetcode.com/problems/minimum-time-to-reach-target-with-limited-power)

[中文文档](/solution/3900-3999/3977.Minimum%20Time%20to%20Reach%20Target%20With%20Limited%20Power/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một đồ thị có trọng số <strong>có hướng</strong> gồm <code>n</code> node, được đánh số từ 0 đến <code>n - 1</code>.</p>

<p>Đồ thị được biểu diễn bằng một mảng số nguyên hai chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, t<sub>i</sub>]</code> biểu thị một cạnh có hướng từ node <code>u<sub>i</sub></code> đến node <code>v<sub>i</sub></code>, mất <code>t<sub>i</sub></code> giây để đi qua.</p>

<p>Bạn cũng được cho một số nguyên <code>power</code> biểu thị năng lượng ban đầu có thể sử dụng, và một mảng số nguyên <code>cost</code> có độ dài <code>n</code>, trong đó <code>cost[u]</code> là năng lượng cần dùng để chuyển tiếp tín hiệu từ node <code>u</code> qua <strong>bất kỳ</strong> cạnh đi ra nào của nó.</p>

<p>Bạn được cho hai số nguyên <code>source</code> và <code>target</code>.</p>

<p>Tín hiệu bắt đầu tại <code>source</code> ở thời điểm 0 với <code>power</code> đơn vị năng lượng và tuân theo các quy tắc sau:</p>

<ul>
	<li>Tín hiệu chỉ có thể đi qua một cạnh có hướng từ node <code>u</code> khi năng lượng còn lại <strong>ít nhất</strong> là <code>cost[u]</code>.</li>
	<li>Tín hiệu không tiêu thụ năng lượng khi đến một node, trừ khi sau đó rời node bằng cách đi qua một cạnh khác.</li>
	<li>Khi tín hiệu được chuyển tiếp từ node <code>u</code>, năng lượng còn lại bị <strong>giảm</strong> đi <code>cost[u]</code> đơn vị.</li>
	<li>Đi qua cạnh <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, t<sub>i</sub>]</code> làm tổng thời gian <strong>tăng</strong> thêm <code>t<sub>i</sub></code> giây.</li>
</ul>

<p>Trả về một mảng số nguyên <code>answer</code> có kích thước 2, trong đó:</p>

<ul>
	<li><code>answer[0]</code> là thời gian <strong>nhỏ nhất</strong> cần để tín hiệu đến node <code>target</code>.</li>
	<li><code>answer[1]</code> là năng lượng còn lại <strong>lớn nhất</strong> trong tất cả các đường đi đạt được <code>answer[0]</code>.</li>
</ul>

<p>Nếu tín hiệu không thể đến <code>target</code>, trả về <code>[-1, -1]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3977.Minimum%20Time%20to%20Reach%20Target%20With%20Limited%20Power/images/g1.png" style="width: 197px; height: 200px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[0,1,1],[1,4,1],[0,2,1],[2,3,1],[3,4,1]], power = 4, cost = [2,3,1,1,1], source = 0, target = 4</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[3,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Tín hiệu bắt đầu tại node 0 với 4 đơn vị năng lượng.</li>
	<li>Đường đi <code>0 -&gt; 1 -&gt; 4</code> không hợp lệ vì sau khi rời node 0, tín hiệu còn 2 đơn vị năng lượng, ít hơn <code>cost[1] = 3</code>.</li>
	<li>Đường đi hợp lệ <code>0 -&gt; 2 -&gt; 3 -&gt; 4</code> mất tổng cộng 3 đơn vị thời gian.</li>
	<li>Tổng năng lượng tiêu thụ trên đường đi này là <code>cost[0] + cost[2] + cost[3] = 4</code>, nên còn lại 0 đơn vị năng lượng.</li>
	<li>Do đó, đáp án là <code>[3, 0]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3977.Minimum%20Time%20to%20Reach%20Target%20With%20Limited%20Power/images/g22.png" style="width: 167px; height: 170px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,2],[1,2,2],[2,0,2]], power = 3, cost = [1,1,1], source = 1, target = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,3]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
	<li>Vì <code>source</code> và <code>target</code> là cùng một node nên không cần đi qua cạnh nào.</li>
	<li>Do đó, tổng thời gian nhỏ nhất là 0 và không tiêu thụ năng lượng.</li>
	<li>Vì vậy, đáp án là <code>[0, 3]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3900-3999/3977.Minimum%20Time%20to%20Reach%20Target%20With%20Limited%20Power/images/g23.png" style="height: 120px; width: 171px;" /></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[0,1,3],[2,3,4]], power = 3, cost = [1,1,1,1], source = 0, target = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[-1,-1]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Không có đường đi hợp lệ từ <code>source</code> đến <code>target</code>, do đó trả về <code>[-1, -1]</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= edges.length &lt;= 1000</code></li>
	<li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, t<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= t<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= power &lt;= 1000</code></li>
	<li><code>cost.length == n</code></li>
	<li><code>1 &lt;= cost[i] &lt;= 2000</code></li>
	<li><code>0 &lt;= source, target &lt;= n - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dijkstra

<!-- thinking:start -->

> **Tư duy**
>
> Trọng số cạnh là thời gian, nhưng mỗi node cũng tiêu thụ năng lượng, nên năng lượng còn lại phải được đưa vào trạng thái. $n$ và $\textit{power}$ không vượt quá $1000$, vì vậy có thể chấp nhận $n\times\textit{power}$ trạng thái.
>
> $\textit{dist}[u][p]$ là thời gian nhỏ nhất để đến $u$ với còn $p$ năng lượng. Dijkstra lấy $(d,p,u)$ ra khỏi heap; nếu thời gian bằng nhau thì ưu tiên trạng thái còn nhiều năng lượng hơn, để lần đầu đến đích giữ được mức dự trữ tốt nhất. Node có $p<\textit{cost}[u]$ không thể chuyển tiếp.
>
> Lần đầu lấy target ra khỏi heap chính là thời gian ngắn nhất cùng lượng năng lượng còn lại lớn nhất.

<!-- thinking:end -->

Đây là một bài toán đường đi ngắn nhất, nhưng trạng thái phải theo dõi năng lượng còn lại bên cạnh node hiện tại.

Ta định nghĩa $\textit{dist}[u][p]$ là thời gian nhỏ nhất cần để đến node $u$ với còn $p$ đơn vị năng lượng. Ban đầu, $\textit{dist}[\textit{source}][\textit{power}] = 0$ và mọi trạng thái khác được đặt bằng vô cực.

Ta dùng thuật toán Dijkstra với một hàng đợi ưu tiên lưu các bộ ba $(d, p, u)$, lần lượt biểu thị thời gian nhỏ nhất hiện tại, năng lượng còn lại và node hiện tại. Để tối đa hóa năng lượng còn lại khi thời gian bằng nhau, ta lưu năng lượng còn lại dưới dạng số âm khi đưa vào hàng đợi, để hàng đợi ưu tiên các trạng thái có nhiều năng lượng hơn khi so sánh.

Khi lấy $(d, p, u)$ ra khỏi hàng đợi:

- Nếu $u = \textit{target}$, trả về trực tiếp $[d, p]$;
- Nếu $d > \textit{dist}[u][p]$ hoặc $p < \textit{cost}[u]$, bỏ qua trạng thái này;
- Ngược lại, chuyển tiếp tín hiệu từ node $u$, giảm năng lượng còn lại đi $\textit{cost}[u]$, sau đó đi qua mọi cạnh đi ra $(v, t)$ và thử cập nhật $\textit{dist}[v][p - \textit{cost}[u]] = \min(\textit{dist}[v][p - \textit{cost}[u]], d + t)$.

Nếu hàng đợi ưu tiên rỗng trước khi đến target, trả về $[-1, -1]$.

Độ phức tạp thời gian là $O((n + m) \times \textit{power} \times \log (n \times \textit{power}))$ và độ phức tạp không gian là $O(n \times \textit{power})$. Trong đó, $n$ và $m$ lần lượt là số node và số cạnh, còn $\textit{power}$ là năng lượng ban đầu.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minTimeMaxPower(
        self,
        n: int,
        edges: List[List[int]],
        power: int,
        cost: List[int],
        source: int,
        target: int,
    ) -> List[int]:
        g = [[] for _ in range(n)]
        for u, v, t in edges:
            g[u].append((v, t))
        dist = [[inf] * (power + 1) for _ in range(n)]
        dist[source][power] = 0
        pq = [(0, -power, source)]
        while pq:
            d, p, u = heappop(pq)
            p = -p
            if u == target:
                return [d, p]
            if d > dist[u][p] or p < cost[u]:
                continue
            p -= cost[u]
            for v, t in g[u]:
                nd = d + t
                if nd < dist[v][p]:
                    dist[v][p] = nd
                    heappush(pq, (nd, -p, v))
        return [-1, -1]
```

#### Java

```java
class Solution {
    public long[] minTimeMaxPower(
        int n, int[][] edges, int power, int[] cost, int source, int target) {
        long inf = Long.MAX_VALUE / 4;

        List<int[]>[] g = new ArrayList[n];
        for (int i = 0; i < n; i++) g[i] = new ArrayList<>();

        for (int[] e : edges) {
            g[e[0]].add(new int[] {e[1], e[2]});
        }

        long[][] dist = new long[n][power + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dist[i], inf);

        PriorityQueue<long[]> pq = new PriorityQueue<>((a, b) -> {
            if (a[0] != b[0]) return Long.compare(a[0], b[0]);
            return Long.compare(a[1], b[1]);
        });

        dist[source][power] = 0;
        pq.offer(new long[] {0, -power, source});

        while (!pq.isEmpty()) {
            long[] cur = pq.poll();
            long d = cur[0];
            int p = (int) -cur[1];
            int u = (int) cur[2];

            if (u == target) return new long[] {d, p};
            if (d > dist[u][p] || p < cost[u]) continue;

            p -= cost[u];

            for (int[] e : g[u]) {
                int v = e[0];
                int t = e[1];

                long nd = d + t;

                if (nd < dist[v][p]) {
                    dist[v][p] = nd;
                    pq.offer(new long[] {nd, -p, v});
                }
            }
        }

        return new long[] {-1, -1};
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<long long> minTimeMaxPower(
        int n,
        vector<vector<int>>& edges,
        int power,
        vector<int>& cost,
        int source,
        int target) {
        using ll = long long;
        const ll inf = LLONG_MAX / 4;

        vector<vector<pair<int, int>>> g(n);
        for (auto& e : edges) {
            g[e[0]].push_back({e[1], e[2]});
        }

        vector<vector<ll>> dist(n, vector<ll>(power + 1, inf));

        using T = tuple<ll, int, int>;
        priority_queue<T, vector<T>, greater<T>> pq;

        dist[source][power] = 0;
        pq.push({0, -power, source});

        while (!pq.empty()) {
            auto [d, negp, u] = pq.top();
            pq.pop();

            int p = -negp;

            if (u == target) return {d, p};
            if (d > dist[u][p] || p < cost[u]) continue;

            p -= cost[u];

            for (auto& [v, t] : g[u]) {
                ll nd = d + t;

                if (nd < dist[v][p]) {
                    dist[v][p] = nd;
                    pq.push({nd, -p, v});
                }
            }
        }

        return {-1, -1};
    }
};
```

#### Go

```go
type State struct {
	d int64
	p int
	u int
}

type PQ []State

func (h PQ) Len() int { return len(h) }
func (h PQ) Less(i, j int) bool {
	if h[i].d != h[j].d {
		return h[i].d < h[j].d
	}
	return h[i].p < h[j].p
}
func (h PQ) Swap(i, j int) { h[i], h[j] = h[j], h[i] }

func (h *PQ) Push(x interface{}) { *h = append(*h, x.(State)) }
func (h *PQ) Pop() interface{} {
	old := *h
	x := old[len(old)-1]
	*h = old[:len(old)-1]
	return x
}

func minTimeMaxPower(
	n int,
	edges [][]int,
	power int,
	cost []int,
	source int,
	target int,
) []int64 {

	inf := int64(1 << 62)

	g := make([][][]int, n)
	for _, e := range edges {
		g[e[0]] = append(g[e[0]], []int{e[1], e[2]})
	}

	dist := make([][]int64, n)
	for i := range dist {
		dist[i] = make([]int64, power+1)
		for j := range dist[i] {
			dist[i][j] = inf
		}
	}

	pq := &PQ{}
	heap.Push(pq, State{0, -power, source})

	dist[source][power] = 0

	for pq.Len() > 0 {
		cur := heap.Pop(pq).(State)
		d := cur.d
		p := -cur.p
		u := cur.u

		if u == target {
			return []int64{d, int64(p)}
		}

		if d > dist[u][p] || p < cost[u] {
			continue
		}

		p -= cost[u]

		for _, e := range g[u] {
			v, t := e[0], e[1]
			nd := d + int64(t)

			if nd < dist[v][p] {
				dist[v][p] = nd
				heap.Push(pq, State{nd, -p, v})
			}
		}
	}

	return []int64{-1, -1}
}
```

#### TypeScript

```ts
function minTimeMaxPower(
    n: number,
    edges: number[][],
    power: number,
    cost: number[],
    source: number,
    target: number,
): number[] {
    const inf = 1e18;

    const g: [number, number][][] = Array.from({ length: n }, () => []);

    for (const [u, v, t] of edges) {
        g[u].push([v, t]);
    }

    const dist: number[][] = Array.from({ length: n }, () => Array(power + 1).fill(inf));

    const pq = new PriorityQueue<number[]>((a, b) => {
        if (a[0] !== b[0]) return a[0] - b[0];
        return a[1] - b[1];
    });

    dist[source][power] = 0;
    pq.enqueue([0, -power, source]);

    while (!pq.isEmpty()) {
        const [d, negp, u] = pq.dequeue();
        let p = -negp;

        if (u === target) return [d, p];
        if (d > dist[u][p] || p < cost[u]) continue;

        p -= cost[u];

        for (const [v, t] of g[u]) {
            const nd = d + t;

            if (nd < dist[v][p]) {
                dist[v][p] = nd;
                pq.enqueue([nd, -p, v]);
            }
        }
    }

    return [-1, -1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
