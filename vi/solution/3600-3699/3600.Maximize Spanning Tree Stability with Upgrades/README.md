---
comments: true
difficulty: Hard
rating: 2301
source: Weekly Contest 456 Q4
tags:
    - Greedy
    - Union Find
    - Graph
    - Binary Search
    - Minimum Spanning Tree
---

<!-- problem:start -->

# [3600. Maximize Spanning Tree Stability with Upgrades](https://leetcode.com/problems/maximize-spanning-tree-stability-with-upgrades)

[中文文档](/solution/3600-3699/3600.Maximize%20Spanning%20Tree%20Stability%20with%20Upgrades/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cung cấp một số nguyên <code>n</code>, biểu thị <code>n</code> node được đánh số từ 0 đến <code>n - 1</code>, và một danh sách <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, s<sub>i</sub>, must<sub>i</sub>]</code>:</p>

<ul>
    <li><code>u<sub>i</sub></code> và <code>v<sub>i</sub></code> biểu thị một cạnh vô hướng giữa các node <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>.</li>
    <li><code>s<sub>i</sub></code> là strength của cạnh.</li>
    <li><code>must<sub>i</sub></code> là một số nguyên (0 hoặc 1). Nếu <code>must<sub>i</sub> == 1</code>, cạnh đó <strong>bắt buộc</strong> phải được đưa vào<strong> </strong><strong>spanning tree</strong>. Các cạnh này <strong>không thể</strong> được <strong>upgrade</strong>.</li>
</ul>

<p>Bạn cũng được cung cấp một số nguyên <code>k</code>, là số lần <strong>upgrade tối đa</strong> có thể thực hiện. Mỗi lần upgrade sẽ <strong>nhân đôi</strong> strength của một cạnh, và mỗi cạnh đủ điều kiện (với <code>must<sub>i</sub> == 0</code>) chỉ có thể được upgrade <strong>tối đa</strong> một lần.</p>

<p><strong>Stability</strong> của một spanning tree được định nghĩa là strength <strong>nhỏ nhất</strong> trong tất cả các cạnh được đưa vào cây.</p>

<p>Trả về stability <strong>lớn nhất</strong> có thể đạt được của một spanning tree hợp lệ. Nếu không thể kết nối tất cả node, trả về <code>-1</code>.</p>

<p><strong>Lưu ý</strong>: <strong>spanning tree</strong> của một đồ thị có <code>n</code> node là một tập con các cạnh kết nối tất cả node với nhau (tức là đồ thị <strong>liên thông</strong>), <em>không</em> tạo thành chu trình và sử dụng <strong>chính xác</strong> <code>n - 1</code> cạnh.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,2,1],[1,2,3,0]], k = 1</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">2</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Cạnh <code>[0,1]</code> với strength = 2 bắt buộc phải được đưa vào spanning tree.</li>
    <li>Cạnh <code>[1,2]</code> là tùy chọn và có thể được upgrade từ 3 lên 6 bằng một lần upgrade.</li>
    <li>Spanning tree kết quả gồm hai cạnh này với strength lần lượt là 2 và 6.</li>
    <li>Strength nhỏ nhất trong spanning tree là 2, đây là stability lớn nhất có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,4,0],[1,2,3,0],[0,2,1,0]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">6</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Vì tất cả các cạnh đều là tùy chọn và cho phép tối đa <code>k = 2</code> lần upgrade.</li>
    <li>Upgrade cạnh <code>[0,1]</code> từ 4 lên 8 và cạnh <code>[1,2]</code> từ 3 lên 6.</li>
    <li>Spanning tree kết quả gồm hai cạnh này với strength lần lượt là 8 và 6.</li>
    <li>Strength nhỏ nhất trong cây là 6, đây là stability lớn nhất có thể đạt được.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,1,1],[1,2,1,1],[2,0,1,1]], k = 0</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">-1</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Tất cả các cạnh đều bắt buộc và tạo thành một chu trình, vi phạm tính chất không chu trình của spanning tree. Vì vậy, đáp án là -1.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>2 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
    <li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, s<sub>i</sub>, must<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li><code>1 &lt;= s<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
    <li><code>must<sub>i</sub></code> là <code>0</code> hoặc <code>1</code>.</li>
    <li><code>0 &lt;= k &lt;= n</code></li>
    <li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Tìm kiếm nhị phân + Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Việc liệt kê các spanning tree và các tập con gồm tối đa $k$ lần upgrade có độ phức tạp lũy thừa, không thể thực hiện với $n,m\le 10^5$. Stability là strength nhỏ nhất của cây, vì vậy tính khả thi của một ngưỡng $x$ có tính đơn điệu và ta có thể dùng tìm kiếm nhị phân để tìm giá trị lớn nhất.
>
> Các cạnh bắt buộc không thể được upgrade, nên strength nhỏ nhất $mn$ của chúng là cận trên. Nếu các cạnh bắt buộc tạo thành chu trình, hoặc đồ thị vẫn không liên thông sau khi thêm mọi cạnh, thì không có đáp án.
>
> Với một $\textit{lim}$, gộp mọi cạnh có strength ít nhất $\textit{lim}$, sau đó dùng tối đa $k$ lần upgrade cho các cạnh thỏa mãn $2s\ge \textit{lim}$. Union-find quyết định tính liên thông, nên một số lượng kiểm tra theo cấp số logarit sẽ cho stability lớn nhất có thể đạt được.

<!-- thinking:end -->

Theo mô tả bài toán, stability của một spanning tree được quyết định bởi cạnh có strength nhỏ nhất trong cây. Nếu stability $x$ khả thi thì với mọi $y < x$, stability $y$ cũng khả thi. Vì vậy, ta có thể dùng tìm kiếm nhị phân để tìm stability lớn nhất.

Trước tiên, ta thêm tất cả các cạnh bắt buộc vào Union-Find và ghi nhận strength nhỏ nhất $mn$ trong số đó. Nếu các cạnh bắt buộc tạo thành chu trình, ta trả về $-1$ ngay lập tức. Sau đó, ta thêm tất cả các cạnh vào Union-Find; nếu số thành phần liên thông cuối cùng lớn hơn $1$, nghĩa là không thể kết nối tất cả các node, và ta trả về $-1$.

Tiếp theo, ta tìm kiếm nhị phân trong đoạn $[1, mn]$. Ta định nghĩa hàm $\text{check}(lim)$ để kiểm tra xem có tồn tại spanning tree với stability ít nhất $lim$ hay không. Trong hàm $\text{check}$, trước tiên ta thêm vào Union-Find tất cả các cạnh có strength không nhỏ hơn $lim$. Sau đó, ta thử dùng số lần upgrade còn lại để kết nối các cạnh còn lại, với điều kiện strength của cạnh ít nhất là $lim/2$ (vì upgrade sẽ nhân đôi strength). Nếu số thành phần liên thông cuối cùng trong Union-Find là $1$, tồn tại một spanning tree thỏa mãn điều kiện.

Độ phức tạp thời gian là $O((m \times \alpha(n) + n) \times \log M)$, và độ phức tạp không gian là $O(n)$. Trong đó, $m$ là số cạnh, $n$ là số node và $M$ là strength lớn nhất của một cạnh.

<!-- tabs:start -->

#### Python3

```python
class UnionFind:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1] * n
        self.cnt = n

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
        self.cnt -= 1
        return True


class Solution:
    def maxStability(self, n: int, edges: List[List[int]], k: int) -> int:
        def check(lim: int) -> bool:
            uf = UnionFind(n)
            for u, v, s, _ in edges:
                if s >= lim:
                    uf.union(u, v)
            rem = k
            for u, v, s, _ in edges:
                if s * 2 >= lim and rem > 0:
                    if uf.union(u, v):
                        rem -= 1
            return uf.cnt == 1

        uf = UnionFind(n)
        mn = 10**6
        for u, v, s, must in edges:
            if must:
                mn = min(mn, s)
                if not uf.union(u, v):
                    return -1
        for u, v, _, _ in edges:
            uf.union(u, v)
        if uf.cnt > 1:
            return -1
        l, r = 1, mn
        while l < r:
            mid = (l + r + 1) >> 1
            if check(mid):
                l = mid
            else:
                r = mid - 1
        return l
```

#### Java

```java
class UnionFind {
    int[] p, size;
    int cnt;

    UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        cnt = n;
        for (int i = 0; i < n; i++) {
            p[i] = i;
            size[i] = 1;
        }
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    boolean union(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) return false;
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        cnt--;
        return true;
    }
}

class Solution {

    int n;
    int[][] edges;
    int k;

    private boolean check(int lim) {
        UnionFind uf = new UnionFind(n);

        for (int[] e : edges) {
            int u = e[0], v = e[1], s = e[2];
            if (s >= lim) {
                uf.union(u, v);
            }
        }

        int rem = k;
        for (int[] e : edges) {
            int u = e[0], v = e[1], s = e[2];
            if (s * 2 >= lim && rem > 0) {
                if (uf.union(u, v)) {
                    rem--;
                }
            }
        }

        return uf.cnt == 1;
    }

    public int maxStability(int n, int[][] edges, int k) {
        this.n = n;
        this.edges = edges;
        this.k = k;

        UnionFind uf = new UnionFind(n);
        int mn = (int)1e6;

        for (int[] e : edges) {
            int u = e[0], v = e[1], s = e[2], must = e[3];
            if (must == 1) {
                mn = Math.min(mn, s);
                if (!uf.union(u, v)) {
                    return -1;
                }
            }
        }

        for (int[] e : edges) {
            uf.union(e[0], e[1]);
        }

        if (uf.cnt > 1) {
            return -1;
        }

        int l = 1, r = mn;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        return l;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    vector<int> p, size;
    int cnt;

    UnionFind(int n) {
        p.resize(n);
        size.assign(n, 1);
        cnt = n;
        for (int i = 0; i < n; i++) p[i] = i;
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }

    bool unite(int a, int b) {
        int pa = find(a), pb = find(b);
        if (pa == pb) return false;
        if (size[pa] > size[pb]) {
            p[pb] = pa;
            size[pa] += size[pb];
        } else {
            p[pa] = pb;
            size[pb] += size[pa];
        }
        cnt--;
        return true;
    }
};

class Solution {
public:
    int n, k;
    vector<vector<int>> edges;

    bool check(int lim) {
        UnionFind uf(n);

        for (auto& e : edges) {
            int u = e[0], v = e[1], s = e[2];
            if (s >= lim) {
                uf.unite(u, v);
            }
        }

        int rem = k;
        for (auto& e : edges) {
            int u = e[0], v = e[1], s = e[2];
            if (s * 2 >= lim && rem > 0) {
                if (uf.unite(u, v)) {
                    rem--;
                }
            }
        }

        return uf.cnt == 1;
    }

    int maxStability(int n, vector<vector<int>>& edges, int k) {
        this->n = n;
        this->edges = edges;
        this->k = k;

        UnionFind uf(n);
        int mn = 1e6;

        for (auto& e : edges) {
            int u = e[0], v = e[1], s = e[2], must = e[3];
            if (must) {
                mn = min(mn, s);
                if (!uf.unite(u, v)) {
                    return -1;
                }
            }
        }

        for (auto& e : edges) {
            uf.unite(e[0], e[1]);
        }

        if (uf.cnt > 1) {
            return -1;
        }

        int l = 1, r = mn;
        while (l < r) {
            int mid = (l + r + 1) >> 1;
            if (check(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        return l;
    }
};
```

#### Go

```go
type UnionFind struct {
    p    []int
    size []int
    cnt  int
}

func NewUnionFind(n int) *UnionFind {
    p := make([]int, n)
    size := make([]int, n)
    for i := range p {
        p[i] = i
        size[i] = 1
    }
    return &UnionFind{p, size, n}
}

func (uf *UnionFind) find(x int) int {
    if uf.p[x] != x {
        uf.p[x] = uf.find(uf.p[x])
    }
    return uf.p[x]
}

func (uf *UnionFind) union(a, b int) bool {
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
    uf.cnt--
    return true
}

var (
    N int
    E [][]int
    K int
)

func check(lim int) bool {
    uf := NewUnionFind(N)

    for _, e := range E {
        u, v, s := e[0], e[1], e[2]
        if s >= lim {
            uf.union(u, v)
        }
    }

    rem := K
    for _, e := range E {
        u, v, s := e[0], e[1], e[2]
        if s*2 >= lim && rem > 0 {
            if uf.union(u, v) {
                rem--
            }
        }
    }

    return uf.cnt == 1
}

func maxStability(n int, edges [][]int, k int) int {
    N = n
    E = edges
    K = k

    uf := NewUnionFind(n)
    mn := int(1e6)

    for _, e := range edges {
        u, v, s, must := e[0], e[1], e[2], e[3]
        if must == 1 {
            if s < mn {
                mn = s
            }
            if !uf.union(u, v) {
                return -1
            }
        }
    }

    for _, e := range edges {
        uf.union(e[0], e[1])
    }

    if uf.cnt > 1 {
        return -1
    }

    l, r := 1, mn
    for l < r {
        mid := (l + r + 1) >> 1
        if check(mid) {
            l = mid
        } else {
            r = mid - 1
        }
    }

    return l
}
```

#### TypeScript

```ts
class UnionFind {
    p: number[];
    size: number[];
    cnt: number;

    constructor(n: number) {
        this.p = Array.from({ length: n }, (_, i) => i);
        this.size = new Array(n).fill(1);
        this.cnt = n;
    }

    find(x: number): number {
        if (this.p[x] !== x) {
            this.p[x] = this.find(this.p[x]);
        }
        return this.p[x];
    }

    union(a: number, b: number): boolean {
        const pa = this.find(a);
        const pb = this.find(b);
        if (pa === pb) return false;

        if (this.size[pa] > this.size[pb]) {
            this.p[pb] = pa;
            this.size[pa] += this.size[pb];
        } else {
            this.p[pa] = pb;
            this.size[pb] += this.size[pa];
        }

        this.cnt--;
        return true;
    }
}

let N: number;
let E: number[][];
let K: number;

function check(lim: number): boolean {
    const uf = new UnionFind(N);

    for (const [u, v, s] of E) {
        if (s >= lim) {
            uf.union(u, v);
        }
    }

    let rem = K;
    for (const [u, v, s] of E) {
        if (s * 2 >= lim && rem > 0) {
            if (uf.union(u, v)) {
                rem--;
            }
        }
    }

    return uf.cnt === 1;
}

function maxStability(n: number, edges: number[][], k: number): number {
    N = n;
    E = edges;
    K = k;

    const uf = new UnionFind(n);
    let mn = 1e6;

    for (const [u, v, s, must] of edges) {
        if (must) {
            mn = Math.min(mn, s);
            if (!uf.union(u, v)) return -1;
        }
    }

    for (const [u, v] of edges) {
        uf.union(u, v);
    }

    if (uf.cnt > 1) return -1;

    let l = 1,
        r = mn;

    while (l < r) {
        const mid = (l + r + 1) >> 1;
        if (check(mid)) {
            l = mid;
        } else {
            r = mid - 1;
        }
    }

    return l;
}
```

#### Rust

```rust
struct UnionFind {
    p: Vec<i32>,
    sz: Vec<i32>,
    cnt: i32,
}

impl UnionFind {
    fn new(n: i32) -> Self {
        Self {
            p: (0..n).collect(),
            sz: vec![1; n as usize],
            cnt: n,
        }
    }

    fn find(&mut self, x: i32) -> i32 {
        let i = x as usize;
        if self.p[i] != x {
            self.p[i] = self.find(self.p[i]);
        }
        self.p[i]
    }

    fn union(&mut self, a: i32, b: i32) -> bool {
        let (pa, pb) = (self.find(a), self.find(b));
        if pa == pb {
            return false;
        }
        let (a, b) = (pa as usize, pb as usize);
        if self.sz[a] < self.sz[b] {
            self.p[a] = pb;
            self.sz[b] += self.sz[a];
        } else {
            self.p[b] = pa;
            self.sz[a] += self.sz[b];
        }
        self.cnt -= 1;
        true
    }
}

impl Solution {
    pub fn max_stability(n: i32, edges: Vec<Vec<i32>>, k: i32) -> i32 {
        let mut uf = UnionFind::new(n);
        let mut mn = 1_000_000;

        for e in &edges {
            if e[3] == 1 {
                mn = mn.min(e[2]);
                if !uf.union(e[0], e[1]) {
                    return -1;
                }
            }
        }

        for e in &edges {
            uf.union(e[0], e[1]);
        }

        if uf.cnt > 1 {
            return -1;
        }

        let check = |lim: i32| {
            let mut uf = UnionFind::new(n);

            for e in &edges {
                if e[2] >= lim {
                    uf.union(e[0], e[1]);
                }
            }

            let mut rem = k;
            for e in &edges {
                if rem > 0 && e[2] * 2 >= lim && uf.union(e[0], e[1]) {
                    rem -= 1;
                }
            }

            uf.cnt == 1
        };

        let (mut l, mut r) = (1, mn);
        while l < r {
            let mid = (l + r + 1) >> 1;
            if check(mid) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        l
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
