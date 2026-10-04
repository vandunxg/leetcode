---
comments: true
difficulty: Hard
rating: 2108
source: Weekly Contest 392 Q4
tags:
    - Bit Manipulation
    - Union Find
    - Graph
    - Array
---

<!-- problem:start -->

# [3108. Minimum Cost Walk in Weighted Graph](https://leetcode.com/problems/minimum-cost-walk-in-weighted-graph)

[中文文档](/solution/3100-3199/3108.Minimum%20Cost%20Walk%20in%20Weighted%20Graph/README.md)

## Mô tả

<!-- description:start -->

<p>Có một đồ thị vô hướng có trọng số gồm <code>n</code> đỉnh được đánh số từ <code>0</code> đến <code>n - 1</code>.</p>

<p>Bạn được cho số nguyên <code>n</code> và một mảng <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, w<sub>i</sub>]</code> cho biết có một cạnh nối hai đỉnh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>, với trọng số <code>w<sub>i</sub></code>.</p>

<p>Một walk trên đồ thị là một dãy các đỉnh và cạnh. Walk bắt đầu và kết thúc tại một đỉnh, đồng thời mỗi cạnh nối đỉnh đứng trước nó với đỉnh đứng sau nó. Lưu ý rằng một walk có thể đi qua cùng một cạnh hoặc đỉnh nhiều hơn một lần.</p>

<p><strong>Cost</strong> của một walk bắt đầu tại node <code>u</code> và kết thúc tại node <code>v</code> được định nghĩa là phép <code>AND</code> bitwise của các trọng số cạnh đã đi qua. Nói cách khác, nếu dãy trọng số cạnh đi qua là <code>w<sub>0</sub>, w<sub>1</sub>, w<sub>2</sub>, ..., w<sub>k</sub></code>, thì cost được tính là <code>w<sub>0</sub> &amp; w<sub>1</sub> &amp; w<sub>2</sub> &amp; ... &amp; w<sub>k</sub></code>, trong đó <code>&amp;</code> là toán tử <code>AND</code> bitwise.</p>

<p>Bạn cũng được cho một mảng 2 chiều <code>query</code>, trong đó <code>query[i] = [s<sub>i</sub>, t<sub>i</sub>]</code>. Với mỗi query, hãy tìm cost nhỏ nhất của walk bắt đầu tại đỉnh <code>s<sub>i</sub></code> và kết thúc tại đỉnh <code>t<sub>i</sub></code>. Nếu không tồn tại walk như vậy, đáp án là <code>-1</code>.</p>

<p>Trả về <em>mảng </em><code>answer</code><em>, trong đó </em><code>answer[i]</code><em> là cost <strong>nhỏ nhất</strong> của một walk cho query </em><code>i</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 5, edges = [[0,1,7],[1,3,7],[1,2,1]], query = [[0,3],[3,4]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,-1]</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3108.Minimum%20Cost%20Walk%20in%20Weighted%20Graph/images/q4_example1-1.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 351px; height: 141px;" />
<p>Để đạt được cost bằng 1 trong query đầu tiên, ta cần đi qua các cạnh sau: <code>0-&gt;1</code> (trọng số 7), <code>1-&gt;2</code> (trọng số 1), <code>2-&gt;1</code> (trọng số 1), <code>1-&gt;3</code> (trọng số 7).</p>

<p>Trong query thứ hai, không có walk nào giữa node 3 và 4, nên đáp án là -1.</p>

<p><strong class="example">Ví dụ 2:</strong></p>
</div>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,2,7],[0,1,15],[1,2,6],[1,2,1]], query = [[1,2]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[0]</span></p>

<p><strong>Giải thích:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3100-3199/3108.Minimum%20Cost%20Walk%20in%20Weighted%20Graph/images/q4_example2e.png" style="padding: 10px; background: rgb(255, 255, 255); border-radius: 0.5rem; width: 211px; height: 181px;" />
<p>Để đạt được cost bằng 0 trong query đầu tiên, ta cần đi qua các cạnh sau: <code>1-&gt;2</code> (trọng số 1), <code>2-&gt;1</code> (trọng số 6), <code>1-&gt;2</code> (trọng số 1).</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
	<li><code>edges[i].length == 3</code></li>
	<li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
	<li><code>0 &lt;= w<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= query.length &lt;= 10<sup>5</sup></code></li>
	<li><code>query[i].length == 2</code></li>
	<li><code>0 &lt;= s<sub>i</sub>, t<sub>i</sub> &lt;= n - 1</code></li>
	<li><code>s<sub>i</sub> !=&nbsp;t<sub>i</sub></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Greedy + Union Find

<!-- thinking:start -->

> **Tư duy**
>
> Cost của walk là phép AND bitwise của các trọng số cạnh, và việc đi lại qua các cạnh chỉ có thể làm cost giảm. Nếu tìm lại từ đầu cho từng query, ta sẽ lặp lại công việc trong cùng một component khi $q$ lớn.
>
> Phép AND của các số nguyên dương có tính đơn điệu giảm, nên cost nhỏ nhất trong một component là phép AND của mọi cạnh trong component đó. Các component khác nhau không có walk nối với nhau.
>
> Union tất cả các cạnh, sau đó AND từng trọng số cạnh vào $g[\textit{root}]$ của component tương ứng. Với mỗi query, trả về giá trị đó nếu hai đầu mút có cùng root, trả về $0$ nếu chúng trùng nhau, và $-1$ trong các trường hợp còn lại.

<!-- thinking:end -->

Ta nhận thấy khi một số nguyên dương thực hiện phép AND bitwise với nhiều số nguyên dương khác, kết quả chỉ có thể nhỏ đi. Vì vậy, để tối thiểu hóa cost của walk, ta nên thực hiện phép AND bitwise trên trọng số của tất cả các cạnh trong cùng một connected component, rồi trả lời query.

Như vậy, bài toán được chuyển thành việc tìm tất cả các cạnh trong cùng một connected component và thực hiện phép AND bitwise.

Ta có thể dùng union-find set để duy trì các connected component.

Cụ thể, ta duyệt qua từng cạnh $(u, v, w)$ và merge $u$ với $v$. Sau đó, ta lại duyệt qua từng cạnh $(u, v, w)$, tìm node gốc $root$ của connected component chứa $u$ và $v$, rồi dùng mảng $g$ để ghi lại kết quả phép AND bitwise của trọng số tất cả các cạnh trong mỗi connected component.

Cuối cùng, với mỗi query $(s, t)$, trước tiên ta kiểm tra xem $s$ có bằng $t$ hay không. Nếu bằng nhau, đáp án là $0$. Nếu không, ta kiểm tra xem $s$ và $t$ có thuộc cùng một connected component hay không. Nếu có, đáp án là giá trị $g$ của node gốc thuộc connected component của query này. Nếu không, đáp án là $-1$.

Độ phức tạp thời gian là $O((n + m + q) \times \alpha(n))$, và độ phức tạp không gian là $O(n)$. Ở đây, $n$, $m$ và $q$ lần lượt là số node, cạnh và query, còn $\alpha(n)$ là hàm nghịch đảo của hàm Ackermann.

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
    def minimumCost(
        self, n: int, edges: List[List[int]], query: List[List[int]]
    ) -> List[int]:
        g = [-1] * n
        uf = UnionFind(n)
        for u, v, _ in edges:
            uf.union(u, v)
        for u, _, w in edges:
            root = uf.find(u)
            g[root] &= w

        def f(u: int, v: int) -> int:
            if u == v:
                return 0
            a, b = uf.find(u), uf.find(v)
            return g[a] if a == b else -1

        return [f(s, t) for s, t in query]
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

    public int size(int x) {
        return size[find(x)];
    }
}

class Solution {
    private UnionFind uf;
    private int[] g;

    public int[] minimumCost(int n, int[][] edges, int[][] query) {
        uf = new UnionFind(n);
        for (var e : edges) {
            uf.union(e[0], e[1]);
        }
        g = new int[n];
        Arrays.fill(g, -1);
        for (var e : edges) {
            int root = uf.find(e[0]);
            g[root] &= e[2];
        }
        int m = query.length;
        int[] ans = new int[m];
        for (int i = 0; i < m; ++i) {
            int s = query[i][0], t = query[i][1];
            ans[i] = f(s, t);
        }
        return ans;
    }

    private int f(int u, int v) {
        if (u == v) {
            return 0;
        }
        int a = uf.find(u), b = uf.find(v);
        return a == b ? g[a] : -1;
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

    int getSize(int x) {
        return size[find(x)];
    }

private:
    vector<int> p, size;
};

class Solution {
public:
    vector<int> minimumCost(int n, vector<vector<int>>& edges, vector<vector<int>>& query) {
        g = vector<int>(n, -1);
        uf = new UnionFind(n);
        for (auto& e : edges) {
            uf->unite(e[0], e[1]);
        }
        for (auto& e : edges) {
            int root = uf->find(e[0]);
            g[root] &= e[2];
        }
        vector<int> ans;
        for (auto& q : query) {
            ans.push_back(f(q[0], q[1]));
        }
        return ans;
    }

private:
    UnionFind* uf;
    vector<int> g;

    int f(int u, int v) {
        if (u == v) {
            return 0;
        }
        int a = uf->find(u), b = uf->find(v);
        return a == b ? g[a] : -1;
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

func (uf *unionFind) getSize(x int) int {
	return uf.size[uf.find(x)]
}

func minimumCost(n int, edges [][]int, query [][]int) (ans []int) {
	uf := newUnionFind(n)
	g := make([]int, n)
	for i := range g {
		g[i] = -1
	}
	for _, e := range edges {
		uf.union(e[0], e[1])
	}
	for _, e := range edges {
		root := uf.find(e[0])
		g[root] &= e[2]
	}
	f := func(u, v int) int {
		if u == v {
			return 0
		}
		a, b := uf.find(u), uf.find(v)
		if a == b {
			return g[a]
		}
		return -1
	}
	for _, q := range query {
		ans = append(ans, f(q[0], q[1]))
	}
	return
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

    getSize(x: number): number {
        return this.size[this.find(x)];
    }
}

function minimumCost(n: number, edges: number[][], query: number[][]): number[] {
    const uf = new UnionFind(n);
    const g: number[] = Array(n).fill(-1);
    for (const [u, v, _] of edges) {
        uf.union(u, v);
    }
    for (const [u, _, w] of edges) {
        const root = uf.find(u);
        g[root] &= w;
    }
    const f = (u: number, v: number): number => {
        if (u === v) {
            return 0;
        }
        const [a, b] = [uf.find(u), uf.find(v)];
        return a === b ? g[a] : -1;
    };
    return query.map(([u, v]) => f(u, v));
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
