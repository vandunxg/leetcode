---
comments: true
difficulty: Medium
tags:
    - Graph
    - Array
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2473. Minimum Cost to Buy Apples 🔒](https://leetcode.com/problems/minimum-cost-to-buy-apples)

[中文文档](/solution/2400-2499/2473.Minimum%20Cost%20to%20Buy%20Apples/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một số nguyên dương <code>n</code> biểu thị <code>n</code> thành phố được đánh số từ <code>1</code> đến <code>n</code>. Ngoài ra, cho một mảng <strong>2D</strong> <code>roads</code>, trong đó <code>roads[i] = [a<sub>i</sub>, b<sub>i</sub>, cost<sub>i</sub>]</code> cho biết có một con đường <strong>hai chiều</strong> giữa thành phố <code>a<sub>i</sub></code> và thành phố <code>b<sub>i</sub></code> với chi phí di chuyển bằng <code>cost<sub>i</sub></code>.</p>

<p>Bạn có thể mua táo ở <strong>bất kỳ</strong> thành phố nào, nhưng mỗi thành phố có chi phí mua táo khác nhau. Cho mảng <code>appleCost</code> đánh số từ 1, trong đó <code>appleCost[i]</code> là chi phí mua một quả táo từ thành phố <code>i</code>.</p>

<p>Bạn bắt đầu ở một thành phố, đi qua nhiều con đường khác nhau và cuối cùng mua <strong>đúng</strong> một quả táo từ <strong>bất kỳ</strong> thành phố nào. Sau khi mua quả táo đó, bạn phải quay lại thành phố <strong>ban đầu</strong>, nhưng lúc này chi phí của tất cả các con đường sẽ được <strong>nhân</strong> với một hệ số <code>k</code>.</p>

<p>Với số nguyên <code>k</code>, hãy trả về <em>một mảng đánh số từ 1</em> <code>answer</code><em> có kích thước </em><code>n</code><em>, trong đó </em><code>answer[i]</code><em> là tổng chi phí <strong>nhỏ nhất</strong> để mua một quả táo nếu bạn bắt đầu ở thành phố </em><code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2473.Minimum%20Cost%20to%20Buy%20Apples/images/graph55.png" style="width: 241px; height: 309px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, roads = [[1,2,4],[2,3,2],[2,4,5],[3,4,1],[1,3,4]], appleCost = [56,42,102,301], k = 2
<strong>Đầu ra:</strong> [54,42,48,51]
<strong>Giải thích:</strong> Chi phí nhỏ nhất cho mỗi thành phố bắt đầu như sau:
- Bắt đầu ở thành phố 1: Bạn đi theo đường 1 -&gt; 2, mua một quả táo ở thành phố 2, rồi đi theo đường 2 -&gt; 1 để quay lại. Tổng chi phí là 4 + 42 + 4 * 2 = 54.
- Bắt đầu ở thành phố 2: Bạn mua trực tiếp một quả táo ở thành phố 2. Tổng chi phí là 42.
- Bắt đầu ở thành phố 3: Bạn đi theo đường 3 -&gt; 2, mua một quả táo ở thành phố 2, rồi đi theo đường 2 -&gt; 3 để quay lại. Tổng chi phí là 2 + 42 + 2 * 2 = 48.
- Bắt đầu ở thành phố 4: Bạn đi theo đường 4 -&gt; 3 -&gt; 2 rồi mua táo ở thành phố 2, sau đó đi theo đường 2 -&gt; 3 -&gt; 4 để quay lại. Tổng chi phí là 1 + 2 + 42 + 1 * 2 + 2 * 2 = 51.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2400-2499/2473.Minimum%20Cost%20to%20Buy%20Apples/images/graph4.png" style="width: 167px; height: 309px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, roads = [[1,2,5],[2,3,1],[3,1,2]], appleCost = [2,3,1], k = 3
<strong>Đầu ra:</strong> [2,3,1]
<strong>Giải thích:</strong> Luôn tối ưu nếu mua táo tại thành phố bắt đầu.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 1000</code></li>
	<li><code>1 &lt;= roads.length &lt;= 2000</code></li>
	<li><code>1 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt;= n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>1 &lt;= cost<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>appleCost.length == n</code></li>
	<li><code>1 &lt;= appleCost[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= k &lt;= 100</code></li>
	<li>Không có cạnh nào bị lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra tối ưu bằng Heap

<!-- thinking:start -->

> **Tư duy**
>
> Có thể mua táo ở bất kỳ thành phố nào; chi phí di chuyển bằng $(k+1)$ lần khoảng cách (phần $k$ là quãng đường quay về). Với mỗi điểm bắt đầu, Dijkstra cập nhật $\textit{appleCost}[u]+dist[u]\cdot(k+1)$ trong quá trình duyệt.
>
> Dijkstra dùng heap có độ phức tạp $O(m\log n)$ cho mỗi nguồn trên đồ thị thưa.

<!-- thinking:end -->

Ta lần lượt chọn từng điểm bắt đầu, rồi dùng thuật toán Dijkstra để tìm khoảng cách ngắn nhất đến tất cả các điểm khác và cập nhật giá trị nhỏ nhất tương ứng.

Độ phức tạp thời gian là $O(n \times m \times \log m)$, trong đó $n$ và $m$ lần lượt là số thành phố và số con đường.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minCost(
        self, n: int, roads: List[List[int]], appleCost: List[int], k: int
    ) -> List[int]:
        def dijkstra(i):
            q = [(0, i)]
            dist = [inf] * n
            dist[i] = 0
            ans = inf
            while q:
                d, u = heappop(q)
                ans = min(ans, appleCost[u] + d * (k + 1))
                for v, w in g[u]:
                    if dist[v] > dist[u] + w:
                        dist[v] = dist[u] + w
                        heappush(q, (dist[v], v))
            return ans

        g = defaultdict(list)
        for a, b, c in roads:
            a, b = a - 1, b - 1
            g[a].append((b, c))
            g[b].append((a, c))
        return [dijkstra(i) for i in range(n)]
```

#### Java

```java
class Solution {
    private int k;
    private int[] cost;
    private int[] dist;
    private List<int[]>[] g;
    private static final int INF = 0x3f3f3f3f;

    public long[] minCost(int n, int[][] roads, int[] appleCost, int k) {
        cost = appleCost;
        g = new List[n];
        dist = new int[n];
        this.k = k;
        for (int i = 0; i < n; ++i) {
            g[i] = new ArrayList<>();
        }
        for (var e : roads) {
            int a = e[0] - 1, b = e[1] - 1, c = e[2];
            g[a].add(new int[] {b, c});
            g[b].add(new int[] {a, c});
        }
        long[] ans = new long[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = dijkstra(i);
        }
        return ans;
    }

    private long dijkstra(int u) {
        PriorityQueue<int[]> q = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        q.offer(new int[] {0, u});
        Arrays.fill(dist, INF);
        dist[u] = 0;
        long ans = Long.MAX_VALUE;
        while (!q.isEmpty()) {
            var p = q.poll();
            int d = p[0];
            u = p[1];
            ans = Math.min(ans, cost[u] + (long) (k + 1) * d);
            for (var ne : g[u]) {
                int v = ne[0], w = ne[1];
                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                    q.offer(new int[] {dist[v], v});
                }
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
using ll = long long;
using pii = pair<int, int>;

class Solution {
public:
    const int inf = 0x3f3f3f3f;

    vector<long long> minCost(int n, vector<vector<int>>& roads, vector<int>& appleCost, int k) {
        vector<vector<pii>> g(n);
        for (auto& e : roads) {
            int a = e[0] - 1, b = e[1] - 1, c = e[2];
            g[a].push_back({b, c});
            g[b].push_back({a, c});
        }
        int dist[n];
        auto dijkstra = [&](int u) {
            memset(dist, 63, sizeof dist);
            priority_queue<pii, vector<pii>, greater<pii>> q;
            q.push({0, u});
            dist[u] = 0;
            ll ans = LONG_MAX;
            while (!q.empty()) {
                auto p = q.top();
                q.pop();
                int d = p.first;
                u = p.second;
                ans = min(ans, appleCost[u] + 1ll * d * (k + 1));
                for (auto& ne : g[u]) {
                    auto [v, w] = ne;
                    if (dist[v] > dist[u] + w) {
                        dist[v] = dist[u] + w;
                        q.push({dist[v], v});
                    }
                }
            }
            return ans;
        };
        vector<ll> ans(n);
        for (int i = 0; i < n; ++i) ans[i] = dijkstra(i);
        return ans;
    }
};
```

#### Go

```go
func minCost(n int, roads [][]int, appleCost []int, k int) []int64 {
	g := make([]pairs, n)
	for _, e := range roads {
		a, b, c := e[0]-1, e[1]-1, e[2]
		g[a] = append(g[a], pair{b, c})
		g[b] = append(g[b], pair{a, c})
	}
	const inf int = 0x3f3f3f3f
	dist := make([]int, n)
	dijkstra := func(u int) int64 {
		var ans int64 = math.MaxInt64
		for i := range dist {
			dist[i] = inf
		}
		dist[u] = 0
		q := make(pairs, 0)
		heap.Push(&q, pair{0, u})
		for len(q) > 0 {
			p := heap.Pop(&q).(pair)
			d := p.first
			u = p.second
			ans = min(ans, int64(appleCost[u]+d*(k+1)))
			for _, ne := range g[u] {
				v, w := ne.first, ne.second
				if dist[v] > dist[u]+w {
					dist[v] = dist[u] + w
					heap.Push(&q, pair{dist[v], v})
				}
			}
		}
		return ans
	}
	ans := make([]int64, n)
	for i := range ans {
		ans[i] = dijkstra(i)
	}
	return ans
}

type pair struct{ first, second int }

var _ heap.Interface = (*pairs)(nil)

type pairs []pair

func (a pairs) Len() int { return len(a) }
func (a pairs) Less(i int, j int) bool {
	return a[i].first < a[j].first || a[i].first == a[j].first && a[i].second < a[j].second
}
func (a pairs) Swap(i int, j int) { a[i], a[j] = a[j], a[i] }
func (a *pairs) Push(x any)       { *a = append(*a, x.(pair)) }
func (a *pairs) Pop() any         { l := len(*a); t := (*a)[l-1]; *a = (*a)[:l-1]; return t }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
