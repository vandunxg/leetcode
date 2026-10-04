---
comments: true
difficulty: Hard
rating: 2644
source: Weekly Contest 446 Q4
tags:
    - Segment Tree
    - Array
    - Math
---

<!-- problem:start -->

# [3525. Find X Value of Array II](https://leetcode.com/problems/find-x-value-of-array-ii)

[中文文档](/solution/3500-3599/3525.Find%20X%20Value%20of%20Array%20II/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng các số nguyên <strong>dương</strong> <code>nums</code> và một số nguyên <strong>dương</strong> <code>k</code>. Bạn cũng được cho một mảng 2 chiều <code>queries</code>, trong đó <code>queries[i] = [index<sub>i</sub>, value<sub>i</sub>, start<sub>i</sub>, x<sub>i</sub>]</code>.</p>

<p>Bạn được phép thực hiện một thao tác <strong>một lần</strong> trên <code>nums</code>, trong đó bạn có thể xóa một <strong>hậu tố</strong> bất kỳ khỏi <code>nums</code> sao cho <code>nums</code> vẫn <strong>không rỗng</strong>.</p>

<p><strong>x-value</strong> của <code>nums</code> <strong>với một</strong> <code>x</code> cho trước được định nghĩa là số cách thực hiện thao tác này sao cho <strong>tích</strong> các phần tử còn lại có <em>phần dư</em> là <code>x</code> <strong>theo modulo</strong> <code>k</code>.</p>

<p>Với mỗi truy vấn trong <code>queries</code>, bạn cần xác định <strong>x-value</strong> của <code>nums</code> với <code>x<sub>i</sub></code> sau khi thực hiện các hành động sau:</p>

<ul>
    <li>Cập nhật <code>nums[index<sub>i</sub>]</code> thành <code>value<sub>i</sub></code>. Chỉ bước này được duy trì cho các truy vấn còn lại.</li>
    <li><strong>Xóa</strong> tiền tố <code>nums[0..(start<sub>i</sub> - 1)]</code> (trong đó <code>nums[0..(-1)]</code> được dùng để biểu diễn tiền tố <strong>rỗng</strong>).</li>
</ul>

<p>Trả về một mảng <code>result</code> có kích thước <code>queries.length</code>, trong đó <code>result[i]</code> là đáp án của truy vấn thứ <code>i<sup>th</sup></code>.</p>

<p><strong>Tiền tố</strong> của một mảng là một <span data-keyword="subarray">mảng con</span> bắt đầu từ đầu mảng và kéo dài đến một vị trí bất kỳ trong mảng.</p>

<p><strong>Hậu tố</strong> của một mảng là một <span data-keyword="subarray">mảng con</span> bắt đầu tại một vị trí bất kỳ trong mảng và kéo dài đến cuối mảng.</p>

<p><strong>Lưu ý</strong> rằng tiền tố và hậu tố được chọn cho thao tác có thể <strong>rỗng</strong>.</p>

<p><strong>Lưu ý</strong> rằng x-value có một định nghĩa <em>khác</em> trong phiên bản này.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,3,4,5], k = 3, queries = [[2,2,0,2],[3,3,3,0],[0,1,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[2,2,2]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với truy vấn 0, <code>nums</code> trở thành <code>[1, 2, 2, 4, 5]</code>, và tiền tố rỗng <strong>bắt buộc</strong> phải được xóa. Các thao tác có thể thực hiện là:

    <ul>
        <li>Xóa hậu tố <code>[2, 4, 5]</code>. <code>nums</code> trở thành <code>[1, 2]</code>.</li>
        <li>Xóa hậu tố rỗng. <code>nums</code> trở thành <code>[1, 2, 2, 4, 5]</code> với tích bằng 80, cho phần dư 2 khi chia cho 3.</li>
    </ul>
    </li>
    <li>Với truy vấn 1, <code>nums</code> trở thành <code>[1, 2, 2, 3, 5]</code>, và tiền tố <code>[1, 2, 2]</code> <strong>bắt buộc</strong> phải được xóa. Các thao tác có thể thực hiện là:
    <ul>
        <li>Xóa hậu tố rỗng. <code>nums</code> trở thành <code>[3, 5]</code>.</li>
        <li>Xóa hậu tố <code>[5]</code>. <code>nums</code> trở thành <code>[3]</code>.</li>
    </ul>
    </li>
    <li>Với truy vấn 2, <code>nums</code> trở thành <code>[1, 2, 2, 3, 5]</code>, và tiền tố rỗng <strong>bắt buộc</strong> phải được xóa. Các thao tác có thể thực hiện là:
    <ul>
        <li>Xóa hậu tố <code>[2, 2, 3, 5]</code>. <code>nums</code> trở thành <code>[1]</code>.</li>
        <li>Xóa hậu tố <code>[3, 5]</code>. <code>nums</code> trở thành <code>[1, 2, 2]</code>.</li>
    </ul>
    </li>

</ul>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,2,4,8,16,32], k = 4, queries = [[0,2,0,2],[0,2,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[1,0]</span></p>

<p><strong>Giải thích:</strong></p>

<ul>
    <li>Với truy vấn 0, <code>nums</code> trở thành <code>[2, 2, 4, 8, 16, 32]</code>. Thao tác duy nhất có thể thực hiện là:

    <ul>
        <li>Xóa hậu tố <code>[2, 4, 8, 16, 32]</code>.</li>
    </ul>
    </li>
    <li>Với truy vấn 1, <code>nums</code> trở thành <code>[2, 2, 4, 8, 16, 32]</code>. Không có cách nào để thực hiện thao tác.</li>

</ul>
</div>

<p><strong class="example">Ví dụ 3:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [1,1,2,1,1], k = 2, queries = [[2,1,0,1]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">[5]</span></p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
    <li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= k &lt;= 5</code></li>
    <li><code>1 &lt;= queries.length &lt;= 2 * 10<sup>4</sup></code></li>
    <li><code>queries[i] == [index<sub>i</sub>, value<sub>i</sub>, start<sub>i</sub>, x<sub>i</sub>]</code></li>
    <li><code>0 &lt;= index<sub>i</sub> &lt;= nums.length - 1</code></li>
    <li><code>1 &lt;= value<sub>i</sub> &lt;= 10<sup>9</sup></code></li>
    <li><code>0 &lt;= start<sub>i</sub> &lt;= nums.length - 1</code></li>
    <li><code>0 &lt;= x<sub>i</sub> &lt;= k - 1</code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Cách đếm tĩnh từ bài trước không còn phù hợp khi có cập nhật một điểm và yêu cầu xóa tiền tố. Vì $k \le 5$, mỗi đoạn chỉ cần lưu tích theo modulo $k$ và số cách tạo ra từng phần dư sau khi xóa một hậu tố.
>
> Ta lưu thông tin này trong segment tree và định nghĩa phép gộp. Sau mỗi lần cập nhật, truy vấn phần dư cần tìm trên đoạn $[start, n)$.

<!-- thinking:end -->

Mỗi truy vấn trước tiên gán $nums[\textit{index}]$ bằng $\textit{value}$ (cập nhật này được duy trì), sau đó xóa tiền tố $nums[0..start-1]$. Tiếp theo, ta chỉ có thể xóa một hậu tố, nên phần còn lại là một tiền tố không rỗng của $nums[start..n-1]$. Vì vậy, truy vấn cần đếm số tiền tố của $[start, n)$ có tích đồng dư với $x$ theo modulo $k$.

Vì $k \le 5$, mỗi node của segment tree lưu:

- $\textit{prod}$: tích của toàn bộ đoạn theo modulo $k$
- $\textit{cnt}[r]$: số tiền tố của đoạn này có tích bằng $r$ theo modulo $k$

Một node lá với $a = nums[i] \bmod k$ có $\textit{prod} = a$ và $\textit{cnt}[a] = 1$.

Gộp hai node con trái và phải $L$ và $R$:

$$
P.\textit{prod} = (L.\textit{prod} \times R.\textit{prod}) \bmod k
$$

Các tiền tố nằm hoàn toàn trong $L$ được sao chép từ $L.\textit{cnt}$. Các tiền tố lấy toàn bộ $L$ rồi thêm một tiền tố của $R$ sẽ đóng góp $R.\textit{cnt}[r]$ vào phần dư $(L.\textit{prod} \times r) \bmod k$.

Sau khi cập nhật một điểm, truy vấn $\textit{cnt}[x]$ trên $[start+1, n]$ (đánh chỉ số từ 1). Khi gộp trong lúc truy vấn, phải kết hợp node trái trước rồi đến node phải.

Độ phức tạp thời gian là $O((n + q) \times k \times \log n)$ và độ phức tạp không gian là $O(n \times k)$, trong đó $n$ là độ dài mảng và $q$ là số truy vấn.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = "l", "r", "prod", "cnt"

    def __init__(self, l: int, r: int, k: int):
        self.l = l
        self.r = r
        self.prod = 1
        self.cnt = [0] * k


class SegmentTree:
    __slots__ = "k", "tr"

    def __init__(self, nums: list[int], k: int):
        self.k = k
        n = len(nums)
        self.tr = [None] * (n << 2)
        self.build(1, 1, n, nums)

    def merge(self, a: Node, b: Node) -> tuple[int, list[int]]:
        k = self.k
        prod = a.prod * b.prod % k
        cnt = a.cnt[:]
        for r, c in enumerate(b.cnt):
            cnt[a.prod * r % k] += c
        return prod, cnt

    def pushup(self, u: int):
        prod, cnt = self.merge(self.tr[u << 1], self.tr[u << 1 | 1])
        self.tr[u].prod = prod
        self.tr[u].cnt = cnt

    def build(self, u: int, l: int, r: int, nums: list[int]):
        self.tr[u] = Node(l, r, self.k)
        if l == r:
            v = nums[l - 1] % self.k
            self.tr[u].prod = v
            self.tr[u].cnt[v] = 1
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid, nums)
        self.build(u << 1 | 1, mid + 1, r, nums)
        self.pushup(u)

    def modify(self, u: int, x: int, v: int):
        if self.tr[u].l == self.tr[u].r:
            v %= self.k
            self.tr[u].prod = v
            self.tr[u].cnt = [0] * self.k
            self.tr[u].cnt[v] = 1
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def query(self, u: int, l: int, r: int) -> Node:
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u]
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if r <= mid:
            return self.query(u << 1, l, r)
        if l > mid:
            return self.query(u << 1 | 1, l, r)
        left = self.query(u << 1, l, r)
        right = self.query(u << 1 | 1, l, r)
        prod, cnt = self.merge(left, right)
        res = Node(0, 0, self.k)
        res.prod = prod
        res.cnt = cnt
        return res


class Solution:
    def resultArray(
        self, nums: list[int], k: int, queries: list[list[int]]
    ) -> list[int]:
        n = len(nums)
        tree = SegmentTree(nums, k)
        ans = []
        for idx, val, start, x in queries:
            tree.modify(1, idx + 1, val)
            ans.append(tree.query(1, start + 1, n).cnt[x])
        return ans
```

#### Java

```java
class Node {
    int l, r, prod;
    int[] cnt;

    Node(int l, int r, int k) {
        this.l = l;
        this.r = r;
        this.prod = 1;
        this.cnt = new int[k];
    }
}

class SegmentTree {
    private int k;
    private Node[] tr;

    SegmentTree(int[] nums, int k) {
        this.k = k;
        int n = nums.length;
        tr = new Node[n << 2];
        build(1, 1, n, nums);
    }

    private Node merge(Node a, Node b) {
        Node c = new Node(0, 0, k);
        c.prod = a.prod * b.prod % k;
        System.arraycopy(a.cnt, 0, c.cnt, 0, k);
        for (int r = 0; r < k; ++r) {
            c.cnt[a.prod * r % k] += b.cnt[r];
        }
        return c;
    }

    private void pushup(int u) {
        Node p = merge(tr[u << 1], tr[u << 1 | 1]);
        tr[u].prod = p.prod;
        tr[u].cnt = p.cnt;
    }

    private void build(int u, int l, int r, int[] nums) {
        tr[u] = new Node(l, r, k);
        if (l == r) {
            int v = nums[l - 1] % k;
            tr[u].prod = v;
            tr[u].cnt[v] = 1;
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid, nums);
        build(u << 1 | 1, mid + 1, r, nums);
        pushup(u);
    }

    void modify(int u, int x, int v) {
        if (tr[u].l == tr[u].r) {
            v %= k;
            tr[u].prod = v;
            Arrays.fill(tr[u].cnt, 0);
            tr[u].cnt[v] = 1;
            return;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (x <= mid) {
            modify(u << 1, x, v);
        } else {
            modify(u << 1 | 1, x, v);
        }
        pushup(u);
    }

    Node query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u];
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (r <= mid) {
            return query(u << 1, l, r);
        }
        if (l > mid) {
            return query(u << 1 | 1, l, r);
        }
        return merge(query(u << 1, l, r), query(u << 1 | 1, l, r));
    }
}

class Solution {
    public int[] resultArray(int[] nums, int k, int[][] queries) {
        int n = nums.length;
        SegmentTree tree = new SegmentTree(nums, k);
        int[] ans = new int[queries.length];
        for (int i = 0; i < queries.length; ++i) {
            int idx = queries[i][0], val = queries[i][1], start = queries[i][2], x = queries[i][3];
            tree.modify(1, idx + 1, val);
            ans[i] = tree.query(1, start + 1, n).cnt[x];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Node {
public:
    int l = 0, r = 0;
    int prod = 1;
    int cnt[5]{};
};

class SegmentTree {
public:
    SegmentTree(vector<int>& nums, int k) {
        this->k = k;
        int n = nums.size();
        tr.resize(n << 2);
        build(1, 1, n, nums);
    }

    void modify(int u, int x, int v) {
        if (tr[u].l == tr[u].r) {
            v %= k;
            tr[u].prod = v;
            memset(tr[u].cnt, 0, sizeof(tr[u].cnt));
            tr[u].cnt[v] = 1;
            return;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (x <= mid) {
            modify(u << 1, x, v);
        } else {
            modify(u << 1 | 1, x, v);
        }
        pushup(u);
    }

    Node query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u];
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (r <= mid) {
            return query(u << 1, l, r);
        }
        if (l > mid) {
            return query(u << 1 | 1, l, r);
        }
        return merge(query(u << 1, l, r), query(u << 1 | 1, l, r));
    }

private:
    int k;
    vector<Node> tr;

    Node merge(const Node& a, const Node& b) {
        Node c;
        c.prod = a.prod * b.prod % k;
        memcpy(c.cnt, a.cnt, sizeof(c.cnt));
        for (int r = 0; r < k; ++r) {
            c.cnt[a.prod * r % k] += b.cnt[r];
        }
        return c;
    }

    void pushup(int u) {
        Node p = merge(tr[u << 1], tr[u << 1 | 1]);
        tr[u].prod = p.prod;
        memcpy(tr[u].cnt, p.cnt, sizeof(tr[u].cnt));
    }

    void build(int u, int l, int r, vector<int>& nums) {
        tr[u].l = l;
        tr[u].r = r;
        if (l == r) {
            int v = nums[l - 1] % k;
            tr[u].prod = v;
            tr[u].cnt[v] = 1;
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid, nums);
        build(u << 1 | 1, mid + 1, r, nums);
        pushup(u);
    }
};

class Solution {
public:
    vector<int> resultArray(vector<int>& nums, int k, vector<vector<int>>& queries) {
        int n = nums.size();
        SegmentTree tree(nums, k);
        vector<int> ans;
        ans.reserve(queries.size());
        for (auto& q : queries) {
            tree.modify(1, q[0] + 1, q[1]);
            ans.push_back(tree.query(1, q[2] + 1, n).cnt[q[3]]);
        }
        return ans;
    }
};
```

#### Go

```go
type node struct {
    l, r, prod int
    cnt        []int
}

type segmentTree struct {
    k  int
    tr []*node
}

func newSegmentTree(nums []int, k int) *segmentTree {
    n := len(nums)
    tr := make([]*node, n<<2)
    t := &segmentTree{k, tr}
    t.build(1, 1, n, nums)
    return t
}

func (t *segmentTree) merge(a, b *node) *node {
    c := &node{prod: a.prod * b.prod % t.k, cnt: make([]int, t.k)}
    copy(c.cnt, a.cnt)
    for r, v := range b.cnt {
        c.cnt[a.prod*r%t.k] += v
    }
    return c
}

func (t *segmentTree) pushup(u int) {
    p := t.merge(t.tr[u<<1], t.tr[u<<1|1])
    t.tr[u].prod = p.prod
    copy(t.tr[u].cnt, p.cnt)
}

func (t *segmentTree) build(u, l, r int, nums []int) {
    t.tr[u] = &node{l: l, r: r, prod: 1, cnt: make([]int, t.k)}
    if l == r {
        v := nums[l-1] % t.k
        t.tr[u].prod = v
        t.tr[u].cnt[v] = 1
        return
    }
    mid := (l + r) >> 1
    t.build(u<<1, l, mid, nums)
    t.build(u<<1|1, mid+1, r, nums)
    t.pushup(u)
}

func (t *segmentTree) modify(u, x, v int) {
    if t.tr[u].l == t.tr[u].r {
        v %= t.k
        t.tr[u].prod = v
        for i := range t.tr[u].cnt {
            t.tr[u].cnt[i] = 0
        }
        t.tr[u].cnt[v] = 1
        return
    }
    mid := (t.tr[u].l + t.tr[u].r) >> 1
    if x <= mid {
        t.modify(u<<1, x, v)
    } else {
        t.modify(u<<1|1, x, v)
    }
    t.pushup(u)
}

func (t *segmentTree) query(u, l, r int) *node {
    if t.tr[u].l >= l && t.tr[u].r <= r {
        return t.tr[u]
    }
    mid := (t.tr[u].l + t.tr[u].r) >> 1
    if r <= mid {
        return t.query(u<<1, l, r)
    }
    if l > mid {
        return t.query(u<<1|1, l, r)
    }
    return t.merge(t.query(u<<1, l, r), t.query(u<<1|1, l, r))
}

func resultArray(nums []int, k int, queries [][]int) []int {
    n := len(nums)
    tree := newSegmentTree(nums, k)
    ans := make([]int, len(queries))
    for i, q := range queries {
        tree.modify(1, q[0]+1, q[1])
        ans[i] = tree.query(1, q[2]+1, n).cnt[q[3]]
    }
    return ans
}
```

#### TypeScript

```ts
class Node {
    l: number;
    r: number;
    prod: number;
    cnt: number[];

    constructor(l: number, r: number, k: number) {
        this.l = l;
        this.r = r;
        this.prod = 1;
        this.cnt = Array(k).fill(0);
    }
}

class SegmentTree {
    private k: number;
    private tr: Node[];

    constructor(nums: number[], k: number) {
        this.k = k;
        this.tr = Array(nums.length << 2);
        this.build(1, 1, nums.length, nums);
    }

    private merge(a: Node, b: Node): Node {
        const c = new Node(0, 0, this.k);
        c.prod = (a.prod * b.prod) % this.k;
        c.cnt = a.cnt.slice();
        for (let r = 0; r < this.k; ++r) {
            c.cnt[(a.prod * r) % this.k] += b.cnt[r];
        }
        return c;
    }

    private pushup(u: number): void {
        const p = this.merge(this.tr[u << 1], this.tr[(u << 1) | 1]);
        this.tr[u].prod = p.prod;
        this.tr[u].cnt = p.cnt;
    }

    private build(u: number, l: number, r: number, nums: number[]): void {
        this.tr[u] = new Node(l, r, this.k);
        if (l === r) {
            const v = nums[l - 1] % this.k;
            this.tr[u].prod = v;
            this.tr[u].cnt[v] = 1;
            return;
        }
        const mid = (l + r) >> 1;
        this.build(u << 1, l, mid, nums);
        this.build((u << 1) | 1, mid + 1, r, nums);
        this.pushup(u);
    }

    modify(u: number, x: number, v: number): void {
        if (this.tr[u].l === this.tr[u].r) {
            v %= this.k;
            this.tr[u].prod = v;
            this.tr[u].cnt.fill(0);
            this.tr[u].cnt[v] = 1;
            return;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        if (x <= mid) {
            this.modify(u << 1, x, v);
        } else {
            this.modify((u << 1) | 1, x, v);
        }
        this.pushup(u);
    }

    query(u: number, l: number, r: number): Node {
        if (this.tr[u].l >= l && this.tr[u].r <= r) {
            return this.tr[u];
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        if (r <= mid) {
            return this.query(u << 1, l, r);
        }
        if (l > mid) {
            return this.query((u << 1) | 1, l, r);
        }
        return this.merge(this.query(u << 1, l, r), this.query((u << 1) | 1, l, r));
    }
}

function resultArray(nums: number[], k: number, queries: number[][]): number[] {
    const n = nums.length;
    const tree = new SegmentTree(nums, k);
    const ans: number[] = [];
    for (const [idx, val, start, x] of queries) {
        tree.modify(1, idx + 1, val);
        ans.push(tree.query(1, start + 1, n).cnt[x]);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
