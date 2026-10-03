---
comments: true
difficulty: Hard
rating: 2470
source: Biweekly Contest 79 Q4
tags:
    - Design
    - Binary Indexed Tree
    - Segment Tree
    - Binary Search
---

<!-- problem:start -->

# [2286. Booking Concert Tickets in Groups](https://leetcode.com/problems/booking-concert-tickets-in-groups)

[中文文档](/solution/2200-2299/2286.Booking%20Concert%20Tickets%20in%20Groups/README.md)

## Mô tả

<!-- description:start -->

<p>Một khán phòng có <code>n</code> hàng được đánh số từ <code>0</code> đến <code>n - 1</code>, mỗi hàng có <code>m</code> ghế được đánh số từ <code>0</code> đến <code>m - 1</code>. Hãy thiết kế một hệ thống bán vé có thể phân bổ ghế trong các trường hợp sau:</p>

<ul>
	<li>Một nhóm gồm <code>k</code> khán giả có thể ngồi <strong>liền nhau</strong> trong một hàng.</li>
	<li><strong>Mỗi</strong> thành viên của một nhóm gồm <code>k</code> khán giả đều có thể được xếp ghế. Họ có thể <strong>ngồi cùng nhau hoặc không</strong>.</li>
</ul>

<p>Lưu ý rằng các khán giả rất kén chọn. Vì vậy:</p>

<ul>
	<li>Họ chỉ đặt ghế khi mỗi thành viên trong nhóm đều có thể nhận một ghế có số hàng <strong>nhỏ hơn hoặc bằng</strong> <code>maxRow</code>. Giá trị <code>maxRow</code> có thể <strong>khác nhau</strong> giữa các nhóm.</li>
	<li>Nếu có nhiều hàng để lựa chọn, hàng có số <strong>nhỏ nhất</strong> sẽ được chọn. Nếu có nhiều ghế để lựa chọn trong cùng một hàng, ghế có số <strong>nhỏ nhất</strong> sẽ được chọn.</li>
</ul>

<p>Hãy triển khai lớp <code>BookMyShow</code>:</p>

<ul>
	<li><code>BookMyShow(int n, int m)</code> Khởi tạo đối tượng với <code>n</code> là số hàng và <code>m</code> là số ghế trong mỗi hàng.</li>
	<li><code>int[] gather(int k, int maxRow)</code> Trả về một mảng có độ dài <code>2</code>, lần lượt biểu diễn số hàng và số ghế của <strong>ghế đầu tiên</strong> được phân bổ cho <code>k</code> thành viên trong nhóm, những người phải <strong>ngồi cùng nhau</strong>. Nói cách khác, trả về <code>r</code> và <code>c</code> nhỏ nhất có thể sao cho tất cả các ghế <code>[c, c + k - 1]</code> đều hợp lệ và còn trống trong hàng <code>r</code>, đồng thời <code>r &lt;= maxRow</code>. Trả về <code>[]</code> nếu <strong>không thể</strong> phân bổ ghế cho nhóm.</li>
	<li><code>boolean scatter(int k, int maxRow)</code> Trả về <code>true</code> nếu có thể phân bổ ghế cho cả <code>k</code> thành viên trong các hàng từ <code>0</code> đến <code>maxRow</code>, họ có thể <strong>ngồi cùng nhau hoặc không</strong>. Nếu có thể phân bổ, cấp <code>k</code> ghế cho nhóm theo số hàng <strong>nhỏ nhất</strong>, và trong mỗi hàng chọn các ghế có số nhỏ nhất có thể. Ngược lại, trả về <code>false</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Ví dụ 1:</strong></p>

<pre>
<strong>Đầu vào</strong>
[&quot;BookMyShow&quot;, &quot;gather&quot;, &quot;gather&quot;, &quot;scatter&quot;, &quot;scatter&quot;]
[[2, 5], [4, 0], [2, 0], [5, 1], [5, 1]]
<strong>Đầu ra</strong>
[null, [0, 0], [], true, false]

<strong>Giải thích</strong>
BookMyShow bms = new BookMyShow(2, 5); // There are 2 rows with 5 seats each
bms.gather(4, 0); // return [0, 0]
                  // The group books seats [0, 3] of row 0.
bms.gather(2, 0); // return []
                  // There is only 1 seat left in row 0,
                  // so it is not possible to book 2 consecutive seats.
bms.scatter(5, 1); // return True
                   // The group books seat 4 of row 0 and seats [0, 3] of row 1.
bms.scatter(5, 1); // return False
                   // There is only one seat left in the hall.
</pre>

<p>&nbsp;</p>
<p><strong>Ràng buộc:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 5 * 10<sup>4</sup></code></li>
	<li><code>1 &lt;= m, k &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= maxRow &lt;= n - 1</code></li>
	<li>Có nhiều nhất <code>5 * 10<sup>4</sup></code> lần gọi <strong>tổng cộng</strong> đến <code>gather</code> và <code>scatter</code>.</li>
</ul>

<!-- description:end -->

## Lời giải

<!-- solution:start -->

### Lời giải 1: Segment Tree

<!-- thinking:start -->

> **Tư duy**
>
> $\textit{gather}$ cần $k$ ghế liên tiếp trong hàng nhỏ nhất có thể; $\textit{scatter}$ chỉ cần bất kỳ $k$ ghế nào trong các hàng nhỏ nhất. Có $5\times 10^4$ hàng, mỗi hàng có kích thước $10^9$, nên không thể dùng mảng ghế hoặc duyệt tuyến tính cho mỗi lần gọi.
>
> Segment tree trên các hàng lưu tổng số ghế còn lại $s$ và giá trị lớn nhất theo từng hàng $mx$. $\textit{gather}$ tìm hàng ngoài cùng bên trái có $mx\ge k$ rồi trừ đi $k$. $\textit{scatter}$ kiểm tra tổng trên đoạn, sau đó duyệt các hàng từ trái sang phải và trừ số ghế tương ứng.

<!-- thinking:end -->

Từ mô tả bài toán, ta có thể suy ra:

- Với thao tác `gather(k, maxRow)`, mục tiêu là xếp $k$ người vào cùng một hàng trên các ghế liên tiếp. Nói cách khác, ta cần tìm hàng nhỏ nhất có số ghế còn lại lớn hơn hoặc bằng $k$.
- Với thao tác `scatter(k, maxRow)`, ta chỉ cần tìm tổng cộng $k$ ghế, nhưng muốn tối thiểu hóa số hàng. Vì vậy, ta cần tìm hàng đầu tiên còn nhiều hơn $0$ ghế, phân bổ ghế ở đó, rồi tiếp tục tìm số ghế còn lại.

Ta có thể triển khai điều này bằng segment tree. Mỗi node của segment tree chứa các thông tin sau:

- `l`: Điểm đầu mút trái của đoạn mà node biểu diễn
- `r`: Điểm đầu mút phải của đoạn mà node biểu diễn
- `s`: Tổng số ghế còn lại trong đoạn tương ứng với node
- `mx`: Số ghế còn lại lớn nhất trong đoạn tương ứng với node

Lưu ý rằng miền chỉ số của segment tree bắt đầu từ $1$.

Các thao tác của segment tree:

- `build(u, l, r)`: Xây dựng node $u$, tương ứng với đoạn $[l, r]$, và đệ quy xây dựng các node con trái và phải.
- `modify(u, x, v)`: Bắt đầu từ node $u$, tìm node đầu tiên tương ứng với đoạn $[l, r]$ trong đó $l = r = x$, sửa giá trị `s` và `mx` của node này thành $v$, sau đó cập nhật ngược lên cây.
- `query_sum(u, l, r)`: Bắt đầu từ node $u$, tính tổng các giá trị `s` trong đoạn $[l, r]$.
- `query_idx(u, l, r, k)`: Bắt đầu từ node $u$, tìm node đầu tiên trong đoạn $[l, r]$ có `mx` lớn hơn hoặc bằng $k$, rồi trả về điểm đầu mút trái `l` của node đó. Khi tìm kiếm, ta bắt đầu từ đoạn lớn nhất $[1, maxRow]$. Vì cần tìm node ngoài cùng bên trái có `mx` lớn hơn hoặc bằng $k$, ta kiểm tra xem `mx` của nửa đầu đoạn có thỏa mãn điều kiện hay không. Nếu có, đáp án nằm ở nửa đầu và ta đệ quy tìm trong nửa đó. Nếu không, đáp án nằm ở nửa sau và ta đệ quy tìm trong nửa sau.
- `pushup(u)`: Cập nhật thông tin của node $u$ từ thông tin của các node con.

Với thao tác `gather(k, maxRow)`, trước tiên ta dùng `query_idx(1, 1, n, k)` để tìm hàng đầu tiên có số ghế còn lại lớn hơn hoặc bằng $k$, ký hiệu là $i$. Sau đó, ta dùng `query_sum(1, i, i)` để lấy số ghế còn lại trong hàng này, ký hiệu là $s$. Tiếp theo, ta dùng `modify(1, i, s - k)` để cập nhật số ghế còn lại của hàng này thành $s - k$, đồng thời cập nhật ngược lên cây. Cuối cùng, ta trả về kết quả $[i - 1, m - s]$.

Với thao tác `scatter(k, maxRow)`, trước tiên ta dùng `query_sum(1, 1, maxRow)` để tính tổng số ghế còn lại trong các hàng đầu tiên đến $maxRow$, ký hiệu là $s$. Nếu $s \lt k$, nghĩa là không đủ ghế, nên ta trả về `false`. Ngược lại, ta dùng `query_idx(1, 1, maxRow, 1)` để tìm hàng đầu tiên có số ghế còn lại lớn hơn hoặc bằng $1$, ký hiệu là $i$. Bắt đầu từ hàng này, ta dùng `query_sum(1, i, i)` để lấy số ghế còn lại trong hàng $i$, ký hiệu là $s_i$. Nếu $s_i \geq k$, ta trực tiếp dùng `modify(1, i, s_i - k)` để cập nhật số ghế còn lại của hàng này thành $s_i - k$, cập nhật ngược lên cây, rồi trả về `true`. Nếu không, ta cập nhật $k = k - s_i$, sửa số ghế còn lại của hàng này thành $0$, và cập nhật ngược lên cây. Cuối cùng, ta trả về `true`.

Độ phức tạp thời gian:

- Độ phức tạp thời gian khởi tạo là $O(n)$.
- Độ phức tạp thời gian của `gather(k, maxRow)` là $O(\log n)$.
- Độ phức tạp thời gian của `scatter(k, maxRow)` là $O((n + q) \times \log n)$.

Độ phức tạp thời gian tổng thể là $O(n + q \times \log n)$, và độ phức tạp không gian là $O(n)$. Trong đó, $n$ là số hàng và $q$ là số thao tác.

<!-- tabs:start -->

#### Python3

```python
class Node:
    __slots__ = "l", "r", "s", "mx"

    def __init__(self):
        self.l = self.r = 0
        self.s = self.mx = 0


class SegmentTree:
    def __init__(self, n, m):
        self.m = m
        self.tr = [Node() for _ in range(n << 2)]
        self.build(1, 1, n)

    def build(self, u, l, r):
        self.tr[u].l, self.tr[u].r = l, r
        if l == r:
            self.tr[u].s = self.tr[u].mx = self.m
            return
        mid = (l + r) >> 1
        self.build(u << 1, l, mid)
        self.build(u << 1 | 1, mid + 1, r)
        self.pushup(u)

    def modify(self, u, x, v):
        if self.tr[u].l == x and self.tr[u].r == x:
            self.tr[u].s = self.tr[u].mx = v
            return
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if x <= mid:
            self.modify(u << 1, x, v)
        else:
            self.modify(u << 1 | 1, x, v)
        self.pushup(u)

    def query_sum(self, u, l, r):
        if self.tr[u].l >= l and self.tr[u].r <= r:
            return self.tr[u].s
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        v = 0
        if l <= mid:
            v += self.query_sum(u << 1, l, r)
        if r > mid:
            v += self.query_sum(u << 1 | 1, l, r)
        return v

    def query_idx(self, u, l, r, k):
        if self.tr[u].mx < k:
            return 0
        if self.tr[u].l == self.tr[u].r:
            return self.tr[u].l
        mid = (self.tr[u].l + self.tr[u].r) >> 1
        if self.tr[u << 1].mx >= k:
            return self.query_idx(u << 1, l, r, k)
        if r > mid:
            return self.query_idx(u << 1 | 1, l, r, k)
        return 0

    def pushup(self, u):
        self.tr[u].s = self.tr[u << 1].s + self.tr[u << 1 | 1].s
        self.tr[u].mx = max(self.tr[u << 1].mx, self.tr[u << 1 | 1].mx)


class BookMyShow:
    def __init__(self, n: int, m: int):
        self.n = n
        self.tree = SegmentTree(n, m)

    def gather(self, k: int, maxRow: int) -> List[int]:
        maxRow += 1
        i = self.tree.query_idx(1, 1, maxRow, k)
        if i == 0:
            return []
        s = self.tree.query_sum(1, i, i)
        self.tree.modify(1, i, s - k)
        return [i - 1, self.tree.m - s]

    def scatter(self, k: int, maxRow: int) -> bool:
        maxRow += 1
        if self.tree.query_sum(1, 1, maxRow) < k:
            return False
        i = self.tree.query_idx(1, 1, maxRow, 1)
        for j in range(i, self.n + 1):
            s = self.tree.query_sum(1, j, j)
            if s >= k:
                self.tree.modify(1, j, s - k)
                return True
            k -= s
            self.tree.modify(1, j, 0)
        return True


# Your BookMyShow object will be instantiated and called as such:
# obj = BookMyShow(n, m)
# param_1 = obj.gather(k,maxRow)
# param_2 = obj.scatter(k,maxRow)
```

#### Java

```java
class Node {
    int l, r;
    long mx, s;
}

class SegmentTree {
    private Node[] tr;
    private int m;

    public SegmentTree(int n, int m) {
        this.m = m;
        tr = new Node[n << 2];
        for (int i = 0; i < tr.length; ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    private void build(int u, int l, int r) {
        tr[u].l = l;
        tr[u].r = r;
        if (l == r) {
            tr[u].s = m;
            tr[u].mx = m;
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    public void modify(int u, int x, long v) {
        if (tr[u].l == x && tr[u].r == x) {
            tr[u].s = v;
            tr[u].mx = v;
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

    public long querySum(int u, int l, int r) {
        if (tr[u].l >= l && tr[u].r <= r) {
            return tr[u].s;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        long v = 0;
        if (l <= mid) {
            v += querySum(u << 1, l, r);
        }
        if (r > mid) {
            v += querySum(u << 1 | 1, l, r);
        }
        return v;
    }

    public int queryIdx(int u, int l, int r, int k) {
        if (tr[u].mx < k) {
            return 0;
        }
        if (tr[u].l == tr[u].r) {
            return tr[u].l;
        }
        int mid = (tr[u].l + tr[u].r) >> 1;
        if (tr[u << 1].mx >= k) {
            return queryIdx(u << 1, l, r, k);
        }
        if (r > mid) {
            return queryIdx(u << 1 | 1, l, r, k);
        }
        return 0;
    }

    private void pushup(int u) {
        tr[u].s = tr[u << 1].s + tr[u << 1 | 1].s;
        tr[u].mx = Math.max(tr[u << 1].mx, tr[u << 1 | 1].mx);
    }
}

class BookMyShow {
    private int n;
    private int m;
    private SegmentTree tree;

    public BookMyShow(int n, int m) {
        this.n = n;
        this.m = m;
        tree = new SegmentTree(n, m);
    }

    public int[] gather(int k, int maxRow) {
        ++maxRow;
        int i = tree.queryIdx(1, 1, maxRow, k);
        if (i == 0) {
            return new int[] {};
        }
        long s = tree.querySum(1, i, i);
        tree.modify(1, i, s - k);
        return new int[] {i - 1, (int) (m - s)};
    }

    public boolean scatter(int k, int maxRow) {
        ++maxRow;
        if (tree.querySum(1, 1, maxRow) < k) {
            return false;
        }
        int i = tree.queryIdx(1, 1, maxRow, 1);
        for (int j = i; j <= n; ++j) {
            long s = tree.querySum(1, j, j);
            if (s >= k) {
                tree.modify(1, j, s - k);
                return true;
            }
            k -= s;
            tree.modify(1, j, 0);
        }
        return true;
    }
}

/**
 * Your BookMyShow object will be instantiated and called as such:
 * BookMyShow obj = new BookMyShow(n, m);
 * int[] param_1 = obj.gather(k,maxRow);
 * boolean param_2 = obj.scatter(k,maxRow);
 */
```

#### C++

```cpp
class Node {
public:
    int l, r;
    long s, mx;
};

class SegmentTree {
public:
    SegmentTree(int n, int m) {
        this->m = m;
        tr.resize(n << 2);
        for (int i = 0; i < n << 2; ++i) {
            tr[i] = new Node();
        }
        build(1, 1, n);
    }

    void modify(int u, int x, int v) {
        if (tr[u]->l == x && tr[u]->r == x) {
            tr[u]->s = tr[u]->mx = v;
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

    long querySum(int u, int l, int r) {
        if (tr[u]->l >= l && tr[u]->r <= r) {
            return tr[u]->s;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        long v = 0;
        if (l <= mid) {
            v += querySum(u << 1, l, r);
        }
        if (r > mid) {
            v += querySum(u << 1 | 1, l, r);
        }
        return v;
    }

    int queryIdx(int u, int l, int r, int k) {
        if (tr[u]->mx < k) {
            return 0;
        }
        if (tr[u]->l == tr[u]->r) {
            return tr[u]->l;
        }
        int mid = (tr[u]->l + tr[u]->r) >> 1;
        if (tr[u << 1]->mx >= k) {
            return queryIdx(u << 1, l, r, k);
        }
        if (r > mid) {
            return queryIdx(u << 1 | 1, l, r, k);
        }
        return 0;
    }

private:
    vector<Node*> tr;
    int m;

    void build(int u, int l, int r) {
        tr[u]->l = l;
        tr[u]->r = r;
        if (l == r) {
            tr[u]->s = m;
            tr[u]->mx = m;
            return;
        }
        int mid = (l + r) >> 1;
        build(u << 1, l, mid);
        build(u << 1 | 1, mid + 1, r);
        pushup(u);
    }

    void pushup(int u) {
        tr[u]->s = tr[u << 1]->s + tr[u << 1 | 1]->s;
        tr[u]->mx = max(tr[u << 1]->mx, tr[u << 1 | 1]->mx);
    }
};

class BookMyShow {
public:
    BookMyShow(int n, int m) {
        this->n = n;
        this->m = m;
        tree = new SegmentTree(n, m);
    }

    vector<int> gather(int k, int maxRow) {
        ++maxRow;
        int i = tree->queryIdx(1, 1, maxRow, k);
        if (i == 0) {
            return {};
        }
        long s = tree->querySum(1, i, i);
        tree->modify(1, i, s - k);
        return {i - 1, (int) (m - s)};
    }

    bool scatter(int k, int maxRow) {
        ++maxRow;
        if (tree->querySum(1, 1, maxRow) < k) {
            return false;
        }
        int i = tree->queryIdx(1, 1, maxRow, 1);
        for (int j = i; j <= n; ++j) {
            long s = tree->querySum(1, j, j);
            if (s >= k) {
                tree->modify(1, j, s - k);
                return true;
            }
            k -= s;
            tree->modify(1, j, 0);
        }
        return true;
    }

private:
    SegmentTree* tree;
    int m, n;
};

/**
 * Your BookMyShow object will be instantiated and called as such:
 * BookMyShow* obj = new BookMyShow(n, m);
 * vector<int> param_1 = obj->gather(k,maxRow);
 * bool param_2 = obj->scatter(k,maxRow);
 */
```

#### Go

```go
type BookMyShow struct {
	n, m int
	tree *segmentTree
}

func Constructor(n int, m int) BookMyShow {
	return BookMyShow{n, m, newSegmentTree(n, m)}
}

func (this *BookMyShow) Gather(k int, maxRow int) []int {
	maxRow++
	i := this.tree.queryIdx(1, 1, maxRow, k)
	if i == 0 {
		return []int{}
	}
	s := this.tree.querySum(1, i, i)
	this.tree.modify(1, i, s-k)
	return []int{i - 1, this.m - s}
}

func (this *BookMyShow) Scatter(k int, maxRow int) bool {
	maxRow++
	if this.tree.querySum(1, 1, maxRow) < k {
		return false
	}
	i := this.tree.queryIdx(1, 1, maxRow, 1)
	for j := i; j <= this.n; j++ {
		s := this.tree.querySum(1, j, j)
		if s >= k {
			this.tree.modify(1, j, s-k)
			return true
		}
		k -= s
		this.tree.modify(1, j, 0)
	}
	return true
}

type node struct {
	l, r, s, mx int
}

type segmentTree struct {
	tr []*node
	m  int
}

func newSegmentTree(n, m int) *segmentTree {
	tr := make([]*node, n<<2)
	for i := range tr {
		tr[i] = &node{}
	}
	t := &segmentTree{tr, m}
	t.build(1, 1, n)
	return t
}

func (t *segmentTree) build(u, l, r int) {
	t.tr[u].l, t.tr[u].r = l, r
	if l == r {
		t.tr[u].s, t.tr[u].mx = t.m, t.m
		return
	}
	mid := (l + r) >> 1
	t.build(u<<1, l, mid)
	t.build(u<<1|1, mid+1, r)
	t.pushup(u)
}

func (t *segmentTree) modify(u, x, v int) {
	if t.tr[u].l == x && t.tr[u].r == x {
		t.tr[u].s, t.tr[u].mx = v, v
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

func (t *segmentTree) querySum(u, l, r int) int {
	if t.tr[u].l >= l && t.tr[u].r <= r {
		return t.tr[u].s
	}
	mid := (t.tr[u].l + t.tr[u].r) >> 1
	v := 0
	if l <= mid {
		v = t.querySum(u<<1, l, r)
	}
	if r > mid {
		v += t.querySum(u<<1|1, l, r)
	}
	return v
}

func (t *segmentTree) queryIdx(u, l, r, k int) int {
	if t.tr[u].mx < k {
		return 0
	}
	if t.tr[u].l == t.tr[u].r {
		return t.tr[u].l
	}
	mid := (t.tr[u].l + t.tr[u].r) >> 1
	if t.tr[u<<1].mx >= k {
		return t.queryIdx(u<<1, l, r, k)
	}
	if r > mid {
		return t.queryIdx(u<<1|1, l, r, k)
	}
	return 0
}

func (t *segmentTree) pushup(u int) {
	t.tr[u].s = t.tr[u<<1].s + t.tr[u<<1|1].s
	t.tr[u].mx = max(t.tr[u<<1].mx, t.tr[u<<1|1].mx)
}

/**
 * Your BookMyShow object will be instantiated and called as such:
 * obj := Constructor(n, m);
 * param_1 := obj.Gather(k,maxRow);
 * param_2 := obj.Scatter(k,maxRow);
 */
```

#### TypeScript

```ts
class Node {
    l: number;
    r: number;
    mx: number;
    s: number;

    constructor() {
        this.l = 0;
        this.r = 0;
        this.mx = 0;
        this.s = 0;
    }
}

class SegmentTree {
    private tr: Node[];
    private m: number;

    constructor(n: number, m: number) {
        this.m = m;
        this.tr = Array.from({ length: n << 2 }, () => new Node());
        this.build(1, 1, n);
    }

    private build(u: number, l: number, r: number): void {
        this.tr[u].l = l;
        this.tr[u].r = r;
        if (l === r) {
            this.tr[u].s = this.m;
            this.tr[u].mx = this.m;
            return;
        }
        const mid = (l + r) >> 1;
        this.build(u << 1, l, mid);
        this.build((u << 1) | 1, mid + 1, r);
        this.pushup(u);
    }

    public modify(u: number, x: number, v: number): void {
        if (this.tr[u].l === x && this.tr[u].r === x) {
            this.tr[u].s = v;
            this.tr[u].mx = v;
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

    public querySum(u: number, l: number, r: number): number {
        if (this.tr[u].l >= l && this.tr[u].r <= r) {
            return this.tr[u].s;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        let v = 0;
        if (l <= mid) {
            v += this.querySum(u << 1, l, r);
        }
        if (r > mid) {
            v += this.querySum((u << 1) | 1, l, r);
        }
        return v;
    }

    public queryIdx(u: number, l: number, r: number, k: number): number {
        if (this.tr[u].mx < k) {
            return 0;
        }
        if (this.tr[u].l === this.tr[u].r) {
            return this.tr[u].l;
        }
        const mid = (this.tr[u].l + this.tr[u].r) >> 1;
        if (this.tr[u << 1].mx >= k) {
            return this.queryIdx(u << 1, l, r, k);
        }
        if (r > mid) {
            return this.queryIdx((u << 1) | 1, l, r, k);
        }
        return 0;
    }

    private pushup(u: number): void {
        this.tr[u].s = this.tr[u << 1].s + this.tr[(u << 1) | 1].s;
        this.tr[u].mx = Math.max(this.tr[u << 1].mx, this.tr[(u << 1) | 1].mx);
    }
}

class BookMyShow {
    private n: number;
    private m: number;
    private tree: SegmentTree;

    constructor(n: number, m: number) {
        this.n = n;
        this.m = m;
        this.tree = new SegmentTree(n, m);
    }

    public gather(k: number, maxRow: number): number[] {
        ++maxRow;
        const i = this.tree.queryIdx(1, 1, maxRow, k);
        if (i === 0) {
            return [];
        }
        const s = this.tree.querySum(1, i, i);
        this.tree.modify(1, i, s - k);
        return [i - 1, this.m - s];
    }

    public scatter(k: number, maxRow: number): boolean {
        ++maxRow;
        if (this.tree.querySum(1, 1, maxRow) < k) {
            return false;
        }
        let i = this.tree.queryIdx(1, 1, maxRow, 1);
        for (let j = i; j <= this.n; ++j) {
            const s = this.tree.querySum(1, j, j);
            if (s >= k) {
                this.tree.modify(1, j, s - k);
                return true;
            }
            k -= s;
            this.tree.modify(1, j, 0);
        }
        return true;
    }
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
