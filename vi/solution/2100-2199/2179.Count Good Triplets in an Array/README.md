---
comments: true
difficulty: Hard
rating: 2272
source: Biweekly Contest 72 Q4
tags:
    - Binary Indexed Tree
    - Segment Tree
    - Array
    - Binary Search
    - Divide and Conquer
    - Ordered Set
    - Merge Sort
---

<!-- problem:start -->

# [2179. Count Good Triplets in an Array](https://leetcode.com/problems/count-good-triplets-in-an-array)

[Tài liệu tiếng Trung](/solution/2100-2199/2179.Count%20Good%20Triplets%20in%20an%20Array/README.md)

## Mô tả

<!-- description:start -->

<p>Bạn được cho hai mảng <strong>0-indexed</strong> <code>nums1</code> và <code>nums2</code> có độ dài <code>n</code>, cả hai đều là <strong>hoán vị</strong> của <code>[0, 1, ..., n - 1]</code>.</p>

<p>Một <strong>bộ ba hợp lệ</strong> là một tập hợp gồm <code>3</code> giá trị <strong>phân biệt</strong>, xuất hiện theo <strong>thứ tự tăng dần</strong> về vị trí trong cả <code>nums1</code> và <code>nums2</code>. Nói cách khác, nếu gọi <code>pos1<sub>v</sub></code> là chỉ số của giá trị <code>v</code> trong <code>nums1</code> và <code>pos2<sub>v</sub></code> là chỉ số của giá trị <code>v</code> trong <code>nums2</code>, thì một bộ ba hợp lệ là một tập hợp <code>(x, y, z)</code> với <code>0 &lt;= x, y, z &lt;= n - 1</code>, sao cho <code>pos1<sub>x</sub> &lt; pos1<sub>y</sub> &lt; pos1<sub>z</sub></code> và <code>pos2<sub>x</sub> &lt; pos2<sub>y</sub> &lt; pos2<sub>z</sub></code>.</p>

<p>Trả về <em><strong>tổng số</strong> bộ ba hợp lệ</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [2,0,1,3], nums2 = [0,1,2,3]
<strong>Output:</strong> 1
<strong>Giải thích:</strong>
Có 4 bộ ba (x,y,z) sao cho pos1<sub>x</sub> &lt; pos1<sub>y</sub> &lt; pos1<sub>z</sub>. Đó là (2,0,1), (2,0,3), (2,1,3) và (0,1,3).
Trong số các bộ ba đó, chỉ có bộ ba (0,1,3) thỏa mãn pos2<sub>x</sub> &lt; pos2<sub>y</sub> &lt; pos2<sub>z</sub>. Vì vậy, chỉ có 1 bộ ba hợp lệ.
</pre>

<p><strong class="example">Ví dụ 2:</strong></p>

<pre>
<strong>Input:</strong> nums1 = [4,0,1,3,2], nums2 = [4,1,0,2,3]
<strong>Output:</strong> 4
<strong>Giải thích:</strong> 4 bộ ba hợp lệ là (4,0,3), (4,0,2), (4,1,3) và (4,1,2).
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>n == nums1.length == nums2.length</code></li>
	<li><code>3 &lt;= n &lt;= 10<sup>5</sup></code></li>
	<li><code>0 &lt;= nums1[i], nums2[i] &lt;= n - 1</code></li>
	<li><code>nums1</code> và <code>nums2</code> là các hoán vị của <code>[0, 1, ..., n - 1]</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Binary Indexed Tree (Fenwick Tree)

<!-- thinking:start -->

> **Tư duy**
>
> Một bộ ba hợp lệ có cùng thứ tự tương đối trong cả hai hoán vị. Việc liệt kê các bộ ba có độ phức tạp $O(n^3)$ và không đáp ứng được với $n\le 10^5$. Khi cố định giá trị ở giữa, số lượng bên trái là số phần tử đã duyệt trong $\textit{nums1}$ nằm trước nó trong $\textit{nums2}$; số lượng bên phải được tính đối xứng trên phần hậu tố chưa dùng.
>
> Duyệt $\textit{nums1}$, truy vấn cây Fenwick tại vị trí của phần tử trong $\textit{nums2}$ để lấy số lượng phần tử trong prefix và số lượng phần tử chưa dùng ở suffix, rồi cộng tích của hai số này.
>
> Các vị trí được đánh số từ $1$; gọi $\texttt{update}$ sau mỗi giá trị.

<!-- thinking:end -->

Với bài toán này, trước tiên chúng ta dùng `pos` để lưu vị trí của mỗi số trong `nums2`, sau đó lần lượt xử lý từng phần tử của `nums1`.

Xét các bộ ba hợp lệ **có số hiện tại làm số ở giữa**. Số đầu tiên phải đã được duyệt và phải xuất hiện trước số hiện tại trong `nums2`. Số thứ ba chưa được duyệt và phải xuất hiện sau số hiện tại trong `nums2`.

Lấy `nums1 = [4,0,1,3,2]` và `nums2 = [4,1,0,2,3]` làm ví dụ. Xét quá trình duyệt:

1. Đầu tiên, xử lý `4`. Lúc này, trạng thái của `nums2` là `[4,X,X,X,X]`. Có `0` giá trị trước `4` và `4` giá trị sau `4`. Vì vậy, khi `4` là số ở giữa thì tạo thành `0` bộ ba hợp lệ.
2. Tiếp theo, xử lý `0`. Trạng thái của `nums2` trở thành `[4,X,0,X,X]`. Có `1` giá trị trước `0` và `2` giá trị sau `0`. Vì vậy, khi `0` là số ở giữa thì tạo thành `2` bộ ba hợp lệ.
3. Tiếp theo, xử lý `1`. Trạng thái của `nums2` trở thành `[4,1,0,X,X]`. Có `1` giá trị trước `1` và `2` giá trị sau `1`. Vì vậy, khi `1` là số ở giữa thì tạo thành `2` bộ ba hợp lệ.
4. ...
5. Cuối cùng, xử lý `2`. Trạng thái của `nums2` trở thành `[4,1,0,2,3]`. Có `4` giá trị trước `2` và `0` giá trị sau `2`. Vì vậy, khi `2` là số ở giữa thì tạo thành `0` bộ ba hợp lệ.

Chúng ta có thể dùng **Binary Indexed Tree (Fenwick Tree)** để cập nhật sự xuất hiện của các giá trị tại từng vị trí trong `nums2`, đồng thời nhanh chóng tính số lượng `1` ở bên trái mỗi giá trị và số lượng `0` ở bên phải mỗi giá trị.

Binary Indexed Tree, còn gọi là Fenwick Tree, hỗ trợ hiệu quả các thao tác sau:

1. **Point Update** `update(x, delta)`: Cộng giá trị `delta` vào phần tử tại vị trí `x` trong dãy.
2. **Prefix Sum Query** `query(x)`: Tính tổng dãy trong đoạn `[1, ..., x]`, tức tổng prefix tại vị trí `x`.

Cả hai thao tác đều có độ phức tạp thời gian $O(\log n)$. Vì vậy, độ phức tạp thời gian tổng thể là $O(n \log n)$, trong đó $n$ là độ dài của mảng $\textit{nums1}$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class BinaryIndexedTree:
    def __init__(self, n):
        self.n = n
        self.c = [0] * (n + 1)

    @staticmethod
    def lowbit(x):
        return x & -x

    def update(self, x, delta):
        while x <= self.n:
            self.c[x] += delta
            x += BinaryIndexedTree.lowbit(x)

    def query(self, x):
        s = 0
        while x > 0:
            s += self.c[x]
            x -= BinaryIndexedTree.lowbit(x)
        return s


class Solution:
    def goodTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        pos = {v: i for i, v in enumerate(nums2, 1)}
        ans = 0
        n = len(nums1)
        tree = BinaryIndexedTree(n)
        for num in nums1:
            p = pos[num]
            left = tree.query(p)
            right = n - p - (tree.query(n) - tree.query(p))
            ans += left * right
            tree.update(p, 1)
        return ans
```

#### Java

```java
class Solution {
    public long goodTriplets(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int[] pos = new int[n];
        BinaryIndexedTree tree = new BinaryIndexedTree(n);
        for (int i = 0; i < n; ++i) {
            pos[nums2[i]] = i + 1;
        }
        long ans = 0;
        for (int num : nums1) {
            int p = pos[num];
            long left = tree.query(p);
            long right = n - p - (tree.query(n) - tree.query(p));
            ans += left * right;
            tree.update(p, 1);
        }
        return ans;
    }
}

class BinaryIndexedTree {
    private int n;
    private int[] c;

    public BinaryIndexedTree(int n) {
        this.n = n;
        c = new int[n + 1];
    }

    public void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += lowbit(x);
        }
    }

    public int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    public static int lowbit(int x) {
        return x & -x;
    }
}
```

#### C++

```cpp
class BinaryIndexedTree {
public:
    int n;
    vector<int> c;

    BinaryIndexedTree(int _n)
        : n(_n)
        , c(_n + 1) {}

    void update(int x, int delta) {
        while (x <= n) {
            c[x] += delta;
            x += lowbit(x);
        }
    }

    int query(int x) {
        int s = 0;
        while (x > 0) {
            s += c[x];
            x -= lowbit(x);
        }
        return s;
    }

    int lowbit(int x) {
        return x & -x;
    }
};

class Solution {
public:
    long long goodTriplets(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<int> pos(n);
        for (int i = 0; i < n; ++i) pos[nums2[i]] = i + 1;
        BinaryIndexedTree* tree = new BinaryIndexedTree(n);
        long long ans = 0;
        for (int& num : nums1) {
            int p = pos[num];
            int left = tree->query(p);
            int right = n - p - (tree->query(n) - tree->query(p));
            ans += 1ll * left * right;
            tree->update(p, 1);
        }
        return ans;
    }
};
```

#### Go

```go
type BinaryIndexedTree struct {
	n int
	c []int
}

func newBinaryIndexedTree(n int) *BinaryIndexedTree {
	c := make([]int, n+1)
	return &BinaryIndexedTree{n, c}
}

func (this *BinaryIndexedTree) lowbit(x int) int {
	return x & -x
}

func (this *BinaryIndexedTree) update(x, delta int) {
	for x <= this.n {
		this.c[x] += delta
		x += this.lowbit(x)
	}
}

func (this *BinaryIndexedTree) query(x int) int {
	s := 0
	for x > 0 {
		s += this.c[x]
		x -= this.lowbit(x)
	}
	return s
}

func goodTriplets(nums1 []int, nums2 []int) int64 {
	n := len(nums1)
	pos := make([]int, n)
	for i, v := range nums2 {
		pos[v] = i + 1
	}
	tree := newBinaryIndexedTree(n)
	var ans int64
	for _, num := range nums1 {
		p := pos[num]
		left := tree.query(p)
		right := n - p - (tree.query(n) - tree.query(p))
		ans += int64(left) * int64(right)
		tree.update(p, 1)
	}
	return ans
}
```

#### TypeScript

```ts
class BinaryIndexedTree {
    private c: number[];
    private n: number;

    constructor(n: number) {
        this.n = n;
        this.c = Array(n + 1).fill(0);
    }

    private static lowbit(x: number): number {
        return x & -x;
    }

    update(x: number, delta: number): void {
        while (x <= this.n) {
            this.c[x] += delta;
            x += BinaryIndexedTree.lowbit(x);
        }
    }

    query(x: number): number {
        let s = 0;
        while (x > 0) {
            s += this.c[x];
            x -= BinaryIndexedTree.lowbit(x);
        }
        return s;
    }
}

function goodTriplets(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    const pos = new Map<number, number>();
    nums2.forEach((v, i) => pos.set(v, i + 1));

    const tree = new BinaryIndexedTree(n);
    let ans = 0;

    for (const num of nums1) {
        const p = pos.get(num)!;
        const left = tree.query(p);
        const total = tree.query(n);
        const right = n - p - (total - left);
        ans += left * right;
        tree.update(p, 1);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- solution:start -->

### Lời giải 2: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> Lời giải 1 lưu số lượng phần tử được chèn trong prefix bằng Fenwick tree. Segment tree với phép cộng tại một điểm và tính tổng trên đoạn hỗ trợ các truy vấn tương tự với cùng độ phức tạp tiệm cận.
>
> Mỗi node lưu số lượng giá trị đã xuất hiện trong đoạn của nó; truy vấn $[1,p]$ và số lượng phần tử chưa dùng trong $(p,n]$, cộng tích của hai số này, rồi tăng giá trị tại điểm đó.
>
> Phần trình bày dưới đây ghi lại cách cài đặt segment tree.

<!-- thinking:end -->

Chúng ta cũng có thể dùng segment tree để giải bài toán này. Segment tree là một cấu trúc dữ liệu hỗ trợ hiệu quả các truy vấn và cập nhật trên đoạn. Ý tưởng cơ bản là chia một đoạn thành nhiều đoạn con, mỗi đoạn con được biểu diễn bởi một node.

Segment tree chia toàn bộ đoạn thành nhiều đoạn con không giao nhau, với số đoạn con không vượt quá `log(width)`. Để cập nhật giá trị của một phần tử, chúng ta chỉ cần cập nhật `log(width)` đoạn, tất cả đều nằm trong một đoạn lớn hơn có chứa phần tử đó.

- Mỗi node của segment tree biểu diễn một đoạn.
- Segment tree có duy nhất một node gốc, biểu diễn toàn bộ đoạn, chẳng hạn `[1, N]`.
- Mỗi node lá của segment tree biểu diễn một đoạn đơn vị `[x, x]`.
- Với mỗi node trong `[l, r]`, node con trái biểu diễn `[l, mid]`, còn node con phải biểu diễn `[mid + 1, r]`, trong đó `mid = ⌊(l + r) / 2⌋` (phép chia lấy phần nguyên).

Độ phức tạp thời gian là $O(n \log n)$, trong đó $n$ là độ dài của mảng $\textit{nums1}$. Độ phức tạp không gian là $O(n)$.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = ("l", "r", "v")

    def __init__(self):
        self.l = 0
        self.r = 0
        self.v = 0


class SegmentTree:
    def __init__(self, n):
        self.tr = [Node() for _ in range(4 * n)]
        self.build(1, 1, n)

    def build(self, u, l, r):
        self.tr[u].l = l
        self.tr[u].r = r
        if l == r:
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)

    def modify(self, u, x, v):
        if self.tr[u].l == x and self.tr[u].r == x:
            self.tr[u].v += v
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def pushup(self, u):
        self.tr[u].v = self.tr[u << 1].v + self.tr[u << 1 | 1].v

    def query(self, u, l, r):
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].v
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        v = 0
        if l <= mid:
            v += self.query(u << 1, l, r)
        if r > mid:
            v += self.query(u << 1 | 1, l, r)
        return v


class Solution:
    def goodTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        pos = {v: i for i, v in enumerate(nums2, 1)}
        ans = 0
        n = len(nums1)
        tree = SegmentTree(n)
        for num in nums1:
            p = pos[num]
            left = tree.query(1, 1, p)
            right = n - p - (tree.query(1, 1, n) - tree.query(1, 1, p))
            ans += left * right
            tree.modify(1, p, 1)
        return ans
```

#### Java

```java
class Solution {
    public long goodTriplets(int[] nums1, int[] nums2) {
        int n = nums1.length;
        int[] pos = new int[n];
        SegmentTree tree = new SegmentTree(n);
        for (int i = 0; i < n; ++i) {
            pos[nums2[i]] = i + 1;
        }
        long ans = 0;
        for (int num : nums1) {
            int p = pos[num];
            long left = tree.query(1, 1, p);
            long right = n - p - (tree.query(1, 1, n) - tree.query(1, 1, p));
            ans += left * right;
            tree.modify(1, p, 1);
        }
        return ans;
    }
}

class Node {
    int l;
    int r;
    int v;
}

class SegmentTree {
    private Node[] tr;

    public SegmentTree(int n) {
        tr = new Node[4 * n];
        for (int i = 0; i < tr.length; ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    public void build(int u, int l, int r) {
        tr[u].l = l;
        tr[u].r = r;
        if (l == r) {
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    public void modify(int u, int x, int v) {
        if (tr[u].l == x && tr[u].r == x) {
            tr[u].v += v;
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

    public void pushup(int u) {
        tr[u].v = tr[u << 1].v + tr[u << 1 | 1].v;
    }

    public int query(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].v;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        int v = 0;
        if (l <= mid) {
            v += query(u << 1, l, r);
        }
        if (r > mid) {
            v += query(u << 1 | 1, l, r);
        }
        return v;
    }
}
```

#### C++

```cpp
class Node {
public:
    int l;
    int r;
    int v;
};

class SegmentTree {
public:
    vector<Node*> tr;

    SegmentTree(int n) {
        tr.resize(4 * n);
        for (int i = 0; i < tr.size(); ++i) tr[i] = new Node();
        build(1, 1, n);
    }

    void build(int u, int l, int r) {
        tr[u]->l = l;
        tr[u]->r = r;
        if (l == r) return;
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
    }

    void modify(int u, int x, int v) {
        if (tr[u]->l == x && tr[u]->r == x) {
            tr[u]->v += v;
            return;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (x <= mid)
            modify(u << 1, x, v);
        else
            modify(u << 1 | 1, x, v);
        pushup(u);
    }

    void pushup(int u) {
        tr[u]->v = tr[u << 1]->v + tr[u << 1 | 1]->v;
    }

    int query(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) return tr[u]->v;
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        int v = 0;
        if (l <= mid) v += query(u << 1, l, r);
        if (r > mid) v += query(u << 1 | 1, l, r);
        return v;
    }
};

class Solution {
public:
    long long goodTriplets(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        vector<int> pos(n);
        for (int i = 0; i < n; ++i) pos[nums2[i]] = i + 1;
        SegmentTree* tree = new SegmentTree(n);
        long long ans = 0;
        for (int& num : nums1) {
            int p = pos[num];
            int left = tree->query(1, 1, p);
            int right = n - p - (tree->query(1, 1, n) - tree->query(1, 1, p));
            ans += 1ll * left * right;
            tree->modify(1, p, 1);
        }
        return ans;
    }
};
```

#### Go

```go
type Node struct {
	l, r, v int
}

type SegmentTree struct {
	tr []Node
}

func NewSegmentTree(n int) *SegmentTree {
	tr := make([]Node, 4*n)
	st := &SegmentTree{tr: tr}
	st.build(1, 1, n)
	return st
}

func (st *SegmentTree) build(u, l, r int) {
	st.tr[u].l = l
	st.tr[u].r = r
	if l == r {
		return
	}
	mid := (l + r) >> 1
	st.build(u<<1, l, mid)
	st.build(u<<1|1, mid+1, r)
}

func (st *SegmentTree) modify(u, x, v int) {
	if st.tr[u].l == x && st.tr[u].r == x {
		st.tr[u].v += v
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

func (st *SegmentTree) pushup(u int) {
	st.tr[u].v = st.tr[u<<1].v + st.tr[u<<1|1].v
}

func (st *SegmentTree) query(u, l, r int) int {
	if st.tr[u].l >= l && st.tr[u].r <= r {
		return st.tr[u].v
	}
	mid := (st.tr[u].l + st.tr[u].r) >> 1
	res := 0
	if l <= mid {
		res += st.query(u<<1, l, r)
	}
	if r > mid {
		res += st.query(u<<1|1, l, r)
	}
	return res
}

func goodTriplets(nums1 []int, nums2 []int) int64 {
	n := len(nums1)
	pos := make(map[int]int)
	for i, v := range nums2 {
		pos[v] = i + 1
	}

	tree := NewSegmentTree(n)
	var ans int64

	for _, num := range nums1 {
		p := pos[num]
		left := tree.query(1, 1, p)
		right := n - p - (tree.query(1, 1, n) - tree.query(1, 1, p))
		ans += int64(left * right)
		tree.modify(1, p, 1)
	}

	return ans
}
```

#### TypeScript

```ts
class Node {
    l: number = 0;
    r: number = 0;
    v: number = 0;
}

class SegmentTree {
    private tr: Node[];

    constructor(n: number) {
        this.tr = Array(4 * n);
        for (let i = 0; i < 4 * n; i++) {
            this.tr[i] = new Node();
        }
        this.build(1, 1, n);
    }

    private build(u: number, l: number, r: number): void {
        this.tr[u].l = l;
        this.tr[u].r = r;
        if (l === r) return;
        const mid = (l + r) >> 1;
        this.build(u << 1, l, mid);
        this.build((u << 1) | 1, mid + 1, r);
    }

    modify(u: number, x: number, v: number): void {
        if (this.tr[u].l === x && this.tr[u].r === x) {
            this.tr[u].v += v;
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

    private pushup(u: number): void {
        this.tr[u].v = this.tr[u << 1].v + this.tr[(u << 1) | 1].v;
    }

    query(u: number, l: number, r: number): number {
        if (this.tr[u].l >= l && this.tr[u].r <= r) {
            return this.tr[u].v;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        let res = 0;
        if (l <= mid) {
            res += this.query(u << 1, l, r);
        }
        if (r > mid) {
            res += this.query((u << 1) | 1, l, r);
        }
        return res;
    }
}

function goodTriplets(nums1: number[], nums2: number[]): number {
    const n = nums1.length;
    const pos = new Map<number, number>();
    nums2.forEach((v, i) => pos.set(v, i + 1));

    const tree = new SegmentTree(n);
    let ans = 0;

    for (const num of nums1) {
        const p = pos.get(num)!;
        const left = tree.query(1, 1, p);
        const total = tree.query(1, 1, n);
        const right = n - p - (total - left);
        ans += left * right;
        tree.modify(1, p, 1);
    }

    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
