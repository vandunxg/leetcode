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

# [2737. Find the Closest Marked Node 🔒](https://leetcode.com/problems/find-the-closest-marked-node)

[中文文档](/solution/2700-2799/2737.Find%20the%20Closest%20Marked%20Node/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên dương <code>n</code>, là số lượng node của một đồ thị <strong>có hướng, có trọng số và được đánh chỉ số từ 0</strong>, cùng một <strong>mảng 2 chiều</strong> <code>edges</code> được <strong>đánh chỉ số từ 0</strong>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> cho biết có một cạnh từ node <code>u<sub>i</sub></code> đến node <code>v<sub>i</sub></code> với trọng số <code>w<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một node <code>s</code> và một mảng node <code>marked</code>; nhiệm vụ của bạn là tìm khoảng cách <strong>nhỏ nhất</strong> từ <code>s</code> đến <strong>bất kỳ</strong> node nào trong <code>marked</code>.</p>

<p>Trả về <em>một số nguyên biểu thị khoảng cách nhỏ nhất từ </em><code>s</code><em> đến bất kỳ node nào trong </em><code>marked</code><em>, hoặc </em><code>-1</code><em> nếu không có đường đi từ s đến bất kỳ node nào được đánh dấu</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1,1],[1,2,3],[2,3,2],[0,3,4]], s = 0, marked = [2,3]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Có một đường đi từ node 0 (node màu xanh lá) đến node 2 (node màu đỏ), đó là 0-&gt;1-&gt;2, với khoảng cách 1 + 3 = 4.
Có hai đường đi từ node 0 đến node 3 (node màu đỏ), đó là 0-&gt;1-&gt;2-&gt;3 và 0-&gt;3. Đường đi thứ nhất có khoảng cách 1 + 3 + 2 = 6, còn đường đi thứ hai có khoảng cách 4.
Giá trị nhỏ nhất trong hai khoảng cách là 4.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2737.Find%20the%20Closest%20Marked%20Node/images/image_2023-06-13_16-34-38.png" style="width: 185px; height: 180px;" /></p>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[0,1,2],[0,2,4],[1,3,1],[2,3,3],[3,4,2]], s = 1, marked = [0,4]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Không có đường đi từ node 1 (node màu xanh lá) đến node 0 (node màu đỏ).
Có một đường đi từ node 1 đến node 4 (node màu đỏ), đó là 1-&gt;3-&gt;4, với khoảng cách 1 + 2 = 3.
Vì vậy, đáp án là 3.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2737.Find%20the%20Closest%20Marked%20Node/images/image_2023-06-13_16-35-13.png" style="width: 300px; height: 285px;" /></p>

<p><strong class="example">Ví dụ 3:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 4, edges = [[0,1,1],[1,2,3],[2,3,2]], s = 3, marked = [0,1]
<strong>Đầu ra:</strong> -1
<strong>Giải thích:</strong> Không có đường đi từ node 3 (node màu xanh lá) đến bất kỳ node được đánh dấu nào (các node màu đỏ), nên đáp án là -1.
</pre>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2700-2799/2737.Find%20the%20Closest%20Marked%20Node/images/image_2023-06-13_16-35-47.png" style="width: 420px; height: 80px;" /></p>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 500</code></li>
	<li><code>1 &lt;= edges.length &lt;= 10<sup>4</sup></code></li>
	<li><code>edges[i].length = 3</code></li>
	<li><code>0 &lt;= edges[i][0], edges[i][1] &lt;= n - 1</code></li>
	<li><code>1 &lt;= edges[i][2] &lt;=&nbsp;10<sup>6</sup></code></li>
	<li><code>1 &lt;= marked.length&nbsp;&lt;= n - 1</code></li>
	<li><code>0 &lt;= s, marked[i]&nbsp;&lt;= n - 1</code></li>
	<li><code>s != marked[i]</code></li>
	<li><code>marked[i] != marked[j]</code> với mọi <code>i != j</code></li>
	<li>Đồ thị có thể có các <strong>cạnh trùng lặp</strong>.</li>
	<li>Đồ thị được tạo sao cho không có <strong>self-loop</strong>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần tìm đường đi ngắn nhất từ $s$ đến bất kỳ node được đánh dấu nào trong một đồ thị có hướng, có trọng số. Vì $n$ đủ nhỏ, ta có thể chạy Dijkstra trên ma trận kề mà không cần heap.
>
> Liên tục chọn node chưa sử dụng gần nhất và relax các cạnh đi ra từ node đó, sau đó lấy giá trị nhỏ nhất của $dist$ trên các node được đánh dấu; nếu giá trị này vẫn là vô cùng thì trả về $-1$.

<!-- thinking:end -->

Trước hết, ta xây dựng một ma trận kề $g$ dựa trên thông tin các cạnh được cung cấp trong đề bài, trong đó $g[i][j]$ biểu diễn khoảng cách từ node $i$ đến node $j$. Nếu không tồn tại cạnh như vậy, $g[i][j]$ được gán bằng dương vô cùng.

Sau đó, ta sử dụng thuật toán Dijkstra để tìm khoảng cách ngắn nhất từ node bắt đầu $s$ đến tất cả các node, ký hiệu là $dist$.

Cuối cùng, ta duyệt qua tất cả các node được đánh dấu và tìm node có khoảng cách nhỏ nhất. Nếu khoảng cách là dương vô cùng, ta trả về $-1$.

Độ phức tạp thời gian là $O(n^2)$, và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def minimumDistance(
        self, n: int, edges: List[List[int]], s: int, marked: List[int]
    ) -> int:
        g = [[inf] * n for _ in range(n)]
        for u, v, w in edges:
            g[u][v] = min(g[u][v], w)
        dist = [inf] * n
        vis = [False] * n
        dist[s] = 0
        for _ in range(n):
            t = -1
            for j in range(n):
                if not vis[j] and (t == -1 or dist[t] > dist[j]):
                    t = j
            vis[t] = True
            for j in range(n):
                dist[j] = min(dist[j], dist[t] + g[t][j])
        ans = min(dist[i] for i in marked)
        return -1 if ans >= inf else ans
```

#### Java

```java
class Solution {
    public int minimumDistance(int n, List<List<Integer>> edges, int s, int[] marked) {
        final int inf = 1 << 29;
        int[][] g = new int[n][n];
        for (var e : g) {
            Arrays.fill(e, inf);
        }
        for (var e : edges) {
            int u = e.get(0), v = e.get(1), w = e.get(2);
            g[u][v] = Math.min(g[u][v], w);
        }
        int[] dist = new int[n];
        Arrays.fill(dist, inf);
        dist[s] = 0;
        boolean[] vis = new boolean[n];
        for (int i = 0; i < n; ++i) {
            int t = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[t] + g[t][j]);
            }
        }
        int ans = inf;
        for (int i : marked) {
            ans = Math.min(ans, dist[i]);
        }
        return ans >= inf ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumDistance(int n, vector<vector<int>>& edges, int s, vector<int>& marked) {
        const int inf = 1 << 29;
        vector<vector<int>> g(n, vector<int>(n, inf));
        vector<int> dist(n, inf);
        dist[s] = 0;
        vector<bool> vis(n);
        for (auto& e : edges) {
            int u = e[0], v = e[1], w = e[2];
            g[u][v] = min(g[u][v], w);
        }
        for (int i = 0; i < n; ++i) {
            int t = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (t == -1 || dist[t] > dist[j])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = min(dist[j], dist[t] + g[t][j]);
            }
        }
        int ans = inf;
        for (int i : marked) {
            ans = min(ans, dist[i]);
        }
        return ans >= inf ? -1 : ans;
    }
};
```

#### Go

```go
func minimumDistance(n int, edges [][]int, s int, marked []int) int {
	const inf = 1 << 29
	g := make([][]int, n)
	dist := make([]int, n)
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = inf
		}
		dist[i] = inf
	}
	dist[s] = 0
	for _, e := range edges {
		u, v, w := e[0], e[1], e[2]
		g[u][v] = min(g[u][v], w)
	}
	vis := make([]bool, n)
	for _ = range g {
		t := -1
		for j := 0; j < n; j++ {
			if !vis[j] && (t == -1 || dist[j] < dist[t]) {
				t = j
			}
		}
		vis[t] = true
		for j := 0; j < n; j++ {
			dist[j] = min(dist[j], dist[t]+g[t][j])
		}
	}
	ans := inf
	for _, i := range marked {
		ans = min(ans, dist[i])
	}
	if ans >= inf {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minimumDistance(n: number, edges: number[][], s: number, marked: number[]): number {
    const inf = 1 << 29;
    const g: number[][] = Array(n)
        .fill(0)
        .map(() => Array(n).fill(inf));
    const dist: number[] = Array(n).fill(inf);
    const vis: boolean[] = Array(n).fill(false);
    for (const [u, v, w] of edges) {
        g[u][v] = Math.min(g[u][v], w);
    }
    dist[s] = 0;
    for (let i = 0; i < n; ++i) {
        let t = -1;
        for (let j = 0; j < n; ++j) {
            if (!vis[j] && (t == -1 || dist[t] > dist[j])) {
                t = j;
            }
        }
        vis[t] = true;
        for (let j = 0; j < n; ++j) {
            dist[j] = Math.min(dist[j], dist[t] + g[t][j]);
        }
    }
    let ans = inf;
    for (const i of marked) {
        ans = Math.min(ans, dist[i]);
    }
    return ans >= inf ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
