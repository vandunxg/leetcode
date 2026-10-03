---
comments: true
difficulty: Hard
rating: 2178
source: Weekly Contest 266 Q4
tags:
    - Graph
    - Array
    - Backtracking
---

<!-- problem:start -->

# [2065. Maximum Path Quality of a Graph](https://leetcode.com/problems/maximum-path-quality-of-a-graph)

[中文文档](/solution/2000-2099/2065.Maximum%20Path%20Quality%20of%20a%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị <strong>vô hướng</strong> gồm <code>n</code> đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm cả hai đầu</strong>). Cho một mảng số nguyên <strong>được đánh chỉ số từ 0</strong> <code>values</code>, trong đó <code>values[i]</code> là <strong>giá trị </strong>của đỉnh <code>i<sup>th</sup></code>. Bạn cũng được cho một mảng số nguyên 2 chiều <strong>được đánh chỉ số từ 0</strong> <code>edges</code>, trong đó mỗi <code>edges[j] = [u<sub>j</sub>, v<sub>j</sub>, time<sub>j</sub>]</code> cho biết có một cạnh vô hướng nối hai đỉnh <code>u<sub>j</sub></code> và <code>v<sub>j</sub></code>,<sub> </sub> và cần <code>time<sub>j</sub></code> giây để đi giữa hai đỉnh này. Cuối cùng, cho một số nguyên <code>maxTime</code>.</p>

<p>Một <strong>đường đi</strong> <strong>hợp lệ</strong> trong đồ thị là đường đi bắt đầu tại đỉnh <code>0</code>, kết thúc tại đỉnh <code>0</code> và mất <strong>nhiều nhất</strong> <code>maxTime</code> giây để hoàn thành. Bạn có thể đi qua cùng một đỉnh nhiều lần. <strong>Chất lượng</strong> của một đường đi hợp lệ là <strong>tổng</strong> giá trị của các <strong>đỉnh phân biệt</strong> đã đi qua trong đường đi (giá trị của mỗi đỉnh được cộng <strong>nhiều nhất một lần</strong> vào tổng).</p>

<p>Trả về <em><strong>chất lượng lớn nhất</strong> của một đường đi hợp lệ</em>.</p>

<p><strong>Lưu ý:</strong> Có <strong>nhiều nhất bốn</strong> cạnh nối với mỗi đỉnh.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2065.Maximum%20Path%20Quality%20of%20a%20Graph/images/ex1drawio.png" style="width: 269px; height: 170px;" />
<pre>
<strong>Đầu vào:</strong> values = [0,32,10,43], edges = [[0,1,10],[1,2,15],[0,3,10]], maxTime = 49
<strong>Đầu ra:</strong> 75
<strong>Giải thích:</strong>
Một đường đi có thể là 0 -&gt; 1 -&gt; 0 -&gt; 3 -&gt; 0. Tổng thời gian cần thiết là 10 + 10 + 10 + 10 = 40 &lt;= 49.
Các đỉnh đã đi qua là 0, 1 và 3, nên chất lượng lớn nhất của đường đi là 0 + 32 + 43 = 75.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2065.Maximum%20Path%20Quality%20of%20a%20Graph/images/ex2drawio.png" style="width: 269px; height: 170px;" />
<pre>
<strong>Đầu vào:</strong> values = [5,10,15,20], edges = [[0,1,10],[1,2,10],[0,3,10]], maxTime = 30
<strong>Đầu ra:</strong> 25
<strong>Giải thích:</strong>
Một đường đi có thể là 0 -&gt; 3 -&gt; 0. Tổng thời gian cần thiết là 10 + 10 = 20 &lt;= 30.
Các đỉnh đã đi qua là 0 và 3, nên chất lượng lớn nhất của đường đi là 5 + 20 = 25.
</pre>

<p><strong class="example">Ví dụ 3:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2065.Maximum%20Path%20Quality%20of%20a%20Graph/images/ex31drawio.png" style="width: 236px; height: 170px;" />
<pre>
<strong>Đầu vào:</strong> values = [1,2,3,4], edges = [[0,1,10],[1,2,11],[2,3,12],[1,3,13]], maxTime = 50
<strong>Đầu ra:</strong> 7
<strong>Giải thích:</strong>
Một đường đi có thể là 0 -&gt; 1 -&gt; 3 -&gt; 1 -&gt; 0. Tổng thời gian cần thiết là 10 + 13 + 13 + 10 = 46 &lt;= 50.
Các đỉnh đã đi qua là 0, 1 và 3, nên chất lượng lớn nhất của đường đi là 1 + 2 + 4 = 7.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == values.length</code></li>
	<li><code>1 &lt;= n &lt;= 1000</code></li>
	<li><code>0 &lt;= values[i] &lt;= 10<sup>8</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 2000</code></li>
	<li><code>edges[j].length == 3 </code></li>
	<li><code>0 &lt;= u<sub>j </sub>&lt; v<sub>j</sub> &lt;= n - 1</code></li>
	<li><code>10 &lt;= time<sub>j</sub>, maxTime &lt;= 100</code></li>
	<li>Mọi cặp <code>[u<sub>j</sub>, v<sub>j</sub>]</code> đều <strong>duy nhất</strong>.</li>
	<li>Có <strong>nhiều nhất bốn</strong> cạnh nối với mỗi đỉnh.</li>
	<li>Đồ thị có thể không liên thông.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Vì $maxTime \le 100$ và thời gian của mỗi cạnh $\ge 10$, một walk bắt đầu từ $0$ có nhiều nhất khoảng $10$ cạnh; bậc của mỗi đỉnh $\le 4$ nên cây tìm kiếm vẫn nhỏ. Giá trị được cộng ở lần đi qua đầu tiên; các cạnh có thể được sử dụng lại.
>
> DFS theo dõi đỉnh hiện tại, thời gian và giá trị, đồng thời cập nhật đáp án mỗi khi quay lại $0$. `vis` được đánh dấu khi đi vào lần đầu và được bỏ đánh dấu khi backtrack, nên các lần đi qua lại không cộng thêm gì.

<!-- thinking:end -->

Ta quan sát phạm vi dữ liệu của bài toán và nhận thấy số cạnh trong mỗi đường đi hợp lệ bắt đầu từ $0$ không vượt quá $\frac{\textit{maxTime}}{\min(time_j)} = \frac{100}{10} = 10$, đồng thời mỗi đỉnh có nhiều nhất bốn cạnh. Vì vậy, ta có thể trực tiếp dùng DFS ngây thơ để tìm kiếm vét cạn mọi đường đi hợp lệ.

Trước tiên, ta lưu các cạnh của đồ thị trong danh sách kề $g$. Sau đó, ta thiết kế hàm $\textit{dfs}(u, \textit{cost}, \textit{value})$, trong đó $u$ biểu diễn số hiệu đỉnh hiện tại, còn $\textit{cost}$ và $\textit{value}$ lần lượt biểu diễn thời gian và giá trị của đường đi hiện tại. Ngoài ra, ta dùng một mảng $\textit{vis}$ có độ dài $n$ để ghi nhận mỗi đỉnh đã được đi qua hay chưa. Ban đầu, ta đánh dấu đỉnh $0$ đã được đi qua.

Logic của hàm $\textit{dfs}(u, \textit{cost}, \textit{value})$ như sau:

- Nếu số hiệu đỉnh hiện tại $u$ bằng $0$, nghĩa là ta đã quay lại điểm xuất phát, nên cập nhật đáp án thành $\max(\textit{ans}, \textit{value})$;
- Với mỗi đỉnh kề $v$ của đỉnh hiện tại $u$, nếu thời gian của đường đi hiện tại cộng với thời gian $t$ của cạnh $(u, v)$ không vượt quá $\textit{maxTime}$, ta có thể chọn tiếp tục đi đến đỉnh $v$;
    - Nếu đỉnh $v$ đã được đi qua, ta gọi đệ quy $\textit{dfs}(v, \textit{cost} + t, \textit{value})$;
    - Nếu đỉnh $v$ chưa được đi qua, ta đánh dấu đỉnh $v$, sau đó gọi đệ quy $\textit{dfs}(v, \textit{cost} + t, \textit{value} + \textit{values}[v])$, rồi khôi phục trạng thái đã đi qua của đỉnh $v$.

Trong hàm chính, ta gọi $\textit{dfs}(0, 0, \textit{values}[0])$ và trả về đáp án $\textit{ans}$.

Độ phức tạp thời gian là $O(n + m + 4^{\frac{\textit{maxTime}}{\min(time_j)}})$, còn độ phức tạp không gian là $O(n + m + \frac{\textit{maxTime}}{\min(time_j)})$. Ở đây, $n$ và $m$ lần lượt là số đỉnh và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def maximalPathQuality(
        self, values: List[int], edges: List[List[int]], maxTime: int
    ) -> int:
        def dfs(u: int, cost: int, value: int):
            if u == 0:
                nonlocal ans
                ans = max(ans, value)
            for v, t in g[u]:
                if cost + t <= maxTime:
                    if vis[v]:
                        dfs(v, cost + t, value)
                    else:
                        vis[v] = True
                        dfs(v, cost + t, value + values[v])
                        vis[v] = False

        n = len(values)
        g = [[] for _ in range(n)]
        for u, v, t in edges:
            g[u].append((v, t))
            g[v].append((u, t))
        vis = [False] * n
        vis[0] = True
        ans = 0
        dfs(0, 0, values[0])
        return ans
```

#### Java

```java
class Solution {
    private List<int[]>[] g;
    private boolean[] vis;
    private int[] values;
    private int maxTime;
    private int ans;

    public int maximalPathQuality(int[] values, int[][] edges, int maxTime) {
        int n = values.length;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1], t = e[2];
            g[u].add(new int[] {v, t});
            g[v].add(new int[] {u, t});
        }
        vis = new boolean[n];
        vis[0] = true;
        this.values = values;
        this.maxTime = maxTime;
        dfs(0, 0, values[0]);
        return ans;
    }

    private void dfs(int u, int cost, int value) {
        if (u == 0) {
            ans = Math.max(ans, value);
        }
        for (var e : g[u]) {
            int v = e[0], t = e[1];
            if (cost + t <= maxTime) {
                if (vis[v]) {
                    dfs(v, cost + t, value);
                } else {
                    vis[v] = true;
                    dfs(v, cost + t, value + values[v]);
                    vis[v] = false;
                }
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int maximalPathQuality(vector<int>& values, vector<vector<int>>& edges, int maxTime) {
        int n = values.size();
        vector<pair<int, int>> g[n];
        for (auto& e : edges) {
            int u = e[0], v = e[1], t = e[2];
            g[u].emplace_back(v, t);
            g[v].emplace_back(u, t);
        }
        bool vis[n];
        memset(vis, false, sizeof(vis));
        vis[0] = true;
        int ans = 0;
        auto dfs = [&](this auto&& dfs, int u, int cost, int value) -> void {
            if (u == 0) {
                ans = max(ans, value);
            }
            for (auto& [v, t] : g[u]) {
                if (cost + t <= maxTime) {
                    if (vis[v]) {
                        dfs(v, cost + t, value);
                    } else {
                        vis[v] = true;
                        dfs(v, cost + t, value + values[v]);
                        vis[v] = false;
                    }
                }
            }
        };
        dfs(0, 0, values[0]);
        return ans;
    }
};
```

#### Go

```go
func maximalPathQuality(values []int, edges [][]int, maxTime int) (ans int) {
	n := len(values)
	g := make([][][2]int, n)
	for _, e := range edges {
		u, v, t := e[0], e[1], e[2]
		g[u] = append(g[u], [2]int{v, t})
		g[v] = append(g[v], [2]int{u, t})
	}
	vis := make([]bool, n)
	vis[0] = true
	var dfs func(u, cost, value int)
	dfs = func(u, cost, value int) {
		if u == 0 {
			ans = max(ans, value)
		}
		for _, e := range g[u] {
			v, t := e[0], e[1]
			if cost+t <= maxTime {
				if vis[v] {
					dfs(v, cost+t, value)
				} else {
					vis[v] = true
					dfs(v, cost+t, value+values[v])
					vis[v] = false
				}
			}
		}
	}
	dfs(0, 0, values[0])
	return
}
```

#### TypeScript

```ts
function maximalPathQuality(values: number[], edges: number[][], maxTime: number): number {
    const n = values.length;
    const g: [number, number][][] = Array.from({ length: n }, () => []);
    for (const [u, v, t] of edges) {
        g[u].push([v, t]);
        g[v].push([u, t]);
    }
    const vis: boolean[] = Array(n).fill(false);
    vis[0] = true;
    let ans = 0;
    const dfs = (u: number, cost: number, value: number) => {
        if (u === 0) {
            ans = Math.max(ans, value);
        }
        for (const [v, t] of g[u]) {
            if (cost + t <= maxTime) {
                if (vis[v]) {
                    dfs(v, cost + t, value);
                } else {
                    vis[v] = true;
                    dfs(v, cost + t, value + values[v]);
                    vis[v] = false;
                }
            }
        }
    };
    dfs(0, 0, values[0]);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
