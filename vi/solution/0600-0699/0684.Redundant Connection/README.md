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

# [684. Redundant Connection](https://leetcode.com/problems/redundant-connection)

[中文文档](/solution/0600-0699/0684.Redundant%20Connection/README.md)

## Mô tả

<!-- description:start -->

<p>Trong bài toán này, cây là một <strong>đồ thị vô hướng</strong> liên thông và không có chu trình.</p>

<p>Cho đồ thị ban đầu là một cây có <code>n</code> node được đánh nhãn từ <code>1</code> đến <code>n</code>, sau đó thêm một cạnh. Cạnh mới nối hai đỉnh <strong>khác nhau</strong> được chọn trong khoảng <code>1</code> đến <code>n</code> và chưa từng có cạnh nối giữa chúng. Đồ thị được biểu diễn bằng mảng <code>edges</code> có độ dài <code>n</code>, trong đó <code>edges[i] = [a<sub>i</sub>, b<sub>i</sub>]</code> cho biết có cạnh nối node <code>a<sub>i</sub></code> và <code>b<sub>i</sub></code>.</p>

<p>Hãy trả về <em>một cạnh có thể xóa để đồ thị còn lại là cây gồm </em><code>n</code><em> node</em>. Nếu có nhiều đáp án, hãy trả về cạnh xuất hiện cuối cùng trong input.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0684.Redundant%20Connection/images/reduntant1-1-graph.jpg" style="width: 222px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[1,2],[1,3],[2,3]]
<strong>Đầu ra:</strong> [2,3]
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0600-0699/0684.Redundant%20Connection/images/reduntant1-2-graph.jpg" style="width: 382px; height: 222px;" />
<pre>
<strong>Đầu vào:</strong> edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
<strong>Đầu ra:</strong> [1,4]
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == edges.length</code></li>
	<li><code>3 &lt;= n &lt;= 1000</code></li>
	<li><code>edges[i].length == 2</code></li>
	<li><code>1 &lt;= a<sub>i</sub> &lt; b<sub>i</sub> &lt;= edges.length</code></li>
	<li><code>a<sub>i</sub> != b<sub>i</sub></code></li>
	<li>Không có cạnh nào bị lặp.</li>
	<li>Đồ thị đã cho là liên thông.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Đồ thị gồm một cây vô hướng và một cạnh bổ sung; cần xóa cạnh dư xuất hiện cuối cùng trong input. Tìm chu trình rồi xác định thứ tự các cạnh là việc không cần thiết.
>
> Duyệt các cạnh theo thứ tự bằng Union-Find. Cặp đầu tiên đã thuộc cùng một component chính là cạnh dư.

<!-- thinking:end -->

Theo đề bài, ta cần tìm một cạnh có thể xóa để phần đồ thị còn lại là cây có $n$ node. Ta duyệt từng cạnh và kiểm tra hai node ở hai đầu cạnh có thuộc cùng một component liên thông hay không. Nếu có, cạnh đó là cạnh dư và có thể xóa, nên ta trả về ngay cạnh này. Nếu không, ta hợp nhất hai node được nối bởi cạnh vào cùng một component.

Độ phức tạp thời gian là $O(n \log n)$ và độ phức tạp không gian là $O(n)$, trong đó $n$ là số cạnh.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        def find(x: int) -> int:
            if p[x] != x:
                p[x] = find(p[x])
            return p[x]

        p = list(range(len(edges)))
        for a, b in edges:
            pa, pb = find(a - 1), find(b - 1)
            if pa == pb:
                return [a, b]
            p[pa] = pb
```

#### Java

```java
class Solution {
    private int[] p;

    public int[] findRedundantConnection(int[][] edges) {
        int n = edges.length;
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        for (int i = 0;; ++i) {
            int pa = find(edges[i][0] - 1);
            int pb = find(edges[i][1] - 1);
            if (pa == pb) {
                return edges[i];
            }
            p[pa] = pb;
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
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> p(n);
        iota(p.begin(), p.end(), 0);
        function<int(int)> find = [&](int x) {
            return x == p[x] ? x : p[x] = find(p[x]);
        };
        for (int i = 0;; ++i) {
            int pa = find(edges[i][0] - 1);
            int pb = find(edges[i][1] - 1);
            if (pa == pb) {
                return edges[i];
            }
            p[pa] = pb;
        }
    }
};
```

#### Go

```go
func findRedundantConnection(edges [][]int) []int {
	n := len(edges)
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
	for i := 0; ; i++ {
		pa, pb := find(edges[i][0]-1), find(edges[i][1]-1)
		if pa == pb {
			return edges[i]
		}
		p[pa] = pb
	}
}
```

#### TypeScript

```ts
function findRedundantConnection(edges: number[][]): number[] {
    const n = edges.length;
    const p: number[] = Array.from({ length: n }, (_, i) => i);
    const find = (x: number): number => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    for (let i = 0; ; ++i) {
        const pa = find(edges[i][0] - 1);
        const pb = find(edges[i][1] - 1);
        if (pa === pb) {
            return edges[i];
        }
        p[pa] = pb;
    }
}
```

#### JavaScript

```js
/**
 * @param {number[][]} edges
 * @return {number[]}
 */
var findRedundantConnection = function (edges) {
    const n = edges.length;
    const p = Array.from({ length: n }, (_, i) => i);
    const find = x => {
        if (p[x] !== x) {
            p[x] = find(p[x]);
        }
        return p[x];
    };
    for (let i = 0; ; ++i) {
        const pa = find(edges[i][0] - 1);
        const pb = find(edges[i][1] - 1);
        if (pa === pb) {
            return edges[i];
        }
        p[pa] = pb;
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Union-Find (Cách dùng template)

<!-- thinking:start -->

> **Tư duy**
>
> Ở lời giải 1, hàm `find` được viết trực tiếp. Cấu trúc `UnionFind` dùng union-by-size trả về trạng thái thành công/thất bại; cạnh đầu tiên có thao tác union thất bại chính là cạnh cần tìm, với hằng số thời gian nhỏ hơn một chút.

<!-- thinking:end -->

Dưới đây là cách dùng template Union-Find để bạn tham khảo.

Độ phức tạp thời gian là $O(n \alpha(n))$ và độ phức tạp không gian là $O(n)$. Ở đây, $n$ là số cạnh và $\alpha(n)$ là hàm Ackermann nghịch đảo, có thể xem như một hằng số rất nhỏ.

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
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        uf = UnionFind(len(edges))
        for a, b in edges:
            if not uf.union(a - 1, b - 1):
                return [a, b]
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
    public int[] findRedundantConnection(int[][] edges) {
        UnionFind uf = new UnionFind(edges.length);
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
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        UnionFind uf(edges.size());
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

func findRedundantConnection(edges [][]int) []int {
	uf := newUnionFind(len(edges))
	for i := 0; ; i++ {
		if !uf.union(edges[i][0]-1, edges[i][1]-1) {
			return edges[i]
		}
	}
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

function findRedundantConnection(edges: number[][]): number[] {
    const uf = new UnionFind(edges.length);
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
var findRedundantConnection = function (edges) {
    const uf = new UnionFind(edges.length);
    for (let i = 0; i < edges.length; i++) {
        if (!uf.union(edges[i][0] - 1, edges[i][1] - 1)) {
            return edges[i];
        }
    }
};
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
