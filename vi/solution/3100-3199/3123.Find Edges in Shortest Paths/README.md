---
comments: true
difficulty: Hard
rating: 2093
source: Weekly Contest 394 Q4
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [3123. Find Edges in Shortest Paths](https://leetcode.com/problems/find-edges-in-shortest-paths)

[中文文档](/solution/3100-3199/3123.Find%20Edges%20in%20Shortest%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một đồ thị vô hướng có trọng số gồm <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>. Đồ thị gồm <code>m</code> cạnh được biểu diễn bằng một mảng 2 chiều <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>, w<sub>i</sub>]</code> cho biết có một cạnh nối node <code>a<sub>i</sub></code> và node <code>b<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code>.</p>

<p>Xét tất cả đường đi ngắn nhất từ node 0 đến node <code>n - 1</code> trong đồ thị. Bạn cần tìm một mảng <strong>boolean</strong> <code>answer</code>, trong đó <code>answer[i]</code> là <code>true</code> nếu cạnh <code>edges[i]</code> thuộc <strong>ít nhất</strong> một đường đi ngắn nhất. Nếu không, <code>answer[i]</code> là <code>false</code>.</p>

<p>Trả về mảng <code>answer</code>.</p>

<p><strong>Lưu ý</strong> rằng đồ thị có thể không liên thông.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3123.Find%20Edges%20in%20Shortest%20Paths/images/graph35drawio-1.png" style="height: 129px; width: 250px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 6, edges = [[0,1,4],[0,2,1],[1,3,2],[1,4,3],[1,5,1],[2,3,1],[3,5,3],[4,5,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,true,true,false,true,true,true,false]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Sau đây là <strong>tất cả</strong> đường đi ngắn nhất giữa node 0 và 5:</p>

<ul>
	<li>Đường đi <code>0 -&gt; 1 -&gt; 5</code>: Tổng trọng số là <code>4 + 1 = 5</code>.</li>
	<li>Đường đi <code>0 -&gt; 2 -&gt; 3 -&gt; 5</code>: Tổng trọng số là <code>1 + 1 + 3 = 5</code>.</li>
	<li>Đường đi <code>0 -&gt; 2 -&gt; 3 -&gt; 1 -&gt; 5</code>: Tổng trọng số là <code>1 + 1 + 2 + 1 = 5</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3123.Find%20Edges%20in%20Shortest%20Paths/images/graphhhh.png" style="width: 185px; height: 136px;" />
<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 4, edges = [[2,0,1],[0,1,1],[0,3,4],[3,2,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[true,false,false,true]</span></p>

<p><strong>Giải thích:</strong></p>

<p>Có một đường đi ngắn nhất giữa node 0 và 3, đó là đường đi <code>0 -&gt; 2 -&gt; 3</code> với tổng trọng số <code>1 + 2 = 3</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>m == edges.length</code></li>
	<li><code>1 &lt;= m &lt;= min(5 * 10<sup>4</sup>, n * (n - 1) / 2)</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li>Không có cạnh nào bị lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Dijkstra tối ưu bằng Heap

<!-- thinking:start -->

> **Tư duy**
>
> Mỗi cạnh cần được kiểm tra xem có thuộc ít nhất một đường đi ngắn nhất từ $0$– $n-1$ hay không. Nếu chạy lại Dijkstra cho từng cạnh, chúng ta sẽ lặp lại cùng một phép tìm kiếm $m$ lần.
>
> Cạnh vô hướng $(a,b,w)$ nằm trên một đường đi ngắn nhất khi lần ngược từ $n-1$ theo điều kiện $dist[a]=dist[b]+w$ có thể đi đến cạnh đó. Một BFS ngược sẽ đánh dấu mọi cạnh như vậy.
>
> Tính $dist$ bằng Dijkstra với heap, sau đó BFS từ $n-1$ và đánh dấu các cạnh thỏa mãn đẳng thức. Nếu không thể đi đến $n-1$, mọi đáp án đều là false.

<!-- thinking:end -->

Đầu tiên, chúng ta tạo một danh sách kề $g$ để lưu các cạnh của đồ thị. Sau đó, chúng ta tạo một mảng $dist$ để lưu khoảng cách ngắn nhất từ node $0$ đến các node khác. Khởi tạo $dist[0] = 0$, còn khoảng cách đến các node khác được khởi tạo bằng vô cùng.

Tiếp theo, chúng ta sử dụng thuật toán Dijkstra để tính khoảng cách ngắn nhất từ node $0$ đến các node khác. Các bước cụ thể như sau:

1. Tạo một hàng đợi ưu tiên $q$ để lưu khoảng cách và số hiệu node. Ban đầu, thêm node $0$ với khoảng cách $0$ vào hàng đợi.
2. Lấy một node $a$ ra khỏi hàng đợi. Nếu khoảng cách $da$ của $a$ lớn hơn $dist[a]$, điều đó có nghĩa là $a$ đã được cập nhật, nên chúng ta bỏ qua node này.
3. Duyệt qua tất cả node kề $b$ của node $a$. Nếu $dist[b] > dist[a] + w$, cập nhật $dist[b] = dist[a] + w$ và thêm node $b$ vào hàng đợi.
4. Lặp lại bước 2 và 3 cho đến khi hàng đợi rỗng.

Tiếp theo, chúng ta tạo một mảng đáp án $ans$ có độ dài $m$, ban đầu tất cả phần tử đều là $false$. Nếu $dist[n - 1]$ là vô cùng, điều đó có nghĩa là không thể đi từ node $0$ đến node $n - 1$, nên trả về trực tiếp $ans$. Nếu không, bắt đầu từ node $n - 1$ và duyệt qua tất cả các cạnh. Nếu cạnh $(a, b, i)$ thỏa mãn $dist[a] = dist[b] + w$, đặt $ans[i]$ thành $true$ và thêm node $b$ vào hàng đợi.

Cuối cùng, trả về đáp án.

Độ phức tạp thời gian là $O(m \times \log m)$ và độ phức tạp không gian là $O(n + m)$, trong đó $n$ và $m$ lần lượt là số node và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findAnswer(self, n: int, edges: List[List[int]]) -> List[bool]:
        g = defaultdict(list)
        for i, (a, b, w) in enumerate(edges):
            g[a].append((b, w, i))
            g[b].append((a, w, i))
        dist = [inf] * n
        dist[0] = 0
        q = [(0, 0)]
        while q:
            da, a = heappop(q)
            if da > dist[a]:
                continue
            for b, w, _ in g[a]:
                if dist[b] > dist[a] + w:
                    dist[b] = dist[a] + w
                    heappush(q, (dist[b], b))
        m = len(edges)
        ans = [False] * m
        if dist[n - 1] == inf:
            return ans
        q = deque([n - 1])
        while q:
            a = q.popleft()
            for b, w, i in g[a]:
                if dist[a] == dist[b] + w:
                    ans[i] = True
                    q.append(b)
        return ans
```

#### Java

```java
class Solution {
    public boolean[] findAnswer(int n, int[][] edges) {
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        int m = edges.length;
        for (int i = 0; i < m; ++i) {
            int a = edges[i][0], b = edges[i][1], w = edges[i][2];
            g[a].add(new int[] {b, w, i});
            g[b].add(new int[] {a, w, i});
        }
        int[] dist = new int[n];
        final int inf = 1 << 30;
        Arrays.fill(dist, inf);
        dist[0] = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] - b[0]);
        pq.offer(new int[] {0, 0});
        while (!pq.isEmpty()) {
            var p = pq.poll();
            int da = p[0], a = p[1];
            if (da > dist[a]) {
                continue;
            }
            for (var e : g[a]) {
                int b = e[0], w = e[1];
                if (dist[b] > dist[a] + w) {
                    dist[b] = dist[a] + w;
                    pq.offer(new int[] {dist[b], b});
                }
            }
        }
        boolean[] ans = new boolean[m];
        if (dist[n - 1] == inf) {
            return ans;
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(n - 1);
        while (!q.isEmpty()) {
            int a = q.poll();
            for (var e : g[a]) {
                int b = e[0], w = e[1], i = e[2];
                if (dist[a] == dist[b] + w) {
                    ans[i] = true;
                    q.offer(b);
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
    vector<bool> findAnswer(int n, vector<vector<int>>& edges) {
        vector<vector<array<int, 3>>> g(n);
        int m = edges.size();
        for (int i = 0; i < m; ++i) {
            auto e = edges[i];
            int a = e[0], b = e[1], w = e[2];
            g[a].push_back({b, w, i});
            g[b].push_back({a, w, i});
        }
        const int inf = 1 << 30;
        vector<int> dist(n, inf);
        dist[0] = 0;

        using pii = pair<int, int>;
        priority_queue<pii, vector<pii>, greater<pii>> pq;
        pq.push({0, 0});

        while (!pq.empty()) {
            auto [da, a] = pq.top();
            pq.pop();
            if (da > dist[a]) {
                continue;
            }

            for (auto [b, w, _] : g[a]) {
                if (dist[b] > dist[a] + w) {
                    dist[b] = dist[a] + w;
                    pq.push({dist[b], b});
                }
            }
        }
        vector<bool> ans(m);
        if (dist[n - 1] == inf) {
            return ans;
        }
        queue<int> q{{n - 1}};
        while (!q.empty()) {
            int a = q.front();
            q.pop();
            for (auto [b, w, i] : g[a]) {
                if (dist[a] == dist[b] + w) {
                    ans[i] = true;
                    q.push(b);
                }
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findAnswer(n int, edges [][]int) []bool {
	g := make([][][3]int, n)
	for i, e := range edges {
		a, b, w := e[0], e[1], e[2]
		g[a] = append(g[a], [3]int{b, w, i})
		g[b] = append(g[b], [3]int{a, w, i})
	}
	dist := make([]int, n)
	const inf int = 1 << 30
	for i := range dist {
		dist[i] = inf
	}
	dist[0] = 0
	pq := hp{{0, 0}}
	for len(pq) > 0 {
		p := heap.Pop(&pq).(pair)
		da, a := p.dis, p.u
		if da > dist[a] {
			continue
		}
		for _, e := range g[a] {
			b, w := e[0], e[1]
			if dist[b] > dist[a]+w {
				dist[b] = dist[a] + w
				heap.Push(&pq, pair{dist[b], b})
			}
		}
	}
	ans := make([]bool, len(edges))
	if dist[n-1] == inf {
		return ans
	}
	q := []int{n - 1}
	for len(q) > 0 {
		a := q[0]
		q = q[1:]
		for _, e := range g[a] {
			b, w, i := e[0], e[1], e[2]
			if dist[a] == dist[b]+w {
				ans[i] = true
				q = append(q, b)
			}
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

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
