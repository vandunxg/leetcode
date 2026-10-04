---
comments: true
difficulty: Medium
rating: 1892
source: Weekly Contest 457 Q3
tags:
    - Union Find
    - Graph
    - Binary Search
    - Sorting
---

<!-- problem:start -->

# [3608. Minimum Time for K Connected Components](https://leetcode.com/problems/minimum-time-for-k-connected-components)

[中文文档](/solution/3600-3699/3608.Minimum%20Time%20for%20K%20Connected%20Components/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một số nguyên <code>n</code> và một đồ thị vô hướng gồm <code>n</code> đỉnh, được đánh số từ 0 đến <code>n - 1</code>. Đồ thị được biểu diễn bằng một mảng 2 chiều <code>edges</code>, trong đó <code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, time<sub>i</sub>]</code> biểu thị một cạnh vô hướng nối các đỉnh <code>u<sub>i</sub></code> và <code>v<sub>i</sub></code>, có thể bị xóa tại <code>time<sub>i</sub></code>.</p>

<p>Bạn cũng được cho một số nguyên <code>k</code>.</p>

<p>Ban đầu, đồ thị có thể liên thông hoặc không liên thông. Nhiệm vụ của bạn là tìm thời điểm <strong>nhỏ nhất</strong> <code>t</code> sao cho sau khi xóa tất cả các cạnh có <code>time &lt;= t</code>, đồ thị có <strong>ít nhất</strong> <code>k</code> thành phần liên thông.</p>

<p>Trả về thời điểm <strong>nhỏ nhất</strong> <code>t</code>.</p>

<p><strong>Thành phần liên thông</strong> là một đồ thị con của đồ thị, trong đó tồn tại một đường đi giữa mọi cặp đỉnh, và không có đỉnh nào của đồ thị con có cạnh nối với một đỉnh bên ngoài đồ thị con.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 2, edges = [[0,1,3]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">3</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3608.Minimum%20Time%20for%20K%20Connected%20Components/images/screenshot-2025-06-01-at-022724.png" style="width: 230px; height: 85px;" /></p>

<ul>
    <li>Ban đầu, có một thành phần liên thông <code>{0, 1}</code>.</li>
    <li>Tại <code>time = 1</code> hoặc <code>2</code>, đồ thị không thay đổi.</li>
    <li>Tại <code>time = 3</code>, cạnh <code>[0, 1]</code> bị xóa, tạo ra <code>k = 2</code> thành phần liên thông <code>{0}</code>, <code>{1}</code>. Do đó, đáp án là 3.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,1,2],[1,2,4]], k = 3</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">4</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3608.Minimum%20Time%20for%20K%20Connected%20Components/images/screenshot-2025-06-01-at-022812.png" style="width: 180px; height: 164px;" /></p>

<ul>
    <li>Ban đầu, có một thành phần liên thông <code>{0, 1, 2}</code>.</li>
    <li>Tại <code>time = 2</code>, cạnh <code>[0, 1]</code> bị xóa, tạo ra hai thành phần liên thông <code>{0}</code>, <code>{1, 2}</code>.</li>
    <li>Tại <code>time = 4</code>, cạnh <code>[1, 2]</code> bị xóa, tạo ra <code>k = 3</code> thành phần liên thông <code>{0}</code>, <code>{1}</code>, <code>{2}</code>. Do đó, đáp án là 4.</li>
</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">n = 3, edges = [[0,2,5]], k = 2</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong></p>

<p><img src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/3600-3699/3608.Minimum%20Time%20for%20K%20Connected%20Components/images/screenshot-2025-06-01-at-022930.png" style="width: 180px; height: 155px;" /></p>

<ul>
    <li>Vì đã có sẵn <code>k = 2</code> thành phần không liên thông <code>{1}</code>, <code>{0, 2}</code>, nên không cần xóa cạnh nào. Do đó, đáp án là 0.</li>
</ul>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= n &lt;= 10<sup>5</sup></code></li>
    <li><code>0 &lt;= edges.length &lt;= 10<sup>5</sup></code></li>
    <li><code>edges[i] = [u<sub>i</sub>, v<sub>i</sub>, time<sub>i</sub>]</code></li>
    <li><code>0 &lt;= u<sub>i</sub>, v<sub>i</sub> &lt; n</code></li>
    <li><code>u<sub>i</sub> != v<sub>i</sub></code></li>
    <li><code>1 &lt;= time<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= k &lt;= n</code></li>
    <li>Không có cạnh trùng lặp.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Union-Find

<!-- thinking:start -->

> **Tư duy**
>
> Sau thời điểm $t$, các cạnh có trọng số lớn hơn $t$ biến mất. Việc dựng lại đồ thị cho từng giá trị $t$ là không thể với $m\le 10^5$ và trọng số lên đến $10^9$.
>
> Các cạnh chỉ biến mất, nên số lượng thành phần có tính đơn điệu. Thêm các cạnh từ trọng số lớn nhất đến nhỏ nhất tương đương với việc quay ngược thời gian.
>
> Bắt đầu với $n$ thành phần và giảm số lượng đi sau mỗi lần hợp nhất thành công. Khi lần hợp nhất tiếp theo làm số lượng giảm xuống dưới $k$, thời điểm của cạnh hiện tại là $t$ nhỏ nhất sao cho vẫn còn ít nhất $k$ thành phần. Nếu số lượng không bao giờ giảm xuống dưới $k$, đáp án là $0$.

<!-- thinking:end -->

Ta có thể sắp xếp các cạnh theo thời gian tăng dần, sau đó bắt đầu từ cạnh có thời gian lớn nhất và lần lượt thêm các cạnh vào đồ thị, đồng thời sử dụng cấu trúc dữ liệu union-find để duy trì số lượng thành phần liên thông trong đồ thị hiện tại. Khi số lượng thành phần liên thông nhỏ hơn $k$, thời gian hiện tại chính là thời gian nhỏ nhất cần tìm.

Độ phức tạp thời gian là $O(n \times \alpha(n))$, và độ phức tạp không gian là $O(n)$, trong đó $\alpha$ là hàm Ackermann ngược.

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
    def minTime(self, n: int, edges: List[List[int]], k: int) -> int:
        edges.sort(key=lambda x: x[2])
        uf = UnionFind(n)
        cnt = n
        for u, v, t in edges[::-1]:
            if uf.union(u, v):
                cnt -= 1
                if cnt < k:
                    return t
        return 0
```

#### Java

```java
class UnionFind {
    private int[] p;
    private int[] size;

    public UnionFind(int n) {
        p = new int[n];
        size = new int[n];
        for (int i = 0; i < n; i++) {
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
        int pa = find(a);
        int pb = find(b);
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
    public int minTime(int n, int[][] edges, int k) {
        Arrays.sort(edges, (a, b) -> Integer.compare(a[2], b[2]));

        UnionFind uf = new UnionFind(n);
        int cnt = n;

        for (int i = edges.length - 1; i >= 0; i--) {
            int u = edges[i][0];
            int v = edges[i][1];
            int t = edges[i][2];

            if (uf.union(u, v)) {
                if (--cnt < k) {
                    return t;
                }
            }
        }
        return 0;
    }
}
```

#### C++

```cpp
class UnionFind {
public:
    vector<int> p;
    vector<int> size;

    UnionFind(int n) {
        p.resize(n);
        size.resize(n, 1);
        for (int i = 0; i < n; i++) {
            p[i] = i;
        }
    }

    int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]);
        }
        return p[x];
    }

    bool unite(int a, int b) {
        int pa = find(a);
        int pb = find(b);
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
};

class Solution {
public:
    int minTime(int n, vector<vector<int>>& edges, int k) {
        sort(edges.begin(), edges.end(), [](const vector<int>& a, const vector<int>& b) {
            return a[2] < b[2];
        });

        UnionFind uf(n);
        int cnt = n;

        for (int i = edges.size() - 1; i >= 0; i--) {
            int u = edges[i][0];
            int v = edges[i][1];
            int t = edges[i][2];

            if (uf.unite(u, v)) {
                cnt--;
                if (cnt < k) {
                    return t;
                }
            }
        }
        return 0;
    }
};
```

#### Go

```go
type UnionFind struct {
    p    []int
    size []int
}

func NewUnionFind(n int) *UnionFind {
    uf := &UnionFind{
        p:    make([]int, n),
        size: make([]int, n),
    }
    for i := 0; i < n; i++ {
        uf.p[i] = i
        uf.size[i] = 1
    }
    return uf
}

func (uf *UnionFind) find(x int) int {
    if uf.p[x] != x {
        uf.p[x] = uf.find(uf.p[x])
    }
    return uf.p[x]
}

func (uf *UnionFind) union(a, b int) bool {
    pa := uf.find(a)
    pb := uf.find(b)
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

func minTime(n int, edges [][]int, k int) int {
    sort.Slice(edges, func(i, j int) bool {
        return edges[i][2] < edges[j][2]
    })

    uf := NewUnionFind(n)
    cnt := n

    for i := len(edges) - 1; i >= 0; i-- {
        u := edges[i][0]
        v := edges[i][1]
        t := edges[i][2]

        if uf.union(u, v) {
            cnt--
            if cnt < k {
                return t
            }
        }
    }
    return 0
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
        return true;
    }
}

function minTime(n: number, edges: number[][], k: number): number {
    edges.sort((a, b) => a[2] - b[2]);

    const uf = new UnionFind(n);
    let cnt = n;

    for (let i = edges.length - 1; i >= 0; i--) {
        const [u, v, t] = edges[i];

        if (uf.union(u, v)) {
            if (--cnt < k) {
                return t;
            }
        }
    }

    return 0;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
