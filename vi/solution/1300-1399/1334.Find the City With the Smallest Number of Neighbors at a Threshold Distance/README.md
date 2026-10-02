---
comments: true
difficulty: Medium
rating: 1854
source: Weekly Contest 173 Q3
tags:
    - Graph
    - Dynamic Programming
    - Shortest Path
    - Dijkstra
    - Floyd–Warshall
    - Bellman–Ford
---

<!-- problem:start -->

# [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance)

[中文文档](/solution/1300-1399/1334.Find%20the%20City%20With%20the%20Smallest%20Number%20of%20Neighbors%20at%20a%20Threshold%20Distance/README.md)

## Mô tả

<!-- description:start -->

<p>Có <code>n</code> thành phố được đánh số từ <code>0</code> đến <code>n-1</code>. Cho mảng <code>edges</code>, trong đó <code>edges[i] = [from<sub>i</sub>, to<sub>i</sub>, weight<sub>i</sub>]</code> biểu diễn một cạnh hai chiều có trọng số nối thành phố <code>from<sub>i</sub></code> và <code>to<sub>i</sub></code>, cùng số nguyên <code>distanceThreshold</code>.</p>

<p>Trả về thành phố có ít thành phố khác nhất có thể đến được qua một đường đi với khoảng cách <strong>không vượt quá</strong> <code>distanceThreshold</code>. Nếu có nhiều thành phố như vậy, trả về thành phố có số hiệu lớn nhất.</p>

<p>Lưu ý, khoảng cách của đường đi nối hai thành phố <em><strong>i</strong></em> và <em><strong>j</strong></em> bằng tổng trọng số các cạnh trên đường đi đó.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1334.Find%20the%20City%20With%20the%20Smallest%20Number%20of%20Neighbors%20at%20a%20Threshold%20Distance/images/problem1334example1.png" style="width: 300px; height: 224px;" /></p>

<pre>
<strong>Input:</strong> n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
<strong>Output:</strong> 3
<strong>Giải thích: </strong>Hình trên minh họa graph.&nbsp;
Các thành phố lân cận trong khoảng distanceThreshold = 4 của từng thành phố là:
Thành phố 0 -&gt; [Thành phố 1, Thành phố 2]&nbsp;
Thành phố 1 -&gt; [Thành phố 0, Thành phố 2, Thành phố 3]&nbsp;
Thành phố 2 -&gt; [Thành phố 0, Thành phố 1, Thành phố 3]&nbsp;
Thành phố 3 -&gt; [Thành phố 1, Thành phố 2]&nbsp;
Thành phố 0 và 3 đều có 2 thành phố lân cận trong khoảng distanceThreshold = 4, nhưng ta phải trả về thành phố 3 vì có số hiệu lớn hơn.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1300-1399/1334.Find%20the%20City%20With%20the%20Smallest%20Number%20of%20Neighbors%20at%20a%20Threshold%20Distance/images/problem1334example0.png" style="width: 300px; height: 224px;" /></p>

<pre>
<strong>Input:</strong> n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
<strong>Output:</strong> 0
<strong>Giải thích: </strong>Hình trên minh họa graph.&nbsp;
Các thành phố lân cận trong khoảng distanceThreshold = 2 của từng thành phố là:
Thành phố 0 -&gt; [Thành phố 1]&nbsp;
Thành phố 1 -&gt; [Thành phố 0, Thành phố 4]&nbsp;
Thành phố 2 -&gt; [Thành phố 3, Thành phố 4]&nbsp;
Thành phố 3 -&gt; [Thành phố 2, Thành phố 4]
Thành phố 4 -&gt; [Thành phố 1, Thành phố 2, Thành phố 3]&nbsp;
Thành phố 0 có 1 thành phố lân cận trong khoảng distanceThreshold = 2.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 100</code></li>
	<li><code>1 &lt;= edges.length &lt;= n * (n - 1) / 2</code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= from<sub>i</sub> &lt; to<sub>i</sub> &lt; n</code></li>
	<li><code>1 &lt;= weight<sub>i</sub>,&nbsp;distanceThreshold &lt;= 10^4</code></li>
	<li>Tất cả các cặp <code>(from<sub>i</sub>, to<sub>i</sub>)</code> đều khác nhau.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1

<!-- thinking:start -->

> **Tư duy**
>
> Với mỗi thành phố, đếm số thành phố khác nằm trong ngưỡng khoảng cách rồi chọn nơi có số lượng nhỏ nhất; nếu hòa thì chọn id lớn hơn. Vì $n \le 100$, ta có thể chạy thuật toán tìm đường đi ngắn nhất từ mọi đỉnh nguồn. Sau khi dựng graph dạng dense, chạy Dijkstra từ id lớn xuống nhỏ và đếm các thành phố thỏa $\textit{dist}[j] \le \textit{distanceThreshold}$. Chỉ cập nhật đáp án khi số lượng giảm nghiêm ngặt, nhờ vậy trường hợp hòa sẽ giữ lại id lớn hơn.

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheCity(
        self, n: int, edges: List[List[int]], distanceThreshold: int
    ) -> int:
        def dijkstra(u: int) -> int:
            dist = [inf] * n
            dist[u] = 0
            vis = [False] * n
            for _ in range(n):
                k = -1
                for j in range(n):
                    if not vis[j] and (k == -1 or dist[k] > dist[j]):
                        k = j
                vis[k] = True
                for j in range(n):
                    # dist[j] = min(dist[j], dist[k] + g[k][j])
                    if dist[k] + g[k][j] < dist[j]:
                        dist[j] = dist[k] + g[k][j]
            return sum(d <= distanceThreshold for d in dist)

        g = [[inf] * n for _ in range(n)]
        for f, t, w in edges:
            g[f][t] = g[t][f] = w
        ans, cnt = n, inf
        for i in range(n - 1, -1, -1):
            if (t := dijkstra(i)) < cnt:
                cnt, ans = t, i
        return ans
```

#### Java

```java
class Solution {
    private int n;
    private int[][] g;
    private int[] dist;
    private boolean[] vis;
    private final int inf = 1 << 30;
    private int distanceThreshold;

    public int findTheCity(int n, int[][] edges, int distanceThreshold) {
        this.n = n;
        this.distanceThreshold = distanceThreshold;
        g = new int[n][n];
        dist = new int[n];
        vis = new boolean[n];
        for (var e : g) {
            Arrays.fill(e, inf);
        }
        for (var e : edges) {
            int f = e[0], t = e[1], w = e[2];
            g[f][t] = w;
            g[t][f] = w;
        }
        int ans = n, cnt = inf;
        for (int i = n - 1; i >= 0; --i) {
            int t = dijkstra(i);
            if (t < cnt) {
                cnt = t;
                ans = i;
            }
        }
        return ans;
    }

    private int dijkstra(int u) {
        Arrays.fill(dist, inf);
        Arrays.fill(vis, false);
        dist[u] = 0;
        for (int i = 0; i < n; ++i) {
            int k = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (k == -1 || dist[k] > dist[j])) {
                    k = j;
                }
            }
            vis[k] = true;
            for (int j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[k] + g[k][j]);
            }
        }
        int cnt = 0;
        for (int d : dist) {
            if (d <= distanceThreshold) {
                ++cnt;
            }
        }
        return cnt;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        int g[n][n];
        int dist[n];
        bool vis[n];
        memset(g, 0x3f, sizeof(g));
        for (auto& e : edges) {
            int f = e[0], t = e[1], w = e[2];
            g[f][t] = g[t][f] = w;
        }
        auto dijkstra = [&](int u) {
            memset(dist, 0x3f, sizeof(dist));
            memset(vis, 0, sizeof(vis));
            dist[u] = 0;
            for (int i = 0; i < n; ++i) {
                int k = -1;
                for (int j = 0; j < n; ++j) {
                    if (!vis[j] && (k == -1 || dist[j] < dist[k])) {
                        k = j;
                    }
                }
                vis[k] = true;
                for (int j = 0; j < n; ++j) {
                    dist[j] = min(dist[j], dist[k] + g[k][j]);
                }
            }
            return count_if(dist, dist + n, [&](int d) { return d <= distanceThreshold; });
        };
        int ans = n, cnt = n + 1;
        for (int i = n - 1; ~i; --i) {
            int t = dijkstra(i);
            if (t < cnt) {
                cnt = t;
                ans = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findTheCity(n int, edges [][]int, distanceThreshold int) int {
	g := make([][]int, n)
	dist := make([]int, n)
	vis := make([]bool, n)
	const inf int = 1e7
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = inf
		}
	}
	for _, e := range edges {
		f, t, w := e[0], e[1], e[2]
		g[f][t], g[t][f] = w, w
	}

	dijkstra := func(u int) (cnt int) {
		for i := range vis {
			vis[i] = false
			dist[i] = inf
		}
		dist[u] = 0
		for i := 0; i < n; i++ {
			k := -1
			for j := 0; j < n; j++ {
				if !vis[j] && (k == -1 || dist[j] < dist[k]) {
					k = j
				}
			}
			vis[k] = true
			for j := 0; j < n; j++ {
				dist[j] = min(dist[j], dist[k]+g[k][j])
			}
		}
		for _, d := range dist {
			if d <= distanceThreshold {
				cnt++
			}
		}
		return
	}

	ans, cnt := n, inf
	for i := n - 1; i >= 0; i-- {
		if t := dijkstra(i); t < cnt {
			cnt = t
			ans = i
		}
	}
	return ans
}
```

#### TypeScript

```ts
function findTheCity(n: number, edges: number[][], distanceThreshold: number): number {
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(Infinity));
    const dist: number[] = Array(n).fill(Infinity);
    const vis: boolean[] = Array(n).fill(false);
    for (const [f, t, w] of edges) {
        g[f][t] = g[t][f] = w;
    }

    const dijkstra = (u: number): number => {
        dist.fill(Infinity);
        vis.fill(false);
        dist[u] = 0;
        for (let i = 0; i < n; ++i) {
            let k = -1;
            for (let j = 0; j < n; ++j) {
                if (!vis[j] && (k === -1 || dist[j] < dist[k])) {
                    k = j;
                }
            }
            vis[k] = true;
            for (let j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[k] + g[k][j]);
            }
        }
        return dist.filter(d => d <= distanceThreshold).length;
    };

    let ans = n;
    let cnt = Infinity;
    for (let i = n - 1; i >= 0; --i) {
        const t = dijkstra(i);
        if (t < cnt) {
            cnt = t;
            ans = i;
        }
    }

    return ans;
}
```

#### JavaScript

```js
function findTheCity(n, edges, distanceThreshold) {
    const g = Array.from({ length: n }, () => Array(n).fill(Infinity));
    const dist = Array(n).fill(Infinity);
    const vis = Array(n).fill(false);
    for (const [f, t, w] of edges) {
        g[f][t] = g[t][f] = w;
    }

    const dijkstra = u => {
        dist.fill(Infinity);
        vis.fill(false);
        dist[u] = 0;
        for (let i = 0; i < n; ++i) {
            let k = -1;
            for (let j = 0; j < n; ++j) {
                if (!vis[j] && (k === -1 || dist[j] < dist[k])) {
                    k = j;
                }
            }
            vis[k] = true;
            for (let j = 0; j < n; ++j) {
                dist[j] = Math.min(dist[j], dist[k] + g[k][j]);
            }
        }
        return dist.filter(d => d <= distanceThreshold).length;
    };

    let ans = n;
    let cnt = Infinity;
    for (let i = n - 1; i >= 0; --i) {
        const t = dijkstra(i);
        if (t < cnt) {
            cnt = t;
            ans = i;
        }
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2

<!-- thinking:start -->

> **Tư duy**
>
> Chạy Dijkstra từ từng nguồn sẽ thực hiện $n$ lượt tìm kiếm. Floyd–Warshall tính khoảng cách giữa mọi cặp đỉnh trong một lượt; sau đó chỉ cần duyệt từng hàng để đếm số thành phố nằm trong ngưỡng. Cách này có code đơn giản hơn và vẫn có độ phức tạp $O(n^3)$. 

<!-- thinking:end -->

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findTheCity(
        self, n: int, edges: List[List[int]], distanceThreshold: int
    ) -> int:
        g = [[inf] * n for _ in range(n)]
        for f, t, w in edges:
            g[f][t] = g[t][f] = w

        for k in range(n):
            g[k][k] = 0
            for i in range(n):
                for j in range(n):
                    # g[i][j] = min(g[i][j], g[i][k] + g[k][j])
                    if g[i][k] + g[k][j] < g[i][j]:
                        g[i][j] = g[i][k] + g[k][j]

        ans, cnt = n, inf
        for i in range(n - 1, -1, -1):
            t = sum(d <= distanceThreshold for d in g[i])
            if t < cnt:
                cnt, ans = t, i
        return ans
```

#### Java

```java
class Solution {
    public int findTheCity(int n, int[][] edges, int distanceThreshold) {
        final int inf = 1 << 29;
        int[][] g = new int[n][n];
        for (var e : g) {
            Arrays.fill(e, inf);
        }
        for (var e : edges) {
            int f = e[0], t = e[1], w = e[2];
            g[f][t] = w;
            g[t][f] = w;
        }
        for (int k = 0; k < n; ++k) {
            g[k][k] = 0;
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
        int ans = n, cnt = inf;
        for (int i = n - 1; i >= 0; --i) {
            int t = 0;
            for (int d : g[i]) {
                if (d <= distanceThreshold) {
                    ++t;
                }
            }
            if (t < cnt) {
                cnt = t;
                ans = i;
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
    int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
        int g[n][n];
        memset(g, 0x3f, sizeof(g));
        for (auto& e : edges) {
            int f = e[0], t = e[1], w = e[2];
            g[f][t] = g[t][f] = w;
        }
        for (int k = 0; k < n; ++k) {
            g[k][k] = 0;
            for (int i = 0; i < n; ++i) {
                for (int j = 0; j < n; ++j) {
                    g[i][j] = min(g[i][j], g[i][k] + g[k][j]);
                }
            }
        }
        int ans = n, cnt = n + 1;
        for (int i = n - 1; ~i; --i) {
            int t = count_if(g[i], g[i] + n, [&](int x) { return x <= distanceThreshold; });
            if (t < cnt) {
                cnt = t;
                ans = i;
            }
        }
        return ans;
    }
};
```

#### Go

```go
func findTheCity(n int, edges [][]int, distanceThreshold int) int {
	g := make([][]int, n)
	const inf int = 1e7
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = inf
		}
	}

	for _, e := range edges {
		f, t, w := e[0], e[1], e[2]
		g[f][t], g[t][f] = w, w
	}

	for k := 0; k < n; k++ {
		g[k][k] = 0
		for i := 0; i < n; i++ {
			for j := 0; j < n; j++ {
				g[i][j] = min(g[i][j], g[i][k]+g[k][j])
			}
		}
	}

	ans, cnt := n, n+1
	for i := n - 1; i >= 0; i-- {
		t := 0
		for _, x := range g[i] {
			if x <= distanceThreshold {
				t++
			}
		}
		if t < cnt {
			cnt, ans = t, i
		}
	}

	return ans
}
```

#### TypeScript

```ts
function findTheCity(n: number, edges: number[][], distanceThreshold: number): number {
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(Infinity));
    for (const [f, t, w] of edges) {
        g[f][t] = g[t][f] = w;
    }
    for (let k = 0; k < n; ++k) {
        g[k][k] = 0;
        for (let i = 0; i < n; ++i) {
            for (let j = 0; j < n; ++j) {
                g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
            }
        }
    }

    let ans = n,
        cnt = n + 1;
    for (let i = n - 1; i >= 0; --i) {
        const t = g[i].filter(x => x <= distanceThreshold).length;
        if (t < cnt) {
            cnt = t;
            ans = i;
        }
    }
    return ans;
}
```

#### JavaScript

```js
function findTheCity(n, edges, distanceThreshold) {
    const g = Array.from({ length: n }, () => Array(n).fill(Infinity));
    for (const [f, t, w] of edges) {
        g[f][t] = g[t][f] = w;
    }
    for (let k = 0; k < n; ++k) {
        g[k][k] = 0;
        for (let i = 0; i < n; ++i) {
            for (let j = 0; j < n; ++j) {
                g[i][j] = Math.min(g[i][j], g[i][k] + g[k][j]);
            }
        }
    }

    let ans = n,
        cnt = n + 1;
    for (let i = n - 1; i >= 0; --i) {
        const t = g[i].filter(x => x <= distanceThreshold).length;
        if (t < cnt) {
            cnt = t;
            ans = i;
        }
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
