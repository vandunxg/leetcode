---
comments: true
difficulty: Easy
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [1971. Find if Path Exists in Graph](https://leetcode.com/problems/find-if-path-exists-in-graph)

[中文文档](/solution/1900-1999/1971.Find%20if%20Path%20Exists%20in%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị <strong>hai chiều</strong> gồm <code>n</code> đỉnh, trong đó mỗi đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code> (<strong>bao gồm cả hai đầu</strong>). Các cạnh trong đồ thị được biểu diễn bằng một mảng số nguyên 2 chiều <code>edges</code>, trong đó mỗi <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>]</code> biểu thị một cạnh hai chiều giữa đỉnh <code>u<sub>i</sub></code> và đỉnh <code>v<sub>i</sub></code>. Mỗi cặp đỉnh có <strong>nhiều nhất một</strong> cạnh nối với nhau và không có đỉnh nào có cạnh nối với chính nó.</p>

<p>Bạn cần xác định xem có tồn tại một <strong>đường đi hợp lệ</strong> từ đỉnh <code>source</code> đến đỉnh <code>destination</code> hay không.</p>

<p>Cho <code>edges</code> cùng các số nguyên <code>n</code>, <code>source</code> và <code>destination</code>, trả về <code>true</code><em> nếu có một <strong>đường đi hợp lệ</strong> từ </em><code>source</code><em> đến </em><code>destination</code><em>, hoặc </em><code>false</code><em> nếu không</em><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1971.Find%20if%20Path%20Exists%20in%20Graph/images/validpath-ex1.png" style="width: 141px; height: 121px;" />
<pre>
<strong>Đầu vào:</strong> n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
<strong>Đầu ra:</strong> true
<strong>Giải thích:</strong> Có hai đường đi từ đỉnh 0 đến đỉnh 2:
- 0 &rarr; 1 &rarr; 2
- 0 &rarr; 2
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1900-1999/1971.Find%20if%20Path%20Exists%20in%20Graph/images/validpath-ex2.png" style="width: 281px; height: 141px;" />
<pre>
<strong>Đầu vào:</strong> n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
<strong>Đầu ra:</strong> false
<strong>Giải thích:</strong> Không có đường đi từ đỉnh 0 đến đỉnh 5.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 2 * 10<sup>5</sup></code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>0 &lt;= source, destination &lt;= n - 1</code></li>
	<li>Không có cạnh trùng lặp.</li>
	<li>Không có cạnh tự nối.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: DFS

<!-- thinking:start -->

> **Tư duy**
>
> Ta chỉ cần biết hai đỉnh có liên thông hay không. Sau khi xây dựng danh sách kề, DFS cùng một tập các đỉnh đã thăm sẽ cho biết thành công ngay khi đến đích, với thời gian tuyến tính.

<!-- thinking:end -->

Trước tiên, ta chuyển $\textit{edges}$ thành danh sách kề $g$, sau đó dùng DFS để xác định có đường đi từ $\textit{source}$ đến $\textit{destination}$ hay không.

Trong quá trình này, ta dùng một mảng $\textit{vis}$ để ghi nhận các đỉnh đã được thăm nhằm tránh thăm lại chúng.

Độ phức tạp thời gian là $O(n + m)$, độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là số đỉnh và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validPath(
        self, n: int, edges: List[List[int]], source: int, destination: int
    ) -> bool:
        def dfs(i: int) -> bool:
            if i == destination:
                return True
            vis.add(i)
            for j in g[i]:
                if j not in vis and dfs(j):
                    return True
            return False

        g = [[] for _ in range(n)]
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        vis = set()
        return dfs(source)
```

#### Java

```java
class Solution {
    private int destination;
    private boolean[] vis;
    private List<Integer>[] g;

    public boolean validPath(int n, int[][] edges, int source, int destination) {
        this.destination = destination;
        vis = new boolean[n];
        g = new List[n];
        Arrays.setAll(g, i -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        return dfs(source);
    }

    private boolean dfs(int i) {
        if (i == destination) {
            return true;
        }
        vis[i] = true;
        for (var j : g[i]) {
            if (!vis[j] && dfs(j)) {
                return true;
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        vector<int> g[n];
        vector<bool> vis(n);
        for (const auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        function<bool(int)> dfs = [&](int i) -> bool {
            if (i == destination) {
                return true;
            }
            vis[i] = true;
            for (int j : g[i]) {
                if (!vis[j] && dfs(j)) {
                    return true;
                }
            }
            return false;
        };
        return dfs(source);
    }
};
```

#### Go

```go
func validPath(n int, edges [][]int, source int, destination int) bool {
	vis := make([]bool, n)
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	var dfs func(int) bool
	dfs = func(i int) bool {
		if i == destination {
			return true
		}
		vis[i] = true
		for _, j := range g[i] {
			if !vis[j] && dfs(j) {
				return true
			}
		}
		return false
	}
	return dfs(source)
}
```

#### TypeScript

```ts
function validPath(n: number, edges: number[][], source: number, destination: number): boolean {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const vis = new Set<number>();
    const dfs = (i: number) => {
        if (i === destination) {
            return true;
        }
        if (vis.has(i)) {
            return false;
        }
        vis.add(i);
        return g[i].some(dfs);
    };
    return dfs(source);
}
```

#### Rust

```rust
impl Solution {
    pub fn valid_path(n: i32, edges: Vec<Vec<i32>>, source: i32, destination: i32) -> bool {
        let n = n as usize;
        let source = source as usize;
        let destination = destination as usize;

        let mut g = vec![Vec::new(); n];
        let mut vis = vec![false; n];

        for e in edges {
            let u = e[0] as usize;
            let v = e[1] as usize;
            g[u].push(v);
            g[v].push(u);
        }

        fn dfs(g: &Vec<Vec<usize>>, vis: &mut Vec<bool>, i: usize, destination: usize) -> bool {
            if i == destination {
                return true;
            }
            vis[i] = true;
            for &j in &g[i] {
                if !vis[j] && dfs(g, vis, j, destination) {
                    return true;
                }
            }
            false
        }

        dfs(&g, &mut vis, source, destination)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: BFS

<!-- thinking:start -->

> **Tư duy**
>
> Đệ quy quá sâu có thể gây tràn stack trên một đường đi dài. BFS với một queue và cùng tập các đỉnh đã thăm sẽ mở rộng cho đến khi gặp đích hoặc queue rỗng.

<!-- thinking:end -->

Ta cũng có thể dùng BFS để xác định có đường đi từ $\textit{source}$ đến $\textit{destination}$ hay không.

Cụ thể, ta định nghĩa một queue $q$, ban đầu thêm $\textit{source}$ vào queue. Ngoài ra, ta dùng một tập $\textit{vis}$ để ghi nhận các đỉnh đã được thăm nhằm tránh thăm lại chúng.

Tiếp theo, ta liên tục lấy các đỉnh $i$ ra khỏi queue. Nếu $i = \textit{destination}$, điều đó có nghĩa là có đường đi từ $\textit{source}$ đến $\textit{destination}$, khi đó ta trả về $\textit{true}$. Nếu không, ta duyệt tất cả các đỉnh kề $j$ của $i$. Nếu $j$ chưa được thăm, ta thêm $j$ vào queue $q$ và đánh dấu $j$ đã được thăm.

Cuối cùng, nếu queue rỗng, điều đó có nghĩa là không có đường đi từ $\textit{source}$ đến $\textit{destination}$, khi đó ta trả về $\textit{false}$.

Độ phức tạp thời gian là $O(n + m)$, độ phức tạp không gian là $O(n + m)$. Trong đó, $n$ và $m$ lần lượt là số đỉnh và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def validPath(
        self, n: int, edges: List[List[int]], source: int, destination: int
    ) -> bool:
        g = [[] for _ in range(n)]
        for u, v in edges:
            g[u].append(v)
            g[v].append(u)
        q = deque([source])
        vis = {source}
        while q:
            i = q.popleft()
            if i == destination:
                return True
            for j in g[i]:
                if j not in vis:
                    vis.add(j)
                    q.append(j)
        return False
```

#### Java

```java
class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        List<Integer>[] g = new List[n];
        Arrays.setAll(g, k -> new ArrayList<>());
        for (var e : edges) {
            int u = e[0], v = e[1];
            g[u].add(v);
            g[v].add(u);
        }
        Deque<Integer> q = new ArrayDeque<>();
        q.offer(source);
        boolean[] vis = new boolean[n];
        vis[source] = true;
        while (!q.isEmpty()) {
            int i = q.poll();
            if (i == destination) {
                return true;
            }
            for (int j : g[i]) {
                if (!vis[j]) {
                    vis[j] = true;
                    q.offer(j);
                }
            }
        }
        return false;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        vector<vector<int>> g(n);
        for (const auto& e : edges) {
            int u = e[0], v = e[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        queue<int> q{{source}};
        vector<bool> vis(n);
        vis[source] = true;
        while (q.size()) {
            int i = q.front();
            q.pop();
            if (i == destination) {
                return true;
            }
            for (int j : g[i]) {
                if (!vis[j]) {
                    vis[j] = true;
                    q.push(j);
                }
            }
        }
        return false;
    }
};
```

#### Go

```go
func validPath(n int, edges [][]int, source int, destination int) bool {
	g := make([][]int, n)
	for _, e := range edges {
		u, v := e[0], e[1]
		g[u] = append(g[u], v)
		g[v] = append(g[v], u)
	}
	q := []int{source}
	vis := make([]bool, n)
	vis[source] = true
	for len(q) > 0 {
		i := q[0]
		q = q[1:]
		if i == destination {
			return true
		}
		for _, j := range g[i] {
			if !vis[j] {
				vis[j] = true
				q = append(q, j)
			}
		}
	}
	return false
}
```

#### TypeScript

```ts
function validPath(n: number, edges: number[][], source: number, destination: number): boolean {
    const g: number[][] = Array.from({ length: n }, () => []);
    for (const [u, v] of edges) {
        g[u].push(v);
        g[v].push(u);
    }
    const vis = new Set<number>([source]);
    const q = [source];
    while (q.length) {
        const i = q.pop()!;
        if (i === destination) {
            return true;
        }
        for (const j of g[i]) {
            if (!vis.has(j)) {
                vis.add(j);
                q.push(j);
            }
        }
    }
    return false;
}
```

#### Rust

```rust
use std::collections::{HashSet, VecDeque};

impl Solution {
    pub fn valid_path(n: i32, edges: Vec<Vec<i32>>, source: i32, destination: i32) -> bool {
        let n = n as usize;
        let source = source as usize;
        let destination = destination as usize;

        let mut g = vec![Vec::new(); n];
        for edge in edges {
            let u = edge[0] as usize;
            let v = edge[1] as usize;
            g[u].push(v);
            g[v].push(u);
        }

        let mut q = VecDeque::new();
        let mut vis = HashSet::new();
        q.push_back(source);
        vis.insert(source);

        while let Some(i) = q.pop_front() {
            if i == destination {
                return true;
            }
            for &j in &g[i] {
                if !vis.contains(&j) {
                    vis.insert(j);
                    q.push_back(j);
                }
            }
        }

        false
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 3: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Khi không cần biết đường đi cụ thể, Union-Find có thể gộp tất cả các cạnh rồi so sánh hai root, hoàn toàn không dùng đệ quy.

<!-- thinking:end -->

Union-Find là một cấu trúc dữ liệu dạng cây, đúng như tên gọi, được dùng để xử lý các bài toán **gộp** và **truy vấn** một số tập hợp rời nhau. Nó hỗ trợ hai thao tác:

1. Find: Xác định một phần tử thuộc tập hợp con nào. Độ phức tạp thời gian của một thao tác là $O(\alpha(n))$.
2. Union: Gộp hai tập hợp con thành một tập hợp. Độ phức tạp thời gian của một thao tác là $O(\alpha(n))$.

Với bài toán này, ta có thể dùng Union-Find để gộp các cạnh trong `edges`, sau đó xác định xem `source` và `destination` có thuộc cùng một tập hợp hay không.

Độ phức tạp thời gian là $O(n \log n + m)$ hoặc $O(n \alpha(n) + m)$, độ phức tạp không gian là $O(n)$. Trong đó, $n$ và $m$ lần lượt là số đỉnh và số cạnh.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a, b):
        pa, pb = self.find(a), self.find(b)
        if pa == pb:
            return False
        if self.size[pa] > self.size[pb]:
            self.p[pb] = pa
            self.size[pa] += self.size[pb]
        else:
            self.p[pa] = pb
            self.size[pb] += self.size[pa]
        return True


class Solution:
    def validPath(
        self, n: int, edges: List[List[int]], source: int, destination: int
    ) -> bool:
        uf = UnionFind(n)
        for u, v in edges:
            uf.union(u, v)
        return uf.find(source) == uf.find(destination)
```

#### Java

```java
class UnionFind {
    private int[] p;
    private int[] size;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
            size[i] = 1;
        }
    }

    public int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    public void union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa != pb) {
            if (size[pa] > size[pb]) {
                p[pb] = pa;
                size[pa] += size[pb];
            } else {
                p[pa] = pb;
                size[pb] += size[pa];
            }
        }
    }
}

class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        UnionFind uf = new UnionFind(n);
        for (var e : edges) {
            uf.union(e[0], e[1]);
        }
        return uf.find(source) == uf.find(destination);
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    UnionFind(int n) {
        p = vector<int>(n);
        size = vector<int>(n, 1);
        iota(p.begin(), p.end(), 0);
    }

    void unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa != pb) {
            if (size[pa] > size[pb]) {
                p[pb] = pa;
                size[pa] += size[pb];
            } else {
                p[pa] = pb;
                size[pb] += size[pa];
            }
        }
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

private:
    vector<int> p, size;
};

class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
        UnionFind uf(n);
        for (const auto& e : edges) {
            uf.unite(e[0], e[1]);
        }
        return uf.find(source) == uf.find(destination);
    }
};
```

#### Go

```go
type unionFind struct {
	p, size []int
}

func newUnionFind(n int) *unionFind {
	p := make([]int, n)
	size := make([]int, n)
	for i := range p {
		p[i] = i
		size[i] = 1
	}
	return &unionFind{p, size}
}

func (uf *unionFind) find(x int) int {
	if uf.p[x] != x {
		uf.p[x] = uf.find(uf.p[x])
	}
	return uf.p[x]
}

func (uf *unionFind) union(a, b int) bool {
	pa, pb := uf.find(a), uf.find(b)
	if pa == pb {
		return false
	}
	if uf.size[pa] > uf.size[pb] {
		uf.p[pb] = pa
		uf.size[pa] += uf.size[pb]
	} else {
		uf.p[pa] = pb
		uf.size[pb] += uf.size[pa]
	}
	return true
}

func validPath(n int, edges [][]int, source int, destination int) bool {
	uf := newUnionFind(n)
	for _, e := range edges {
		uf.union(e[0], e[1])
	}
	return uf.find(source) == uf.find(destination)
}
```

#### TypeScript

```ts
class UnionFind {
    p: number[];
    size: number[];
    constructor(n: number) {
        this.p = Array(n)
            .fill(0)
            .map((_, i) => i);
        this.size = Array(n).fill(1);
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a: number, b: number): boolean {
        const [pa, pb] = [this.find(a), this.find(b)];
        if (pa === pb) {
            return false;
        }
        if (this.size[pa] > this.size[pb]) {
            this.p[pb] = pa;
            this.size[pa] += this.size[pb];
        } else {
            this.p[pa] = pb;
            this.size[pb] += this.size[pa];
        }
        return true;
    }
}

function validPath(n: number, edges: number[][], source: number, destination: number): boolean {
    const uf = new UnionFind(n);
    edges.forEach(([u, v]) => uf.union(u, v));
    return uf.find(source) === uf.find(destination);
}
```

#### Rust

```rust
struct UnionFind {
    p: Vec<usize>,
    size: Vec<usize>,
}

impl UnionFind {
    fn new(n: usize) -> Self {
        let p = (0..n).collect();
        let size = vec![1; n];
        UnionFind { p, size }
    }

    fn find(&mut self, x: usize) -> usize {
        if self.p[x] != x {
            self.p[x] = self.find(self.p[x]);
        }
        self.p[x]
    }

    fn union(&mut self, a: usize, b: usize) {
        let pa = self.find(a);
        let pb = self.find(b);
        if pa != pb {
            if self.size[pa] > self.size[pb] {
                self.p[pb] = pa;
                self.size[pa] += self.size[pb];
            } else {
                self.p[pa] = pb;
                self.size[pb] += self.size[pa];
            }
        }
    }
}

impl Solution {
    pub fn valid_path(n: i32, edges: Vec<Vec<i32>>, source: i32, destination: i32) -> bool {
        let n = n as usize;
        let mut uf = UnionFind::new(n);

        for e in edges {
            let u = e[0] as usize;
            let v = e[1] as usize;
            uf.union(u, v);
        }

        uf.find(source as usize) == uf.find(destination as usize)
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
