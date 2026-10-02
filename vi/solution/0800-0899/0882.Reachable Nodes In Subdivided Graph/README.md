---
comments: true
difficulty: Hard
tags:
    - Graph
    - Shortest Path
    - Dijkstra
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [882. Reachable Nodes In Subdivided Graph](https://leetcode.com/problems/reachable-nodes-in-subdivided-graph)

[中文文档](/solution/0800-0899/0882.Reachable%20Nodes%20In%20Subdivided%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Cho đồ thị vô hướng (gọi là <strong>&quot;đồ thị ban đầu&quot;</strong>) gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Ta sẽ <strong>chia nhỏ</strong> mỗi cạnh thành một chuỗi node; số node mới có thể khác nhau ở mỗi cạnh.</p>

<p>Đồ thị được biểu diễn bằng mảng 2 chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, cnt<sub>i</sub>]</code> cho biết có một cạnh nối node <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> trong đồ thị ban đầu, còn <code>cnt<sub>i</sub></code> là tổng số node mới sẽ được thêm khi <strong>chia nhỏ</strong> cạnh đó. Lưu ý, <code>cnt<sub>i</sub> == 0</code> nghĩa là cạnh không bị chia nhỏ.</p>

<p>Để <strong>chia nhỏ</strong> cạnh <code>[u<sub>i</sub>, v<sub>i</sub>]</code>, thay cạnh đó bằng <code>(cnt<sub>i</sub> + 1)</code> cạnh mới và <code>cnt<sub>i</sub></code> node mới. Các node mới là <code>x<sub>1</sub></code>, <code>x<sub>2</sub></code>, ..., <code>x<sub>cnt<sub>i</sub></sub></code>; các cạnh mới lần lượt là <code>[u<sub>i</sub>, x<sub>1</sub>]</code>, <code>[x<sub>1</sub>, x<sub>2</sub>]</code>, <code>[x<sub>2</sub>, x<sub>3</sub>]</code>, ..., <code>[x<sub>cnt<sub>i</sub>-1</sub>, x<sub>cnt<sub>i</sub></sub>]</code>, <code>[x<sub>cnt<sub>i</sub></sub>, v<sub>i</sub>]</code>.</p>

<p>Trong <strong>đồ thị mới</strong>, hãy xác định có bao nhiêu node <strong>có thể đến được</strong> từ node <code>0</code>, tức là có khoảng cách không quá <code>maxMoves</code>.</p>

<p>Cho đồ thị ban đầu và <code>maxMoves</code>, trả về <em>số node trong đồ thị mới có thể <strong>đến được</strong> từ node </em><code>0</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0800-0899/0882.Reachable%20Nodes%20In%20Subdivided%20Graph/images/origfinal.png" style="width: 600px; height: 247px;" />
<pre>
<strong>Input:</strong> edges = [[0,1,10],[0,2,1],[1,2,2]], maxMoves = 6, n = 3
<strong>Output:</strong> 13
<strong>Giải thích:</strong> Các cạnh sau khi chia nhỏ được minh họa trong hình trên.
Các node đến được tô màu vàng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> edges = [[0,1,4],[1,2,6],[0,2,8],[1,3,1]], maxMoves = 10, n = 4
<strong>Output:</strong> 23
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Input:</strong> edges = [[1,2,4],[1,4,5],[1,3,1],[2,3,4],[3,4,5]], maxMoves = 17, n = 5
<strong>Output:</strong> 1
<strong>Giải thích:</strong> Node 0 không kết nối với phần còn lại của đồ thị, nên chỉ có thể đến được node 0.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>0 &lt;= edges.length &lt;= min(n * (n - 1) / 2, 10<sup>4</sup>)</code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= u<sub>i</sub> &lt; v<sub>i</sub> &lt; n</code></li>
	<li>Đồ thị <strong>không có cạnh song song</strong>.</li>
	<li><code>0 &lt;= cnt<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= maxMoves &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= n &lt;= 3000</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cạnh ban đầu được chia thành $\textit{cnt}$ node; ta cần đếm số node có thể đi đến từ $0$ trong phạm vi $\textit{maxMoves}$. Đồ thị sau khi chia nhỏ quá lớn để dựng trực tiếp.
>
> Xem mỗi cạnh có trọng số $\textit{cnt}+1$ rồi chạy Dijkstra trên các node ban đầu. Mỗi node ban đầu có thể đến được sẽ được tính là một node; số node mới trên một cạnh đi đến được bằng tổng số bước còn lại từ hai đầu, giới hạn tối đa là $\textit{cnt}$.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def reachableNodes(self, edges: List[List[int]], maxMoves: int, n: int) -> int:
        g = defaultdict(list)
        for u, v, cnt in edges:
            g[u].append((v, cnt + 1))
            g[v].append((u, cnt + 1))
        q = [(0, 0)]
        dist = [0] + [inf] * n
        while q:
            d, u = heappop(q)
            for v, cnt in g[u]:
                if (t := d + cnt) < dist[v]:
                    dist[v] = t
                    heappush(q, (t, v))
        ans = sum(d <= maxMoves for d in dist)
        for u, v, cnt in edges:
            a = min(cnt, max(0, maxMoves - dist[u]))
            b = min(cnt, max(0, maxMoves - dist[v]))
            ans += min(cnt, a + b)
        return ans
```

#### Java

```java
class Solution {
    public int reachableNodes(int[][] edges, int maxMoves, int n) {
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, e -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1], cnt = e[2] + 1;
            g[u].add(new int[] {v, cnt});
            g[v].add(new int[] {u, cnt});
        }
        int[] dist = new int[n];
        Arrays.fill(dist, 1 << 30);
        PriorityQueue<int[]> q = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        q.offer(new int[] {0, 0});
        dist[0] = 0;
        while (!q.isEmpty()) {
            var p = q.poll();
            int d = p[0], u = p[1];
            for (var nxt : g[u]) {
                int v = nxt[0], cnt = nxt[1];
                if (d + cnt < dist[v]) {
                    dist[v] = d + cnt;
                    q.offer(new int[] {dist[v], v});
                }
            }
        }
        int ans = 0;
        for (int d : dist) {
            if (d <= maxMoves) {
                ++ans;
            }
        }
        for (var e : edges) {
            int u = e[0], v = e[1], cnt = e[2];
            int a = Math.min(cnt, Math.max(0, maxMoves - dist[u]));
            int b = Math.min(cnt, Math.max(0, maxMoves - dist[v]));
            ans += Math.min(cnt, a + b);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int reachableNodes(vector<vector<int>>& edges, int maxMoves, int n) {
        using pii = pair<int, int>;
        vector<vector<pii>> g(n);
        for (auto& e : edges) {
            int u = e[0], v = e[1], cnt = e[2] + 1;
            g[u].emplace_back(v, cnt);
            g[v].emplace_back(u, cnt);
        }
        priority_queue<pii, vector<pii>, greater<pii>> q;
        q.emplace(0, 0);
        int dist[n];
        memset(dist, 0x3f, sizeof dist);
        dist[0] = 0;
        while (!q.empty()) {
            auto [d, u] = q.top();
            q.pop();
            for (auto& [v, cnt] : g[u]) {
                if (d + cnt < dist[v]) {
                    dist[v] = d + cnt;
                    q.emplace(dist[v], v);
                }
            }
        }
        int ans = 0;
        for (int& d : dist) ans += d <= maxMoves;
        for (auto& e : edges) {
            int u = e[0], v = e[1], cnt = e[2];
            int a = min(cnt, max(0, maxMoves - dist[u]));
            int b = min(cnt, max(0, maxMoves - dist[v]));
            ans += min(cnt, a + b);
        }
        return ans;
    }
};
```

#### Go

```go
func reachableNodes(edges [][]int, maxMoves int, n int) (ans int) {
	g := make([][]pair, n)
	for _, e := range edges {
		u, v, cnt := e[0], e[1], e[2]+1
		g[u] = append(g[u], pair{cnt, v})
		g[v] = append(g[v], pair{cnt, u})
	}
	dist := make([]int, n)
	for i := range dist {
		dist[i] = 1 << 30
	}
	dist[0] = 0
	q := hp{{0, 0}}
	for len(q) > 0 {
		p := heap.Pop(&q).(pair)
		d, u := p.v, p.i
		for _, nxt := range g[u] {
			v, cnt := nxt.i, nxt.v
			if t := d + cnt; t < dist[v] {
				dist[v] = t
				heap.Push(&q, pair{t, v})
			}
		}
	}
	for _, d := range dist {
		if d <= maxMoves {
			ans++
		}
	}
	for _, e := range edges {
		u, v, cnt := e[0], e[1], e[2]
		a := min(cnt, max(0, maxMoves-dist[u]))
		b := min(cnt, max(0, maxMoves-dist[v]))
		ans += min(cnt, a+b)
	}
	return
}

type pair struct{ v, i int }
type hp []pair

func (h hp) Len() int           { return len(h) }
func (h hp) Less(i, j int) bool { return h[i].v < h[j].v }
func (h hp) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *hp) Push(v any)        { *h = append(*h, v.(pair)) }
func (h *hp) Pop() any          { a := *h; v := a[len(a)-1]; *h = a[:len(a)-1]; return v }
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
