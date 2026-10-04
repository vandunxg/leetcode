---
comments: true
difficulty: Hard
tags:
    - Graph
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2714. Find Shortest Path with K Hops 🔒](https://leetcode.com/problems/find-shortest-path-with-k-hops)

[中文文档](/solution/2700-2799/2714.Find%20Shortest%20Path%20with%20K%20Hops/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>, là số lượng node của một đồ thị <strong>vô hướng, có trọng số và liên thông, được đánh chỉ số từ 0</strong>, cùng một <strong>mảng 2 chiều</strong> <code>edges</code> được <strong>đánh chỉ số từ 0</strong>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> cho biết có một cạnh nối giữa node <code>u<sub>i</sub></code> và node <code>v<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code>.</p>

<p>Bạn cũng được cho hai node <code>s</code> và <code>d</code>, cùng một số nguyên dương <code>k</code>. Nhiệm vụ của bạn là tìm đường đi <strong>ngắn nhất</strong> từ <code>s</code> đến <code>d</code>, nhưng bạn có thể nhảy qua <strong>nhiều nhất</strong> <code>k</code> cạnh. Nói cách khác, hãy đặt trọng số của <strong>nhiều nhất</strong> <code>k</code> cạnh thành <code>0</code>, sau đó tìm đường đi <strong>ngắn nhất</strong> từ <code>s</code> đến <code>d</code>.</p>

<p>Trả về <em>độ dài của đường đi <strong>ngắn nhất</strong> từ </em><code>s</code><em> đến </em><code>d</code><em> thỏa mãn điều kiện trên</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1,4],[0,2,2],[2,3,6]], s = 1, d = 3, k = 2
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Trong ví dụ này, chỉ có một đường đi từ node 1 (node màu xanh lá) đến node 3 (node màu đỏ), đó là (1-&gt;0-&gt;2-&gt;3), với độ dài 4 + 2 + 6 = 12. Bây giờ ta có thể đặt trọng số của hai cạnh thành 0 bằng cách đặt trọng số của các cạnh màu xanh dương thành 0, khi đó ta có 0 + 2 + 0 = 2. Có thể chứng minh rằng 2 là độ dài nhỏ nhất của một đường đi có thể đạt được với điều kiện trên.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2714.Find%20Shortest%20Path%20with%20K%20Hops/images/1.jpg" style="width: 170px; height: 171px;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[3,1,9],[3,2,4],[4,0,9],[0,5,6],[3,6,2],[6,0,4],[1,2,4]], s = 4, d = 1, k = 2
<strong>Đầu ra:</strong> 6
<strong>Giải thích:</strong> Trong ví dụ này, có 2 đường đi từ node 4 (node màu xanh lá) đến node 1 (node màu đỏ), đó là (4-&gt;0-&gt;6-&gt;3-&gt;2-&gt;1) và (4-&gt;0-&gt;6-&gt;3-&gt;1). Đường đi thứ nhất có độ dài 9 + 4 + 2 + 4 + 4 = 23, còn đường đi thứ hai có độ dài 9 + 4 + 2 + 9 = 24. Nếu đặt trọng số của các cạnh màu xanh dương thành 0, ta nhận được đường đi ngắn nhất có độ dài 0 + 4 + 2 + 0 = 6. Có thể chứng minh rằng 6 là độ dài nhỏ nhất của một đường đi có thể đạt được với điều kiện trên.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2714.Find%20Shortest%20Path%20with%20K%20Hops/images/2.jpg" style="width: 400px; height: 171px;" /></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[0,4,2],[0,1,3],[0,2,1],[2,1,4],[1,3,4],[3,4,7]], s = 2, d = 3, k = 1
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Trong ví dụ này, có 4 đường đi từ node 2 (node màu xanh lá) đến node 3 (node màu đỏ), đó là (2-&gt;1-&gt;3), (2-&gt;0-&gt;1-&gt;3), (2-&gt;1-&gt;0-&gt;4-&gt;3) và (2-&gt;0-&gt;4-&gt;3). Hai đường đi đầu tiên có độ dài 4 + 4 = 1 + 3 + 4 = 8, đường đi thứ ba có độ dài 4 + 3 + 2 + 7 = 16, còn đường đi cuối cùng có độ dài 1 + 2 + 7 = 10. Nếu đặt trọng số của cạnh màu xanh dương thành 0, ta nhận được đường đi ngắn nhất có độ dài 1 + 2 + 0 = 3. Có thể chứng minh rằng 3 là độ dài nhỏ nhất của một đường đi có thể đạt được với điều kiện trên.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2714.Find%20Shortest%20Path%20with%20K%20Hops/images/3.jpg" style="width: 300px; height: 296px;" /></p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 500</code></li>
    <li><code>n - 1 &lt;= edges.length &lt;= min(10<sup>4</sup>, n * (n - 1) / 2)</code></li>
    <li><code>edges[i].length = 3</code></li>
    <li><code>0 &lt;= edges[i][0], edges[i][1] &lt;= n - 1</code></li>
    <li><code>1 &lt;= edges[i][2] &lt;=&nbsp;10<sup>6</sup></code></li>
    <li><code>0 &lt;= s, d, k&nbsp;&lt;= n - 1</code></li>
    <li><code>s != d</code></li>
    <li>Dữ liệu đầu vào được tạo sao cho đồ thị <strong>liên thông</strong> và <strong>không có</strong>&nbsp;<strong>cạnh trùng lặp</strong> hoặc <strong>self-loop</strong></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm đường đi ngắn nhất từ $s$– $d$, trong đó nhiều nhất $k$ cạnh có thể được xem như cạnh miễn phí. Trạng thái chỉ gồm node không thể ghi nhớ còn bao nhiêu lần nhảy. Vì $k$ nhỏ, số lần nhảy được đưa thêm vào trạng thái.
>
> Chạy Dijkstra trên các cặp $(u,t)$: với cạnh $(u,v,w)$, ta có thể trả $w$ và giữ nguyên $t$, hoặc nếu $t<k$ thì đi đến $v$ với chi phí $0$ và chuyển sang trạng thái $t+1$. Đáp án là khoảng cách nhỏ nhất trong tất cả các trạng thái $t$ tại node đích $d$.

<!-- thinking:end -->

Trước hết, ta xây dựng một đồ thị $g$ dựa trên các cạnh đã cho, trong đó $g[u]$ biểu diễn tất cả các node kề với node $u$ và trọng số cạnh tương ứng.

Sau đó, ta sử dụng thuật toán Dijkstra để tìm đường đi ngắn nhất từ node $s$ đến node $d$. Tuy nhiên, ta cần chỉnh sửa thuật toán Dijkstra như sau:

- Ta cần ghi lại độ dài đường đi ngắn nhất từ mỗi node $u$ đến node $d$. Nhưng vì ta có thể đi qua nhiều nhất $k$ cạnh, ta cần ghi lại độ dài đường đi ngắn nhất từ mỗi node $u$ đến node $d$ cùng với số cạnh đã đi qua $t$, tức là $dist[u][t]$ biểu diễn độ dài đường đi ngắn nhất từ node $u$ đến node $d$ khi số cạnh đã đi qua là $t$.
- Ta cần sử dụng priority queue để duy trì đường đi ngắn nhất hiện tại. Tuy nhiên, vì cần ghi lại số cạnh đã đi qua, ta sử dụng bộ ba $(dis, u, t)$ để biểu diễn đường đi ngắn nhất hiện tại, trong đó $dis$ là độ dài đường đi ngắn nhất hiện tại, còn $u$ và $t$ lần lượt là node hiện tại và số cạnh đã đi qua.

Cuối cùng, ta chỉ cần trả về giá trị nhỏ nhất trong $dist[d][0..k]$.

Độ phức tạp thời gian là $O(n^2 \times \log n)$, và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ là số node và $k$ là số cạnh tối đa có thể đi qua.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def shortestPathWithHops(
        self, n: int, edges: List[List[int]], s: int, d: int, k: int
    ) -> int:
        g = [[] for _ in range(n)]
        for u, v, w in edges:
            g[u].append((v, w))
            g[v].append((u, w))
        dist = [[inf] * (k + 1) for _ in range(n)]
        dist[s][0] = 0
        pq = [(0, s, 0)]
        while pq:
            dis, u, t = heappop(pq)
            for v, w in g[u]:
                if t + 1 <= k and dist[v][t + 1] > dis:
                    dist[v][t + 1] = dis
                    heappush(pq, (dis, v, t + 1))
                if dist[v][t] > dis + w:
                    dist[v][t] = dis + w
                    heappush(pq, (dis + w, v, t))
        return int(min(dist[d]))
```

#### Java

```java
class Solution {
    public int shortestPathWithHops(int n, int[][] edges, int s, int d, int k) {
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (int[] e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].add(new int[] {v, w});
            g[v].add(new int[] {u, w});
        }
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, s, 0});
        int[][] dist = new int[n][k + 1];
        final int inf = 1 << 30;
        for (int[] e : dist) {
            Arrays.fill(e, inf);
        }
        dist[s][0] = 0;
        while (!pq.isEmpty()) {
            int[] p = pq.poll();
            int dis = p[0], u = p[1], t = p[2];
            for (int[] e : g[u]) {
                int v = e[0], w = e[1];
                if (t + 1 <= k && dist[v][t + 1] > dis) {
                    dist[v][t + 1] = dis;
                    pq.offer(new int[] {dis, v, t + 1});
                }
                if (dist[v][t] > dis + w) {
                    dist[v][t] = dis + w;
                    pq.offer(new int[] {dis + w, v, t});
                }
            }
        }
        int ans = inf;
        for (int i = 0; i <= k; ++i) {
            ans = Math.min(ans, dist[d][i]);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int shortestPathWithHops(int n, vector<vector<int>>& edges, int s, int d, int k) {
        vector<pair<int, int>> g[n];
        for (auto& e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].emplace_back(v, w);
            g[v].emplace_back(u, w);
        }
        priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>, greater<tuple<int, int, int>>> pq;
        pq.emplace(0, s, 0);
        int dist[n][k + 1];
        memset(dist, 0x3f, sizeof(dist));
        dist[s][0] = 0;
        while (!pq.empty()) {
            auto [dis, u, t] = pq.top();
            pq.pop();
            for (auto [v, w] : g[u]) {
                if (t + 1 <= k && dist[v][t + 1] > dis) {
                    dist[v][t + 1] = dis;
                    pq.emplace(dis, v, t + 1);
                }
                if (dist[v][t] > dis + w) {
                    dist[v][t] = dis + w;
                    pq.emplace(dis + w, v, t);
                }
            }
        }
        return *min_element(dist[d], dist[d] + k + 1);
    }
};
```

#### Go

```go
func shortestPathWithHops(n int, edges [][]int, s int, d int, k int) int {
	g := make([][][2]int, n)
	for _, e := range edges {
		u, v, w := e[0], e[1], e[2]
		g[u] = append(g[u], [2]int{v, w})
		g[v] = append(g[v], [2]int{u, w})
	}
	pq := hp{{0, s, 0}}
	dist := make([][]int, n)
	for i := range dist {
		dist[i] = make([]int, k+1)
		for j := range dist[i] {
			dist[i][j] = math.MaxInt32
		}
	}
	dist[s][0] = 0
	for len(pq) > 0 {
		p := heap.Pop(&pq).(tuple)
		dis, u, t := p.dis, p.u, p.t
		for _, e := range g[u] {
			v, w := e[0], e[1]
			if t+1 <= k && dist[v][t+1] > dis {
				dist[v][t+1] = dis
				heap.Push(&pq, tuple{dis, v, t + 1})
			}
			if dist[v][t] > dis+w {
				dist[v][t] = dis + w
				heap.Push(&pq, tuple{dis + w, v, t})
			}
		}
	}
	return slices.Min(dist[d])
}

type tuple struct{ dis, u, t int }
type hp []tuple

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].dis < h[j].dis }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(tuple)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
