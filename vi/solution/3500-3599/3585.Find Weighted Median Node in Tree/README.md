---
comments: true
difficulty: Hard
rating: 2428
source: Weekly Contest 454 Q4
tags:
    - Bit Manipulation
    - Tree
    - Depth-First Search
    - Array
    - Binary Search
    - Dynamic Programming
---

<!-- problem:start -->

# [3585. Find Weighted Median Node in Tree](https://leetcode.com/problems/find-weighted-median-node-in-tree)

[中文文档](/solution/3500-3599/3585.Find%20Weighted%20Median%20Node%20in%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một số nguyên <code>n</code> và một cây <strong>vô hướng có trọng số</strong>, được gốc tại nút 0, gồm <code>n</code> nút được đánh số từ 0 đến <code>n - 1</code>. Cây được biểu diễn bằng một mảng 2D <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> biểu thị một cạnh từ nút <code>u<sub>i</sub></code> đến nút <code>v<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code>.</p>

<p><strong>nút trung vị có trọng số</strong> được định nghĩa là nút <strong>đầu tiên</strong> <code>x</code> trên đường đi từ <code>u<sub>i</sub></code> đến <code>v<sub>i</sub></code> sao cho tổng trọng số các cạnh từ <code>u<sub>i</sub></code> đến <code>x</code> <strong>lớn hơn hoặc bằng một nửa</strong> tổng trọng số của toàn bộ đường đi.</p>

<p>Bạn được cung cấp một mảng số nguyên 2D <code>queries</code>. Với mỗi <code>queries[j] = [u<sub>j</sub>, v<sub>j</sub>]</code>, hãy xác định nút trung vị có trọng số trên đường đi từ <code>u<sub>j</sub></code> đến <code>v<sub>j</sub></code>.</p>

<p>Trả về một mảng <code>ans</code>, trong đó <code>ans[j]</code> là chỉ số nút trung vị có trọng số của <code>queries[j]</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1,7]], queries = [[1,0],[0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0,1]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3585.Find%20Weighted%20Median%20Node%20in%20Tree/images/screenshot-2025-05-26-at-193447.png" style="width: 200px; height: 64px;" /></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Truy vấn</th>
			<th style="border: 1px solid black;">Đường đi</th>
			<th style="border: 1px solid black;">Trọng số<br />
			cạnh</th>
			<th style="border: 1px solid black;">Tổng<br />
			trọng số<br />
			đường đi</th>
			<th style="border: 1px solid black;">Một nửa</th>
			<th style="border: 1px solid black;">Giải thích</th>
			<th style="border: 1px solid black;">Đáp án</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 0]</code></td>
			<td style="border: 1px solid black;"><code>1 &rarr; 0</code></td>
			<td style="border: 1px solid black;"><code>[7]</code></td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">3.5</td>
			<td style="border: 1px solid black;">Tổng từ <code>1 &rarr; 0 = 7 &gt;= 3.5</code>, nút trung vị là nút 0.</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[0, 1]</code></td>
			<td style="border: 1px solid black;"><code>0 &rarr; 1</code></td>
			<td style="border: 1px solid black;"><code>[7]</code></td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">3.5</td>
			<td style="border: 1px solid black;">Tổng từ <code>0 &rarr; 1 = 7 &gt;= 3.5</code>, nút trung vị là nút 1.</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,2],[2,0,4]], queries = [[0,1],[2,0],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,0,2]</span></p>

<p><strong>G</strong><strong>iải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3585.Find%20Weighted%20Median%20Node%20in%20Tree/images/screenshot-2025-05-26-at-193610.png" style="width: 180px; height: 149px;" /></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Truy vấn</th>
			<th style="border: 1px solid black;">Đường đi</th>
			<th style="border: 1px solid black;">Trọng số<br />
			cạnh</th>
			<th style="border: 1px solid black;">Tổng<br />
			trọng số<br />
			đường đi</th>
			<th style="border: 1px solid black;">Một nửa</th>
			<th style="border: 1px solid black;">Giải thích</th>
			<th style="border: 1px solid black;">Đáp án</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;"><code>[0, 1]</code></td>
			<td style="border: 1px solid black;"><code>0 &rarr; 1</code></td>
			<td style="border: 1px solid black;"><code>[2]</code></td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">1</td>
			<td style="border: 1px solid black;">Tổng từ <code>0 &rarr; 1 = 2 &gt;= 1</code>, nút trung vị là nút 1.</td>
			<td style="border: 1px solid black;">1</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[2, 0]</code></td>
			<td style="border: 1px solid black;"><code>2 &rarr; 0</code></td>
			<td style="border: 1px solid black;"><code>[4]</code></td>
			<td style="border: 1px solid black;">4</td>
			<td style="border: 1px solid black;">2</td>
			<td style="border: 1px solid black;">Tổng từ <code>2 &rarr; 0 = 4 &gt;= 2</code>, nút trung vị là nút 0.</td>
			<td style="border: 1px solid black;">0</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;"><code>1 &rarr; 0 &rarr; 2</code></td>
			<td style="border: 1px solid black;"><code>[2, 4]</code></td>
			<td style="border: 1px solid black;">6</td>
			<td style="border: 1px solid black;">3</td>
			<td style="border: 1px solid black;">Tổng từ <code>1 &rarr; 0 = 2 &lt; 3</code>.<br />
			Tổng từ <code>1 &rarr; 2 = 2 + 4 = 6 &gt;= 3</code>, nút trung vị là nút 2.</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[0,1,2],[0,2,5],[1,3,1],[2,4,3]], queries = [[3,4],[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3500-3599/3585.Find%20Weighted%20Median%20Node%20in%20Tree/images/screenshot-2025-05-26-at-193857.png" style="width: 150px; height: 229px;" /></p>

<table style="border: 1px solid black;">
	<thead>
		<tr>
			<th style="border: 1px solid black;">Truy vấn</th>
			<th style="border: 1px solid black;">Đường đi</th>
			<th style="border: 1px solid black;">Trọng số<br />
			cạnh</th>
			<th style="border: 1px solid black;">Tổng<br />
			trọng số<br />
			đường đi</th>
			<th style="border: 1px solid black;">Một nửa</th>
			<th style="border: 1px solid black;">Giải thích</th>
			<th style="border: 1px solid black;">Đáp án</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style="border: 1px solid black;"><code>[3, 4]</code></td>
			<td style="border: 1px solid black;"><code>3 &rarr; 1 &rarr; 0 &rarr; 2 &rarr; 4</code></td>
			<td style="border: 1px solid black;"><code>[1, 2, 5, 3]</code></td>
			<td style="border: 1px solid black;">11</td>
			<td style="border: 1px solid black;">5.5</td>
			<td style="border: 1px solid black;">Tổng từ <code>3 &rarr; 1 = 1 &lt; 5.5</code>.<br />
			Tổng từ <code>3 &rarr; 0 = 1 + 2 = 3 &lt; 5.5</code>.<br />
			Tổng từ <code>3 &rarr; 2 = 1 + 2 + 5 = 8 &gt;= 5.5</code>, nút trung vị là nút 2.</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
		<tr>
			<td style="border: 1px solid black;"><code>[1, 2]</code></td>
			<td style="border: 1px solid black;"><code>1 &rarr; 0 &rarr; 2</code></td>
			<td style="border: 1px solid black;"><code>[2, 5]</code></td>
			<td style="border: 1px solid black;">7</td>
			<td style="border: 1px solid black;">3.5</td>
			<td style="border: 1px solid black;">
			<p>Tổng từ <code>1 &rarr; 0 = 2 &lt; 3.5</code>.<br />
			Tổng từ <code>1 &rarr; 2 = 2 + 5 = 7 &gt;= 3.5</code>, nút trung vị là nút 2.</p>
			</td>
			<td style="border: 1px solid black;">2</td>
		</tr>
	</tbody>
</table>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i] == [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= w<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[j] == [u<sub>j</sub>, v<sub>j</sub>]</code></li>
	<li><code>0 &lt;= u<sub>j</sub>, v<sub>j</sub> &lt; n</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: LCA + Binary Lifting

<!-- thinking:start -->

> **Tư duy**
>
> Nút trung vị có trọng số là đỉnh đầu tiên trên $u \to v$ mà tổng trọng số prefix tính từ $u$ lớn hơn hoặc bằng một nửa đường đi. Với $n,q \le 10^5$, ta cần LCA và các prefix có trọng số.
>
> Sau khi có $lca$ và tổng $W$, ta binary lift trên $u \to lca$ hoặc $lca \to v$ để tìm nút xa nhất có prefix vẫn $< W/2$, rồi đi thêm một bước.

<!-- thinking:end -->

Gốc hóa cây tại nút $0$. Dùng BFS để tính độ sâu $\textit{depth}$ của mỗi nút, nút cha $p$ và khoảng cách có trọng số $\textit{dist}$ từ gốc, đồng thời xây dựng bảng binary lifting $f[i][j]$ là tổ tiên thứ $2^j$ của $i$.

Với một truy vấn $(u, v)$: nếu $u = v$, đáp án là $u$. Nếu không, đặt $x = \textit{lca}(u, v)$ và $W = \textit{dist}[u] + \textit{dist}[v] - 2 \cdot \textit{dist}[x]$. Nút trung vị có trọng số là nút đầu tiên trên đường đi bắt đầu từ $u$ có tổng trọng số prefix lớn hơn hoặc bằng $W / 2$. Ta so sánh $2 \cdot \textit{pref} \ge W$ để tránh phép tính số thực.

- Nếu $2 \cdot (\textit{dist}[u] - \textit{dist}[x]) \ge W$, median nằm trên $u \to x$ (bao gồm cả $x$). Nâng từ $u$ lên tổ tiên xa nhất $k$ vẫn thỏa mãn $2 \cdot (\textit{dist}[u] - \textit{dist}[k]) < W$, sau đó đi lên cha của nó một bước đến $p[k]$.
- Ngược lại, median nằm trên $x \to v$ (không bao gồm $x$). Nâng từ $v$ lên nút cao nhất có độ sâu lớn hơn $x$ và $2 \cdot (\textit{dist}[u] + \textit{dist}[k] - 2 \cdot \textit{dist}[x]) \ge W$.

Độ phức tạp thời gian là $O((n + q) \times \log n)$, còn độ phức tạp không gian là $O(n \times \log n)$, trong đó $n$ là số nút và $q$ là số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findMedian(
        self, n: int, edges: List[List[int]], queries: List[List[int]]
    ) -> List[int]:
        m = n.bit_length()
        g = [[] for _ in range(n)]
        for u, v, w in edges:
            g[u].append((v, w))
            g[v].append((u, w))
        f = [[0] * m for _ in range(n)]
        p = [0] * n
        depth = [0] * n
        dist = [0] * n
        q = deque([0])
        while q:
            i = q.popleft()
            f[i][0] = p[i]
            for j in range(1, m):
                f[i][j] = f[f[i][j - 1]][j - 1]
            for j, w in g[i]:
                if j != p[i]:
                    p[j] = i
                    depth[j] = depth[i] + 1
                    dist[j] = dist[i] + w
                    q.append(j)
        ans = []
        for u, v in queries:
            if u == v:
                ans.append(u)
                continue
            x, y = u, v
            if depth[x] < depth[y]:
                x, y = y, x
            for j in range(m - 1, -1, -1):
                if depth[x] - depth[y] >= (1 << j):
                    x = f[x][j]
            for j in range(m - 1, -1, -1):
                if f[x][j] != f[y][j]:
                    x, y = f[x][j], f[y][j]
            if x != y:
                x = p[x]
            w = dist[u] + dist[v] - 2 * dist[x]
            if 2 * (dist[u] - dist[x]) >= w:
                cur = u
                for j in range(m - 1, -1, -1):
                    k = f[cur][j]
                    if depth[k] >= depth[x] and 2 * (dist[u] - dist[k]) < w:
                        cur = k
                ans.append(p[cur])
            else:
                cur = v
                for j in range(m - 1, -1, -1):
                    k = f[cur][j]
                    if (
                        depth[k] > depth[x]
                        and 2 * (dist[u] + dist[k] - 2 * dist[x]) >= w
                    ):
                        cur = k
                ans.append(cur)
        return ans
```

#### Java

```java
class Solution {
    public int[] findMedian(int n, int[][] edges, int[][] queries) {
        int m = 32 - Integer.numberOfLeadingZeros(n);
        List<int[]>[] g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].add(new int[] {v, w});
            g[v].add(new int[] {u, w});
        }
        int[][] f = new int[n][m];
        int[] p = new int[n];
        int[] depth = new int[n];
        long[] dist = new long[n];
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(0);
        while (!q.isEmpty()) {
            int i = q.poll();
            f[i][0] = p[i];
            for (int j = 1; j < m; ++j) {
                f[i][j] = f[f[i][j - 1]][j - 1];
            }
            for (var nxt : g[i]) {
                int j = nxt[0], w = nxt[1];
                if (j != p[i]) {
                    p[j] = i;
                    depth[j] = depth[i] + 1;
                    dist[j] = dist[i] + w;
                    q.offer(j);
                }
            }
        }
        int[] ans = new int[queries.length];
        for (int i = 0; i < queries.length; ++i) {
            int u = queries[i][0], v = queries[i][1];
            if (u == v) {
                ans[i] = u;
                continue;
            }
            int x = u, y = v;
            if (depth[x] < depth[y]) {
                int t = x;
                x = y;
                y = t;
            }
            for (int j = m - 1; j >= 0; --j) {
                if (depth[x] - depth[y] >= (1 << j)) {
                    x = f[x][j];
                }
            }
            for (int j = m - 1; j >= 0; --j) {
                if (f[x][j] != f[y][j]) {
                    x = f[x][j];
                    y = f[y][j];
                }
            }
            if (x != y) {
                x = p[x];
            }
            long w = dist[u] + dist[v] - 2 * dist[x];
            if (2 * (dist[u] - dist[x]) >= w) {
                int cur = u;
                for (int j = m - 1; j >= 0; --j) {
                    int k = f[cur][j];
                    if (depth[k] >= depth[x] && 2 * (dist[u] - dist[k]) < w) {
                        cur = k;
                    }
                }
                ans[i] = p[cur];
            } else {
                int cur = v;
                for (int j = m - 1; j >= 0; --j) {
                    int k = f[cur][j];
                    if (depth[k] > depth[x] && 2 * (dist[u] + dist[k] - 2 * dist[x]) >= w) {
                        cur = k;
                    }
                }
                ans[i] = cur;
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
    vector<int> findMedian(int n, vector<vector<int>>& edges, vector<vector<int>>& queries) {
        int m = 32 - __builtin_clz(n);
        vector<vector<pair<int, int>>> g(n);
        for (auto& e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].emplace_back(v, w);
            g[v].emplace_back(u, w);
        }
        vector<vector<int>> f(n, vector<int>(m));
        vector<int> p(n), depth(n);
        vector<long long> dist(n);
        queue<int> q;
        q.push(0);
        while (!q.empty()) {
            int i = q.front();
            q.pop();
            f[i][0] = p[i];
            for (int j = 1; j < m; ++j) {
                f[i][j] = f[f[i][j - 1]][j - 1];
            }
            for (auto [j, w] : g[i]) {
                if (j != p[i]) {
                    p[j] = i;
                    depth[j] = depth[i] + 1;
                    dist[j] = dist[i] + w;
                    q.push(j);
                }
            }
        }
        vector<int> ans;
        for (auto& qq : queries) {
            int u = qq[0], v = qq[1];
            if (u == v) {
                ans.push_back(u);
                continue;
            }
            int x = u, y = v;
            if (depth[x] < depth[y]) {
                swap(x, y);
            }
            for (int j = m - 1; ~j; --j) {
                if (depth[x] - depth[y] >= (1 << j)) {
                    x = f[x][j];
                }
            }
            for (int j = m - 1; ~j; --j) {
                if (f[x][j] != f[y][j]) {
                    x = f[x][j];
                    y = f[y][j];
                }
            }
            if (x != y) {
                x = p[x];
            }
            long long w = dist[u] + dist[v] - 2 * dist[x];
            if (2 * (dist[u] - dist[x]) >= w) {
                int cur = u;
                for (int j = m - 1; ~j; --j) {
                    int k = f[cur][j];
                    if (depth[k] >= depth[x] && 2 * (dist[u] - dist[k]) < w) {
                        cur = k;
                    }
                }
                ans.push_back(p[cur]);
            } else {
                int cur = v;
                for (int j = m - 1; ~j; --j) {
                    int k = f[cur][j];
                    if (depth[k] > depth[x] && 2 * (dist[u] + dist[k] - 2 * dist[x]) >= w) {
                        cur = k;
                    }
                }
                ans.push_back(cur);
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findMedian(n int, edges [][]int, queries [][]int) []int {
	m := bits.Len(uint(n))
	g := make([][][2]int, n)
	for _, e := range edges {
		u, v, w := e[0], e[1], e[2]
		g[u] = append(g[u], [2]int{v, w})
		g[v] = append(g[v], [2]int{u, w})
	}
	f := make([][]int, n)
	for i := range f {
		f[i] = make([]int, m)
	}
	p := make([]int, n)
	depth := make([]int, n)
	dist := make([]int, n)
	q := []int{0}
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		f[i][0] = p[i]
		for j := 1; j < m; j++ {
			f[i][j] = f[f[i][j-1]][j-1]
		}
		for _, nxt := range g[i] {
			j, w := nxt[0], nxt[1]
			if j != p[i] {
				p[j] = i
				depth[j] = depth[i] + 1
				dist[j] = dist[i] + w
				q = append(q, j)
			}
		}
	}
	ans := make([]int, len(queries))
	for i, qq := range queries {
		u, v := qq[0], qq[1]
		if u == v {
			ans[i] = u
			continue
		}
		x, y := u, v
		if depth[x] < depth[y] {
			x, y = y, x
		}
		for j := m - 1; j >= 0; j-- {
			if depth[x]-depth[y] >= 1<<j {
				x = f[x][j]
			}
		}
		for j := m - 1; j >= 0; j-- {
			if f[x][j] != f[y][j] {
				x, y = f[x][j], f[y][j]
			}
		}
		if x != y {
			x = p[x]
		}
		w := dist[u] + dist[v] - 2*dist[x]
		if 2*(dist[u]-dist[x]) >= w {
			cur := u
			for j := m - 1; j >= 0; j-- {
				k := f[cur][j]
				if depth[k] >= depth[x] && 2*(dist[u]-dist[k]) < w {
					cur = k
				}
			}
			ans[i] = p[cur]
		} else {
			cur := v
			for j := m - 1; j >= 0; j-- {
				k := f[cur][j]
				if depth[k] > depth[x] && 2*(dist[u]+dist[k]-2*dist[x]) >= w {
					cur = k
				}
			}
			ans[i] = cur
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findMedian(n: number, edges: number[][], queries: number[][]): number[] {
    const m = 32 - Math.clz32(n);
    const g: number[][][] = Array.from({ length: n }, () => []);
    for (const [u, v, w] of edges) {
        g[u].push([v, w]);
        g[v].push([u, w]);
    }
    const f: number[][] = Array.from({ length: n }, () => Array(m).fill(0));
    const p: number[] = Array(n).fill(0);
    const depth: number[] = Array(n).fill(0);
    const dist: number[] = Array(n).fill(0);
    const q: number[] = [0];
    for (let qq = 0; qq < q.length; ++qq) {
        const i = q[qq];
        f[i][0] = p[i];
        for (let j = 1; j < m; ++j) {
            f[i][j] = f[f[i][j - 1]][j - 1];
        }
        for (const [j, w] of g[i]) {
            if (j !== p[i]) {
                p[j] = i;
                depth[j] = depth[i] + 1;
                dist[j] = dist[i] + w;
                q.push(j);
            }
        }
    }
    const ans: number[] = [];
    for (const [u, v] of queries) {
        if (u === v) {
            ans.push(u);
            continue;
        }
        let x = u,
            y = v;
        if (depth[x] < depth[y]) {
            [x, y] = [y, x];
        }
        for (let j = m - 1; j >= 0; --j) {
            if (depth[x] - depth[y] >= 1 << j) {
                x = f[x][j];
            }
        }
        for (let j = m - 1; j >= 0; --j) {
            if (f[x][j] !== f[y][j]) {
                x = f[x][j];
                y = f[y][j];
            }
        }
        if (x !== y) {
            x = p[x];
        }
        const w = dist[u] + dist[v] - 2 * dist[x];
        if (2 * (dist[u] - dist[x]) >= w) {
            let cur = u;
            for (let j = m - 1; j >= 0; --j) {
                const k = f[cur][j];
                if (depth[k] >= depth[x] && 2 * (dist[u] - dist[k]) < w) {
                    cur = k;
                }
            }
            ans.push(p[cur]);
        } else {
            let cur = v;
            for (let j = m - 1; j >= 0; --j) {
                const k = f[cur][j];
                if (depth[k] > depth[x] && 2 * (dist[u] + dist[k] - 2 * dist[x]) >= w) {
                    cur = k;
                }
            }
            ans.push(cur);
        }
    }
    return ans;
}
```

#### Rust

```rust
use std::collections::VecDeque;

impl Solution {
    pub fn find_median(n: i32, edges: Vec<Vec<i32>>, queries: Vec<Vec<i32>>) -> Vec<i32> {
        let n = n as usize;
        let m = 32 - (n as u32).leading_zeros() as usize;
        let mut g = vec![vec![]; n];
        for e in &edges {
            let u = e[0] as usize;
            let v = e[1] as usize;
            let w = e[2] as i64;
            g[u].push((v, w));
            g[v].push((u, w));
        }
        let mut f = vec![vec![0; m]; n];
        let mut p = vec![0; n];
        let mut depth = vec![0; n];
        let mut dist = vec![0i64; n];
        let mut q = VecDeque::new();
        q.push_back(0);
        while let Some(i) = q.pop_front() {
            f[i][0] = p[i];
            for j in 1..m {
                f[i][j] = f[f[i][j - 1]][j - 1];
            }
            for &(j, w) in &g[i] {
                if j != p[i] {
                    p[j] = i;
                    depth[j] = depth[i] + 1;
                    dist[j] = dist[i] + w;
                    q.push_back(j);
                }
            }
        }
        let mut ans = Vec::with_capacity(queries.len());
        for qq in &queries {
            let u = qq[0] as usize;
            let v = qq[1] as usize;
            if u == v {
                ans.push(u as i32);
                continue;
            }
            let (mut x, mut y) = (u, v);
            if depth[x] < depth[y] {
                std::mem::swap(&mut x, &mut y);
            }
            for j in (0..m).rev() {
                if depth[x] - depth[y] >= (1 << j) {
                    x = f[x][j];
                }
            }
            for j in (0..m).rev() {
                if f[x][j] != f[y][j] {
                    x = f[x][j];
                    y = f[y][j];
                }
            }
            if x != y {
                x = p[x];
            }
            let w = dist[u] + dist[v] - 2 * dist[x];
            if 2 * (dist[u] - dist[x]) >= w {
                let mut cur = u;
                for j in (0..m).rev() {
                    let k = f[cur][j];
                    if depth[k] >= depth[x] && 2 * (dist[u] - dist[k]) < w {
                        cur = k;
                    }
                }
                ans.push(p[cur] as i32);
            } else {
                let mut cur = v;
                for j in (0..m).rev() {
                    let k = f[cur][j];
                    if depth[k] > depth[x] && 2 * (dist[u] + dist[k] - 2 * dist[x]) >= w {
                        cur = k;
                    }
                }
                ans.push(cur as i32);
            }
        }
        ans
    }
}
```

#### C#

```cs
public class Solution {
    public int[] FindMedian(int n, int[][] edges, int[][] queries) {
        int m = 32 - BitOperations.LeadingZeroCount((uint)n);
        List<int[]>[] g = new List<int[]>[n];
        for (int i = 0; i < n; ++i) {
            g[i] = new List<int[]>();
        }
        foreach (var e in edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u].Add(new int[] { v, w });
            g[v].Add(new int[] { u, w });
        }
        int[][] f = new int[n][];
        for (int i = 0; i < n; ++i) {
            f[i] = new int[m];
        }
        int[] p = new int[n];
        int[] depth = new int[n];
        long[] dist = new long[n];
        Queue<int> q = new Queue<int>();
        q.Enqueue(0);
        while (q.Count > 0) {
            int i = q.Dequeue();
            f[i][0] = p[i];
            for (int j = 1; j < m; ++j) {
                f[i][j] = f[f[i][j - 1]][j - 1];
            }
            foreach (var nxt in g[i]) {
                int j = nxt[0], w = nxt[1];
                if (j != p[i]) {
                    p[j] = i;
                    depth[j] = depth[i] + 1;
                    dist[j] = dist[i] + w;
                    q.Enqueue(j);
                }
            }
        }
        int[] ans = new int[queries.Length];
        for (int i = 0; i < queries.Length; ++i) {
            int u = queries[i][0], v = queries[i][1];
            if (u == v) {
                ans[i] = u;
                continue;
            }
            int x = u, y = v;
            if (depth[x] < depth[y]) {
                int t = x;
                x = y;
                y = t;
            }
            for (int j = m - 1; j >= 0; --j) {
                if (depth[x] - depth[y] >= (1 << j)) {
                    x = f[x][j];
                }
            }
            for (int j = m - 1; j >= 0; --j) {
                if (f[x][j] != f[y][j]) {
                    x = f[x][j];
                    y = f[y][j];
                }
            }
            if (x != y) {
                x = p[x];
            }
            long w = dist[u] + dist[v] - 2 * dist[x];
            if (2 * (dist[u] - dist[x]) >= w) {
                int cur = u;
                for (int j = m - 1; j >= 0; --j) {
                    int k = f[cur][j];
                    if (depth[k] >= depth[x] && 2 * (dist[u] - dist[k]) < w) {
                        cur = k;
                    }
                }
                ans[i] = p[cur];
            } else {
                int cur = v;
                for (int j = m - 1; j >= 0; --j) {
                    int k = f[cur][j];
                    if (depth[k] > depth[x] && 2 * (dist[u] + dist[k] - 2 * dist[x]) >= w) {
                        cur = k;
                    }
                }
                ans[i] = cur;
            }
        }
        return ans;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
