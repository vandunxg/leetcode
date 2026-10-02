---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Dynamic Programming
    - Shortest Path
    - Heap (Priority Queue)
---

<!-- problem:start -->

# [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops)

[中文文档](/solution/0700-0799/0787.Cheapest%20Flights%20Within%20K%20Stops/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> thành phố được kết nối bằng các chuyến bay. Cho mảng <code>flights</code>, trong đó <code>flights[i] = [from<sub>i</sub>, to<sub>i</sub>, price<sub>i</sub>]</code> biểu thị chuyến bay từ thành phố <code>from<sub>i</sub></code> đến thành phố <code>to<sub>i</sub></code> với giá <code>price<sub>i</sub></code>.</p>

<p>Cho thêm ba số nguyên <code>src</code>, <code>dst</code> và <code>k</code>. Hãy trả về <em><strong>giá vé rẻ nhất</strong> từ </em><code>src</code><em> đến </em><code>dst</code><em> với tối đa </em><code>k</code><em> điểm dừng. </em>Nếu không có lộ trình phù hợp, trả về <code>-1</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0787.Cheapest%20Flights%20Within%20K%20Stops/images/cheapest-flights-within-k-stops-3drawio.png" style="width: 332px; height: 392px;" />
<pre>
<strong>Đầu vào:</strong> n = 4, flights = [[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]], src = 0, dst = 3, k = 1
<strong>Đầu ra:</strong> 700
<strong>Giải thích:</strong>
Đồ thị được minh họa ở trên.
Đường đi tối ưu với tối đa 1 điểm dừng từ thành phố 0 đến 3 được đánh dấu màu đỏ, có chi phí 100 + 600 = 700.
Lưu ý đường đi qua các thành phố [0,1,2,3] rẻ hơn nhưng không hợp lệ vì có 2 điểm dừng.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0787.Cheapest%20Flights%20Within%20K%20Stops/images/cheapest-flights-within-k-stops-1drawio.png" style="width: 332px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
<strong>Đầu ra:</strong> 200
<strong>Giải thích:</strong>
Đồ thị được minh họa ở trên.
Đường đi tối ưu với tối đa 1 điểm dừng từ thành phố 0 đến 2 được đánh dấu màu đỏ, có chi phí 100 + 100 = 200.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0700-0799/0787.Cheapest%20Flights%20Within%20K%20Stops/images/cheapest-flights-within-k-stops-2drawio.png" style="width: 332px; height: 242px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
<strong>Đầu ra:</strong> 500
<strong>Giải thích:</strong>
Đồ thị được minh họa ở trên.
Đường đi tối ưu không có điểm dừng từ thành phố 0 đến 2 được đánh dấu màu đỏ, có chi phí 500.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>0 &lt;= flights.length &lt;= (n * (n - 1) / 2)</code></li>
	<li><code>flights[i].length == 3</code></li>
	<li><code>0 &lt;= from<sub>i</sub>, to<sub>i</sub> &lt; n</code></li>
	<li><code>from<sub>i</sub> != to<sub>i</sub></code></li>
	<li><code>1 &lt;= price<sub>i</sub> &lt;= 10<sup>4</sup></code></li>
	<li>Sẽ không có nhiều chuyến bay giữa cùng một cặp thành phố.</li>
	<li><code>0 &lt;= src, dst, k &lt; n</code></li>
	<li><code>src != dst</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Tìm lộ trình rẻ nhất với tối đa $k$ điểm dừng. Dijkstra cần thêm một chiều trạng thái cho số điểm dừng; với $n\le 100$, Bellman-Ford chạy $k+1$ vòng là đủ.
>
> Tối đa $k$ điểm dừng tương ứng với $k+1$ cạnh. Mỗi vòng relax dựa trên bản sao khoảng cách của vòng trước, nên mỗi vòng chỉ xét thêm một cạnh.
>
> Sau $k+1$ vòng, nếu $dist[dst]$ vẫn là vô cực thì không thể đến đích.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findCheapestPrice(
        self, n: int, flights: List[List[int]], src: int, dst: int, k: int
    ) -> int:
        INF = 0x3F3F3F3F
        dist = [INF] * n
        dist[src] = 0
        for _ in range(k + 1):
            backup = dist.copy()
            for f, t, p in flights:
                dist[t] = min(dist[t], backup[f] + p)
        return -1 if dist[dst] == INF else dist[dst]
```

#### Java

```java
class Solution {
    private static final int INF = 0x3f3f3f3f;

    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        int[] dist = new int[n];
        int[] backup = new int[n];
        Arrays.fill(dist, INF);
        dist[src] = 0;
        for (int i = 0; i < k + 1; ++i) {
            System.arraycopy(dist, 0, backup, 0, n);
            for (int[] e : flights) {
                int f = e[0], t = e[1], p = e[2];
                dist[t] = Math.min(dist[t], backup[f] + p);
            }
        }
        return dist[dst] == INF ? -1 : dist[dst];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        const int inf = 0x3f3f3f3f;
        vector<int> dist(n, inf);
        vector<int> backup;
        dist[src] = 0;
        for (int i = 0; i < k + 1; ++i) {
            backup = dist;
            for (auto& e : flights) {
                int f = e[0], t = e[1], p = e[2];
                dist[t] = min(dist[t], backup[f] + p);
            }
        }
        return dist[dst] == inf ? -1 : dist[dst];
    }
};
```

#### Go

```go
func findCheapestPrice(n int, flights [][]int, src int, dst int, k int) int {
	const inf = 0x3f3f3f3f
	dist := make([]int, n)
	backup := make([]int, n)
	for i := range dist {
		dist[i] = inf
	}
	dist[src] = 0
	for i := 0; i < k+1; i++ {
		copy(backup, dist)
		for _, e := range flights {
			f, t, p := e[0], e[1], e[2]
			dist[t] = min(dist[t], backup[f]+p)
		}
	}
	if dist[dst] == inf {
		return -1
	}
	return dist[dst]
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Bellman-Ford duyệt mọi cạnh. Có thể dùng memoization cho $dfs(u,k)$, là chi phí nhỏ nhất từ $u$ khi còn $k$ cạnh: chi phí bằng $0$ tại $dst$ và vô cực khi đã hết cạnh.
>
> Lời gọi ban đầu dùng $k+1$ cạnh. Có $O(nk)$ trạng thái.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findCheapestPrice(
        self, n: int, flights: List[List[int]], src: int, dst: int, k: int
    ) -> int:
        @cache
        def dfs(u, k):
            if u == dst:
                return 0
            if k <= 0:
                return inf
            k -= 1
            ans = inf
            for v, p in g[u]:
                ans = min(ans, dfs(v, k) + p)
            return ans

        g = defaultdict(list)
        for u, v, p in flights:
            g[u].append((v, p))
        ans = dfs(src, k + 1)
        return -1 if ans >= inf else ans
```

#### Java

```java
class Solution {
    private int[][] memo;
    private int[][] g;
    private int dst;
    private static final int INF = (int) 1e6;

    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        n += 10;
        memo = new int[n][n];
        for (int i = 0; i < n; ++i) {
            Arrays.fill(memo[i], -1);
        }
        g = new int[n][n];
        for (int[] e : flights) {
            g[e[0]][e[1]] = e[2];
        }
        this.dst = dst;
        int ans = dfs(src, k + 1);
        return ans >= INF ? -1 : ans;
    }

    private int dfs(int u, int k) {
        if (memo[u][k] != -1) {
            return memo[u][k];
        }
        if (u == dst) {
            return 0;
        }
        if (k <= 0) {
            return INF;
        }
        int ans = INF;
        for (int v = 0; v < g[u].length; ++v) {
            if (g[u][v] > 0) {
                ans = Math.min(ans, dfs(v, k - 1) + g[u][v]);
            }
        }
        memo[u][k] = ans;
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<vector<int>> memo;
    vector<vector<int>> g;
    int dst;
    int inf = 1e6;

    int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
        n += 10;
        memo.resize(n, vector<int>(n, -1));
        g.resize(n, vector<int>(n));
        for (auto& e : flights) g[e[0]][e[1]] = e[2];
        this->dst = dst;
        int ans = dfs(src, k + 1);
        return ans >= inf ? -1 : ans;
    }

    int dfs(int u, int k) {
        if (memo[u][k] != -1) return memo[u][k];
        if (u == dst) return 0;
        if (k <= 0) return inf;
        int ans = inf;
        for (int v = 0; v < g[u].size(); ++v)
            if (g[u][v] > 0)
                ans = min(ans, dfs(v, k - 1) + g[u][v]);
        memo[u][k] = ans;
        return memo[u][k];
    }
};
```

#### Go

```go
func findCheapestPrice(n int, flights [][]int, src int, dst int, k int) int {
	n += 10
	memo := make([][]int, n)
	g := make([][]int, n)
	for i := range memo {
		memo[i] = make([]int, n)
		g[i] = make([]int, n)
		for j := range memo[i] {
			memo[i][j] = -1
		}
	}

	for _, e := range flights {
		g[e[0]][e[1]] = e[2]
	}
	inf := int(1e6)
	var dfs func(u, k int) int
	dfs = func(u, k int) int {
		if memo[u][k] != -1 {
			return memo[u][k]
		}
		if u == dst {
			return 0
		}
		if k <= 0 {
			return inf
		}
		ans := inf
		for v, p := range g[u] {
			if p > 0 {
				ans = min(ans, dfs(v, k-1)+p)
			}
		}
		memo[u][k] = ans
		return ans
	}
	ans := dfs(src, k+1)
	if ans >= inf {
		return -1
	}
	return ans
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
