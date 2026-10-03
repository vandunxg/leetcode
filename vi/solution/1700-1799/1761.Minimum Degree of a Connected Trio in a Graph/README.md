---
comments: true
difficulty: Hard
rating: 2005
source: Weekly Contest 228 Q4
tags:
    - Graph
    - Enumeration
---

<!-- problem:start -->

# [1761. Minimum Degree of a Connected Trio in a Graph](https://leetcode.com/problems/minimum-degree-of-a-connected-trio-in-a-graph)

[中文文档](/solution/1700-1799/1761.Minimum%20Degree%20of%20a%20Connected%20Trio%20in%20a%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một đồ thị vô hướng. Cho số nguyên <code>n</code> là số node trong đồ thị và mảng <code>edges</code>, trong đó mỗi <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh vô hướng giữa <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>.</p>

<p>Một <strong>bộ ba liên thông</strong> là một tập gồm <strong>ba</strong> node, trong đó có cạnh nối <b>mọi</b> cặp node.</p>

<p><strong>Bậc của một bộ ba liên thông</strong> là số cạnh có một đầu mút thuộc bộ ba và đầu mút còn lại không thuộc bộ ba.</p>

<p>Trả về <em>bậc <strong>nhỏ nhất</strong> của một bộ ba liên thông trong đồ thị, hoặc</em> <code>-1</code> <em>nếu đồ thị không có bộ ba liên thông.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1761.Minimum%20Degree%20of%20a%20Connected%20Trio%20in%20a%20Graph/images/trios1.png" style="width: 388px; height: 164px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[1,2],[1,3],[3,2],[4,1],[5,2],[3,6]]
<strong>Đầu ra:</strong> 3
<strong>Giải thích:</strong> Có đúng một bộ ba là [1,2,3]. Các cạnh tạo nên bậc của bộ ba được in đậm trong hình trên.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1700-1799/1761.Minimum%20Degree%20of%20a%20Connected%20Trio%20in%20a%20Graph/images/trios2.png" style="width: 388px; height: 164px;" />
<pre>
<strong>Đầu vào:</strong> n = 7, edges = [[1,3],[4,1],[4,3],[2,5],[5,6],[6,7],[7,5],[2,6]]
<strong>Đầu ra:</strong> 0
<strong>Giải thích:</strong> Có đúng ba bộ ba:
1) [1,4,3] có bậc 0.
2) [2,5,6] có bậc 2.
3) [5,6,7] có bậc 2.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 400</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= edges.length &lt;= n * (n-1) / 2</code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i </sub>!= v<sub>i</sub></code></li>
	<li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Liệt kê vét cạn

<!-- thinking:start -->

> **Tư duy**
>
> Bậc của một bộ ba bằng tổng bậc của ba đỉnh trừ $6$. Đồ thị đủ nhỏ để liệt kê các tam giác trong $O(n^3)$.
>
> Ma trận kề dùng để kiểm tra cạnh, còn $\textit{deg}$ lưu bậc của các đỉnh. Với $i<j<k$ và cả ba cạnh đều tồn tại, cập nhật $\textit{deg}[i]+\textit{deg}[j]+\textit{deg}[k]-6$. Trả về $-1$ nếu không tìm thấy bộ ba nào.

<!-- thinking:end -->

Trước hết, ta lưu tất cả các cạnh vào ma trận kề $\textit{g}$, rồi lưu bậc của mỗi node vào mảng $\textit{deg}$. Khởi tạo đáp án $\textit{ans} = +\infty$.

Sau đó, ta liệt kê mọi bộ ba $(i, j, k)$ với $i \lt j \lt k$. Nếu $\textit{g}[i][j] = \textit{g}[j][k] = \textit{g}[i][k] = 1$, ba node này tạo thành một bộ ba liên thông. Khi đó, cập nhật đáp án thành $\textit{ans} = \min(\textit{ans}, \textit{deg}[i] + \textit{deg}[j] + \textit{deg}[k] - 6)$.

Sau khi liệt kê mọi bộ ba, nếu đáp án vẫn là $+\infty$ thì đồ thị không có bộ ba liên thông, khi đó trả về $-1$. Nếu không, trả về đáp án.

Độ phức tạp thời gian là $O(n^3)$ và độ phức tạp không gian là $O(n^2)$. Trong đó, $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
def min(a: int, b: int) -> int:
    return a if a < b else b


class Solution:
    def minTrioDegree(self, n: int, edges: List[List[int]]) -> int:
        g = [[False] * n for _ in range(n)]
        deg = [0] * n
        for u, v in edges:
            u, v = u - 1, v - 1
            g[u][v] = g[v][u] = True
            deg[u] += 1
            deg[v] += 1
        ans = inf
        for i in range(n):
            for j in range(i + 1, n):
                if g[i][j]:
                    for k in range(j + 1, n):
                        if g[i][k] and g[j][k]:
                            ans = min(ans, deg[i] + deg[j] + deg[k] - 6)
        return -1 if ans == inf else ans
```

#### Java

```java
class Solution {
    public int minTrioDegree(int n, int[][] edges) {
        boolean[][] g = new boolean[n][n];
        int[] deg = new int[n];
        for (var e : edges) {
            int u = e[0] - 1, v = e[1] - 1;
            g[u][v] = true;
            g[v][u] = true;
            ++deg[u];
            ++deg[v];
        }
        int ans = 1 << 30;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (g[i][j]) {
                    for (int k = j + 1; k < n; ++k) {
                        if (g[i][k] && g[j][k]) {
                            ans = Math.min(ans, deg[i] + deg[j] + deg[k] - 6);
                        }
                    }
                }
            }
        }
        return ans == 1 << 30 ? -1 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minTrioDegree(int n, vector<vector<int>>& edges) {
        bool g[n][n];
        memset(g, 0, sizeof g);
        int deg[n];
        memset(deg, 0, sizeof deg);
        for (auto& e : edges) {
            int u = e[0] - 1, v = e[1] - 1;
            g[u][v] = g[v][u] = true;
            deg[u]++, deg[v]++;
        }
        int ans = INT_MAX;
        for (int i = 0; i < n; ++i) {
            for (int j = i + 1; j < n; ++j) {
                if (g[i][j]) {
                    for (int k = j + 1; k < n; ++k) {
                        if (g[j][k] && g[i][k]) {
                            ans = min(ans, deg[i] + deg[j] + deg[k] - 6);
                        }
                    }
                }
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

#### Go

```go
func minTrioDegree(n int, edges [][]int) int {
	g := make([][]bool, n)
	deg := make([]int, n)
	for i := range g {
		g[i] = make([]bool, n)
	}
	for _, e := range edges {
		u, v := e[0]-1, e[1]-1
		g[u][v], g[v][u] = true, true
		deg[u]++
		deg[v]++
	}
	ans := 1 << 30
	for i := 0; i < n; i++ {
		for j := i + 1; j < n; j++ {
			if g[i][j] {
				for k := j + 1; k < n; k++ {
					if g[i][k] && g[j][k] {
						ans = min(ans, deg[i]+deg[j]+deg[k]-6)
					}
				}
			}
		}
	}
	if ans == 1<<30 {
		return -1
	}
	return ans
}
```

#### TypeScript

```ts
function minTrioDegree(n: number, edges: number[][]): number {
    const g = Array.from({ length: n }, () => Array(n).fill(false));
    const deg: number[] = Array(n).fill(0);
    for (let [u, v] of edges) {
        u--;
        v--;
        g[u][v] = g[v][u] = true;
        ++deg[u];
        ++deg[v];
    }
    let ans = Infinity;
    for (let i = 0; i < n; ++i) {
        for (let j = i + 1; j < n; ++j) {
            if (g[i][j]) {
                for (let k = j + 1; k < n; ++k) {
                    if (g[i][k] && g[j][k]) {
                        ans = Math.min(ans, deg[i] + deg[j] + deg[k] - 6);
                    }
                }
            }
        }
    }
    return ans === Infinity ? -1 : ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
