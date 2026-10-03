---
comments: true
difficulty: Hard
rating: 2364
source: Weekly Contest 284 Q4
tags:
    - Graph
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [2203. Minimum Weighted Subgraph With the Required Paths](https://leetcode.com/problems/minimum-weighted-subgraph-with-the-required-paths)

[中文文档](/solution/2200-2299/2203.Minimum%20Weighted%20Subgraph%20With%20the%20Required%20Paths/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> biểu thị số lượng đỉnh của một <strong>đồ thị có hướng</strong> có trọng số. Các đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn cũng được cho một mảng số nguyên hai chiều <code>edges</code>, trong đó <code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>, weight<sub>i</sub>]</code> biểu thị rằng tồn tại một cạnh <strong>có hướng</strong> từ <code>from<sub>i</sub></code> đến <code>to<sub>i</sub></code> với trọng số <code>weight<sub>i</sub></code>.</p>

<p>Cuối cùng, bạn được cho ba số nguyên <strong>phân biệt</strong> <code>src1</code>, <code>src2</code> và <code>dest</code>, biểu thị ba đỉnh phân biệt của đồ thị.</p>

<p>Hãy trả về <em><strong>trọng số nhỏ nhất</strong> của một đồ thị con sao cho <strong>có thể đi đến</strong></em> <code>dest</code> <em>từ cả</em> <code>src1</code> <em>và</em> <code>src2</code> <em>thông qua một tập hợp các cạnh của đồ thị con đó</em>. Nếu không tồn tại đồ thị con như vậy, hãy trả về <code>-1</code>.</p>

<p><strong>Đồ thị con</strong> là một đồ thị có các đỉnh và cạnh là các tập con của đồ thị ban đầu. <strong>Trọng số</strong> của đồ thị con là tổng trọng số của các cạnh cấu thành nó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2203.Minimum%20Weighted%20Subgraph%20With%20the%20Required%20Paths/images/example1drawio.png" style="width: 263px; height: 250px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,2,2],[0,5,6],[1,0,3],[1,4,5],[2,1,1],[2,3,3],[2,3,4],[3,4,2],[4,5,1]], src1 = 0, src2 = 1, dest = 5
<strong>Đầu ra:</strong> 9
<strong>Giải thích:</strong>
Hình trên biểu diễn đồ thị đầu vào.
Các cạnh màu xanh biểu diễn một trong những đồ thị con cho kết quả tối ưu.
Lưu ý rằng đồ thị con [[1,0,3],[0,5,6]] cũng cho kết quả tối ưu. Không thể tạo ra đồ thị con có trọng số nhỏ hơn mà vẫn thỏa mãn tất cả các ràng buộc.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2200-2299/2203.Minimum%20Weighted%20Subgraph%20With%20the%20Required%20Paths/images/example2-1drawio.png" style="width: 350px; height: 51px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1,1],[2,1,1]], src1 = 0, src2 = 1, dest = 2
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong>
Hình trên biểu diễn đồ thị đầu vào.
Có thể thấy rằng không tồn tại đường đi từ đỉnh 1 đến đỉnh 2, do đó không có đồ thị con nào thỏa mãn tất cả các ràng buộc.
</pre>

<p>&nbsp;</p>
<p><strong>Các ràng buộc:</strong></p>

<ul>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= from<sub>i</sub>, to<sub>i</sub>, src1, src2, dest &lt;= n - 1</code></li>
	<li><code>from<sub>i</sub> != to<sub>i</sub></code></li>
	<li><code>src1</code>, <code>src2</code> và <code>dest</code> đôi một phân biệt.</li>
	<li><code>1 &lt;= weight[i] &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần một đồ thị con có trọng số nhỏ nhất chứa cả hai đường đi $src_1 \to dest$ và $src_2 \to dest$. Nếu tìm riêng hai đường đi ngắn nhất, phần đuôi chung sẽ bị tính hai lần nên kết quả không phải lúc nào cũng tối ưu. Với $n, m \le 10^5$, việc liệt kê các đồ thị con là bất khả thi.
>
> Cả hai đường đi đều kết thúc tại $dest$, vì vậy chúng gặp nhau tại một đỉnh nào đó $p$ (có thể chính là $dest$). Đáp án tối ưu là tổng của ba đường đi ngắn nhất: $src_1 \to p$, $src_2 \to p$ và $p \to dest$.
>
> Chạy Dijkstra từ $src_1$ và $src_2$ trên đồ thị ban đầu, đồng thời chạy Dijkstra từ $dest$ trên đồ thị đảo. Duyệt qua mọi $p$ và lấy $\min(d_1[p]+d_2[p]+d_3[p])$, hoặc trả về $-1$ nếu giá trị vẫn là vô cùng.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumWeight(
        self, n: int, edges: List[List[int]], src1: int, src2: int, dest: int
    ) -> int:
        def dijkstra(g, u):
            dist = [inf] * n
            dist[u] = 0
            q = [(0, u)]
            while q:
                d, u = heappop(q)
                if d > dist[u]:
                    continue
                for v, w in g[u]:
                    if dist[v] > dist[u] + w:
                        dist[v] = dist[u] + w
                        heappush(q, (dist[v], v))
            return dist

        g = defaultdict(list)
        rg = defaultdict(list)
        for f, t, w in edges:
            g[f].append((t, w))
            rg[t].append((f, w))
        d1 = dijkstra(g, src1)
        d2 = dijkstra(g, src2)
        d3 = dijkstra(rg, dest)
        ans = min(sum(v) for v in zip(d1, d2, d3))
        return -1 if ans >= inf else ans
```

#### Java

```java
class Solution {
    private static final Long INF = Long.MAX_VALUE;

    public long minimumWeight(int n, int[][] edges, int src1, int src2, int dest) {
        List<Pair<Integer, Long>>[] g = new List[n];
        List<Pair<Integer, Long>>[] rg = new List[n];
        for (int i = 0; i < n; ++i) {
            g[i] = new ArrayList<>();
            rg[i] = new ArrayList<>();
        }
        for (int[] e : edges) {
            int f = e[0], t = e[1];
            long w = e[2];
            g[f].add(new Pair<>(t, w));
            rg[t].add(new Pair<>(f, w));
        }
        long[] d1 = dijkstra(g, src1);
        long[] d2 = dijkstra(g, src2);
        long[] d3 = dijkstra(rg, dest);
        long ans = -1;
        for (int i = 0; i < n; ++i) {
            if (d1[i] == INF || d2[i] == INF || d3[i] == INF) {
                continue;
            }
            long t = d1[i] + d2[i] + d3[i];
            if (ans == -1 || ans > t) {
                ans = t;
            }
        }
        return ans;
    }

    private long[] dijkstra(List<Pair<Integer, Long>>[] g, int u) {
        int n = g.length;
        long[] dist = new long[n];
        Arrays.fill(dist, INF);
        dist[u] = 0;
        PriorityQueue<Pair<Long, Integer>> q
            = new PriorityQueue<>(Comparator.comparingLong(Pair::getKey));
        q.offer(new Pair<>(0L, u));
        while (!q.isEmpty()) {
            Pair<Long, Integer> p = q.poll();
            long d = p.getKey();
            u = p.getValue();
            if (d > dist[u]) {
                continue;
            }
            for (Pair<Integer, Long> e : g[u]) {
                int v = e.getKey();
                long w = e.getValue();
                if (dist[v] > dist[u] + w) {
                    dist[v] = dist[u] + w;
                    q.offer(new Pair<>(dist[v], v));
                }
            }
        }
        return dist;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
