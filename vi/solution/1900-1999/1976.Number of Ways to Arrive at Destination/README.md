---
comments: true
difficulty: Medium
rating: 2094
source: Biweekly Contest 59 Q3
tags:
    - Graph
    - Topological Sort
    - Dynamic Programming
    - Shortest Path
    - Dijkstra
---

<!-- problem:start -->

# [1976. Number of Ways to Arrive at Destination](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination)

[中文文档](/solution/1900-1999/1976.Number%20of%20Ways%20to%20Arrive%20at%20Destination/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn đang ở một thành phố gồm <code>n</code> giao lộ được đánh số từ <code>0</code> đến <code>n - 1</code>, với các con đường <strong>hai chiều</strong> nối một số giao lộ. Dữ liệu đầu vào được tạo sao cho bạn có thể đi từ bất kỳ giao lộ nào đến bất kỳ giao lộ nào khác, và giữa hai giao lộ bất kỳ có nhiều nhất một con đường.</p>

<p>Bạn được cho một số nguyên <code>n</code> và một mảng số nguyên 2 chiều <code>roads</code>, trong đó <code>roads[i] = [u<sub>i</sub>, v<sub>i</sub>, time<sub>i</sub>]</code> cho biết có một con đường giữa các giao lộ <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>, mất <code>time<sub>i</sub></code> phút để đi qua. Bạn muốn biết có bao nhiêu cách đi từ giao lộ <code>0</code> đến giao lộ <code>n - 1</code> trong <strong>thời gian ngắn nhất</strong>.</p>

<p>Hãy trả về <em><strong>số cách</strong> bạn có thể đến đích trong <strong>thời gian ngắn nhất</strong></em>. Vì đáp án có thể rất lớn, hãy trả về kết quả <strong>chia lấy dư</strong> cho <code>10<sup>9</sup> + 7</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1976.Number%20of%20Ways%20to%20Arrive%20at%20Destination/images/1976_corrected.png" style="width: 255px; height: 400px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Thời gian ngắn nhất để đi từ giao lộ 0 đến giao lộ 6 là 7 phút.
Có bốn cách để đến đó trong 7 phút:
- 0 ➝ 6
- 0 ➝ 4 ➝ 6
- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Đầu vào:</strong> n = 2, roads = [[1,0,10]]
<strong>Đầu ra:</strong> 1
<strong>Giải thích:</strong> Chỉ có một cách đi từ giao lộ 0 đến giao lộ 1, mất 10 phút.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 200</code></li>
	<li><code>n - 1 &lt;= roads.length &lt;= n * (n - 1) / 2</code></li>
	<li><code>roads[i].length == 3</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>1 &lt;= time<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
	<li><code>u<sub>i </sub>!= v<sub>i</sub></code></li>
	<li>Giữa hai giao lộ bất kỳ có nhiều nhất một con đường.</li>
	<li>Bạn có thể đi từ bất kỳ giao lộ nào đến bất kỳ giao lộ nào khác.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Thuật toán Dijkstra ngây thơ

<!-- thinking:start -->

> **Tư duy**
>
> Ta cần đồng thời tìm đường đi ngắn nhất và đếm số đường đi như vậy. Vì $n\le 200$, Dijkstra trên ma trận với độ phức tạp $O(n^2)$ là đủ.
>
> Khi xuất hiện khoảng cách nhỏ hơn, ta thay số lượng bằng số lượng của nút trước; khi khoảng cách bằng nhau, ta cộng thêm số lượng đó. Số lượng đường đến đích được lấy modulo $10^9+7$.

<!-- thinking:end -->

Ta định nghĩa các mảng sau:

- `g` biểu diễn ma trận kề của đồ thị. `g[i][j]` biểu diễn độ dài đường đi ngắn nhất từ điểm `i` đến điểm `j`. Ban đầu, tất cả đều là vô cùng, còn `g[0][0]` bằng 0. Sau đó, ta duyệt qua `roads` và cập nhật `g[u][v]` và `g[v][u]` thành `t`.
- `dist[i]` biểu diễn độ dài đường đi ngắn nhất từ điểm bắt đầu đến điểm `i`. Ban đầu, tất cả đều là vô cùng, còn `dist[0]` bằng 0.
- `f[i]` biểu diễn số đường đi ngắn nhất từ điểm bắt đầu đến điểm `i`. Ban đầu, tất cả đều bằng 0, còn `f[0]` bằng 1.
- `vis[i]` cho biết điểm `i` đã được thăm hay chưa. Ban đầu, tất cả đều là `False`.

Tiếp theo, ta dùng thuật toán Dijkstra ngây thơ để tìm độ dài đường đi ngắn nhất từ điểm bắt đầu đến điểm cuối, đồng thời ghi nhận số đường đi ngắn nhất đến mỗi điểm trong quá trình này.

Cuối cùng, ta trả về `f[n - 1]`. Vì đáp án có thể rất lớn, ta cần lấy phần dư khi chia cho $10^9 + 7$.

Độ phức tạp thời gian là $O(n^2)$, còn độ phức tạp không gian là $O(n^2)$, trong đó $n$ là số điểm.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def countPaths(self, n: int, roads: List[List[int]]) -> int:
        g = [[inf] * n for _ in range(n)]
        for u, v, t in roads:
            g[u][v] = g[v][u] = t
        g[0][0] = 0
        dist = [inf] * n
        dist[0] = 0
        f = [0] * n
        f[0] = 1
        vis = [False] * n
        for _ in range(n):
            t = -1
            for j in range(n):
                if not vis[j] and (t == -1 or dist[j] < dist[t]):
                    t = j
            vis[t] = True
            for j in range(n):
                if j == t:
                    continue
                ne = dist[t] + g[t][j]
                if dist[j] > ne:
                    dist[j] = ne
                    f[j] = f[t]
                elif dist[j] == ne:
                    f[j] += f[t]
        mod = 10**9 + 7
        return f[-1] % mod
```

#### Java

```java
class Solution {
    public int countPaths(int n, int[][] roads) {
        final long inf = Long.MAX_VALUE / 2;
        final int mod = (int) 1e9 + 7;
        long[][] g = new long[n][n];
        for (var e : g) {
            Arrays.fill(e, inf);
        }
        for (var r : roads) {
            int u = r[0], v = r[1], t = r[2];
            g[u][v] = t;
            g[v][u] = t;
        }
        g[0][0] = 0;
        long[] dist = new long[n];
        Arrays.fill(dist, inf);
        dist[0] = 0;
        long[] f = new long[n];
        f[0] = 1;
        boolean[] vis = new boolean[n];
        for (int i = 0; i < n; ++i) {
            int t = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (t == -1 || dist[j] < dist[t])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (int j = 0; j < n; ++j) {
                if (j == t) {
                    continue;
                }
                long ne = dist[t] + g[t][j];
                if (dist[j] > ne) {
                    dist[j] = ne;
                    f[j] = f[t];
                } else if (dist[j] == ne) {
                    f[j] = (f[j] + f[t]) % mod;
                }
            }
        }
        return (int) f[n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countPaths(int n, vector<vector<int>>& roads) {
        const long long inf = LLONG_MAX / 2;
        const int mod = 1e9 + 7;

        vector<vector<long long>> g(n, vector<long long>(n, inf));
        for (auto& e : g) {
            fill(e.begin(), e.end(), inf);
        }

        for (auto& r : roads) {
            int u = r[0], v = r[1], t = r[2];
            g[u][v] = t;
            g[v][u] = t;
        }

        g[0][0] = 0;

        vector<long long> dist(n, inf);
        fill(dist.begin(), dist.end(), inf);
        dist[0] = 0;

        vector<long long> f(n);
        f[0] = 1;

        vector<bool> vis(n);
        for (int i = 0; i < n; ++i) {
            int t = -1;
            for (int j = 0; j < n; ++j) {
                if (!vis[j] && (t == -1 || dist[j] < dist[t])) {
                    t = j;
                }
            }
            vis[t] = true;
            for (int j = 0; j < n; ++j) {
                if (j == t) {
                    continue;
                }
                long long ne = dist[t] + g[t][j];
                if (dist[j] > ne) {
                    dist[j] = ne;
                    f[j] = f[t];
                } else if (dist[j] == ne) {
                    f[j] = (f[j] + f[t]) % mod;
                }
            }
        }
        return (int) f[n - 1];
    }
};
```

#### Go

```go
func countPaths(n int, roads [][]int) int {
	const inf = math.MaxInt64 / 2
	const mod = int(1e9 + 7)

	g := make([][]int, n)
	dist := make([]int, n)
	for i := range g {
		g[i] = make([]int, n)
		for j := range g[i] {
			g[i][j] = inf
			dist[i] = inf
		}
	}

	for _, r := range roads {
		u, v, t := r[0], r[1], r[2]
		g[u][v] = t
		g[v][u] = t
	}

	f := make([]int, n)
	vis := make([]bool, n)
	f[0] = 1
	g[0][0] = 0
	dist[0] = 0

	for i := 0; i < n; i++ {
		t := -1
		for j := 0; j < n; j++ {
			if !vis[j] && (t == -1 || dist[j] < dist[t]) {
				t = j
			}
		}
		vis[t] = true
		for j := 0; j < n; j++ {
			if j == t {
				continue
			}
			ne := dist[t] + g[t][j]
			if dist[j] > ne {
				dist[j] = ne
				f[j] = f[t]
			} else if dist[j] == ne {
				f[j] = (f[j] + f[t]) % mod
			}
		}
	}
	return f[n-1]
}
```

#### TypeScript

```ts
function countPaths(n: number, roads: number[][]): number {
    const mod: number = 1e9 + 7;
    const g: number[][] = Array.from({ length: n }, () => Array(n).fill(Infinity));
    for (const [u, v, t] of roads) {
        g[u][v] = t;
        g[v][u] = t;
    }
    g[0][0] = 0;

    const dist: number[] = Array(n).fill(Infinity);
    dist[0] = 0;

    const f: number[] = Array(n).fill(0);
    f[0] = 1;

    const vis: boolean[] = Array(n).fill(false);
    for (let i = 0; i < n; ++i) {
        let t: number = -1;
        for (let j = 0; j < n; ++j) {
            if (!vis[j] && (t === -1 || dist[j] < dist[t])) {
                t = j;
            }
        }
        vis[t] = true;
        for (let j = 0; j < n; ++j) {
            if (j === t) {
                continue;
            }
            const ne: number = dist[t] + g[t][j];
            if (dist[j] > ne) {
                dist[j] = ne;
                f[j] = f[t];
            } else if (dist[j] === ne) {
                f[j] = (f[j] + f[t]) % mod;
            }
        }
    }
    return f[n - 1];
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
