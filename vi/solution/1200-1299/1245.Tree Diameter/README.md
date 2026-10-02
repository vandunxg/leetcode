---
comments: true
difficulty: Medium
rating: 1792
source: Biweekly Contest 12 Q3
tags:
    - Tree
    - Depth-First Search
    - Breadth-First Search
    - Graph
    - Topological Sort
    - Tree DP
---

<!-- problem:start -->

# [1245. Tree Diameter 🔒](https://leetcode.com/problems/tree-diameter)

[中文文档](/solution/1200-1299/1245.Tree%20Diameter/README.md)

## Mô tả

<!-- description:start -->

<p><strong>Đường kính</strong> của một tree là <strong>số cạnh</strong> trên đường đi dài nhất của tree đó.</p>

<p>Cho một tree vô hướng gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho mảng 2 chiều <code>edges</code>, trong đó <code>edges.length == n - 1</code> và <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị một cạnh vô hướng nối node <code>a<sub>i</sub></code> với node <code>b<sub>i</sub></code> trong tree.</p>

<p>Trả về <em><strong>đường kính</strong> của tree</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1245.Tree%20Diameter/images/tree1.jpg" style="width: 224px; height: 145px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[0,2]]
<strong>Đầu ra:</strong> 2
<strong>Giải thích:</strong> Đường đi dài nhất của tree là 1 - 0 - 2.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1200-1299/1245.Tree%20Diameter/images/tree2.jpg" style="width: 224px; height: 225px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[0,1],[1,2],[2,3],[1,4],[4,5]]
<strong>Đầu ra:</strong> 4
<strong>Giải thích:</strong> Đường đi dài nhất của tree là 3 - 2 - 1 - 4 - 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length + 1</code></li>
	<li><code>1 &lt;= n &lt;= 10<sup>4</sup></code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Hai lượt DFS

<!-- thinking:start -->

> **Tư duy**
>
> Đường kính là đường đi đơn dài nhất. Vì $n \le 10^4$, không thể thử mọi cặp đầu mút. Từ một đỉnh bất kỳ, đỉnh xa nhất sẽ là một đầu mút của một đường kính; duyệt lần thứ hai từ đầu mút đó sẽ tìm được đường kính.
>
> Thực hiện hai lượt DFS: lượt đầu bắt đầu từ $0$ để tìm đỉnh xa nhất $a$; lượt thứ hai bắt đầu từ $a$ để tìm độ dài đường kính. Vì graph là tree, cả DFS và BFS đều tìm được đỉnh xa nhất.

<!-- thinking:end -->

Trước tiên, ta chọn tùy ý một node và chạy depth-first search (DFS) từ node đó để tìm node xa nhất, gọi là node $a$. Tiếp theo, ta chạy DFS lần nữa từ node $a$ để tìm node xa nhất tính từ $a$, gọi là node $b$. Có thể chứng minh đường đi giữa node $a$ và node $b$ chính là đường kính của tree.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số node.

Bài toán tương tự:

- [1522. Diameter of N-Ary Tree 🔒](https://github.com/doocs/leetcode/blob/main/solution/1500-1599/1522.Diameter%20of%20N-Ary%20Tree/README_EN.md)

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def treeDiameter(self, edges: List[List[int]]) -> int:
        def dfs(i: int, fa: int, t: int):
            for j in g[i]:
                if j != fa:
                    dfs(j, i, t + 1)
            nonlocal ans, a
            if ans < t:
                ans = t
                a = i

        g = defaultdict(list)
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        ans = a = 0
        dfs(0, -1, 0)
        dfs(a, -1, 0)
        return ans
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private int ans;
    private int a;

    public int treeDiameter(int[][] edges) {
        int n = edges.length + 1;
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs(0, -1, 0);
        dfs(a, -1, 0);
        return ans;
    }

    private void dfs(int i, int fa, int t) {
        for (int j : g[i]) {
            if (j != fa) {
                dfs(j, i, t + 1);
            }
        }
        if (ans < t) {
            ans = t;
            a = i;
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    int treeDiameter(vector<vector<int>>& edges) {
        int n = edges.size() + 1;
        vector<int> g[n];
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        int ans = 0, a = 0;
        auto dfs = [&](this auto&& dfs, int i, int fa, int t) -> void {
            for (int j : g[i]) {
                if (j != fa) {
                    dfs(j, i, t + 1);
                }
            }
            if (ans < t) {
                ans = t;
                a = i;
            }
        };
        dfs(0, -1, 0);
        dfs(a, -1, 0);
        return ans;
    }
};
```

#### Go

```go
func treeDiameter(edges [][]int) (ans int) {
	n := len(edges) + 1
	g := make([][]int, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	a := 0
	var dfs func(i, fa, t int)
	dfs = func(i, fa, t int) {
		for _, j := range g[i] {
			if j != fa {
				dfs(j, i, t+1)
			}
		}
		if ans < t {
			ans = t
			a = i
		}
	}
	dfs(0, -1, 0)
	dfs(a, -1, 0)
	return
}
```

#### TypeScript

```ts
function treeDiameter(edges: number[][]): number {
    const n = edges.length + 1;
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    let [ans, a] = [0, 0];
    const dfs = (i: number, fa: number, t: number): void => {
        for (const j of g[i]) {
            if (j !== fa) {
                dfs(j, i, t + 1);
            }
        }
        if (ans < t) {
            ans = t;
            a = i;
        }
    };
    dfs(0, -1, 0);
    dfs(a, -1, 0);
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
