---
comments: true
difficulty: Hard
tags:
    - Depth-First Search
    - Breadth-First Search
    - Union Find
    - Graph
---

<!-- problem:start -->

# [685. Redundant Connection II](https://leetcode.com/problems/redundant-connection-ii)

[中文文档](/solution/0600-0699/0685.Redundant%20Connection%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Trong bài này, rooted tree là một đồ thị <b>có hướng</b> có đúng một node (root) mà tất cả node còn lại đều là hậu duệ của nó; đồng thời mỗi node có đúng một parent, ngoại trừ root không có parent nào.</p>

<p>Đầu vào là một đồ thị có hướng được tạo từ rooted tree gồm <code>n</code> node (có giá trị phân biệt từ <code>1</code> đến <code>n</code>) bằng cách thêm một cạnh có hướng. Hai đầu của cạnh mới là hai đỉnh khác nhau được chọn trong khoảng từ <code>1</code> đến <code>n</code>, và cạnh đó chưa tồn tại trong cây.</p>

<p>Đồ thị thu được được biểu diễn bằng mảng 2 chiều <code>edges</code>. Mỗi phần tử là một cặp <code>[u<sub>i</sub>, v<sub>i</sub>]</code>, biểu diễn cạnh <b>có hướng</b> nối node <code>u<sub>i</sub></code> với node <code>v<sub>i</sub></code>, trong đó <code>u<sub>i</sub></code> là parent của <code>v<sub>i</sub></code>.</p>

<p>Hãy trả về <em>một cạnh cần xóa để đồ thị còn lại là rooted tree gồm</em> <code>n</code> <em>node</em>. Nếu có nhiều đáp án, trả về cạnh xuất hiện sau cùng trong mảng 2 chiều đã cho.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0685.Redundant%20Connection%20II/images/graph1.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[1,2],[1,3],[2,3]]
<strong>Đầu ra:</strong> [2,3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0685.Redundant%20Connection%20II/images/graph2.jpg" style="width: 222px; height: 382px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
<strong>Đầu ra:</strong> [4,1]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>3 &lt;= n &lt;= 1000</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Thêm một cạnh có hướng vào rooted tree có thể tạo ra node có in-degree bằng $2$, tạo chu trình đi vào root, hoặc cả hai. Union-Find trên đồ thị vô hướng không tự xử lý được trường hợp một node có hai parent.
>
> Tìm hai cạnh đi vào node có in-degree bằng $2$. Bỏ qua cạnh xuất hiện sau rồi chạy Union-Find: nếu phát hiện chu trình thì phải xóa cạnh xuất hiện trước. Nếu không có node nào có hai parent, xóa cạnh đầu tiên tạo chu trình.

<!-- thinking:end -->

Theo đề bài, trong rooted tree, in-degree của root bằng $0$, còn in-degree của các node khác bằng $1$. Sau khi thêm một cạnh vào cây, có thể xảy ra ba trường hợp:

1. Cạnh mới đi vào một node không phải root, khiến in-degree của node đó thành $2$. Trường hợp này đồ thị không có chu trình có hướng:

    ```plaintext
       1
      / \
     v   v
     2-->3
    ```

2. Cạnh mới đi vào một node không phải root, khiến in-degree của node đó thành $2$. Trường hợp này đồ thị có chu trình có hướng:

    ```plaintext
       1
       |
       v
       2 <--> 3
    ```

3. Cạnh mới đi vào root, khiến in-degree của root thành $1$. Trường hợp này đồ thị có chu trình có hướng nhưng không có node nào có in-degree bằng $2$.

    ```plaintext
        1
        /^
       v  \
       2-->3
    ```

Vì vậy, trước tiên ta tính in-degree của từng node. Nếu có node có in-degree bằng $2$, ta tìm hai cạnh đi vào node đó, ký hiệu là $\textit{dup}[0]$ và $\textit{dup}[1]$. Nếu xóa $\textit{dup}[1]$ mà các cạnh còn lại không tạo thành cây, thì cần xóa $\textit{dup}[0]$; ngược lại, cần xóa $\textit{dup}[1]$.

Nếu không có node nào có in-degree bằng $2$, ta duyệt mảng $\textit{edges}$. Với mỗi cạnh $(u, v)$, dùng cấu trúc dữ liệu Union-Find để theo dõi tính liên thông giữa các node. Nếu $u$ và $v$ đã liên thông, đồ thị có chu trình có hướng và cạnh hiện tại chính là cạnh cần xóa.

Độ phức tạp thời gian là $O(n \log n)$, còn độ phức tạp không gian là $O(n)$, trong đó $n$ là số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        n = len(edges)
        ind = [0] * n
        for _, v in edges:
            ind[v - 1] += 1
        dup = [i for i, (_, v) in enumerate(edges) if ind[v - 1] == 2]
        p = list(range(n))
        if dup:
            for i, (u, v) in enumerate(edges):
                if i == dup[1]:
                    continue
                pu, pv = find(u - 1), find(v - 1)
                if pu == pv:
                    return edges[dup[0]]
                p[pu] = pv
            return edges[dup[1]]
        for i, (u, v) in enumerate(edges):
            pu, pv = find(u - 1), find(v - 1)
            if pu == pv:
                return edges[i]
            p[pu] = pv
```

#### Java

```java
class Solution {
    private int[] p;

    public int[] findRedundantDirectedConnection(int[][] edges) {
        int n = edges.length;
        int[] ind = new int[n];
        for (var e : edges) {
            ++ind[e[1] - 1];
        }
        List<Integer> dup = new ArrayList<>();
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            if (ind[edges[i][1] - 1] == 2) {
                dup.add(i);
            }
            p[i] = i;
        }
        if (!dup.isEmpty()) {
            for (int i = 0; i < n; ++i) {
                if (i == dup.get(1)) {
                    continue;
                }
                int pu = find(edges[i][0] - 1);
                int pv = find(edges[i][1] - 1);
                if (pu == pv) {
                    return edges[dup.get(0)];
                }
                p[pu] = pv;
            }
            return edges[dup.get(1)];
        }
        for (int i = 0;; ++i) {
            int pu = find(edges[i][0] - 1);
            int pv = find(edges[i][1] - 1);
            if (pu == pv) {
                return edges[i];
            }
            p[pu] = pv;
        }
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
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> ind(n);
        for (const auto& e : edges) {
            ++ind[e[1] - 1];
        }
        vector<int> dup;
        for (int i = 0; i < n; ++i) {
            if (ind[edges[i][1] - 1] == 2) {
                dup.push_back(i);
            }
        }
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        function<int(int)> find = [&](int x) {
            return x == p[x] ? x : p[x] = find(p[x]);
        };
        if (!dup.empty()) {
            for (int i = 0; i < n; ++i) {
                if (i == dup[1]) {
                    continue;
                }
                int pu = find(edges[i][0] - 1);
                int pv = find(edges[i][1] - 1);
                if (pu == pv) {
                    return edges[dup[0]];
                }
                p[pu] = pv;
            }
            return edges[dup[1]];
        }
        for (int i = 0;; ++i) {
            int pu = find(edges[i][0] - 1);
            int pv = find(edges[i][1] - 1);
            if (pu == pv) {
                return edges[i];
            }
            p[pu] = pv;
        }
    }
};
```

#### Go

```go
func findRedundantDirectedConnection(edges [][]int) []int {
	n := len(edges)
	ind := make([]int, n)
	for _, e := range edges {
		ind[e[1]-1]++
	}
	dup := []int{}
	for i, e := range edges {
		if ind[e[1]-1] == 2 {
			dup = append(dup, i)
		}
	}
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
	if len(dup) > 0 {
		for i, e := range edges {
			if i == dup[1] {
				continue
			}
			pu, pv := find(e[0]-1), find(e[1]-1)
			if pu == pv {
				return edges[dup[0]]
			}
			p[pu] = pv
		}
		return edges[dup[1]]
	}
	for _, e := range edges {
		pu, pv := find(e[0]-1), find(e[1]-1)
		if pu == pv {
			return e
		}
		p[pu] = pv
	}
	return nil
}
```

#### TypeScript

```ts
function findRedundantDirectedConnection(edges: number[][]): number[] {
    const n = edges.length;
    const ind: number[] = Array(n).fill(0);
    for (const [_, v] of edges) {
        ++ind[v - 1];
    }
    const dup: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (ind[edges[i][1] - 1] === 2) {
            dup.push(i);
        }
    }
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    if (dup.length) {
        for (let i = 0; i < n; ++i) {
            if (i === dup[1]) {
                continue;
            }
            const [pu, pv] = [find(edges[i][0] - 1), find(edges[i][1] - 1)];
            if (pu === pv) {
                return edges[dup[0]];
            }
            p[pu] = pv;
        }
        return edges[dup[1]];
    }
    for (let i = 0; ; ++i) {
        const [pu, pv] = [find(edges[i][0] - 1), find(edges[i][1] - 1)];
        if (pu === pv) {
            return edges[i];
        }
        p[pu] = pv;
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} edges
 * @return {number[]}
 */
var findRedundantDirectedConnection = function (edges) {
    const n = edges.length;
    const ind = Array(n).fill(0);
    for (const [_, v] of edges) {
        ++ind[v - 1];
    }
    const dup = [];
    for (let i = 0; i < n; ++i) {
        if (ind[edges[i][1] - 1] === 2) {
            dup.push(i);
        }
    }
    const p = Array.from({ length: n }, (_, i) => i);
    const find = x => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    if (dup.length) {
        for (let i = 0; i < n; ++i) {
            if (i === dup[1]) {
                continue;
            }
            const [pu, pv] = [find(edges[i][0] - 1), find(edges[i][1] - 1)];
            if (pu === pv) {
                return edges[dup[0]];
            }
            p[pu] = pv;
        }
        return edges[dup[1]];
    }
    for (let i = 0; ; ++i) {
        const [pu, pv] = [find(edges[i][0] - 1), find(edges[i][1] - 1)];
        if (pu === pv) {
            return edges[i];
        }
        p[pu] = pv;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Union-Find (Cách triển khai mẫu)

<!-- thinking:start -->

> **Tư duy**
>
> Vẫn chia các trường hợp như trên, nhưng dùng path compression và union-by-size trong class `UnionFind`. Phần xử lý chính chỉ cần xử lý `dup` và trường hợp union thất bại.

<!-- thinking:end -->

Dưới đây là cách triển khai mẫu bằng Union-Find để bạn tham khảo.

Độ phức tạp thời gian là $O(n \alpha(n))$, còn độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số cạnh và $\alpha(n)$ là hàm inverse Ackermann, có thể xem như một hằng số rất nhỏ.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    __slots__ = "p", "size"

    def __init__(self, n: int):
        self.p: List[int] = list(range(n))
        self.size: List[int] = [1] * n

    def find(self, x: int) -> int:
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, a: int, b: int) -> bool:
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
    def findRedundantDirectedConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        ind = [0] * n
        for _, v in edges:
            ind[v - 1] += 1
        dup = [i for i, (_, v) in enumerate(edges) if ind[v - 1] == 2]
        uf = UnionFind(n)
        if dup:
            for i, (u, v) in enumerate(edges):
                if i == dup[1]:
                    continue
                if not uf.union(u - 1, v - 1):
                    return edges[dup[0]]
            return edges[dup[1]]
        for i, (u, v) in enumerate(edges):
            if not uf.union(u - 1, v - 1):
                return edges[i]
```

#### Java

```java
class UnionFind {
    private final int[] p;
    private final int[] size;

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

    public boolean union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
    }
}

class Solution {
    public int[] findRedundantDirectedConnection(int[][] edges) {
        int n = edges.length;
        int[] ind = new int[n];
        for (var e : edges) {
            ++ind[e[1] - 1];
        }
        List<Integer> dup = new ArrayList<>();
        for (int i = 0; i < n; ++i) {
            if (ind[edges[i][1] - 1] == 2) {
                dup.add(i);
            }
        }
        UnionFind uf = new UnionFind(n);
        if (!dup.isEmpty()) {
            for (int i = 0; i < n; ++i) {
                if (i == dup.get(1)) {
                    continue;
                }
                if (!uf.union(edges[i][0] - 1, edges[i][1] - 1)) {
                    return edges[dup.get(0)];
                }
            }
            return edges[dup.get(1)];
        }
        for (int i = 0;; ++i) {
            if (!uf.union(edges[i][0] - 1, edges[i][1] - 1)) {
                return edges[i];
            }
        }
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

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) {
            return false;
        }
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        return true;
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
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> ind(n);
        for (const auto& e : edges) {
            ++ind[e[1] - 1];
        }
        vector<int> dup;
        for (int i = 0; i < n; ++i) {
            if (ind[edges[i][1] - 1] == 2) {
                dup.push_back(i);
            }
        }
        UnionFind uf(n);
        if (!dup.empty()) {
            for (int i = 0; i < n; ++i) {
                if (i == dup[1]) {
                    continue;
                }
                if (!uf.unite(edges[i][0] - 1, edges[i][1] - 1)) {
                    return edges[dup[0]];
                }
            }
            return edges[dup[1]];
        }
        for (int i = 0;; ++i) {
            if (!uf.unite(edges[i][0] - 1, edges[i][1] - 1)) {
                return edges[i];
            }
        }
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

func findRedundantDirectedConnection(edges [][]int) []int {
	n := len(edges)
	ind := make([]int, n)
	for _, e := range edges {
		ind[e[1]-1]++
	}
	dup := []int{}
	for i, e := range edges {
		if ind[e[1]-1] == 2 {
			dup = append(dup, i)
		}
	}
	uf := newUnionFind(n)
	if len(dup) > 0 {
		for i, e := range edges {
			if i == dup[1] {
				continue
			}
			if !uf.union(e[0]-1, e[1]-1) {
				return edges[dup[0]]
			}
		}
		return edges[dup[1]]
	}
	for _, e := range edges {
		if !uf.union(e[0]-1, e[1]-1) {
			return e
		}
	}
	return nil
}
```

#### TypeScript

```ts
class UnionFind {
    p: number[];
    size: number[];
    constructor(n: number) {
        this.p = Array.from({ length: n }, (_, i) => i);
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

function findRedundantDirectedConnection(edges: number[][]): number[] {
    const n = edges.length;
    const ind: number[] = Array(n).fill(0);
    for (const [_, v] of edges) {
        ++ind[v - 1];
    }
    const dup: number[] = [];
    for (let i = 0; i < n; ++i) {
        if (ind[edges[i][1] - 1] === 2) {
            dup.push(i);
        }
    }
    const uf = new UnionFind(n);
    if (dup.length) {
        for (let i = 0; i < n; ++i) {
            if (i === dup[1]) {
                continue;
            }
            if (!uf.union(edges[i][0] - 1, edges[i][1] - 1)) {
                return edges[dup[0]];
            }
        }
        return edges[dup[1]];
    }
    for (let i = 0; ; ++i) {
        if (!uf.union(edges[i][0] - 1, edges[i][1] - 1)) {
            return edges[i];
        }
    }
}
```

#### JavaScript

```js
class UnionFind {
    constructor(n) {
        this.p = Array.from({ length: n }, (_, i) => i);
        this.size = Array(n).fill(1);
    }

    find(x) {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a, b) {
        const pa = this.find(a);
        const pb = this.find(b);
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

/**
 * @param {number[][]} edges
 * @return {number[]}
 */
var findRedundantDirectedConnection = function (edges) {
    const n = edges.length;
    const ind = Array(n).fill(0);
    for (const [_, v] of edges) {
        ++ind[v - 1];
    }
    const dup = [];
    for (let i = 0; i < n; ++i) {
        if (ind[edges[i][1] - 1] === 2) {
            dup.push(i);
        }
    }
    const uf = new UnionFind(n);
    if (dup.length) {
        for (let i = 0; i < n; ++i) {
            if (i === dup[1]) {
                continue;
            }
            if (!uf.union(edges[i][0] - 1, edges[i][1] - 1)) {
                return edges[dup[0]];
            }
        }
        return edges[dup[1]];
    }
    for (let i = 0; ; ++i) {
        if (!uf.union(edges[i][0] - 1, edges[i][1] - 1)) {
            return edges[i];
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
