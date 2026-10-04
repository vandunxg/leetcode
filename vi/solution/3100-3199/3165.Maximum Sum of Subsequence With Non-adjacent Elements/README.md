---
comments: true
difficulty: Hard
rating: 2697
source: Weekly Contest 399 Q4
tags:
    - Segment Tree
    - Array
    - Divide and Conquer
    - Dynamic Programming
---

<!-- problem:start -->

# [3165. Maximum Sum of Subsequence With Non-adjacent Elements](https://leetcode.com/problems/maximum-sum-of-subsequence-with-non-adjacent-elements)

[中文文档](/solution/3100-3199/3165.Maximum%20Sum%20of%20Subsequence%20With%20Non-adjacent%20Elements/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho một mảng <code>nums</code> gồm các số nguyên. Bạn cũng được cho một mảng 2D <code>queries</code>, trong đó <code>queries[i] = [pos<sub>i</sub>, x<sub>i</sub>]</code>.</p>

<p>Với truy vấn <code>i</code>, trước tiên ta gán <code>nums[pos<sub>i</sub>]</code> bằng <code>x<sub>i</sub></code>, sau đó tính đáp án của truy vấn <code>i</code>, là tổng <strong>lớn nhất</strong> của một <span data-keyword="subsequence-array">dãy con</span> của <code>nums</code> sao cho <strong>không chọn hai phần tử liền kề</strong>.</p>

<p>Trả về <em>tổng</em> các đáp án của tất cả truy vấn.</p>

<p>Vì đáp án cuối cùng có thể rất lớn, hãy trả về đáp án <strong>theo modulo</strong> <code>10<sup>9</sup> + 7</code>.</p>

<p><strong>Dãy con</strong> là một mảng có thể được tạo ra từ một mảng khác bằng cách xóa một số hoặc không xóa phần tử nào mà không thay đổi thứ tự của các phần tử còn lại.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [3,5,9], queries = [[1,-2],[0,-3]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">21</span></p>

<p><strong>Giải thích:</strong><br />
Sau truy vấn thứ <sup>1</sup>, <code>nums = [3,-2,9]</code> và tổng lớn nhất của một dãy con có các phần tử không liền kề là <code>3 + 9 = 12</code>.<br />
Sau truy vấn thứ <sup>2</sup>, <code>nums = [-3,-2,9]</code> và tổng lớn nhất của một dãy con có các phần tử không liền kề là 9.</p>
</div>

<p><strong class="example">Ví dụ 2:</strong></p>

<div class="example-block">
<p><strong>Đầu vào:</strong> <span class="example-io">nums = [0,-1], queries = [[0,-5]]</span></p>

<p><strong>Đầu ra:</strong> <span class="example-io">0</span></p>

<p><strong>Giải thích:</strong><br />
Sau truy vấn thứ <sup>1</sup>, <code>nums = [-5,-1]</code> và tổng lớn nhất của một dãy con có các phần tử không liền kề là 0 (chọn dãy con rỗng).</p>
</div>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
    <li><code>1 &lt;= nums.length &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>-10<sup>5</sup> &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
    <li><code>1 &lt;= queries.length &lt;= 5 * 10<sup>4</sup></code></li>
    <li><code>queries[i] == [pos<sub>i</sub>, x<sub>i</sub>]</code></li>
    <li><code>0 &lt;= pos<sub>i</sub> &lt;= nums.length - 1</code></li>
    <li><code>-10<sup>5</sup> &lt;= x<sub>i</sub> &lt;= 10<sup>5</sup></code></li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Sau mỗi lần cập nhật một điểm, ta cần tìm tổng lớn nhất của dãy con không có hai phần tử liền kề. Tính lại DP tuyến tính cho mỗi truy vấn sẽ tốn $O(nq)$.
>
> Khi gộp các đoạn, chỉ cần biết mỗi đầu mút có được chọn hay không, từ đó có bốn trạng thái $s_{00},s_{01},s_{10},s_{11}$.
>
> Xây dựng một segment tree, thực hiện từng phép gán và truy vấn $s_{11}$ trên toàn bộ đoạn. Cộng các đáp án đó theo modulo $10^9+7$.

<!-- thinking:end -->

Theo mô tả bài toán, ta cần thực hiện nhiều lần cập nhật tại một điểm và truy vấn trên đoạn. Trong trường hợp này, ta có thể dùng segment tree để giải bài toán.

Trước tiên, ta định nghĩa một lớp $\textit{Node}$ để lưu thông tin của các nút trong segment tree, bao gồm hai đầu mút trái và phải $l$ và $r$, cùng với bốn giá trị trạng thái $s_{00}$, $s_{01}$, $s_{10}$ và $s_{11}$. Cụ thể:

- $s_{00}$ biểu diễn tổng lớn nhất của dãy con không bao gồm đầu mút trái và đầu mút phải của nút hiện tại;
- $s_{01}$ biểu diễn tổng lớn nhất của dãy con không bao gồm đầu mút trái của nút hiện tại;
- $s_{10}$ biểu diễn tổng lớn nhất của dãy con không bao gồm đầu mút phải của nút hiện tại;
- $s_{11}$ biểu diễn tổng lớn nhất của dãy con bao gồm cả đầu mút trái và đầu mút phải của nút hiện tại.

Tiếp theo, ta định nghĩa lớp $\textit{SegmentTree}$ để xây dựng segment tree. Trong quá trình xây dựng segment tree, ta cần đệ quy xây dựng cây con trái và cây con phải, sau đó cập nhật các giá trị trạng thái của nút hiện tại dựa trên các giá trị trạng thái của hai cây con.

Trong hàm chính, trước tiên ta xây dựng segment tree dựa trên mảng $\textit{nums}$ đã cho và xử lý từng truy vấn. Với mỗi truy vấn, trước tiên ta thực hiện cập nhật tại một điểm, sau đó truy vấn các giá trị trạng thái trên toàn bộ đoạn và cộng dồn kết quả vào đáp án.

Độ phức tạp thời gian là $O((n + q) \times \log n)$, còn độ phức tạp không gian là $O(n)$. Trong đó, $n$ là độ dài của mảng $\textit{nums}$ và $q$ là số lượng truy vấn.

<!-- tabs:start -->

#### Python3

```python
def max(a: int, b: int) -> int:
    return a if a > b else b


class Node:
    __slots__ = "l", "r", "s00", "s01", "s10", "s11"

    def __init__(self, l: int, r: int):
        self.l = l
        self.r = r
        self.s00 = self.s01 = self.s10 = self.s11 = 0


class SegmentTree:
    __slots__ = "tr"

    def __init__(self, n: int):
        self.tr: List[Node | None] = [None] * (n << 2)
        self.build(1, 1, n)

    def build(self, u: int, l: int, r: int):
        self.tr[u] = Node(l, r)
        if l == r:
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)

    def query(self, u: int, l: int, r: int) -> int:
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].s11
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        ans = 0
        if r <= mid:
            ans = self.query(u << 1, l, r)
        if l > mid:
            ans = max(ans, self.query(u << 1 | 1, l, r))
        return ans

    def pushup(self, u: int):
        left, right = self.tr[u << 1], self.tr[u << 1 | 1]
        self.tr[u].s00 = max(left.s00 + right.s10, left.s01 + right.s00)
        self.tr[u].s01 = max(left.s00 + right.s11, left.s01 + right.s01)
        self.tr[u].s10 = max(left.s10 + right.s10, left.s11 + right.s00)
        self.tr[u].s11 = max(left.s10 + right.s11, left.s11 + right.s01)

    def modify(self, u: int, x: int, v: int):
        if self.tr[u].l == self.tr[u].r:
            self.tr[u].s11 = max(0, v)
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)


class Solution:
    def maximumSumSubsequence(self, nums: List[int], queries: List[List[int]]) -> int:
        n = len(nums)
        tree = SegmentTree(n)
        for i, x in enumerate(nums, 1):
            tree.modify(1, i, x)
        ans = 0
        mod = 10**9 + 7
        for i, x in queries:
            tree.modify(1, i + 1, x)
            ans = (ans + tree.query(1, 1, n)) % mod
        return ans
```

#### Java

```java
class Node {
    int l, r;
    long s00, s01, s10, s11;

    Node(int l, int r) {
        this.l = l;
        this.r = r;
        this.s00 = this.s01 = this.s10 = this.s11 = 0;
    }
}

class SegmentTree {
    Node[] tr;

    SegmentTree(int n) {
        tr = new Node[n * 4];
        build(1, 1, n);
    }

    void build(int u, int l, int r) {
        tr[u] = new Node(l, r);
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    long query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].s11;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        long ans = 0;
        if (r <= mid) {
            ans = query(u << 1, l, r);
        }
        if (l > mid) {
            ans = Math.max(ans, query(u << 1 | 1, l, r));
        }
        return ans;
    }

    void pushup(int u) {
        Node left = tr[u << 1];
        Node right = tr[u << 1 | 1];
        tr[u].s00 = Math.max(left.s00 + right.s10, left.s01 + right.s00);
        tr[u].s01 = Math.max(left.s00 + right.s11, left.s01 + right.s01);
        tr[u].s10 = Math.max(left.s10 + right.s10, left.s11 + right.s00);
        tr[u].s11 = Math.max(left.s10 + right.s11, left.s11 + right.s01);
    }

    void modify(int u, int x, int v) {
        if (tr[u].l == tr[u].r) {
            tr[u].s11 = Math.max(0, v);
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
}

class Solution {
    public int maximumSumSubsequence(int[] nums, int[][] queries) {
        int n = nums.length;
        SegmentTree tree = new SegmentTree(n);
        for (int i = 0; i < n; ++i) {
            tree.modify(1, i + 1, nums[i]);
        }
        long ans = 0;
        final int mod = (int) 1e9 + 7;
        for (int[] q : queries) {
            tree.modify(1, q[0] + 1, q[1]);
            ans = (ans + tree.query(1, 1, n)) % mod;
        }
        return (int) ans;
    }
}
```

#### C++

```cpp
class Node {
public:
    int l, r;
    long long s00, s01, s10, s11;

    Node(int l, int r)
        : l(l)
        , r(r)
        , s00(0)
        , s01(0)
        , s10(0)
        , s11(0) {}
};

class SegmentTree {
public:
    vector<Node*> tr;

    SegmentTree(int n)
        : tr(n << 2) {
        build(1, 1, n);
    }

    void build(int u, int l, int r) {
        tr[u] = new Node(l, r);
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    long long query(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) {
            return tr[u]->s11;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        long long ans = 0;
        if (r <= mid) {
            ans = query(u << 1, l, r);
        }
        if (l > mid) {
            ans = max(ans, query(u << 1 | 1, l, r));
        }
        return ans;
    }

    void pushup(int u) {
        Node* left = tr[u << 1];
        Node* right = tr[u << 1 | 1];
        tr[u]->s00 = max(left->s00 + right->s10, left->s01 + right->s00);
        tr[u]->s01 = max(left->s00 + right->s11, left->s01 + right->s01);
        tr[u]->s10 = max(left->s10 + right->s10, left->s11 + right->s00);
        tr[u]->s11 = max(left->s10 + right->s11, left->s11 + right->s01);
    }

    void modify(int u, int x, int v) {
        if (tr[u]->l == tr[u]->r) {
            tr[u]->s11 = max(0LL, (long long) v);
            return;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (x <= mid) {
            modify(u << 1, x, v);
        } else {
            modify(u << 1 | 1, x, v);
        }
        pushup(u);
    }

    ~SegmentTree() {
        for (auto node : tr) {
            delete node;
        }
    }
};

class Solution {
public:
    int maximumSumSubsequence(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        SegmentTree tree(n);
        for (int i = 0; i < n; ++i) {
            tree.modify(1, i + 1, nums[i]);
        }
        long long ans = 0;
        const int mod = 1e9 + 7;
        for (const auto& q : queries) {
            tree.modify(1, q[0] + 1, q[1]);
            ans = (ans + tree.query(1, 1, n)) % mod;
        }
        return (int) ans;
    }
};
```

#### Go

```go
type Node struct {
    l, r               int
    s00, s01, s10, s11 int
}

func NewNode(l, r int) *Node {
    return &Node{l: l, r: r, s00: 0, s01: 0, s10: 0, s11: 0}
}

type SegmentTree struct {
    tr []*Node
}

func NewSegmentTree(n int) *SegmentTree {
    tr := make([]*Node, n*4)
    tree := &SegmentTree{tr: tr}
    tree.build(1, 1, n)
    return tree
}

func (st *SegmentTree) build(u, l, r int) {
    st.tr[u] = NewNode(l, r)
    if l == r {
        return
    }
    mid := (l + r) >> 1
    st.build(u<<1, l, mid)
    st.build(u<<1|1, mid+1, r)
}

func (st *SegmentTree) query(u, l, r int) int {
    if st.tr[u].l >= l && st.tr[u].r <= r {
        return st.tr[u].s11
    }
    mid := (st.tr[u].l + st.tr[u].r) >> 1
    ans := 0
    if r <= mid {
        ans = st.query(u<<1, l, r)
    }
    if l > mid {
        ans = max(ans, st.query(u<<1|1, l, r))
    }
    return ans
}

func (st *SegmentTree) pushup(u int) {
    left := st.tr[u<<1]
    right := st.tr[u<<1|1]
    st.tr[u].s00 = max(left.s00+right.s10, left.s01+right.s00)
    st.tr[u].s01 = max(left.s00+right.s11, left.s01+right.s01)
    st.tr[u].s10 = max(left.s10+right.s10, left.s11+right.s00)
    st.tr[u].s11 = max(left.s10+right.s11, left.s11+right.s01)
}

func (st *SegmentTree) modify(u, x, v int) {
    if st.tr[u].l == st.tr[u].r {
        st.tr[u].s11 = max(0, v)
        return
    }
    mid := (st.tr[u].l + st.tr[u].r) >> 1
    if x <= mid {
        st.modify(u<<1, x, v)
    } else {
        st.modify(u<<1|1, x, v)
    }
    st.pushup(u)
}

func maximumSumSubsequence(nums []int, queries [][]int) (ans int) {
    n := len(nums)
    tree := NewSegmentTree(n)
    for i, x := range nums {
        tree.modify(1, i+1, x)
    }
    const mod int = 1e9 + 7
    for _, q := range queries {
        tree.modify(1, q[0]+1, q[1])
        ans = (ans + tree.query(1, 1, n)) % mod
    }
    return
}
```

#### TypeScript

```ts
class Node {
    s00 = 0;
    s01 = 0;
    s10 = 0;
    s11 = 0;

    constructor(
        public l: number,
        public r: number,
    ) {}
}

class SegmentTree {
    tr: Node[];

    constructor(n: number) {
        this.tr = Array(n * 4);
        this.build(1, 1, n);
    }

    build(u: number, l: number, r: number) {
        this.tr[u] = new Node(l, r);
        if (l === r) {
            return;
        }
        const mid = (l + r) >> 1;
        this.build(u << 1, l, mid);
        this.build((u << 1) | 1, mid + 1, r);
    }

    query(u: number, l: number, r: number): number {
        if (this.tr[u].l >= l && this.tr[u].r <= r) {
            return this.tr[u].s11;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        let ans = 0;
        if (r <= mid) {
            ans = this.query(u << 1, l, r);
        }
        if (l > mid) {
            ans = Math.max(ans, this.query((u << 1) | 1, l, r));
        }
        return ans;
    }

    pushup(u: number) {
        const left = this.tr[u << 1];
        const right = this.tr[(u << 1) | 1];
        this.tr[u].s00 = Math.max(left.s00 + right.s10, left.s01 + right.s00);
        this.tr[u].s01 = Math.max(left.s00 + right.s11, left.s01 + right.s01);
        this.tr[u].s10 = Math.max(left.s10 + right.s10, left.s11 + right.s00);
        this.tr[u].s11 = Math.max(left.s10 + right.s11, left.s11 + right.s01);
    }

    modify(u: number, x: number, v: number) {
        if (this.tr[u].l === this.tr[u].r) {
            this.tr[u].s11 = Math.max(0, v);
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
}

function maximumSumSubsequence(nums: number[], queries: number[][]): number {
    const n = nums.length;
    const tree = new SegmentTree(n);
    for (let i = 0; i < n; i++) {
        tree.modify(1, i + 1, nums[i]);
    }
    let ans = 0;
    const mod = 1e9 + 7;
    for (const [i, x] of queries) {
        tree.modify(1, i + 1, x);
        ans = (ans + tree.query(1, 1, n)) % mod;
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
