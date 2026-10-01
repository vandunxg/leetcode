---
comments: true
difficulty: Medium
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [261. Graph Valid Tree 🔒](https://leetcode.com/problems/graph-valid-tree)

[中文文档](/solution/0200-0299/0261.Graph%20Valid%20Tree/README.md)

## Mô tả

<!-- description:start -->

<p>Cho một graph gồm <code>n</code> node được đánh số từ <code>0</code> đến <code>n - 1</code>. Bạn được cho số nguyên n và danh sách <code>edges</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> biểu thị có một cạnh vô hướng nối hai node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code> trong graph.</p>

<p>Trả về <code>true</code> <em>nếu các cạnh của graph đã cho tạo thành một cây hợp lệ, nếu không thì trả về</em> <code>false</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0261.Graph%20Valid%20Tree/images/tree1-graph.jpg" style="width: 222px; height: 302px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[0,1],[0,2],[0,3],[1,4]]
<strong>Đầu ra:</strong> true
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0200-0299/0261.Graph%20Valid%20Tree/images/tree2-graph.jpg" style="width: 382px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> n = 5, edges = [[0,1],[1,2],[2,3],[1,3],[1,4]]
<strong>Đầu ra:</strong> false
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2000</code></li>
	<li><code>0 &lt;= edges.length &lt;= 5000</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= a<sub>i</sub>, b<sub>i</sub> &lt; n</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có self-loop hoặc cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Graph vô hướng là cây khi và chỉ khi nó liên thông và không có chu trình, do đó có đúng $n-1$ cạnh. Union-find phát hiện chu trình khi một cạnh nối hai node vốn đã thuộc cùng một tập.
>
> Mỗi lần union thành công làm giảm số thành phần liên thông; đến cuối số này phải bằng $1$.

<!-- thinking:end -->

Để xác định graph có phải là cây hay không, cần thỏa mãn hai điều kiện sau:

1. Số cạnh bằng số node trừ một;
2. Không có chu trình.

Ta có thể dùng union-find để xác định graph có chu trình hay không. Duyệt qua các cạnh: nếu hai node đã thuộc cùng một tập thì graph có chu trình; nếu chưa, gộp hai node vào cùng một tập. Sau đó giảm số thành phần liên thông đi một và cuối cùng kiểm tra xem số thành phần liên thông có bằng $1$ hay không.

Độ phức tạp thời gian là $O(n \times \log n)$, độ phức tạp không gian là $O(n)$, với $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(n))
        for a, b in edges:
            pa, pb = find(a), find(b)
            if pa == pb:
                return False
            p[pa] = pb
            n -= 1
        return n == 1
```

#### Java

```java
class Solution {
    private int[] p;

    public boolean validTree(int n, int[][] edges) {
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (var e : edges) {
            int pa = find(e[0]), pb = find(e[1]);
            if (pa == pb) {
                return false;
            }
            p[pa] = pb;
            --n;
        }
        return n == 1;
    }

    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<int> p;

    bool validTree(int n, vector<vector<int>>& edges) {
        p.resize(n);
        for (int i = 0; i < n; ++i) p[i] = i;
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            if (find(a) == find(b)) return 0;
            p[find(a)] = find(b);
            --n;
        }
        return n == 1;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};
```

#### Go

```go
func validTree(n int, edges [][]int) bool {
	p := make([]int, n)
	for i := range p {
		p[i] = i
	}
	var find func(int) int
	find = func(x int) int {
		if p[x] != x {
			p[x] = find(p[x])
		}
		return p[x]
	}
	for _, e := range edges {
		pa, pb := find(e[0]), find(e[1])
		if pa == pb {
			return false
		}
		p[pa] = pb
		n--
	}
	return n == 1
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {boolean}
 */
var validTree = function (n, edges) {
    const p = Array.from({ length: n }, (_, i) => i);
    const find = x => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    for (const [a, b] of edges) {
        const pa = find(a);
        const pb = find(b);
        if (pa === pb) {
            return false;
        }
        p[pa] = pb;
        --n;
    }
    return n === 1;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Union-find kiểm tra chu trình trên từng cạnh. Một cách khác là yêu cầu $|E|=n-1$ rồi chạy DFS từ $0$: nếu duyệt được $n$ node thì graph liên thông và không có cạnh thừa.

<!-- thinking:end -->

Ta cũng có thể dùng depth-first search để xác định graph có chu trình hay không. Dùng mảng $vis$ để ghi lại các node đã thăm. Trong quá trình tìm kiếm, trước tiên đánh dấu node hiện tại đã được thăm, rồi duyệt các node kề với nó. Nếu node kề đã được thăm thì bỏ qua, nếu chưa thì gọi đệ quy để thăm node đó. Cuối cùng, kiểm tra xem đã thăm hết tất cả các node chưa. Nếu còn node chưa được thăm thì graph không thể tạo thành cây, vì vậy trả về `false`.

Độ phức tạp thời gian là $O(n)$ và độ phức tạp không gian là $O(n)$, với $n$ là số node.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validTree(self, n: int, edges: List[List[int]]) -> bool:
        def dfs(i: int):
            vis.add(i)
            for j in g[i]:
                if j not in vis:
                    dfs(j)

        if len(edges) != n - 1:
            return False
        g = [[] for _ in range(n)]
        for a, b in edges:
            g[a].append(b)
            g[b].append(a)
        vis = set()
        dfs(0)
        return len(vis) == n
```

#### Java

```java
class Solution {
    private List<Integer>[] g;
    private Set<Integer> vis = new HashSet<>();

    public boolean validTree(int n, int[][] edges) {
        if (edges.length != n - 1) {
            return false;
        }
        g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int a = e[0], b = e[1];
            g[a].add(b);
            g[b].add(a);
        }
        dfs(0);
        return vis.size() == n;
    }

    private void dfs(int i) {
        vis.add(i);
        for (int j : g[i]) {
            if (!vis.contains(j)) {
                dfs(j);
            }
        }
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validTree(int n, vector<vector<int>>& edges) {
        if (edges.size() != n - 1) {
            return false;
        }
        vector<int> g[n];
        vector<int> vis(n);
        function<void(int)> dfs = [&](int i) {
            vis[i] = true;
            --n;
            for (int j : g[i]) {
                if (!vis[j]) {
                    dfs(j);
                }
            }
        };
        for (auto& e : edges) {
            int a = e[0], b = e[1];
            g[a].push_back(b);
            g[b].push_back(a);
        }
        dfs(0);
        return n == 0;
    }
};
```

#### Go

```go
func validTree(n int, edges [][]int) bool {
	if len(edges) != n-1 {
		return false
	}
	g := make([][]int, n)
	vis := make([]bool, n)
	for _, e := range edges {
		a, b := e[0], e[1]
		g[a] = append(g[a], b)
		g[b] = append(g[b], a)
	}
	var dfs func(int)
	dfs = func(i int) {
		vis[i] = true
		n--
		for _, j := range g[i] {
			if !vis[j] {
				dfs(j)
			}
		}
	}
	dfs(0)
	return n == 0
}
```

#### JavaScript

```js
/**
 * @param {number} n
 * @param {number[][]} edges
 * @return {boolean}
 */
var validTree = function (n, edges) {
    if (edges.length !== n - 1) {
        return false;
    }
    const g = Array.from({ length: n }, () => []);
    const vis = Array.from({ length: n }, () => false);
    for (const [a, b] of edges) {
        g[a].push(b);
        g[b].push(a);
    }
    const dfs = i => {
        vis[i] = true;
        --n;
        for (const j of g[i]) {
            if (!vis[j]) {
                dfs(j);
            }
        }
    };
    dfs(0);
    return n === 0;
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
