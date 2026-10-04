---
comments: true
difficulty: Hard
tags:
    - Tree
    - Depth-First Search
---

<!-- problem:start -->

# [3313. Find the Last Marked Nodes in Tree 🔒](https://leetcode.com/problems/find-the-last-marked-nodes-in-tree)

[中文文档](/solution/3300-3399/3313.Find%20the%20Last%20Marked%20Nodes%20in%20Tree/README.md)

## Mô tả

<!-- description:start -->
<p>Có một cây <strong>vô hướng</strong> gồm <code>n</code> đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho một mảng số nguyên 2 chiều <code>edges</code> có độ dài <code>n - 1</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> cho biết có một cạnh nối giữa các đỉnh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> trong cây.</p>

<p>Ban đầu, <strong>tất cả</strong> các đỉnh đều <strong>chưa được đánh dấu</strong>. Sau mỗi giây, bạn đánh dấu tất cả các đỉnh chưa được đánh dấu có <strong>ít nhất</strong> một đỉnh <em>kề</em> đã được đánh dấu.</p>

<p>Trả về một mảng <code>nodes</code>, trong đó <code>nodes[i]</code> là đỉnh cuối cùng được đánh dấu trong cây nếu bạn đánh dấu đỉnh <code>i</code> tại thời điểm <code>t = 0</code>. Nếu <code>nodes[i]</code> có <em>nhiều</em> đáp án với một đỉnh <code>i</code> bất kỳ, bạn có thể chọn<strong> bất kỳ</strong> đáp án nào.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2]]</span></p>

<p><strong>Đầu ra:</strong> [2,2,1]</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3313.Find%20the%20Last%20Marked%20Nodes%20in%20Tree/images/screenshot-2024-06-02-122236.png" style="width: 450px; height: 217px;" /></p>

<ul>
	<li>Với <code>i = 0</code>, các đỉnh được đánh dấu theo thứ tự: <code>[0] -&gt; [0,1,2]</code>. 1 hoặc 2 đều có thể là đáp án.</li>
	<li>Với <code>i = 1</code>, các đỉnh được đánh dấu theo thứ tự: <code>[1] -&gt; [0,1] -&gt; [0,1,2]</code>. Đỉnh 2 được đánh dấu cuối cùng.</li>
	<li>Với <code>i = 2</code>, các đỉnh được đánh dấu theo thứ tự: <code>[2] -&gt; [0,2] -&gt; [0,1,2]</code>. Đỉnh 1 được đánh dấu cuối cùng.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1]]</span></p>

<p><strong>Đầu ra:</strong> [1,0]</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3313.Find%20the%20Last%20Marked%20Nodes%20in%20Tree/images/screenshot-2024-06-02-122249.png" style="width: 350px; height: 180px;" /></p>

<ul>
	<li>Với <code>i = 0</code>, các đỉnh được đánh dấu theo thứ tự: <code>[0] -&gt; [0,1]</code>.</li>
	<li>Với <code>i = 1</code>, các đỉnh được đánh dấu theo thứ tự: <code>[1] -&gt; [0,1]</code>.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">edges = [[0,1],[0,2],[2,3],[2,4]]</span></p>

<p><strong>Đầu ra:</strong> [3,3,1,1,1]</p>

<p><strong>Giải thích:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3300-3399/3313.Find%20the%20Last%20Marked%20Nodes%20in%20Tree/images/screenshot-2024-06-03-210550.png" style="height: 240px; width: 450px;" /></p>

<ul>
	<li>Với <code>i = 0</code>, các đỉnh được đánh dấu theo thứ tự: <code>[0] -&gt; [0,1,2] -&gt; [0,1,2,3,4]</code>.</li>
	<li>Với <code>i = 1</code>, các đỉnh được đánh dấu theo thứ tự: <code>[1] -&gt; [0,1] -&gt; [0,1,2] -&gt; [0,1,2,3,4]</code>.</li>
	<li>Với <code>i = 2</code>, các đỉnh được đánh dấu theo thứ tự: <code>[2] -&gt; [0,2,3,4] -&gt; [0,1,2,3,4]</code>.</li>
	<li>Với <code>i = 3</code>, các đỉnh được đánh dấu theo thứ tự: <code>[3] -&gt; [2,3] -&gt; [0,2,3,4] -&gt; [0,1,2,3,4]</code>.</li>
	<li>Với <code>i = 4</code>, các đỉnh được đánh dấu theo thứ tự: <code>[4] -&gt; [2,4] -&gt; [0,2,3,4] -&gt; [0,1,2,3,4]</code>.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>edges.length == n - 1</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= edges[i][0], edges[i][1] &lt;= n - 1</code></li>
	<li>Dữ liệu đầu vào được tạo sao cho <code>edges</code> biểu diễn một cây hợp lệ.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm đường kính của cây + DFS

<!-- thinking:start -->

> **Tư duy**
>
> Khi quá trình đánh dấu bắt đầu từ $i$, đỉnh cuối cùng được đánh dấu là một đỉnh xa nhất so với $i$. Nếu tính điều đó cho mọi đỉnh bắt đầu thì độ phức tạp là $O(n^2)$, quá chậm với $n \le 10^5$.
>
> Trong một cây, đỉnh xa nhất luôn nằm ở một đầu mút của đường kính. Chỉ cần xác định hai đầu mút $a$ và $b$ đó.
>
> Ba lượt DFS sẽ tìm $a$, $b$ và khoảng cách đến cả hai đầu mút. Với mỗi $i$, ta so sánh $\textit{dist}(i,a)$ và $\textit{dist}(i,b)$ rồi trả về đầu mút xa hơn.

<!-- thinking:end -->

Theo mô tả của bài toán, đỉnh cuối cùng được đánh dấu phải là một trong hai đầu mút của đường kính của cây, vì khoảng cách từ một đỉnh bất kỳ trên đường kính đến các đỉnh khác trên đường kính là lớn nhất.

Trước tiên, ta có thể bắt đầu tìm kiếm theo chiều sâu (DFS) từ một đỉnh bất kỳ để tìm đỉnh xa nhất $a$, đây là một đầu mút của đường kính cây.

Sau đó, bắt đầu từ đỉnh $a$, ta thực hiện một lượt tìm kiếm theo chiều sâu khác để tìm đỉnh xa nhất $b$, đây là đầu mút còn lại của đường kính cây. Trong quá trình này, ta tính khoảng cách từ mỗi đỉnh đến đỉnh $a$, ký hiệu là $\textit{dist2}$.

Tiếp theo, ta thực hiện tìm kiếm theo chiều sâu bắt đầu từ đỉnh $b$ để tính khoảng cách từ mỗi đỉnh đến đỉnh $b$, ký hiệu là $\textit{dist3}$.

Với mỗi đỉnh $i$, nếu $\textit{dist2}[i] > $\textit{dist3}[i]$, then the distance from node $a$ to node $i$ is greater, so node $a$ is the last marked node; otherwise, node $b$ is the last marked node.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số lượng đỉnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def lastMarkedNodes(self, edges: List[List[int]]) -> List[int]:
        def dfs(i: int, fa: int, dist: List[int]):
            for j in g[i]:
                if j != fa:
                    dist[j] = dist[i] + 1
                    dfs(j, i, dist)

        n = len(edges) + 1
        g = [[] for _ in range(n)]
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)

        dist1 = [-1] * n
        dist1[0] = 0
        dfs(0, -1, dist1)
        a = dist1.index(max(dist1))

        dist2 = [-1] * n
        dist2[a] = 0
        dfs(a, -1, dist2)
        b = dist2.index(max(dist2))

        dist3 = [-1] * n
        dist3[b] = 0
        dfs(b, -1, dist3)

        return [a if x > y else b for x, y in zip(dist2, dist3)]
```

#### Java

```java
class Solution {
    private List<Integer>[] g;

    public int[] lastMarkedNodes(int[][] edges) {
        int n = edges.length + 1;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        int[] dist1 = new int[n];
        dist1[0] = 0;
        dfs(0, -1, dist1);
        int a = maxNode(dist1);

        int[] dist2 = new int[n];
        dist2[a] = 0;
        dfs(a, -1, dist2);
        int b = maxNode(dist2);

        int[] dist3 = new int[n];
        dist3[b] = 0;
        dfs(b, -1, dist3);

        int[] ans = new int[n];
        for (int i = 0; i < n; ++i) {
            ans[i] = dist2[i] > dist3[i] ? a : b;
        }
        return ans;
    }

    private void dfs(int i, int fa, int[] dist) {
        for (int j : g[i]) {
            if (j != fa) {
                dist[j] = dist[i] + 1;
                dfs(j, i, dist);
            }
        }
    }

    private int maxNode(int[] dist) {
        int mx = 0;
        for (int i = 0; i < dist.length; ++i) {
            if (dist[mx] < dist[i]) {
                mx = i;
            }
        }
        return mx;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> lastMarkedNodes(vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        g.resize(n);
        for (const auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        vector<int> dist1(n);
        dfs(0, -1, dist1);
        int a = max_element(dist1.begin(), dist1.end()) - dist1.begin();

        vector<int> dist2(n);
        dfs(a, -1, dist2);
        int b = max_element(dist2.begin(), dist2.end()) - dist2.begin();

        vector<int> dist3(n);
        dfs(b, -1, dist3);

        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            ans.push_back(dist2[i] > dist3[i] ? a : b);
        }
        return ans;
    }

private:
    vector<vector<int>> g;

    void dfs(int i, int fa, vector<int>& dist) {
        for (int j : g[i]) {
            if (j != fa) {
                dist[j] = dist[i] + 1;
                dfs(j, i, dist);
            }
        }
    }
};
```

#### Go

```go
func lastMarkedNodes(edges [][]int) (ans []int) {
	n := len(edges) + 1
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	var dfs func(int, int, []int)
	dfs = func(i, fa int, dist []int) {
		for _, j := range g[i] {
			if j != fa {
				dist[j] = dist[i] + 1
				dfs(j, i, dist)
			}
		}
	}
	maxNode := func(dist []int) int {
		mx := 0
		for i, d := range dist {
			if dist[mx] < d {
				mx = i
			}
		}
		return mx
	}

	dist1 := make([]int, n)
	dfs(0, -1, dist1)
	a := maxNode(dist1)

	dist2 := make([]int, n)
	dfs(a, -1, dist2)
	b := maxNode(dist2)

	dist3 := make([]int, n)
	dfs(b, -1, dist3)

	for i, x := range dist2 {
		if x > dist3[i] {
			ans = append(ans, a)
		} else {
			ans = append(ans, b)
		}
	}
	return
}
```

#### TypeScript

```ts
function lastMarkedNodes(edges: number[][]): number[] {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const dfs = (i: number, fa: number, dist: number[]) => {
        for (const j of g[i]) {
            if (j !== fa) {
                dist[j] = dist[i] + 1;
                dfs(j, i, dist);
            }
        }
    };

    const dist1: number[] = Array(n).fill(0);
    dfs(0, -1, dist1);
    const a = dist1.indexOf(Math.max(...dist1));

    const dist2: number[] = Array(n).fill(0);
    dfs(a, -1, dist2);
    const b = dist2.indexOf(Math.max(...dist2));

    const dist3: number[] = Array(n).fill(0);
    dfs(b, -1, dist3);

    const ans: number[] = [];
    for (let i = 0; i < n; ++i) {
        ans.push(dist2[i] > dist3[i] ? a : b);
    }
    return ans;
}
```

#### JavaScript

```js
/**
 * @param {number[][]} edges
 * @return {number[]}
 */
var lastMarkedNodes = function (edges) {
    const n = edges.length + 1;
    const g = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const dfs = (i, fa, dist) => {
        for (const j of g[i]) {
            if (j !== fa) {
                dist[j] = dist[i] + 1;
                dfs(j, i, dist);
            }
        }
    };

    const dist1 = Array(n).fill(0);
    dfs(0, -1, dist1);
    const a = dist1.indexOf(Math.max(...dist1));

    const dist2 = Array(n).fill(0);
    dfs(a, -1, dist2);
    const b = dist2.indexOf(Math.max(...dist2));

    const dist3 = Array(n).fill(0);
    dfs(b, -1, dist3);

    const ans = [];
    for (let i = 0; i < n; ++i) {
        ans.push(dist2[i] > dist3[i] ? a : b);
    }
    return ans;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
